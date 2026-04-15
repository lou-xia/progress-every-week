## 问题

在调度框架的恢复执行流的过程中，需要涉及到切换栈的问题。如果在一个rust函数内通过汇编改变了sp寄存器，可能会出现未定义的行为。

由于编译器在编译时会存在代码优化、指令重排和保存寄存器的值到栈中，如果在函数内突然改变了sp寄存器就会导致编译器生成的汇编会访问错误的位置。

## 想到的解决方案

1. 需要在切换栈前得到要切换到的栈的地址，如果本身就是空栈则得到栈顶地址。
2. 写一个跳板函数，需要是`#[naked]`函数，其内容为修改sp寄存器为得到的栈的地址，并跳转到真正运行用户执行流的函数中。在原函数中调用这个跳板函数（这里使用call或jump应该都可以才对）
3. 在新的函数内运行用户执行流，并且不能通过`return`返回。

本质上，是通过`#[naked]`的方式，让编译器对新的函数不生成函数调用栈帧且不进行编译优化，从而打断编译器对旧栈的访问。在`#[naked]`中切换栈，之后的编译器生成的代码使用新的栈。

## 参考其他代码

### Async-OS

主要看的是线程yield的过程，位置在`/modules/trampoline/src/task_api.rs`，其中需要把当前栈保存给线程，再分配一个新栈给调度器。与我们的设计上的区别是，进入调度器前完成切换栈。选择这个部分的原因是，切换栈发生的情况就是出现了线程，yield是一个典型的情况。

保存栈的过程：在`thread_yield`中调用一个裸汇编函数`TrapFrame::thread_ctx`，实质上是构建了一个栈帧，并且在栈帧里把ra改为`set_task_tf`，旧的ra存在构建出来的栈帧里。通过ret返回到`set_task_tf`函数，本质上只是跳转代码，ret不涉及其他内容。

当进入`set_task_tf`后，sp位置不断发生变化，但不再关系，保存进入前的sp值作为trap_frame的指针，然后分配一个新的栈（通过栈池）。判断一些情况并修改状态后，修改sp然后跳转到trampoline。

本质上，这与我在[想到的解决方案](#想到的解决方案)的原理是一样的，都是通过打断编译器对旧栈的访问。这里是通过在`TrapFrame::thread_ctx`中的ret进行跳转，使得编译器把后面的`set_task_tf`不认为是通过函数调用进入，从而保证换栈后跳转的安全性（没有了局部变量回收、后面不返回、没有epilogue（即编译器知道不返回））

另外，当线程恢复时，通过`thread_return`恢复，换栈原理与上面相同

### embassy_preempt

我没有详细的研究其切换过程，但是根据它的文档和找到的部分代码，能得到这样的结果：

首先，在它[上下文切换架构设计文档](https://github.com/Oveln/embassy_preempt/blob/1a826af6cece153d2318ab89cf34a08334f1b1ed/blogs/docs/context-switch-arch.md)里的图显示，切换任务是需要进内核的，对栈的操作也在内核完成，因此感觉对我们的设计没有很大的参考意义。

在它对切换sp与编译器的处理方面，我找到了两处：[第一处](https://github.com/Oveln/embassy_preempt/blob/1a826af6cece153d2318ab89cf34a08334f1b1ed/modules/embassy-preempt-platform/archs/riscv64-rt/src/trap/entry.rs#L100)和[第二处](https://github.com/Oveln/embassy_preempt/blob/1a826af6cece153d2318ab89cf34a08334f1b1ed/modules/embassy-preempt-platform/archs/riscv64-rt/src/stack.rs#L41)。其中第一处完全由汇编进行各种寄存器操作，符合rust规范，第二处具体位置为38~47行，我的理解是：这里修改了sp值并且返回了，这里看似有问题，但实际上mscratch寄存器在最开始的`OSInit`函数中就赋值了，值为当前的sp，因此这个函数看似改变了sp，但实际上只是把参数传给了mscratch。

### starry-os

starry-os里，以线程为调度单位，栈切换与线程切换同时进行，都在`switch_to`中进行。对于sp改变的问题，在最底层的汇编`context_switch`仍然是使用了#[unsafe(naked)]来实现，并且其前后的栈帧结构相同（都是通过外层的`switch_to`）进来的，因此切换sp是安全的。