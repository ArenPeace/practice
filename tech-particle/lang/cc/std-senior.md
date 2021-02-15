# C++语言专题

## STL 六大组件
1. 容器 Container  
各种数据结构+模板机制，数据结构用于存放数据，模板用于扩展。
2. 算法 Algorithm  
Function+模板机制，Sort/Search/Copy/Erase/Insert算法用于处理数据，模板用于扩展。
3. 迭代器 Iterator  
把数据的操作封装成统一的使用指针风格的访问，泛型指针机制。（五种类型及其衍生？）
```cpp
operator*();
operator->();
operator++();
operator--();
/* ... */
```
每种容器有各自的迭代器。（C++原生指针Native Pointer（就是一般指针）也是一种迭代器）

4. 仿函数 Fuctor  
把算法的操作封装成统一的使用面向对象风格的对象（算法的策略类？Policy）,重载了operator()的`Class/Class<Template>`（或者带有参数多个）（具有函数特征的对象vs具有对象特征的函数？）。
Policy + 模板机制，一般的函数指针也可以看作是仿函数对象。
**smart function**
STL标准函数对象在头文件<functional>。运算三类：算数、关系、逻辑。
5. 适配器 Adapter  
STL的queue和stack看似独立容器，实际封装了deque，适配出queue和stack。
STL的反向迭代器/插入迭代器/IO流迭代器？
STL的成员函数适配器/函数对象适配器？
简化或者约束或者定制，提供特别的功能。
**详细的适配器机制是怎样？**
6. 分配器 Allocator  
动态内存配置与管理 + 模板机制

隐藏在容器后的内存管理工作是通过STL提供的一个默认的allocator实现的。用户也可以定制自己的allocator，只要实现allocator模板所定义的接口方法即可，然后通过将自定义的allocator作为模板参数传递给STL容器，创建一个使用自定义allocator的STL容器对象，如：`stl::vector<int, UserDefinedAllocator> array;`

+ 频繁使用操作系统的malloc，free开辟释放小块内存带来的性能效率的低下
+ 内存碎片问题，导致不连续内存不可用的浪费  
程序中的小对象的分配极易造成内存碎片，给操作系统的内存管理带来了很大压力，系统中碎片的增多不但会影响内存分配的速度，而且会极大地降低内存的利用率。

**STL默认的allocator**  
这个allocator是一个由两级分配器构成的内存管理器，当申请的内存大小大于128byte时，就启动第一级分配器通过malloc直接向系统的堆空间分配，如果申请的内存大小小于128byte时，就启动第二级分配器，从一个预先分配好的内存池中取一块内存交付给用户，这个内存池由16个不同大小（8的倍数，8~128byte）的空闲列表组成，allocator会根据申请内存的大小（将这个大小round up成8的倍数）从对应的空闲块列表取表头块给用户。

这种做法有两个优点：
+ 1)小对象的快速分配。  
小对象是从内存池分配的，这个内存池是系统调用一次malloc分配一块足够大的区域给程序备用，当内存池耗尽时再向系统申请一块新的区域。
+ 2)避免了内存碎片的生成。  
以内存池组织小对象的内存，从系统的角度看，只是一大块内存池，看不到小对象内存的分配和释放。

问题：  
+ 内存池会带来一些内存的浪费。比如当只需分配一个小对象时，为了这个小对象可能要申请一大块的内存池。以空间换时间。
+ 内存持使用自由链表管理，被进程占用，进程退出才会释放。
大多数情况下足够了。


## C++的数据结构
结构    | 名称  | 有序无序  |
----- | ---- | ---- |
map     |红黑树      |有序|
list    |双向链表    |无序|
vector  |数组        |无序    2倍扩容，拷贝|
set     |红黑树      |有序|
queue   |单向队列    |无序|
deque   |双向队列    |无序|
c++11 unordered_map hashmap |无序|

### C++数组是什么？
是C++内置的数据结构，指向一片连续内存。

### stl vector和list的区别
vector和数组类似，拥有一段连续的内存空间，且起始地址不变。  
高效随机存取，查询速度O(1)，插入和删除的操作，时间复杂度为O(n)。  
当内存不够时，会重新申请一块内存空间并拷贝。  
list是双向链表，内存空间是不连续的。  
通过指针访问数据，随机存取没效率O(n)，能高效插入和删除。  

### map的数据结构
vector封装数组，list封装了链表  
C++ STL中标准关联容器set, multiset, map, multimap内部采用的就是一种非常高效的平衡检索二叉树：红黑树，也成为RB树(Red-Black Tree)。RB树的统计性能要好于一般的平衡二叉树(平衡二叉树，一般指AVL树（根据作者姓名，Adelson-Velskii和Landis），AVL树与其他平衡二叉树的区别是算法不同)，所以被STL选择作为了关联容器的内部结构。

查找算法复杂度：O(log n)  
log 10000 约等于 14；log 20000 约等于 15；

**C++的STL的map和set与C语言包装库的效率比较**  
在许多unix和linux平台下，都有库（比如下面的isc）提供map的效果
```cpp
void tree_init(void **tree);
void *tree_srch(void **tree, int (*compare)(), void *data);
void tree_add(void **tree, int (*compare)(), void *data, void (*del_uar)());
int tree_delete(void **tree, int (*compare)(), void *data,void (*del_uar)());
int tree_trav(void **tree, int (*trav_uar)());
void tree_mung(void **tree, void (*del_uar)());
```
许多人认为直接使用这些函数会比STL map速度快，因为STL map中使用了许多模板什么的。其实不然，它们的区别并不在于算法，而在于内存碎片。如果直接使用这些函数，需要自己去new一些节点，当节点特别多，而且进行频繁的删除和插入的时候，内存碎片就会存在。STL采用自己的Allocator分配内存，以内存池的方式来管理这些内存，会大大减少内存碎片，从而会提升系统的整体性能。把以前所有直接用isc函数的代码替换成map，程序速度基本一致，当运行很长时间后（例如后台服务程序），map的优势就会体现出来。从另外一个方面讲，使用map会大大降低编码难度，同时增加程序的可读性。

### STL Allocator 内存碎片


### C++迭代器失效问题
vector/deque（序列式容器）插入和删除，导致（当前及其后的）迭代器失效（数组结构导致）   
list插入，导致迭代器失效（当前迭代器失效）  
对于关联容器(如map, set,multimap,multiset)，删除当前的iterator，仅仅会使当前的iterator失效，只要在erase时，递增当前iterator即可。这是因为map之类的容器，使用了红黑树来实现，插入、删除一个结点不会对其他结点造成影响。erase迭代器只是被删元素的迭代器失效，但是返回值为void，所以要采用erase(iter++)的方式删除迭代器。

**set和map迭代器为什么不会失效？**  
set和map内部使用红黑树，红黑树在插入和删除后，不会为了保持平衡而做旋转么，如果旋转了，不就失效了么？

set和map插入后会返回当前iter，删除时使用erase(iter++)，当前迭代器都不会失效。而红黑树旋转，其元素地址没有变化，只是元素之前的顺序发生变化。

### 2GB的数据可以存放到一个map结构中么
抛离具体硬件和操作系统，这个问题没啥意义，32位勉强，64位就看具体内存了。  
linux下勉强可以，几乎不能干别的了。

### 排序算法
std库中的vector list dequeue容器上sort函数?

std的sort()在数据量大的时候使用快速排序，效率为O(log n);
分段后的数据量小于某阈值，改成插入排序，此时基本有序，所以复杂度可达O(n)；
在递归过程中，如果递归层次过深，分割有恶化倾向，则侦测出来后，使用堆排序处理，效果维持在O(n * log n);

## 继承与多态
### 为什么继承中析构函数一定要定义为虚函数
如果不是虚函数，就相当于覆盖了，实例的析构只调用子类的析构函数（调用多次？），可能会造成内存泄漏。

### 64位C++指向虚函数表的指针vfptr占8个字节


### 什么是多态
多态，另一个极端：同构，可以代码复用。  
+ 如相同的接口=》继承多态  
+ 相同的函数名=》重载多态cpp
+ 相同的代码实现=》模板多态

继承多态，函数F(基类指针或引用),F的代码也相同，但其参数的成员函数的行为呈现多态，是不同的。
### 什么是封装
### 什么是类型安全

## FAQ
### 1.共输出几个A
```cpp
for (int i = 0; i < 2; i++) {
    fork();
    printf("A");
} 
```
8个，子进程会继承父进程的缓冲区  
如果是printf("A\n");则是6个，因为\n会将缓冲区flush掉。

### 2.atoi的实现
判断指针非空，异常返回0，下同;  
过滤空格;  
检查'-'号;  
判断'0'-'9'的字符，其他字符则停止，返回当前累计;

### 3.c++的单例实现
singleton

private修饰构造函数、拷贝构造函数、赋值函数;  
静态实例，和静态get函数;

### 4.柔性数组
```cpp
char *p = new char[0];
char a[0];
// 都是gcc合法的

sizeof(a) = 0；
sizeof(*p) = 1；
// p[10] a[10]都可以读写，内容是乱码；
// p[10000] a[10000]一访问就段错误；

// char a[0]同char a[]作为struct最后一个成员时，可以实现一种柔性数组flexible array
```
### 内存泄露 
堆上的内存没有及时释放掉，导致程序运行期间不可重用，即泄露。
泄露过多导致内存耗尽，内存分配失败，程序因此崩溃。

### 缓冲区溢出
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

### 内存访问越界
访问了申请的内存之外的空间，结果不具确定性，可能没有问题，可能程序报错崩溃。常见的有数组越界、字符串拷贝越界(sprintf/strcpy/memcpy/等)。  
+ 1.可能破坏了堆中的内存分配信息数据，导致glibc分配和释放内存块时，报free() invalid pointer  或者 malloc() memory corruption 或者 double free or corruption 或者 corruption double-linked list
+ 2.破坏了程序内其他对象的内存空间，导致程序执行结果错误，或者引发coredump，如改变了指针数据
+ 3.破坏了空闲的内存块，影响未知

有时，代码错误被激发是偶然的。排查时，首先保证重现错误，根据错误估计发生位置，逐步裁剪代码，缩小排查空间，检查所有内存操作函数、内存越界可能

sprintf snprintf vsprintf vsnprintf

strcpy strncpy strcat 

memcpy memncpy memmove memset bcopy

如有用到自己编写的动态库，保证编译环境和程序的一致

## char[]和char*的区别
```cpp
char *func() {
    char a[] = "hi";
    return a;
}

char *func() {
    char *p = "hi";
    return p;
}
```
“hi”是一个代码区的字符串常量，放在程序的静态数据区，被写保护，不能写;  
前者在g++编译后返回的是nil，后者返回的是地址；  
前者可以通过a[0] = 'a'修改；后者*p = 'a';运行时报段错误;  
编译时，前者return处警告返回一个局部变量，后者在p初始化处警告将一个常量赋给char*。

## C++ 衰退
只有sizeof alignof（返回对齐的字节数） & 这三个操作符与使用初始化数组的字符串字面量外，表达式中的数据都会被自动转换成指向其首元素的指针。  
不可给数组赋值，因为先转换为指针，且这个指针不可作为左值，这是decay（衰退）。  
左值，放在左边是它的行为。本质上意味着在内存中有确切的位置，可以定位。
只可以用数组下标访问数组的元素。

## c++的流 ，为何cout<<NULL后被关闭
cout << NULL;后，标准输出被关闭，后续不能输出。 
NULL默认关闭cout，再怎么打开呢？  

## int* 转 int
```cpp
// 64位机器测试
    long long a = (long long)(((int*)0)+4);
    // cout << a，输出16
    // int* a = 0, a++, a: 4，a++，a:8
    // 指针的加法运算是按照指针值类型占用空间来计算的
    // char *a = 0, a++, a: 1
    // int64 *a = 0, a++, a: 8
    
    int a = (int)(((int*)0)+4); // 编译报错，error: cast from ‘int*’ to ‘int’ loses precision [-fpermissive]

    long long lval = 123456789012345;
    int a = (int)lval;  // a = -2045911175

    float fval = 22.2;
    int a = (int)fval;  // a = 22
```

### C内存对齐

## C++ future
**C++如何实现类似Go的Context**

C++ Barrier