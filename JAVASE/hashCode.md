# 散列码：

### 1、概念理解：

<p>  散列的价值在于速度：散列使得查询得以快速执行。由于速度的瓶颈是对“键”进行查询，而存储一组元素最快的数据结构是数组，所以用它来代表键的信息，注意：数组并不保存“键”的本身。而通过“键”对象生成一个数字，将其作为数组的下标索引。这个数字就是散列码，由定义在Object的hashCode()生成(或成为散列函数)。同时，为了解决数组容量被固定的问题，不同的“键”可以产生相同的下标。那对于数组来说？怎么在同一个下标索引保存多个值呢？？原来数组并不直接保存“值”，而是保存“值”的 List。然后对 List中的“值”使用equals()方法进行线性的查询。这部分的查询自然会比较慢，但是如果有好的散列函数，每个下标索引只保存少量的值，只对很少的元素进行比较，就会快的多。

(**注意：如果两个对象相同，那么他们的hashCode值一定相同； 如果两个对象的hashCode值相同，他们不一定相同**)


### 2、正确的equals()需满足的要求：

 - 自反性: x.equals(x) 一定成立

 - 对称性: 如果x.equals(y)成立，那么y.equals(x)也一定成立
  
 - 传递性：如果x.equals(y)=true ,y.equals(z)=true,那么x.equals(z)=true也成立 

 - 一致性：无论调用x.equal(y)多少次，返回的结果应该保持一致

 - 对任何不是null的x，x.equals(null)一定返回false

### 3、hashcode 和 equals的使用：

#### **1.hashcode()和equals()是在哪里被用到的？什么用的？**

HashMap是基于散列函数，在jdk1.7中以数组和链表的方式实现、在jdk1.8中以数组+链表+红黑树的形式实现。而对于每一个对象，通过其hashCode()方法可为其生成一个整形值（散列码），该整型值被处理后，将会作为数组(Map.Entry[])下标，存放该对象所对应的Entry（存放该对象及其对应值）。

equals()方法则是在HashMap中插入值或查询时会使用到。当HashMap中插入 值或查询值对应的散列码与数组中的散列码相等时，则会通过equals方法比较key值是否相等，所以想以自建对象作为HashMap的key，必须重写 该对象继承object的equals方法。

####  **2.本来不就有hashcode()和equals()了么？干嘛要重写，直接用原来的不行么？**

   HashMap中，如果要比较key是否相等，要同时使用这两个函数！因为自定义的类的hashcode()方法继承于Object类，其hashcode码为默认的内存地 址，这样即便有相同含义的两个对象，比较也是不相等的，案例如下：

```java
自定义  PhoneNumber类
 
package comjk;
 
public class PhoneNumber {
   private int prefix; //区号
   private int phoneNumber; //电话号
public int getPrefix() {
    return prefix;
}
public void setPrefix(int prefix) {
    this.prefix = prefix;
}
public int getPhoneNumber() {
    return phoneNumber;
}
public void setPhoneNumber(int phoneNumber) {
    this.phoneNumber = phoneNumber;
}
   
   
}
 
//main 方法
 public static void main(String[] args) {
 
     Map<PhoneNumber, String> map =new  HashMap<PhoneNumber, String>();
     PhoneNumber phoneNumber1=new PhoneNumber();
     phoneNumber1.setPhoneNumber(111);
     phoneNumber1.setPrefix(111);
     PhoneNumber phoneNumber2=new PhoneNumber();
     phoneNumber2.setPhoneNumber(222);
     phoneNumber2.setPrefix(222);
     map.put(phoneNumber1, "111");
     map.put(phoneNumber2, "222");
 
     System.out.println(map.get(phoneNumber1));
     System.out.println(map.get(phoneNumber2));
 
     PhoneNumber phoneNumber3=new PhoneNumber();
     //参数内容和phoneNumber2一样
     phoneNumber3.setPhoneNumber(222);
     phoneNumber3.setPrefix(222);
     System.out.println(map.get(phoneNumber3));
   
 }
 
 
 输出结果
 
 111
 222
 null    //因为没有重写hashCode和equals方法，所以取不出phoneNumber2的值，因为这里phonenumber2和phonenumber3地址不同。
 
 
  //在PhoneNumber 类中重写equals()和hashCode()方法；
  @Override
   public boolean equals(Object o)
   {
       if(this == o)
       {
           return true;
       }
       if(!(o instanceof PhoneNumber))
       {
           return false;
       }
       PhoneNumber pn = (PhoneNumber)o;
       return pn.prefix == prefix && pn.phoneNumber == phoneNumber;
   }
 
 
   @Override
   public int hashCode()
   {
       int result = 17;
       result = 31 * result + prefix;
       result = 31 * result + phoneNumber;
       return result;
   }
 
 
 输出结果
 
  111
  222
  222
```

