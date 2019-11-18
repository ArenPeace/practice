## 16.for (int i = 0; i < 2; i++) { fork(); printf("A"); } 共输出几个A
8个，子进程会继承父进程的缓冲区

如果是printf("A\n");则是6个，因为\n会将缓冲区flush掉。

## 17.atoi的实现
判断指针非空，异常返回0，下同;
过滤空格;
检查'-'号;
判断'0'-'9'的字符，其他字符则停止，返回当前累计;

## 18.c++的单例实现
singleton

private修饰构造函数、拷贝构造函数、赋值函数;
静态实例，和静态get函数;

## 柔性数组
char *p = new char[0];
char a[0];
都是gcc合法的

sizeof(a) = 0；
sizeof(*p) = 1；
p[10] a[10]都可以读写，内容是乱码；
p[10000] a[10000]一访问就段错误；

char a[0]同char a[]作为struct最后一个成员时，可以实现一种柔性数组flexible array

## 内存泄露 
堆上的内存没有及时释放掉，导致程序运行期间不可重用，即泄露。
泄露过多导致内存耗尽，内存分配失败，程序因此崩溃。

## 缓冲区溢出
绝大多数程序都假设所分配的储存空间足够数据储存，当后者过大时，就产生溢出；
操作系统使用的缓冲区，即堆栈，各操作进程的指令被临时存储在堆栈中，它也会溢出；
栈溢出是缓冲区溢出的一种；
### 内存溢出
栈满时，再做进栈造成空间上溢；栈空时，再做出栈造成下溢出；

内存写越界，即是缓冲区溢出;
内存读越界，如果地址无效，程序报错崩溃；如果地址有效，读到数据随机;

排查方法：
+ 1.BoundChecker,不是很好用
+ 2.类内部变量莫名其妙变化时，查看this指针变化
+ 3.调试，查看变量值
+ 4.二分法注释代码，看错误是否重现
+ 5.反汇编分析？
+ 6.代码复查

## 内存访问越界
访问了申请的内存之外的空间，结果不具确定性，可能没有问题，可能程序报错崩溃。常见的有数组越界、字符串拷贝越界(sprintf/strcpy/memcpy/等)。
+ 1.可能破坏了堆中的内存分配信息数据，导致glibc分配和释放内存块时，报free() invalid pointer  或者 malloc() memory corruption 或者 double free or corruption 或者 corruption double-linked list
+ 2.破坏了程序内其他对象的内存空间，导致程序执行结果错误，或者引发coredump，如改变了指针数据
+ 3.破坏了空闲的内存块，影响未知

有时，代码错误被激发是偶然的。排查时，首先保证重现错误，根据错误估计发生位置，逐步裁剪代码，缩小排查空间，检查所有内存操作函数、内存越界可能

sprintf snprintf vsprintf vsnprintf

strcpy strncpy strcat 

memcpy memncpy memmove memset bcopy

如有用到自己编写的动态库，保证编译环境和程序的一致

## 9.进程地址空间
+ 1.进程使用虚拟内存中的地址，操作系统和硬件把它转成物理地址。 虚拟地址通过页表PageTable映射到物理内存
+ 2.内核空间在所有页表中拥有较高特权级别，因此用户态程序访问它将会导致一个页错误。
+ 3.内核空间是持续存在的，并且在所有进程中都映射到同样的物理内存。
+ 4.linux进程在虚拟内存中的标准内存段布局包括高地址的内核空间1GB与底地址用户空间3GB。用户空间地址有高到低包括命令行参数和环境变量、栈、堆、未初始化的数据、初始化的数据、正文。正文段，cpu指令部分，可共享的（内存中多个程序共享一个副本），只读的，写导致错误。初始化数据段，即数据段，包含程序中的全局变量。非初始化数据段，bss段，未初始化的全局变量和静态变量。栈，自动变量及函数调用信息。堆，动态存储分配。堆始于非初始化数据段顶和栈底之间，两者间隔很大。在编译时，函数地址及其他地址就已经确定了。readelf命令可以查看二进制文件中函数入口地址。
+ 5.32位windows，进程高位2G留给内核，低位2G留给用户，所以进程堆的大小小于2G，进程栈的大小默认是1M，vs的编译属性可以修改程序运行时进程的栈大小，一个线程栈的大小默认为1M，所以一个进程最多开2048个线程，实际最多大概2000个。
+ 6.linux下，进程高位1G留给内核，低位3G留给用户，所以进程堆的大小小于3G，进程栈的默认大小是10M，可以通过ulimit -s查看并修改默认栈大小。一个线程栈的大小默认为8M，所以最多380个左右线程。（ulimit -a查看linux的最大进程数大概7000多个。）
+ 7.线程共享的环境包括进程代码段、进程共有数据、进程打开的文件描述符、信号的处理器、进程的当前目录、进程用户ID与进程组ID。线程独立拥有自己的线程ID、寄存器组的指、线程函数栈区、错误返回码、线程信号屏蔽码、线程的优先级。window平台不同线程缺省使用同一个堆，所以用c的malloc分配内存的时候是使用了同步保护的。
+ 8.x86_64下，虚拟地址只用了48位，C程序里打印的地址只有12位16进制地址，48位对应了256TB的地址空间。64位地址前16位，4位16进制地址，全0是用户地址，全F是内核地址，这样分为两个128TB地址空间。内核空间有很多空洞hole，其中仅64TB才是直接映射物理内存的区域，PAGE——OFFSET为ffff8800 00000000
+ 9.pmap -x pid报告进程的内存映射关系
+ 10.malloc/free函数是通过brk，mmap，munmap等实现

## 10.2GB的数据可以存放到一个map结构中么
抛离具体硬件和操作系统，这个问题没啥意义，32位勉强，64位就看具体内存了。
linux下勉强可以，几乎不能干别的了

## 11.char[]和char*的区别
char *func() { char a[] = "hi"; return a; }

char *func() { char *p = "hi"; return p; }

“hi”是一个代码区的字符串常量，放在程序的静态数据区，被写保护，不能写;
前者在g++编译后返回的是nil，后者返回的是地址；
前者可以通过a[0] = 'a';修改，后者*p = 'a';运行时报段错误;
编译时，前者return处警告返回一个局部变量，后者在p初始化处警告将一个常量赋给char*。
cout << NULL;后，标准输出被关闭，后续不能输出。
数组是什么？是c++内置的数据结构。
只有sizeof alignof（返回对齐的字节数） &这三个操作符与使用初始化数组的字符串字面量外，表达式中的数据都会被自动转换成指向其首元素的指针。
不可赋值给数组，因为先转换为指针，且这个指针不可作为左值，这是decay（衰退）。
左值，放在左边是它的行为。本质上意味着在内存中有确切的位置，可以定位。
只可以用数组下标访问数组的元素。

## 12.c++的流 ，为何cout<<NULL后被关闭
NULL默认关闭cout，再怎么打开呢？

## 64位C++指向虚函数表的指针vfptr占8个字节