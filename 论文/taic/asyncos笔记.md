# trampoline 模块

## trampoline 函数

### 调用

调度主函数，被调到用的地方有：
1. 初始化：`rust_entry`中调用
2. 用户态 Trap：在`rust_entry`中调用`trampoline/src/arch/riscv/mod.rs/init_interrupt()`，其中在`trap_vector_base()`函数中判断`sscratch`来确定内核态还是用户态中断。中断时跳转到 `trampoline()`
3. 内核态 Trap：除了上述用户态 Trap方式，在任务切换时（`task_api.rs`中的`thread_yield、thread_blocked、thread_exit`）调用`set_task_tf()`，其中设置新任务跳转到`trampoline()`

> 所以tf是怎么传递的？没看懂

### 内容

1. 如果是内核态发生 Trap：对 irq 进行处理，对异常报出来
2. 否则取当前任务，调用`run_task()`
3. 如果没用任务就循环等 irq

## run_task 函数

### 调用

只有 `tampoline()` 中调用了

### 内容

1. 切换 page_table_root
2. 恢复当前任务：`restore_from_stack_ctx()`函数中对任务做区分，区分是线程恢复还是抢断恢复
> 有什么区别吗？
3. 对当前任务的 Future 进行一次 `poll()`，并处理结果
4. `Poll::Ready`的认为是任务退出，如果是初始化任务退出要关机
5. `Poll::Pending`的判断任务状态，做出相应处理。其中的`TaskState::Running`处理为：如果有用户态陷入帧且陷入处理结束，则`user_return()`，否则设置状态为`Runable`并继续调度。
> 这里有两个问题：（1）只有`disable_irqs()`，在哪里进行`enable_irqs()`？（2）`tf.kernel_sp = taskctx::current_stack_top();`的意义是什么？为什么要在 TrapFrame 中保存一份 TaskCtx 里面已经有的内容？

## user_task_top 函数

### 调用

在`executor::init()`中作为参数传入，被当成用户任务的 `utrap_handler()`

### 内容

1. 取当前任务以及其陷入帧，判断陷入帧状态
2. 如果状态是`TrapStatus::Blocked`：判断陷入原因，并进行处理。其中如果是用户的系统调用，则分异步与否分别处理
   1. 非异步：调用`handle_syscall`并在最后调用`.await`
   2. 异步：构建一个新的内核任务，并通过`poll`来推动，实现异步系统调用。成功时返回结果，还没成功时返回`EAGAIN`

> 这个函数最后的这段：
> ```
> poll_fn(|_cx| {
>   if tf.trap_status == TrapStatus::Done {
>       Poll::Pending
>   } else {
>       Poll::Ready(())
>   }
> })
> .await
> ```
> 以及外层的`loop`是什么？没看懂