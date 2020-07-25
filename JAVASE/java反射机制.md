# 反射机制

### 1、含义：这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制

+ 功能：
	- JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；
	- 对于任意一个对象，都能够调用它的任意方法和属性；

### 2、反射机制的运用

#### 2.1 实体类

```Java
package test;
 
public class Student {
	public Student(){
		
	}
	public Student(int num,String name){
		
	}
	private int num;
	
	public String name;
	
	protected String address;
}


```

#### 2.2 通过反射机制获取类

```java

	//第一种方式：
	Classc1 = Class.forName("Employee");
	//第二种方式：
	//java中每个类型都有class 属性.
	Classc2 = Employee.class;
	 
	//第三种方式：
	//java语言中任何一个java对象都有getClass 方法
	Employeee = new Employee();
	Classc3 = e.getClass(); //c3是运行时类 (e的运行时类是Employee)

```

#### 2.3 通过获取到的类新建对象：
```java

    Class c =Class.forName("Employee");
    //创建此Class 对象所表示的类的一个新实例
    Objecto = c.newInstance(); //调用了Employee的无参数构造方法.

```

#### 2.4 获取构造其中的方法：

```java

    //测试类中的代码
        Class c=Student.class;
		//getConstructors只能获得公共构造器//getDeclaredConstructors获得所有构造器
        //constructor是获取构造器时所用
		Constructor[] cs= c.getConstructors;
		for (Constructor constructor : cs) {
		Student st=(Student)constructor.newInstance();
		System.out.println(st);
	}
 
```

#### 2.5 获取类中属性
```java
public class Test{
    
       public static void main(String[] args) throws InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
	        //获得反射，获取属性的关键代码
	 
	        Class c=Student.class;
			Field[] fs=	c.getDeclaredFields();
			for (Field field : fs) {
				System.out.println(field);
			}
	       //输出的结果为
	       /*   private int com.qm.test.Student.num
	        *   public java.lang.String com.qm.test.Student.name
	        *   protected java.lang.String com.qm.test.Student.address
	        */
	    }
}

```

#### 2.6 获取类中方法：
```java
public class Test{
    
       public static void main(String[] args) throws InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException {
     //获取方法的关键代码
 
        Class c=Student.class;
		Method[] ms=c.getMethods();
		for (Method method : ms) {
			System.out.println(method);
		}
 
    //此段代码将会输出Student类中继承了Object类的一些方法
 
}
}

```
