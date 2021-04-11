 # 目录

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330000438503.png" alt="image-20210330000438503" style="zoom:50%;" />



# 定义

![image-20210330000733157](/Users/tony/Library/Application Support/typora-user-images/image-20210330000733157.png)

装各种各样对象的东西

BG：数组的数目是提前定义好的，很难按需分配（eg：我不知道我预先要分配多少，但是如果安一个比较大的数字去预先分配回很浪费）

一句话：可以添加别的对象（除了数组以外的容器）

# 不同的接口

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330001154779.png" alt="image-20210330001154779" style="zoom:50%;" />



1136

1个图，1个类，3个知识点，6个接口

**Set 没有顺序 不可以重复**

**List 有顺序 可以重复( equals定义)**

Collection提供了对外提供的方法

下面黄色的，是实现这些接口的各种各样的类（黄色）

![image-20210330002102880](/Users/tony/Library/Application Support/typora-user-images/image-20210330002102880.png)



# Collection 接口

例子

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330002604927.png" alt="image-20210330002604927" style="zoom: 33%;" />

注意这里有 **父类引用指向子类对象**！！！

这里老师给出的原因是： 比喻：用桶装馒头，如果有 ArrayList c =new Arraylist() 这样会用到他专门的方法，这样其实如果后面要改别的list类型，代码修改的很多，但是如果用collection，因为他是父类，所以后面的代码不用改

这里只能添加对象，不能添加基础引用类型 也就是  new Interger(100)  int 100（分配在栈上面，栈的内容有可能随时清空），所以装的必须是object

```java
import java.util.*;

public class BasicContainer {
    public static void main(String[] args) {
        Collection c = new HashSet();
        c.add("hello");
        c.add(new Name("f1","l1"));
        c.add(new Integer(100));
        c.remove("hello"); 
        c.remove(new Integer(100));
        System.out.println
                  (c.remove(new Name("f1","l1")));
        System.out.println(c);
    }


}
```

注意这里的remove采用了 equals的方法来进行对象之间的比较

因为这里的 Name对象没有重写 equals方法

BG：对象如果相等， hashcode应该相等（equals）



## 关于 equals和 hashcode方法

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330143847897.png" alt="image-20210330143847897" style="zoom:50%;" />

hashcode（当这个对象在map作为键值对的键的时候），hashcode适合做索引

结论：**要重写equals方法，就要重写hashcode方法** 当作键的时候有用 （类比：字典的目录--对应的值）

> 要重写equals方法，必须要重写hashcode方法！！！互相equals <-> 相同的hash code
>
> 我的理解：equals 比较的是值，hashcode是键，

因为你不确定别人调用的是什么办法，当用作 map的时候，有可能就会调用这个 hashcode方法，当你不确定别人用什么规则判断的时候，就要重写这两个，保持一致性（猜测是可能会出现 equal一样，但是hashcode又不一样的行为）

![image-20210330161457312](/Users/tony/Library/Application Support/typora-user-images/image-20210330161457312.png)

>  总结：
>
> 1. 如果 equals--- 那么hash code 就应该相等；容器是装各种各样对象的东西，而且list,map,set 只是一个接口，要靠下面的抽象类来具体实现
> 2. hashcode具体使用场景是 当作 map的键值对的键
> 3. 先记住 再印证



## Iterator 接口

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330182745466.png" alt="image-20210330182745466" style="zoom:50%;" />

> 复习：接口里面定义了方法的定义
>
> 为什么要有这个接口？
>
> Linked List 是一个 链表，不是连续的，而是在每一个里面存储了指向到下一个的引用；ArrayList 本质上是一个数组，连续存储空间；但是他们都是list，要是想针对list写一个循环，这两种东西难以用同一种方式来遍历

Iterator要求返回一个实现了 Iterator的对象出来，根据这个对象进行遍历，老师的理解：iterator可以理解成是一个指针

具体例子（具体返回是一个 Iterator的引用）

![image-20210330184049340](/Users/tony/Library/Application Support/typora-user-images/image-20210330184049340.png)



## 我的疑惑

？？ 我的疑问： iterator是个抽象类，为什么可以返回 iterator？？？

> 1. 实际当中，返回的是一个 实现了 Iterator接口的对象（老师的判断是这里已经有继承了，或者实现（一样的！？！））
> 2. new出来一个对象把他当成 iterator接口来用了，然后只能看到实现方法的那个部分（所以也是父类引用指向子类对象）

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330204306220.png" alt="image-20210330204306220" style="zoom:25%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330205354665.png" alt="image-20210330205354665" style="zoom:50%;" />

```java
import java.io.*;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.Collection;
public class Test329{
	public static void main(String[] args){

		Singer s = new Student("shabi");
		s.sing();
		System.out.println( s instanceof Singer );
		System.out.println( s instanceof Student );
		}

	
	public static void tree (File f,int level ){
		String preStr="";
		for (int i =0;i<level;i++){
			preStr+="    ";
		}
		File[] child= f.listFiles();
		for(int i=0;i<child.length;i++){
			System.out.println(preStr+child[i].getName());
			if(child[i].isDirectory()){
				tree(child[i],level+1);
			}
		}
		
	}
	

	public static double[][]  erwei(String s1){

		String[] s2=s1.split(";");

		int length_1= s2.length;

		double a[][]= new double [length_1][];

		for (int i=0;i<length_1;i++){
		String[] temp=s2[i].split(",");
		//这里获取了每个二维数组里面的字符串
		a[i]=new double [temp.length];
		for (int j=0;j<temp.length;j++){

			double d1=Double.valueOf(temp[j]);
			a[i][j]=d1;
		}
		}
		return a;
// Double.parseDouble()
	}
}
interface Singer {
		void sing();
		void sleep();
	}
	class Student implements Singer{
		private String name;
		Student (String s){
			this.name=s;
		}
		public void sing(){
			System.out.println("stduent "+name+ "sing");
		}
		public void sleep(){
			System.out.println("stduent "+name+ " sleep");
		}
		void eat() {
			System.out.println("stduent "+name+ " eat");
		}
	}

	
 
```

> 经验教训：
>
> 1. 第一：main方法里面如果定义了（动态）函数要调用的话，要么new一个对象，在这个对象上调用，要么就
> 2. 这个例子印证了今天的想法，就是 （1）Singer s = new Student("shabi"); 这个是可以的，而这里 Singer也是是只能看到这个接口所能看到的东西，其他方法和成员变量是看不到的 （2）这个 算是父类引用指向子类对象，算是多态的前提，用instanceof 来判断，他既是Student 也是 Singer类（也就是师兄说的 接口也可以看做是个类，但是更抽象而已）

[无法从静态上下文中引用非静态 变量 this_shengzhu1的博客-CSDN博客

[![image-20210330230338574](/Users/tony/Library/Application Support/typora-user-images/image-20210330230338574.png)](https://blog.csdn.net/shengzhu1/article/details/71375028)

## 增强for循环

```java
import java.util.*;

public class EnhancedFor {
	public static void main(String[] args) {
		int[] arr = {1, 2, 3, 4, 5};
		for(int i : arr) {
			System.out.println(i);
		}
		
		Collection c = new ArrayList();
		c.add(new String("aaa"));
		c.add(new String("bbb"));
		c.add(new String("ccc"));
		for(Object o : c) {
			System.out.println(o);
		}
	}
}
```

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330211228373.png" alt="image-20210330211228373" style="zoom:50%;" />



## Set接口

是collection的子接口，没有提供额外方法，但是不能有重复元素，无顺序，对应数学集合的概念



例子1：

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330211655963.png" alt="image-20210330211655963" style="zoom:50%;" />

> 个人理解：可以将这个父类引用指向子类对象的思想类比到iterator上面，只是iterator更抽象一点



例子2：

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330211946510.png" alt="image-20210330211946510" style="zoom:50%;" />



经验：看懂代码以后直接敲，而不是type writer



## List接口

![image-20210330214111305](/Users/tony/Library/Application Support/typora-user-images/image-20210330214111305.png)

这个和数组的区别是，他的大小改变起来非常方便

**疑问：强制转换中的执行过程？？？？？？？？**

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330214502113.png" alt="image-20210330214502113" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330214853395.png" alt="image-20210330214853395" style="zoom:50%;" />

1136 1个图 1个类 3个知识点 6个接口

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210330215102484.png" alt="image-20210330215102484" style="zoom:50%;" />



## comparable

支持排序的方法，应该里面内涵了 可以比的东西，但是事先不同的类肯定有不同的方法，所以这样的话他是调用类的接口--->结论：把对象当成是接口来调用！

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210331002926913.png" alt="image-20210331002926913" style="zoom:50%;" />

> 



# 如何选择数据结构



<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210401230629531.png" alt="image-20210401230629531" style="zoom:50%;" />

2021年04月05日11:32:26

内存空间不连续，访问第五个需要知道前四个

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405113332547.png" alt="image-20210405113332547" style="zoom:50%;" />



# Map接口

![image-20210405115211201](/Users/tony/Library/Application Support/typora-user-images/image-20210405115211201.png)

键值不能重复指的是 equals，但是比较equal效率太慢所以比较hashcode

所以重写equal要重写hashcode方法

在这里，put方法，如果key存在，就会替换值，替换掉的对象会返回成object

![image-20210405120215737](/Users/tony/Library/Application Support/typora-user-images/image-20210405120215737.png)



小疑问： new integer 和 integer

不是基础类型，是基础类型的包装类

map规定两边都要是对象（分配在堆上面）

## 打包机制

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405120741571.png" alt="image-20210405120741571" style="zoom:50%;" />

现在可以直接写 put('one',1)

```java
int i = (Integer)m1.get("two");
```

这里要加上强制转换，因为 m1.get(2) 返回的是object



exer

```java
import java.util.*;
public class TestArgsWords {
  //private static final Integer ONE = new Integer(1);
  private static final int ONE = 1;
  public static void main(String args[]) {
    Map m = new HashMap();
    for (int i = 0; i < args.length; i++) {
      //Integer freq = (Integer) m.get(args[i]);
      
      int freq = (Integer) m.get(args[i]) == null ? 0 : (Integer) m.get(args[i]);
      //m.put(args[i],(freq == null? ONE : new Integer(freq.intValue() + 1)));
      m.put(args[i], freq==0 ? ONE : freq + 1);
    }
    System.out.println
        (m.size() + " distinct words detected:");
    System.out.println(m);
  }
}
```

这里不懂的地方--基础包装类和基础类型的转换



# 泛型

![image-20210405191456558](/Users/tony/Library/Application Support/typora-user-images/image-20210405191456558.png)

```java
import java.util.*;

public class BasicGeneric {
	public static void main(String[] args) {
		List<String> c = new ArrayList<String>();
		c.add("aaa");
		c.add("bbb");
		c.add("ccc");
		for(int i=0; i<c.size(); i++) {
			String s = c.get(i);
			System.out.println(s);
		}
		
		Collection<String> c2 = new HashSet<String>();
		c2.add("aaa"); c2.add("bbb"); c2.add("ccc");
		for(Iterator<String> it = c2.iterator(); it.hasNext(); ) {
			String s = it.next();
			System.out.println(s);
		}
	}
}

class MyName implements Comparable<MyName> {
	int age;
	
	public int compareTo(MyName mn) {  //原来compareto 写的是object类型
		if(this.age > mn.age) return 1;
		else if(this.age < mn.age) return -1;
		else return 0;
	}
}
```



<要装的类型>

在这里 get以后在之前输出是一个object（可以通过查api文档），现在知道输出是一个string，就不需要强制类型转换了

![image-20210405192103651](/Users/tony/Library/Application Support/typora-user-images/image-20210405192103651.png)

这里有尖括号，就可以跟

![image-20210405192243366](/Users/tony/Library/Application Support/typora-user-images/image-20210405192243366.png)

好处：不用强制转换了



练习：

```java
 import java.util.*;
public class TestMap {
  public static void main(String args[]) {
    Map m1 = new HashMap(); 
    Map m2 = new TreeMap();
    //m1.put("one",new Integer(1));
    m1.put("one", 1);
    //m1.put("two",new Integer(2));
    m1.put("two", 2);
    //m1.put("three",new Integer(3));
    m1.put("three", 3);
    //m2.put("A",new Integer(1));
   	m2.put("A", 1);
    //m2.put("B",new Integer(2));
    m2.put("B", 2);
    System.out.println(m1.size());
    System.out.println(m1.containsKey("one"));
    System.out.println
        //(m2.containsValue(new Integer(1)));
        (m2.containsValue(1));
    if(m1.containsKey("two")) {
      //int i = ((Integer)m1.get("two")).intValue();
      int i = (Integer)m1.get("two");
      System.out.println(i);
    }
    Map m3 = new HashMap(m1);
    m3.putAll(m2);
    System.out.println(m3);
  }
}
```



更改后

```java
 import java.util.*;
public class TestMap {
  public static void main(String args[]) {
    Map <String,Integer> m1 = new HashMap <String,Integer> (); 
    Map <String,Integer> m2 = new TreeMap <String,Integer> ();
    //m1.put("one",new Integer(1));
    m1.put("one", 1);
    //m1.put("two",new Integer(2));
    m1.put("two", 2);
    //m1.put("three",new Integer(3));
    m1.put("three", 3);
    //m2.put("A",new Integer(1));
   	m2.put("A", 1);
    //m2.put("B",new Integer(2));
    m2.put("B", 2);
    System.out.println(m1.size());
    System.out.println(m1.containsKey("one"));
    System.out.println
        //(m2.containsValue(new Integer(1)));
        (m2.containsValue(1));
    if(m1.containsKey("two")) {
      //int i = ((Integer)m1.get("two")).intValue();
      //int i = (Integer)m1.get("two");
      int i = m1.get("two");
      System.out.println(i);
    }
    Map m3 = new HashMap(m1);
    m3.putAll(m2);
    System.out.println(m3);
  }
}
```

经验总结： Map两个都要求是对象类uu11111

int i = m1.get("two"); 这里包含了自动解包，解包成为 int



# 总结

![image-20210405194921037](/Users/tony/Library/Application Support/typora-user-images/image-20210405194921037.png)

collection封装了常用list的算法

Collection  Set List Map

Iterator  Comparable



2021年04月05日19:53:12