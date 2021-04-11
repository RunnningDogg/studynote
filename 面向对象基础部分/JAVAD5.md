 

# This 关键字



类的方法定义中使用 this关键字代表 使用该方法的对象的引用

场景：

1. 指出当前使用方法的对象是谁
2. this可以处理方法中成员变量和参数重名的情况
3. 可看作是个变量，对当前对象的引用



图解：

![image-20210325210538575](/Users/tony/Library/Application Support/typora-user-images/image-20210325210538575.png)



就近声明原则：

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210325212807778.png" alt="image-20210325212807778" style="zoom:50%;" />

```java
class  Leaf{
    int i=0;
    Leaf(int i){ this.i=i;}  //就近声明 i 是就近，离形参近
    Leaf increment(){
        i++;
        return this;
    }
}
```



# static 关键字

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210325213839315.png" alt="image-20210325213839315" style="zoom:50%;" />

原来，每次new一个新的对象就有一个新的对应的成员变量

注意： static 成员变量分在了 data segment

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210325214543394.png" alt="image-20210325214543394" style="zoom:80%;" />

Name 和 id 叫做 非静态变量

sid，属于某个类，任何一个类访问这个，同一块内存地址；data segment放字符串常量和静态变量

没有对象也可以访问：用  类名.sid<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210325214824266.png" alt="image-20210325214824266" style="zoom:67%;" />

```java
public class Cat {
    private static int sid = 0;
    private String name; 
    int id;
    Cat(String name) {
        this.name = name;  
        id = sid++;
    }
    public void info(){
        System.out.println
               ("My name is "+name+" No."+id);
    }
    public static void main(String arg[]){

        Cat.sid = 100;
        Cat mimi = new Cat("mimi");
        mimi.sid = 2000;
        Cat pipi = new Cat("pipi");
        mimi.info(); 
        pipi.info();
    }
}
```

静态变量可以计数用

（复习：基础变量就是一块内存，引用就是2块内存）

如果把 static去掉 ，main方法有问题，因为这样的话 sid属于某一个特定的对象，不是属于某一个类



![image-20210325220627962](/Users/tony/Library/Application Support/typora-user-images/image-20210325220627962.png)

报错是说 不能用在静态上下文![image-20210325220718767](/Users/tony/Library/Application Support/typora-user-images/image-20210325220718767.png)

知识点2：

方法针对某个对象来调用的，（前提是动态方法），如果声明了是静态方法，调用这个方法也不会将改对象的引用传递给它；  



# package  import

 BG: 解决类的命名冲突问题 eg 我写了个 cat类，小明也写了个cat类，引入包（package）管理机制，提供类多重命名空间



作为源文件的第一条语句，指明该类所在的包

格式： package pkg1 .pgk2.pkg3.....

![image-20210325222806971](/Users/tony/Library/Application Support/typora-user-images/image-20210325222806971.png)

![image-20210326171423546](/Users/tony/Library/Application Support/typora-user-images/image-20210326171423546.png)类要在正确的目录下面



```java
//package com;
import com.bjsxt.java140.*;
//import com.*;
public class Dog {
	public static void main(String[] args) {
		Cat c = new Cat(); //没有import是错的，因为会认为cat是裸体类，实际上不是
	}
}
```







如果不在同一个目录里面

![image-20210325223229274](/Users/tony/Library/Application Support/typora-user-images/image-20210325223229274.png)

![image-20210325223720894](/Users/tony/Library/Application Support/typora-user-images/image-20210325223720894.png)

类似文件系统一样，类放到包，写package，跟多层级的包名，编译出来的class要在正确的目录下来（和package一致）

![image-20210325224134909](/Users/tony/Library/Application Support/typora-user-images/image-20210325224134909.png)



Classpath?

执行一个类，需要写全包名



# 常用包

![image-20210325230913748](/Users/tony/Library/Application Support/typora-user-images/image-20210325230913748.png)



获取 java 文件路径

/usr/libexec/java_home -V

/Users/tony/Downloads/src/java.base/java

/Library/Java/JavaVirtualMachines/amazon-corretto-11.jdk/Contents/Home

然后把jar的路径加到 classpath里面就可以



# 访问控制



public protected default private

因为 protected 和继承有关，先讲继承



## 类的继承

![image-20210325233102591](/Users/tony/Library/Application Support/typora-user-images/image-20210325233102591.png)



定义： <modifer> class <name> [extends <superclass>]

多重继承：一个人有多个爸爸（多个维度）



```java
class Person {
    private String name;
    private int age;
    public void setName(String name) {
    	this.name=name;
    }
    public void setAge(int age) {
    	this.age=age;
    }
    public String getName(){
    	return name;
    }
    public int getAge(){
    	return age;
    }
}

class Student extends Person {
    private String school;
    public String getSchool() {
    	return school;
    }
    public void setSchool(String school) {
    	this.school =school;
    } 
}

public class TestPerson {
    public static void main(String arg[]){
        Student student = new Student();
        student.setName("John");
        student.setAge(18);
        student.setSchool("SCH");
        System.out.println(student.getName());
        System.out.println(student.getAge());
        System.out.println(student.getSchool());
    }
}
```

![image-20210325235238460](/Users/tony/Library/Application Support/typora-user-images/image-20210325235238460.png)

访问控制

![image-20210325232537156](/Users/tony/Library/Application Support/typora-user-images/image-20210325232537156.png)

可以修饰的东西：

1. 成员变量、方法
2. class





Eg1：  说明 类的private成员变量只能让该类的其他方法使用，另外的类

```java
public class TestAccess {

}
class T {
	private int i=0;
			int j=0;
	protected int k=0;
	public int m=0;
}

class TT{
	public void m() {
		T t =new T();
		System.out.println(t.i);
    System.out.println(t.j); //  j是可以访问的，因为这个算是在同一个目录下面（同一个包下面）
	}
}
```

![image-20210326155943101](/Users/tony/Library/Application Support/typora-user-images/image-20210326155943101.png)



总结：private，只能在类内部访问，子类拥有这个变量但是无法访问,public 任何地方都可以访问

修饰class的时候，只可以用private，default；

​	public类可以被任何地方访问，default只能被同一个包的内部类访问



猫类如果没有写public class 会报错

![image-20210326171902500](/Users/tony/Library/Application Support/typora-user-images/image-20210326171902500.png)

2021年03月26日17:35:12

# 方法重写（区分重载：参数或者类型不同）

Overwrite/override

- 子类可以根据需要对继承来的方法进行重写

+ 重写方法要有相同的名称，参数列表和返回类型

* 不能使用更严格的访问权限

![image-20210326173652871](/Users/tony/Library/Application Support/typora-user-images/image-20210326173652871.png)

```java
class Person {
    private String name;
    private int age;
    public void setName(String name){this.name=name;}
    public void setAge(int age) {this.age=age;}
    public String getName(){return name;}
    public int getAge(){return age;}
    public String getInfo() {
          return "Name: "+ name + "\n" +"age: "+ age;
  }
}

class Student extends Person {
    private String school;
    public String getSchool() {return school;}
    public void setSchool(String school)
    {this.school =school;}
    public String getInfo() {
      return  "Name: "+ getName() + "\nage: "+ getAge() 
                    + "\nschool: "+ school;
		}
}

public class TestOverWrite {
public static void main(String arg[]){
        Student student = new Student();
        Person person = new Person();
        person.setName("none");
        person.setAge(1000);
        student.setName("John");    
        student.setAge(18);
        student.setSchool("SCH");
        System.out.println(person.getInfo());
        System.out.println(student.getInfo());
    }
}
```



最怕的是什么：是我想重新接，但是因为大小写不是完全匹配，就导致写了一个新的方法



# super (内存分析)

```java
class FatherClass {
    public int value;
    public void f(){
        value = 100;
        System.out.println
        ("FatherClass.value="+value);
    }
}

class ChildClass extends FatherClass {
    public int value;
    public void f() {
        super.f();
        value = 200;
        System.out.println
             ("ChildClass.value="+value);
        System.out.println(value);
        System.out.println(super.value);
    }
}

public class TestInherit {
	public static void main(String[] args) {
		ChildClass cc = new ChildClass();
		cc.f();
	}
}

```

this 当前对象的引用， super 父类对象的引用

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210326181637070.png" alt="image-20210326181637070" style="zoom:50%;" />

  

# 继承构造方法

![image-20210326183420181](/Users/tony/Library/Application Support/typora-user-images/image-20210326183420181.png)

* 因为子类对象里面有父类对象，看内存图，super(arg list) 是因为 构造方法可以重载（同名但是功能不同的参数）

  

![image-20210326184535924](/Users/tony/Library/Application Support/typora-user-images/image-20210326184535924.png)

```java
class SuperClass {
    private int n;
  	
  	
    // SuperClass() {
    //     System.out.println("SuperClass()");
    // }
    
    
  
    SuperClass(int n) {
        System.out.println("SuperClass(" + n + ")");
        this.n = n;
    }
} 

class SubClass extends SuperClass {
    private int n;
    
    SubClass(int n) {
    		//super();
        System.out.println("SubClass(" + n + ")");
        this.n = n;
    }
    
    SubClass() {
    	super(300);	
        System.out.println("SubClass()");

    }
}

public class TestSuperSub {
    public static void main(String arg[]) {
        //SubClass sc1 = new SubClass();
        SubClass sc2 = new SubClass(400);
    }
}


```



练习：分析这个构造方法时候的内存（注意构造方法其实也有形参，也就有局部变量

![image-20210326192043870](/Users/tony/Library/Application Support/typora-user-images/image-20210326192043870.png)



```java
class Person{
	private String name;
	private String location;
	Person (String name){
		this.name=name;
		this.location="beijing";
	}
	Person (String name, String location){
		this.name=name;
		this.location=location;
	}
	public String info(){
		return "name: "+name+" location:"+location;
	}
	public String toString() {
		return "This is a person";
	}

}

class Student extends Person{
	private String school;
	Student (String name, String school){
		this(name,"beijing",school);
	}
	Student (String n, String l,String school){
		super(n,l);
		this.school=school;
	}
	public String info(){
		return super.info()+ " school:"+school;
	}
	public String toString() {
		return "This is a student";
	}
}

class Teacher extends Person{
	private String capital;
	Teacher (String name, String capital){
		this(name,"beijing",capital);
	}
	Teacher (String n, String l,String capital){
		super(n,l);
		this.capital=capital;
	}
	public String info(){
		return super.info()+ " capital:"+capital;
	}
	public String toString() {
		return "This is a professor";
	}
	public boolean equals(Object obj) {
		if (obj==null) return false;
		else{
			if(obj instanceof Teacher){
				//Teacher c =(Teacher) Object;
				// if(c.name==this.name & c.location==this.location & c.capital==this.capital){
				// 	return true;
				// }
				return true;
			}
		}
		return false;
	}
}
public class Test322 {
	public static void main(String[] args){
		// Person p1= new Person("A");
		// Person p2= new Person("B","shanghai");
		// Student s1=new Student("c","s1");
		// Student s1=new Student("c","shanghai","s2");

		Teacher t1=new Teacher("D","prof");
		Teacher t2=new Teacher("D","prof");
		System.out.println(t1.info());
		System.out.println(t1.toString());
		System.out.println(t1.equals(t2));
		System.out.println(t1==t2);
	}
}
```



# Object类

![image-20210326211303092](/Users/tony/Library/Application Support/typora-user-images/image-20210326211303092.png)



JAVA 单继承 （根类）之前定义的类，都可以看作是object这个类的继承

C++根类有好多个，但是JAVA只有一个



Class类，class关键字

哈希编码：可以根据哈希编码找到内存的位置



## toString

![image-20210326213910602](/Users/tony/Library/Application Support/typora-user-images/image-20210326213910602.png)

[Object (Java 2 Platform SE v1.5.0) (washington.edu)](https://nick-lab.gs.washington.edu/java/jdk1.5b/api/index.html)



## hashcode 

hashcode table

每个对象有独一无二的哈希编码，确定所在的位置 



## 提示

两个class文件一样，根据classpath找，这样可能会有冲突，把当前路径写在前面，用分号隔开

## equals

![image-20210326221908827](/Users/tony/Library/Application Support/typora-user-images/image-20210326221908827.png)

```java
public class TestEquals {
	public static void main(String[] args) {
		Cat c1 = new Cat(1, 2, 3);
		Cat c2 = new Cat(1, 2, 3);
		System.out.println(c1 == c2);
		System.out.println(c1.equals(c2));
		
		String s1 = new String("hello");
		String s2 = new String("hello");
		System.out.println(s1 == s2);
		System.out.println(s1.equals(s2)); 
	}
}

class Cat {
	int color;
	int height, weight;
	
	public Cat(int color, int height, int weight) {
		this.color = color;
		this.height = height;
		this.weight = weight;
	}
	
	// public boolean equals(Object obj) {
	// 	if(obj == null) return false;
	// 	else {
	// 		if(obj instanceof Cat) {
	// 			Cat c = (Cat)obj;
	// 			if(c.color == this.color && c.height == this.height && c.weight == this.weight) {
	// 				return true;
	// 			}
	// 		}
	// 	}
		
	// 	return false;
	// }
	
}
```

重点，这里比较的是地址，比较的是引用的内容

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210326222954472.png" alt="image-20210326222954472" style="zoom:50%;" />

如果想比较对象？

```java
//练习
class Person{
	private String name;
	private String location;
	Person (String name){
		this.name=name;
		this.location="beijing";
	}
	Person (String name, String location){
		this.name=name;
		this.location=location;
	}
	public String info(){
		return "name: "+name+" location:"+location;
	}
	public String toString() {
		return "This is a person";
	}
	// public boolean equals(Object obj) {
	// 	if (obj==null) return false;
	// 	else{
	// 		if(obj instanceof Person){
	// 			Person c =(Person) obj;
	// 			if(c.name==this.name & c.location==this.location){
	// 				return true;
	// 			}
	// 		}
	// 	}
	// }
}

class Student extends Person{
	private String school;
	Student (String name, String school){
		this(name,"beijing",school);
	}
	Student (String n, String l,String school){
		super(n,l);
		this.school=school;
	}
	public String info(){
		return super.info()+ " school:"+school;
	}
	public String toString() {
		return "This is a student";
	}
}

class Teacher extends Person{
	private String capital;
	Teacher (String name, String capital){
		this(name,"beijing",capital);
	}
	Teacher (String n, String l,String capital){
		super(n,l);
		this.capital=capital;
	}
	public String info(){
		return super.info()+ " capital:"+capital;
	}
	public String toString() {
		return "This is a professor";
	}
	public boolean equals(Object obj) {
		if (obj==null) return false;
		else{
			if(obj instanceof Teacher){
				//Teacher c =(Teacher) Object;
				// if(c.name==this.name & c.location==this.location & c.capital==this.capital){
				// 	return true;
				// }
				return true;
			}
		}
		return false;
	}
}
public class Test322 {
	public static void main(String[] args){
		// Person p1= new Person("A");
		// Person p2= new Person("B","shanghai");
		// Student s1=new Student("c","s1");
		// Student s1=new Student("c","shanghai","s2");

		Teacher t1=new Teacher("D","prof");
		Teacher t2=new Teacher("D","prof");
		System.out.println(t1.info());
		System.out.println(t1.toString());
		System.out.println(t1.equals(t2));
		System.out.println(t1==t2);
	}
}
```





# 对象转型（casting)

![image-20210326231721768](/Users/tony/Library/Application Support/typora-user-images/image-20210326231721768.png)

 

传入动物，可以传入狗；但是不能引入狗特有的变量和方法

![image-20210326232246407](/Users/tony/Library/Application Support/typora-user-images/image-20210326232246407.png)

父类引用指向子类：注意a是一个父类的引用，指向了dog，所以在这里，a这里是把它看作动物了，而不是dog，但是这里的，a看成是animal，但是实际上是dog

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210326233057514.png" alt="image-20210326233057514" style="zoom:50%;" />

![image-20210326235640474](/Users/tony/Library/Application Support/typora-user-images/image-20210326235640474.png)

这个例子说明对象转型对于可扩展性的好处（因为这里可以传入子类）

总结：

父类引用可以指向子类 -》 定义父类引用，传入子类对象

# 多态（动态绑定）（核心）

![image-20210327000238811](/Users/tony/Library/Application Support/typora-user-images/image-20210327000238811.png)

 ```java
abstract class Animal {
  private String name;
  Animal(String name) {this.name = name;}
  /*
  public void enjoy(){
    System.out.println("½ÐÉù......");
  }
  */
  public abstract void enjoy();
}

abstract class Cat extends Animal {
  private String eyesColor;
  Cat(String n,String c) {super(n); eyesColor = c;}
  /*
  public void enjoy() {
    System.out.println("Ã¨½ÐÉù......");
  }
  */
  //public abstract void enjoy();
}

class Dog extends Animal {
  private String furColor;
  Dog(String n,String c) {super(n); furColor = c;}
 
  public void enjoy() {
    System.out.println("¹·½ÐÉù......");
  }
}

class Bird extends Animal {
	 Bird() {
	 	 super("bird");
	 }
	 public void enjoy() {
    System.out.println("Äñ½ÐÉù......");
  }
}

class Lady {
    private String name;
    private Animal pet;
    Lady(String name,Animal pet) {
        this.name = name; this.pet = pet;
    }
    public void myPetEnjoy(){pet.enjoy();}
}

public class Test {
    public static void main(String args[]){
        Cat c = new Cat("catname","blue");
        Dog d = new Dog("dogname","black");
        Bird b = new Bird();
        //Lady l1 = new Lady("l1",c);
        Lady l2 = new Lady("l2",d);
        Lady l3 = new Lady("l3",b);
       //l1.myPetEnjoy();
        l2.myPetEnjoy();
        l3.myPetEnjoy();
    }
}

 ```

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327001455784.png" alt="image-20210327001455784" style="zoom:75%;" />

在这里 Animal这个指向的应该是指向 dog中动物的部分，刚刚讲过不能引用成员变量，但是方法不同（方法是在code segment中）；注意这里是三个方法（*区分和前面说的对象转型的限制*），而调用的方法是根据实际类型，不是引用类型（在这里引用类型是animal）

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327001943500.png" alt="image-20210327001943500" style="zoom:50%;" />

说明：实际new的是什么类型，就调用谁的enjoy方法（说明要在实际运行的时候才能确定调度的是哪一个方法）（说明只要方法重写了，实际当中调动的是什么方法看new的是什么对象而确定）

核心机制: 指针（一开始方法指向animal，但是后面随着new的对象回改变指针的地址，）



目的：可扩展性

>  存在条件
>
> 1. 继承
> 2. 要有重写
> 3. 父类引用指向子类对象

# 抽象类

![image-20210327004514761](/Users/tony/Library/Application Support/typora-user-images/image-20210327004514761.png)

当类里面有抽象方法的时候，必须被声明为抽象类

可以再声明，但是不能实例化

（24-42）



# final

修饰变量，方法，类

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327010015246.png" alt="image-20210327010015246" style="zoom:50%;" />

![image-20210327010155313](/Users/tony/Library/Application Support/typora-user-images/image-20210327010155313.png)

```java
public class TestFinal {
	public static void main(String[] args) {
		T t = new T();
		//t.i = 8;
	}
}

final class T {
	final int i = 8;
	public final void m() {
		j = 9;
	}
}

class TT extends T {
	public final void m() {
		j = 9;
	}
}
```





# 接口：抽象类（暴露了一部分方法）

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327114607001.png" alt="image-20210327114607001" style="zoom:50%;" />

 ![image-20210327114711156](/Users/tony/Library/Application Support/typora-user-images/image-20210327114711156.png)

![image-20210327115256523](/Users/tony/Library/Application Support/typora-user-images/image-20210327115256523.png)例子： 金丝猴是动物，也是受保护对象，但是语法 extends只能继承一个，模拟现实有多继承的情况

为什么：多重继承

为什么静态常量： 多父类有相同成员变量

## 实现接口=实现接口的抽象方法

![image-20210327115639631](/Users/tony/Library/Application Support/typora-user-images/image-20210327115639631.png)

![image-20210327115858577](/Users/tony/Library/Application Support/typora-user-images/image-20210327115858577.png)

这里 类可以实现多个接口

![image-20210327120051688](/Users/tony/Library/Application Support/typora-user-images/image-20210327120051688.png)

接口类型的变量，singer s1 <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327120625649.png" alt="image-20210327120625649" style="zoom:50%;" />

在这里是类似的，定义了 singer类型的变量，所以不能看到 study方法，这里也是 父类对象指向子类，实现了多态

p1把teacher当painter来看

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327121238945.png" alt="image-20210327121238945" style="zoom:50%;" />

这里定义的f是父类引用指向子类对象，因为子类实现了singer，所以这里实现的sing方法有多态的意思



## 接口第二部分：介绍

![image-20210327121918603](/Users/tony/Library/Application Support/typora-user-images/image-20210327121918603.png)

comparable 可以让我的排序方法适用于所有的对象（不用为每个对象重复写）

```java
public interface Valuable {
	public double getMoney();
}

interface Protectable {
	public void beProtected();
}

interface A extends Protectable {
	void m();   //相当于定义了两个方法，需要实现 beProtected 和 m（）
	//void getMoney();
}

abstract class Animal {
	private String name;
	
	abstract void enjoy();
}

class GoldenMonkey extends Animal implements Valuable, Protectable {
	public double getMoney() {
		return 10000;
	}
	
	public void beProtected() {
		System.out.println("live in the room");
	}
	
	public void enjoy() {
		
	}
	
	public void test() {
		Valuable v = new GoldenMonkey(); //定义了valueable变量，所以只能看到 get money方法
		v.getMoney();
		Protectable p = (Protectable)v;  //强制转换
		p.beProtected();
	}
}

class Hen implements A  {
	public void m() {}
	public void beProtected() {}
	public double getMoney() {
		return 1.0;
	}
	
	public void getMoney() {}

}
```



接口之间可以相互继承，**类只能实现接口**，类之间相互继承（接口也可以）

## 第三部分

两个接口的方法一样





# 总结

![image-20210327130134032](/Users/tony/Library/Application Support/typora-user-images/image-20210327130134032.png)

![image-20210327123529875](/Users/tony/Library/Application Support/typora-user-images/image-20210327123529875.png)

```java

class People implements Feedpet {
	private String name;
	People(String s){
		this.name=s;
	}
	public void play() {
		System.out.println("People: "+name +" play with pet");
	}
	public void feed() {
		System.out.println("People: " +name +" feed pet");
	}
}

public class Test327 {
	public static void main(String[] args){
		People a = new People("tony");
		a.feed();
		a.play();
	}
}
```

![image-20210327130054578](/Users/tony/Library/Application Support/typora-user-images/image-20210327130054578.png)

```java
public interface Feedpet {
	public void play();
	public void feed();
}
```

