## Comparable和Comparator的理解：

### 一、Comparable简介：

####   1、 Comparable是排序接口。若一个类实现了Comparable接口，就意味着该类支持排序。实现了Comparable接口的类的对象的列表或数组可以通过Collections.sort或Arrays.sort进行自动排序。此外，实现此接口的对象可以用作有序映射中的键或有序集合中的集合，无需指定比较器。该接口定义如下：

```java
package java.lang;
import java.util.*;
public interface Comparable<T> 
{
    public int compareTo(T o);
}
```

注：T表示可以与此对象进行比较的那些对象的类型。此接口只有一个方法compare，比较此对象与指定对象的顺序，如果该对象小于、等于或大于指定对象，则分别返回负整数（-1）、零（0）或正整数（1）。

#### 2、案例学习：

```java
public class Person implements Comparable<Person>
{
    String name;
    int age;
    public Person(String name, int age)
    {
        super();
        this.name = name;
        this.age = age;
    }
    public String getName()
    {
        return name;
    }
    public int getAge()
    {
        return age;
    }
    @Override
    public int compareTo(Person p)
    {
        return this.age-p.getAge();
    }
    public static void main(String[] args)
    {
        Person[] people=new Person[]{new Person("xujian", 20),new Person("xiewei", 10)};
        System.out.println("排序前");
        for (Person person : people)
        {
            System.out.print(person.getName()+":"+person.getAge());
        }
        Arrays.sort(people);
        System.out.println("\n排序后");
        for (Person person : people)
        {
            System.out.print(person.getName()+":"+person.getAge());
        }
    }
}
```



### 二、Comparator简介：

####     1、 Comparator是比较接口，我们如果需要控制某个类的次序，而该类本身不支持排序(即没有实现Comparable接口)，那么我们就可以建立一个“该类的比较器”来进行排序，这个“比较器”只需要实现Comparator接口即可。也就是说，我们可以通过实现Comparator来新建一个比较器，然后通过这个比较器对类进行排序。该接口定义如下：

```java
package java.util;
public interface Comparator<T>
 {
    int compare(T o1, T o2);
    boolean equals(Object obj);
 }
```

注意：

+ 1、若一个类要实现Comparator接口：它一定要实现compare(T o1, T o2) 函数，但可以不实现 equals(Object obj) 函数。

+ 2、int compare(T o1, T o2) 是“比较o1和o2的大小”。返回“负数”，意味着“o1比o2小”；返回“零”，意味着“o1等于o2”；返回“正数”，意味着“o1大于o2”。

  ####  2、案例学习：

  新建构造器：

  ```java
  public class PersonCompartor implements Comparator<Person>
  {
      @Override
      public int compare(Person o1, Person o2)
      {
          return o1.getAge()-o2.getAge();
      }
  }
  ```

  使用构造器：

  ```java
  public class Person
  {
      String name;
      int age;
      public Person(String name, int age)
      {
          super();
          this.name = name;
          this.age = age;
      }
      public String getName()
      {
          return name;
      }
      public int getAge()
      {
          return age;
      }
      public static void main(String[] args)
      {
          Person[] people=new Person[]{new Person("xujian", 20),new Person("xiewei", 10)};
          System.out.println("排序前");
          for (Person person : people)
          {
              System.out.print(person.getName()+":"+person.getAge());
          }
          
          //调用自定义的Compartor比较器
          Arrays.sort(people,new PersonCompartor());
          
          
          System.out.println("\n排序后");
          for (Person person : people)
          {
              System.out.print(person.getName()+":"+person.getAge());
          }
      }
  }
  ```

  

### 区别：

#### Comparable

Comparable可以认为是一个**内比较器**，实现了Comparable接口的类有一个特点，就是这些类是可以和自己比较的，至于具体和另一个实现了Comparable接口的类如何比较，则依赖compareTo方法的实现，compareTo方法也被称为**自然比较方法**。如果开发者add进入一个Collection的对象想要Collections的sort方法帮你自动进行排序的话，那么这个对象必须实现Comparable接口。compareTo方法的返回值是int，有三种情况：

1、比较者大于被比较者（也就是compareTo方法里面的对象），那么返回正整数

2、比较者等于被比较者，那么返回0

3、比较者小于被比较者，那么返回负整数



#### Comparator

Comparator可以认为是是一个**外比较器**，个人认为有两种情况可以使用实现Comparator接口的方式：

1、一个对象不支持自己和自己比较（没有实现Comparable接口），但是又想对两个对象进行比较

2、一个对象实现了Comparable接口，但是开发者认为compareTo方法中的比较方式并不是自己想要的那种比较方式

Comparator接口里面有一个compare方法，方法有两个参数T o1和T o2，是泛型的表示方式，分别表示待比较的两个对象，方法返回值和Comparable接口一样是int，有三种情况：

1、o1大于o2，返回正整数

2、o1等于o2，返回0

3、o1小于o3，返回负整数