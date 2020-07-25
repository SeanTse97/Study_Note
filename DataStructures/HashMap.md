# JAVA 中的HashMap

## 1、hashMap的哈希冲突处理：

JDK 1.6, JDK 1.7 HashMap 采用位桶 + 链表实现。

JDK 1.8 HashMap **采用位桶 + 链表 + 红黑树**实现。（当链表长度超过阈值 “8” 时，将链表转换为红黑树）

开放定址法（发生冲突，继续寻找下一块未被占用的存储地址），再散列函数法，链地址法，而HashMap即是采用了**链地址法**，也就是**数组+链表**的方式。

## 2、其中的重要字段：

```
/**实际存储的key-value键值对的个数*/
transient int size;

/**阈值，当table == {}时，该值为初始容量（初始容量默认为16）；当table被填充了，也就是为table分配内存空间后，
threshold一般为 capacity*loadFactory。HashMap在进行扩容时需要参考threshold，后面会详细谈到*/
int threshold;

/**负载因子，代表了table的填充度有多少，默认是0.75
加载因子存在的原因，还是因为减缓哈希冲突，如果初始桶为16，等到满16个元素才扩容，某些桶里可能就有不止一个元素了。
所以加载因子默认为0.75，也就是说大小为16的HashMap，到了第13个元素，就会扩容成32。
*/
final float loadFactor;

/**HashMap被改变的次数，由于HashMap非线程安全，在对HashMap进行迭代时，
如果期间其他线程的参与导致HashMap的结构发生变化了（比如put，remove等操作），
需要抛出异常ConcurrentModificationException*/
transient int modCount;

```

HashMap类中有以下主要成员变量：

transient int size ： 记录了Map中KV对的个数
loadFactor ： 装载印子，用来衡量HashMap满的程度。loadFactor的默认值为0.75f（static final float DEFAULT_LOAD_FACTOR = 0.75f;）。
int threshold：临界值，当实际KV个数超过threshold时，HashMap会将容量扩容，threshold＝容量*加载因子
除了以上这些重要成员变量外，HashMap中还有一个和他们紧密相关的概念：capacity
容量，如果不指定，默认容量是16(static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;)



## 3、相关疑问：

### 3.1 为什么负载因子要0.75？

答：在理想情况下，使用随机哈希吗，节点出现的频率在hash桶中遵循泊松分布，同时给出了桶中元素的个数和概率的对照表。
从上表可以看出当桶中元素到达8个的时候，概率已经变得非常小，也就是说用0.75作为负载因子，每个碰撞位置的链表长度超过8个是几乎不可能的。

### 3.2 为什么HashMap的长度是2的N次幂

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //这里使用与运算来计算当前插入的元素的下标位置
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
```

(n - 1) & hash 这个操作如果在n为2的N次幂的情况下是等同于 hash % n 取余数的值

　　至于为什么要使用与（&）运算呢：

　　　　因为与运算的效率要高于hash % n取余的运算

这也就解释了为什么HashMap的数组长度是2的N次幂