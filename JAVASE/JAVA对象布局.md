# JAVA对象的内存布局

> 参考链接：https://www.jianshu.com/p/91e398d5d17c

### 一、java对象结构：

 一个Java对象在内存中包括对象头、实例数据和补齐填充3个部分：

![](D:\桌面\编程学习\Java\JAVASE\images\对象内存布局.jpg)



### 二、对象头

![](D:\桌面\编程学习\Java\JAVASE\images\对象头.png)

- **Mark Word**：包含一系列的标记位，比如轻量级锁的标记位，偏向锁标记位等等。在32位系统占4字节，在64位系统中占8字节；
- **Class Pointer**：用来指向对象对应的Class对象（其对应的元数据对象）的内存地址。在32位系统占4字节，在64位系统中占8字节；
- **Length**：如果是数组对象，还有一个保存数组长度的空间，占4个字节；



**注：对象头中的hashcode是调用 对象.hashcode 才会有的，syncthronized 的锁就在头部字段中，还有Eden区的对象达到年龄达到15岁就进入老年区是因为这个字段在对象头中只占4bit**



### 三、对象实际数据

对象实际数据包括了对象的所有成员变量，其大小由各个成员变量的大小决定，比如：byte和boolean是1个字节，short和char是2个字节，int和float是4个字节，long和double是8个字节，reference是4个字节（64位系统中是8个字节）。

| Primitive Type | Memory Required(bytes) |
| -------------- | ---------------------- |
| boolean        | 1                      |
| byte           | 1                      |
| short          | 2                      |
| char           | 2                      |
| int            | 4                      |
| float          | 4                      |
| long           | 8                      |
| double         | 8                      |

对于reference类型来说，在32位系统上占用4bytes, 在64位系统上占用8bytes。

### 四、对齐填充

Java对象占用空间是8字节对齐的，即所有Java对象占用bytes数必须是8的倍数。例如，一个包含两个属性的对象：int和byte，这个对象需要占用8+4+1=13个字节，这时就需要加上大小为3字节的padding进行8字节对齐，最终占用大小为16个字节。

注意：以上对64位操作系统的描述是未开启指针压缩的情况，关于指针压缩会在下文中介绍。

### 五、对象头占用空间大小

这里说明一下32位系统和64位系统中对象所占用内存空间的大小：

- 在32位系统下，存放Class Pointer的空间大小是4字节，MarkWord是4字节，对象头为8字节;
- 在64位系统下，存放Class Pointer的空间大小是8字节，MarkWord是8字节，对象头为16字节;
- 64位开启指针压缩的情况下，存放Class Pointer的空间大小是4字节，`MarkWord`是8字节，对象头为12字节;
- 如果是数组对象，对象头的大小为：数组对象头8字节+数组长度4字节+对齐4字节=16字节。其中对象引用占4字节（未开启指针压缩的64位为8字节），数组`MarkWord`为4字节（64位未开启指针压缩的为8字节）;
- 静态属性不算在对象大小内。

### 六、指针压缩

从上文的分析中可以看到，64位JVM消耗的内存会比32位的要多大约1.5倍，这是因为对象指针在64位JVM下有更宽的寻址。对于那些将要从32位平台移植到64位的应用来说，平白无辜多了1/2的内存占用，这是开发者不愿意看到的。

从JDK 1.6 update14开始，64位的JVM正式支持了 -XX:+UseCompressedOops 这个可以压缩指针，起到节约内存占用的新参数。

### 七、什么是OOP？

OOP的全称为：Ordinary Object Pointer，就是普通对象指针。启用CompressOops后，会压缩的对象：

- 每个Class的属性指针（静态成员变量）；
- 每个对象的属性指针；
- 普通对象数组的每个元素指针。

当然，压缩也不是所有的指针都会压缩，对一些特殊类型的指针，JVM是不会优化的，例如指向PermGen的Class对象指针、本地变量、堆栈元素、入参、返回值和NULL指针不会被压缩。

### 八、启用指针压缩

在Java程序启动时增加JVM参数：`-XX:+UseCompressedOops`来启用。

*注意：32位HotSpot VM是不支持UseCompressedOops参数的，只有64位HotSpot VM才支持。*

本文中使用的是JDK 1.8，默认该参数就是开启的。



### 九、Monitor 对象

**Monitor对象**

每个对象都有一个Monitor对象相关联，Monitor对象中记录了持有锁的线程信息、等待队列等。Monitor对象包含以下三个字段：

- _owner 记录当前持有锁的线程
- _EntryList 是一个队列，记录所有阻塞等待锁的线程
- _WaitSet 也是一个队列，记录调用 wait() 方法并还未被通知的线程

当线程持有锁的时候，线程id等信息会拷贝进owner字段，其余线程会进入阻塞队列entrylist，当持有锁的线程执行wait方法，会立即释放锁进入waitset，当线程释放锁的时候，owner会被置空，公平锁条件下，entrylist中的线程会竞争锁，竞争成功的线程id会写入owner，其余线程继续在entrylist中等待。

------



**Monitor与Synchronized**

对于Synchronized的同步代码块，JVM会在进入代码块之前加上monitorenter ，如果进入monitor成功，线程便获取了锁，一个对象的monitor同一时刻只能被一个线程锁占有；

对于同步方法，JVM会讲方法设置 ACC_SYNCHRONIZED 标志，调用的时候 JVM 根据这个标志判断是否是同步方法。

采用Synchronized给对象加锁会使线程阻塞，因而会造成线程状态的切换，而线程状态的切换必须要操作系统来执行，因此需要将用户态切换为内核态，这个切换的过程是十分耗时的都需要操作系统来帮忙，有可能比用户执行代码的时间还要长。

------

synchronized是悲观锁，在操作同步资源之前需要给同步资源先加锁，这把锁就是存在Java对象头里的，而Java对象头又是什么呢？

Monitor

Monitor可以理解为一个同步工具或一种同步机制，通常被描述为一个对象。每一个Java对象就有一把看不见的锁，称为内部锁或者Monitor锁。

Monitor是线程私有的数据结构，每一个线程都有一个可用monitor record列表，同时还有一个全局的可用列表。每一个被锁住的对象都会和一个monitor关联，同时monitor中有一个Owner字段存放拥有该锁的线程的唯一标识，表示该锁被这个线程占用。

现在话题回到synchronized，synchronized通过Monitor来实现线程同步，Monitor是依赖于底层的操作系统的Mutex Lock（互斥锁）来实现的线程同步。