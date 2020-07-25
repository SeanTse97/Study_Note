# JAVA IO

### 一、 基本概念：

​		区分 Java 的输入和输出：把自己当成程序， 当你从外边读数据到自己这里就用输入（InputStream/Reader）， 向外边写数据就用输出（OutputStream/Writer）。

​		Stream：Java 中将数据的输入输出抽象为流，流是一组有顺序的，单向的，有起点和终点的数据集合，就像水流。按照流中的最小数据单元又分为字节流和字符流。

1、**字节流**：以 8 位（即 1 byte，8 bit）作为一个数据单元，数据流中最小的数据单元是字节。

2、**字符流**：以 16 位（即 1 char，2 byte，16 bit）作为一个数据单元，数据流中最小的数据单元是字符， Java 					 中的字符是 Unicode 编码，一个字符占用两个字节。

3、**字节流和字符流的区别：**

- 处理对象不同：
  - 字节流能处理所有类型的数据（如图片、多媒体等）
  - 字符流只能处理字符类型的文本数据。
- 读写单位不同：
  - 字节流以字节byte为单位，1byte=8bit。
  - 字符流以字符为单位，1个字符=2个字节(java中采用unicode编码)。根据码表映射字符，一次  可能读多个字节。
- 有无缓冲区：
  - 字节流没有缓冲区，是直接输出的。字节流不调用colse()方法时，信息就已经输出了。
  - 字符流是输出到缓冲区的。只有在调用close()方法关闭缓冲区时，信息才输出。要想字符流在未关闭时输出信息，则需要手动调用flush()方法。

![](D:\桌面\编程学习\Java\JAVASE\images\字节流字符流.jpg)

### 二、IO 框架图：

#### 2.1  Java中IO流的体系结构如图：

![](D:\桌面\编程学习\Java\JAVASE\images\IO体系.png)



#### 2.2 IOl流框架继承图：

![](D:\桌面\编程学习\Java\JAVASE\images\io框架图.jpg)



#### 2.2  Java IO 体系中常用的流类：

![](D:\桌面\编程学习\Java\JAVASE\images\IO常用流.jpg)





### 三、转换流：

1. **InputStreamReader:*字节到字符的桥梁***

2. **OutputStreamWriter:*字符到字节的桥梁***

### 四、ObjectInputStream 与 ObjectOutputStream

对象序列化的实现



### 五、File 类

> 参考：https://blog.csdn.net/qq_39304441/article/details/99670966

### 参考资料：

csdn博客： https://blog.csdn.net/feather_wch/article/details/82665902