# this 关键字的作用：

### 1、this调用当前属性：其主要作用是当需要给类中的数据进行初始化时，可以通过this来进行赋值，而不用随便定义一个变量来进行赋值，更有利于代码的阅读与理解

```java
	class Book{//定义书的类
	    private String name;//书本名字
	    private int price;//书本价格
	    public Book(String name,int price){ //使用this对类的数据进行初始化
	        this.name = name;        
	　　　 　this.price = price;
	    }
    
 		.....

	}

```

### 2、this调用方法（普通方法、构造方法）

##### 2.1 普通方法：

```java
	class Book{//定义书的类
    private String name;//书本名字
	    private int price;//书本价格
	    public Book(String name,int price){//使用this对类的数据进行初始化
	        this.name = name; 6         this.price = price;

	        this.print();//调用本类普通方法，虽然可以不用使用this也可以进行本类普通方法的调用，但是好的习惯最好应该加上，目的是可以区分方法的定义来源

	    }
	    public String getInfo(){
	        return "书籍：" + name + ",价格：" + price;
	    }
	    public void print(){
	        System.out.printIn("***********");
	    }
}

``` 

##### 2.2 构造方法：普通方法与构造方法的区别是构造方法在创建后只能掉用一次，用来初始化数据，而普通方法在创建后可以调用多次。

```java
	class Book{//定义书的类
	    private String name;//书本名字
	    private int price;//书本价格
	    public Book(){//无参构造
	        System.out.printIn("*************");
	    }
	    public Book(String name){//一参构造
	        this();//调用本类中的无参构造
	        this.name = name;
	    }
	    public Book(String name,int price){//二参构造
	       
			this(name);        //调用本类中的一参构造
	        
			this.price = price;
	     }

}


```

### 3、this表示当前方法：只要有某一个对象调用本类中的方法，此时this就表示当前执行的对象

```java
	class ThisFun {
		public void fun(){
	        System.out.println("FUN方法"+this);
	    }
	}
	public class ThisTest{
		public static void main(String[] args) {
			System.out.println("******a1*****");
			ThisFun a1 = new ThisFun();
			a1.fun();
			
			System.out.println("******a2*****");
			ThisFun a2 = new ThisFun();
			a2.fun();
			
		}
	}

```