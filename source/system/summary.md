# 操作系统面经

## 进程与线程
1. 如何理解进程与线程，谈谈它们的联系与区别？
   > 联系：
   > - 进程是正在执行程序的实例，线程是进程中执行运算的最小单元，是进程中可调度的实体。进程中可以有多个线程，而一个线程只属于一个进程。
   > 区别：
   > - 进程是拥有资源的基本单位，而线程是调度和执行的基本单位。
   > - 进程的创建和回收需要操作系统分配和回收资源，开销大于线程。

   
2. 当用浏览器打开网页，使用的是进程还是线程？
   > 现在的浏览器使用的是多进程，有这几个原因：
   > 1. 使用多个进程，其中一个进程崩溃不会影响其他进程
   > 2. 更安全，由于线程之间是共享资源的，因此如果打开了一个恶意网站，可能会获得该浏览器中其他所有资源。

3. 如果有一个4核CPU，I/O复用比较密集，该开多少线程？
   
   > 4核按照双线程，则最多可以有8个线程，I/O复用密集，按照I/O时间核CPU计算时间的比例，选择开启的线程数。

4. 场景题：如果有一个很大的数组，如何开启多个线程进行并行？
   
   > 将数组的不同索引区间交给不同的线程进行计算，最后进行结果合并，返回主线程。

5. 不同进程如何共享变量？
   > 1. 通过共享内存共享变量，信号量辅助。


6. 信号量是如何使用的？修改信号量如何保证其他线程无法使用？
   > 信号量一般分为计数信号量和二进制信号量，由于对信号量的操作是原子性的，因此可以保证读写信号量时其他线程/进程无法使用。

7. 说一下常见的进程调度算法
   > 1. 批处理系统中：
        > - 先来先服务
        > - 最短作业优先
        > - 最短剩余时间优先
   > 2. 交互式系统中：
        > - 轮转调度
        > - 优先级调度
        > - 多级队列
        > - 最短进程优先
        > - 保证调度
        > - 彩票调度


8. Linux操作系统的进程管理机制是怎样的？有什么特性？
   > Linux系统是一个多用户多任务操作系统，Linux内核把进程称为任务。
   > Linux系统中，新进程一定是某个父进程复制创建出来的，当使用fork系统调用时，会产生一个父进程的完整副本。
   > Linux系统中进程有7种状态，包括就绪态、运行态、轻度睡眠、中度睡眠、深度睡眠、僵尸状态、死亡状态。

    可以回答进程调度、进程回收等

9.  子进程和父进程有什么关系？
    > 子进程最初都是父进程的复制品（除了代码段，代码段是共享的），在fork之后，父进程先执行，然后一个时间片之后子进程再执行。
    > 子进程结束后会向父进程发送wait消息，然后子进程成为僵尸进程，直到父进程将其从进程表中删除。

10. 什么是孤儿进程？
    > 一个进程退出，而它的一个子进程或多个子进程还在运行，那么这些子进程将会成为孤儿进程。孤儿进程会被init进程收养。

11. 什么是僵尸进程？
    > 一个进程使用fork创建子进程，如果子进程退出，而父进程没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中，这种进程称为僵尸进程。
    > 僵尸进程是已经结束的进程，但在进程表中仍然占据了一个位置，由于进程表的容量是有限的，所以僵尸进程多了占用系统资源，可能会导致系统瘫痪。
    
12. 进程间通信的方式有哪些？
    > 1. 管道pipe
    > 2. 命名管道FIFO
    > 3. 消息队列
    > 4. 共享存储
    > 5. 信号量

13. 阻塞和非阻塞的概念？
    


14. 线程同步的机制有哪些？
    > - 临界区
    > - 互斥量
    > - 事件
    > - 信号量
15. 常用的锁和对应的使用场景？
    > 1. 互斥锁：一次只能一个线程拥有互斥锁，其他线程只能等待。互斥锁在抢锁失败的情况下主动放弃CPU进入睡眠状态直到锁的状态改变时再唤醒。
    > 2. 自旋锁：一次只能一个线程访问，当获取锁失败时，不会进入睡眠，而在原地自旋，直到锁被释放。这节省了线程从睡眠到唤醒的时间消耗，在短时间内可以提高效率，但是时间长了会浪费CPU资源。
    > 3. 读写锁：读写锁是一种特殊的自旋锁，特点是读共享，写互斥。
    > 4. 悲观锁：认为每次拿数据的时候别人都会修改，因此每次拿数据的时候都会上锁，在操作前先上锁。
    > 5. 乐观锁：认为每次拿数据的时候别人都不会修改，因此每次拿数据不会上锁，但是在更新的时候会判断一下再次期间别人有没有更新这个数据。
    > 6. 信号量：用于线程间同步和互斥。


16. fork函数执行的过程。
    > 父进程调用fork会创建子进程，子进程是父进程的副本。
    > fork函数调用一次，返回两次，如果是父进程则返回子进程pid，如果是子进程则返回0.

17. 用户态和内核态的区别？
    > 内核态运行操作系统程序，拥有对硬件的完全控制访问全，有完整的指令集。
    > 用户态运行用户程序，只能运行部分指令集，没有访问控制硬件的权限。

18. 进程切换的过程？
    > 1. 中断


19. 进程上下文切换的开销有哪些？
    > 1. 切换内核态堆栈、寄存器
    > 2. 切换硬件上下文
    > 3. 切换页表目录
    > 4. 刷新TLB


20. 进程控制块中有哪些字段？
    > - 进程管理相关：寄存器、程序计数器（存放正在执行的指令的地址）、程序状态字（指示处理器状态的寄存器）、堆栈指针、进程状态、优先级、进程ID、父进程、信号、使用的CPU时间
    > - 存储管理相关：代码段指针、数据段指针、堆栈段指针
    > - 文件管理相关：根目录、工作目录、文件描述符、用户ID、组ID

21. 线程切换的过程？
22. 进程间状态的转换是如何转换的？
23. 进程在什么情况下会进行切换？
    > 1. CPU时间片耗尽
    > 2. 高优先级进程运行
    > 3. 发生硬件中断，I/O中断
    > 4. 进程主动sleep或者终结
24. 协程了解吗？请详细说一说？
    > 协程是轻量级的线程，但是携程的切换不需要操作系统内核的帮助，而是由开发人员决定的。
    > 进程和线程的切换是需要系统调用的，因此每次切入内核然后由调度程序选择下一个线程，而对于协程，CPU的角度其实是单线程，只是由用户指定上下文的切换。

    > 使用协程必须和异步I/O一起使用，因为如果协程调用了一个阻塞IO，操作系统不知道协程的存在，只知道线程，因此会让线程进入阻塞，当前的协程和其他绑定在该线程上的协程都会被阻塞。

25. 操作系统的中断的步骤？

26. 中断和系统调用的上下文切换，与进程的上下文切换有什么区别？
    > 中断和系统调用都属于内核空间的切换，不会涉及用户态的堆栈、内存。而进程的上下文切换，既要切换内核态的堆栈、寄存器，还要切换用户态的堆栈、内存。

27. 文件描述符是什么？
    > 文件描述符file descriptor简称fd，实际上是数组的索引，这个数组就是`file_struct`，是由操作系统进行维护的数组。同时也就说明，文件是共享的。
    > 总结来说，用户得到fd句柄，通过fd句柄来操作对应的文件，层次关系是：进程结构体task_struct——文件表项管理结构files_struct——file。
    > [参考](https://zhuanlan.zhihu.com/p/364617329)


## IO

1. 简述Linux的五种I/O处理方式，并说一下他们的使用场景。
    > IO的本质是socket的读取，数据先拷贝到内核的缓冲区，然后拷贝到应用程序的地址空间。
    > 1. (同步)阻塞I/O：程序阻塞知道数据准备好，并将数据从内核复制到应用程序。
    > 2. (同步)非阻塞I/O：在数据准备阶段，程序一直轮询盲等，期间可以做一些其他任务，直到数据准备完毕，程序阻塞，拷贝数据到地址空间。
    > 3. (同步)I/O复用(select和poll)：可能有多个任务在同时进行，如果只要有一个任务完成，就可以去处理。IO多路复用是阻塞在select之上的。
    > 4. (同步)信号驱动I/O
    > 5. (异步)异步I/O
    ![](./image/summary/IO_1.png)
    > [参考](https://zhuanlan.zhihu.com/p/127170201)
3. 什么是同步IO，什么是异步IO，什么是阻塞，什么是非阻塞，有什么区别？
    > - 同步：发出一个调用后，在没有得到结果之前，该调用不返回，也就是必须等一件事做完才能做下一件。
    > - 异步：和同步相对，调用者没有得到结果前，还可以做别的事情，结果可以通过状态、通知和回调来告知调用者。
    > - 阻塞：指调用结果返回之前，当前线程会被挂起。
    > - 非阻塞：调用后立即返回，不会导致线程被挂起，结果会通过select通知调用者。
    > [参考](https://zhuanlan.zhihu.com/p/36344554)

4. select\poll\epoll的区别？
    > 1. seletc和poll需要自己不断轮询所有


## 死锁
1. 死锁的概念是什么？什么情况下会产生死锁？
   > 如果一个进程集合中的每个进程都在等待只能由该进程集合中的其他进程才能引发的事件，那么该进程集合就是死锁的。
   > 满足死锁的四个必要条件就会产生死锁。

2. 死锁产生的四个必要条件是？
   > - 互斥条件
   > - 占有和等待条件
   > - 不可抢占条件
   > - 循环等待条件

3. 如何预防死锁？
   > - 银行家算法
   > 
4. 如何避免死锁？
   - 可以通过破坏死锁的四个条件来预防死锁：
   - 
5. 简述Dijkstra算法

## 内存
1. 什么是地址空间？地址空间的作用是什么？
2. 页面、页表和页框分别是什么？
3. 虚拟地址空间到物理地址空间是如何转换的？物理地址空间是如何寻址的？
4. 一个进程的地址空间有哪些区域？
5. 分页和分段有什么区别？
6. 常见的页面置换算法有哪些？