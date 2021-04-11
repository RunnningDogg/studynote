# Classloader动态加载机制

Class 和 Classloader

> <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408110937399.png" alt="image-20210408110937399" style="zoom: 67%;" />
>
> <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408114934866.png" alt="image-20210408114934866" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408153153120.png" alt="image-20210408153153120" style="zoom:50%;" />



eg：

```java
public class TestJDKClassLoader{
	public static void main(String[] args){
		// System.out.println(String.class.getClassLoader());//最核心的classloader
		// System.out.println(TestJDKClassLoader.class.getClassLoader());//最核心的classloader
		new C();
		new C();
		new D();
		new D();
	}
}

class A {

}
class B {
	
}
class C {
	static {
		System.out.println("ccccc");
	}
}

class D {
	 {
		System.out.println("ddddd");
	}
}
```

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408154435183.png" alt="image-20210408154435183" style="zoom:50%;" />

说明 classloader不是一次性的加载完成全部的class，而是分别加载

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408153544382.png" alt="image-20210408153544382" style="zoom:50%;" />

这里是先加载A这个class，然后再加载b这个class，而不是一次性加载全部class

# classloader分类

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408114955179.png" alt="image-20210408114955179" style="zoom:50%;" />

用到的时候加载class文件，运行期间动态加载

用到的时候才加载

static	只执行一次

dynamic 每次new都执行

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408111911494.png" alt="image-20210408111911494" style="zoom:50%;" />

最核心的类被 bootstrap加载；

extension class loader 扩展类

application classloader

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408113333291.png" alt="image-20210408113333291" style="zoom:50%;" />

```java
public class TestJDKClassLoader{
	public static void main(String[] args){
		System.out.println(String.class.getClassLoader());//最核心的classloader
		System.out.println(TestJDKClassLoader.class.getClassLoader());//最核心的classloader
	}
}
```



## 层次关系

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408113705555.png" alt="image-20210408113705555" style="zoom:50%;" />

现在是研究对象之间的关系而不是继承的关系

有一个引用指向

>  加载机制：确认parent classloader有没有加载进来，如果已经加载进来了就不会重新进行加载



# 反射机制

BG：

> <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408114934866.png" alt="image-20210408114934866" style="zoom:50%;" />
>
> <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408114955179.png" alt="image-20210408114955179" style="zoom:50%;" />
>
> <img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408115058619.png" alt="image-20210408115058619" style="zoom:50%;" />

在classloader角度，**每个class是一个个的对象，如果再放大看，class里面的方法和成员变量也是对象**

问题：我告诉你从一个文件读类的名字，然后构建类，这里是没办法通过之前的知识说明  T t= new T()；因为这样的话，只要我改变名字，这个T的类又要变，

思路：

（1）先把这个类load进来，但是这个类现在不确定有没有load进来，所以要用 classloader  

实现方法： Class.forName()

```java
import java.lang.reflect.Method;

//import java.lang.reflect.*;
public class TestReflection{
    public  static void main(String[] args) throws Exception{
        String str="T";
        
        //System.out.println(Class.forName(str));
        Class c =Class.forName(str);
        Object o= c.getDeclaredConstructor().newInstance();
        Method[] methods= c.getMethods();
        for (Method m : methods){
            //System.out.println(m.getName());
            if (m.getName().equals("mm")) {
                m.invoke(o);  //调用方法 注意这里需要new一个对象，在这个对象上引用方法
            }
        }



        }
    }


class T {
    static {
        System.out.println("T loaded");
    }
    public T(){
        System.out.println("T constructed");
    }
    int i;
    String s;
    public void m1 (int i){
        this.i=i;
    }
    public String getS (){
        return s;
    }
    public void mm(){
        System.out.println("m invoked");
    }
}
```

invoke 是可变参数的方法（可以传0个或者多个参数）

应用场景
<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408165428716.png" alt="image-20210408165428716" style="zoom:50%;" />

getParameterTypes可以获取参数类型



> 总结：反射机制，在运行期间探索他的 class文件内部机制，根据内部结构执行相对应的方法

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408171354520.png" alt="image-20210408171354520" style="zoom:50%;" />

静态工厂方法： 不new对象，通过调用方法来new

抽象工厂

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408171434917.png" alt="image-20210408171434917" style="zoom:50%;" />

什么工厂产生什么坦克/子弹/爆炸....

？单例模式