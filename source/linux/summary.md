# Linux和Shell面经

## Linux命令
1. 给一个文件有很多行，每个行都是ip地址，用Linux命令找出出现最多的三个ip地址
2. Linux kill杀掉进程的处理流程？
   > 首先shell会执行fork和execve，执行kill命令。
   > kill命令会发送对应的信号到进程中，存放在进程结构体的变量中。
   > 当CPU调度到该进程时，执行对应信号的操作。
3. Linux查看内存使用情况的命令：
    > free -m 以M为单位显示内存使用情况
4. Linux查看CPU核数/逻辑核数/线程数
    > cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l
    > cat /proc/cpuinfo | grep "core id" | sort | uniq | wc -l
    > cat /proc/cpuinfo | grep "processor" | sort | uniq | wc -l

5. Linux中显示文件使用空间命令
    > du
6. Linux中显示磁盘使用空间命令
    > df

7. Linux中查看进程的命令
    > - ps
    > - top
    > ps命令只是查看进程，是命令执行的瞬间的进程信息，而top可以持续监视。并且top还会显示进程使用的资源情况。


## 哈希
1. 解决哈希冲突的几种方式
    > - 开放定址法
        > - 线性探测法：当前位置冲突，逐个往后寻找。
        > - 平方探测法：当前位置冲突，平方个往后寻找。
    > - 再哈希法：使用多个哈希函数，如果发生哈希冲突就是用其他哈希函数。
    > - 链地址法：将所有哈希地址相同的记录都链接在同一个链表中。
    > - 建立公共溢出区：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表。
2. 一致性哈希算法有哪些？
    > 不了解
3. 说一下常用的哈希算法
    > - MD5
    > - SHA1
    > - SHA256
    > - Memehash