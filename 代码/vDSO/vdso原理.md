## 原理

vdso是由内核提供、映射到用户空间的一段共享库，以elf形式构成，用于在用户态直接执行部分系统调用，避免陷入内核。主要包括两部分：

- 虚拟动态共享对象（vDSO）：主要为代码部分，存储共享的可执行代码
- 共享数据页（vVAR）：主要为vDSO访问的数据

在内核进行初始化时，主要分配页面，记录代码和数据在内核的地址，以及其虚拟内存映射。

在用户态初始化时，首先在内核执行execve系统调用时，就将vdso映射到用户内存，并把代码的地址记录在用户栈中；之后用户在动态链接时找到vdso代码地址并加载；最后在用户态执行libc init时对静态链接的程序进行初始化vDSO函数的地址

## 读写

读：用户代码使用libc库函数，直接读取vdso的相关函数并执行。用户对vVAR的数据读取通过vdso中的可执行代码间接访问。如果读取失败（比如不支持vdso）则发起系统调用

写：时钟中断触发时，中断处理程序更新vdso的系统时间信息；或者由用户进行系统调用，更新系统时间信息。总之要进内核进行修改。

## starry-vdso 的内容

starry-vdso 中，内容与linux的vdso基本相同，主要内容是系统时间相关的库函数。由于没有提供.so的源代码，我通过`rust-objdump -T vdso/vdso_riscv64.so`命令，得到结果为：
```
DYNAMIC SYMBOL TABLE:
00000000000004e8 l    D  .rodata	0000000000000000              .rodata
00000000000008d0 g    DF .text	00000000000001c0  LINUX_4.15  __vdso_gettimeofday
0000000000000a90 g    DF .text	0000000000000086  LINUX_4.15  __vdso_clock_getres
0000000000000000 g    DO *ABS*	0000000000000000  LINUX_4.15  LINUX_4.15
0000000000000b2e g    DF .text	000000000000010a  LINUX_4.15  __vdso_riscv_hwprobe
0000000000000c42 g    DF .text	0000000000000216  LINUX_4.15  __vdso_getrandom
00000000000005e0 g    DF .text	0000000000000008  LINUX_4.15  __vdso_rt_sigreturn
00000000000005fe g    DF .text	00000000000002d2  LINUX_4.15  __vdso_clock_gettime
0000000000000b24 g    DF .text	000000000000000a  LINUX_4.15  __vdso_flush_icache
0000000000000b18 g    DF .text	000000000000000a  LINUX_4.15  __vdso_getcpu
```
与linux基本相同
