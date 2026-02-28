---
tags: 
up:
  - "[[C++]]"
down: 
relation:
  - "[[std--thread]]"
  - "[[进程的操作函数]]"
  - "[[fork函数的读时共享、写时拷贝]]"
---
- fork() 是 Unix 和类 Unix 系统（如 Linux）上的一个系统调用，它用于创建一个新的进程。这个新进程被称为子进程，而调用 fork() 的进程被称为父进程。子进程是父进程的一个几乎完全相同的副本。
- fork() 执行以下操作：
    - 创建一个新的进程。
    - 新的子进程继承父进程的代码、数据、堆和栈的副本
    - 复制的资源：程序计数器、CPU寄存器、用户ID、组ID、进程装填、环境、优先级、资源数据
    - 共享的资源：文件描述符（文件打开模式是独立的）
- 返回值：
    - 在父进程中，fork() 返回子进程的 PID (Process ID)。
    - 在子进程中，fork() 返回 0。
    - 如果出现错误（例如，系统中的进程数已达到上限），fork() 返回 -1。

```C
\#include <sys/types.h>
\#include <unistd.h>
\#include <stdio.h>

int main() {
    pid_t pid;
    pid = fork();

    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    if (pid == 0)
        printf("I am the child process.\n");
    else
        printf("I am the parent process, and my child's PID is %d.\n", pid);

    return 0;
}
```

---

- 由于 fork() 会复制父进程的当前状态，如果在 fork() 之前打开了文件或获取了其他资源，这些资源也会在子进程中被复制。这可能会导致一些问题，例如，如果父子进程都试图写入同一个文件，可能会发生数据竞争。因此，通常需要在子进程或父进程中关闭不再需要的资源或进行其他适当的同步操作。
- fork() 创建的是一个新的进程，而不是线程。与线程不同，进程在默认情况下不共享内存空间。如果父子进程需要通信，他们通常需要使用 inter-process communication (IPC) 机制，如管道、消息队列或共享内存。