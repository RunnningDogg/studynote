**Day3**

# JAVA面向对象



1. 习题

```java
public class TestIF {
    public static void main(String[] args){
        
     /*   int num=0;
        for(int i=0;i<=100;i++){

            if (i%3==0) {
                System.out.println(i);
                num++;
            }
        
            if (num==5)
            break; 
        }
        m();
        m2(3);
        System.out.println(m3(1));
    */
        System.out.println(m4(40));

    }
    public static  void m(){
        System.out.println("ok");
        System.out.println("main");

    }
    public static  void m2(int i ){
        if(i>3)
            return;
        System.out.println(i);

    }
    public static  int m3(int i ){
        if(i>3)
            return i;
        else 
            return i+5;

    }
    public static int m4(int i){
        int result=0;
        int x=1;
        int y=1;
        int count=3;
        if (i<=2){System.out.println(1);}
        while (count <= i) {
            result=y+x;
            x=y;
            y=result; //第一个
            count++;
        }
        return result;

    }
}

```



## 面向对象

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318131222730.png" alt="image-20210318131222730" style="zoom:50%;" />

## 编程语言的发展：

机器语言，汇编语言，高级语言（面向对象语言--直接描述问题和客观存在的事物）

面向对象

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318131602625.png" alt="image-20210318131602625" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318132230217.png" alt="image-20210318132230217" style="zoom:50%;" />

我的理解：封装了一些信息（方法在内部定义）不用了解具体实现的细节，车暴露的方法就是去



## 基本概念

### 对象/类

对象用计算机语言对问题事物进行描述，对象通过属性和方法对应事物的静态/动态属性 也可以说成成员变量/方法

类是描述一类对象的抽象概念，类定义了对象拥有的静态和动态属性

类看作是对象的模板，对象看作是类的实例

eg：

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318132530519.png" alt="image-20210318132530519" style="zoom:33%;" />



**Notation: 属性和成员变量是一回事**  

每个对象都有自己的属性值，一般不一样

实例/对象： instance

类: object

-----

### 类之间的关系（设计模式）

1. 关联关系

学生--老师

具体表现：方法的参数是另一个类的对象



2. 继承关系 一般和特殊

xx是一种xx

eg：游泳运动员是运动员

3. 聚合关系 整体和部分

球队和 1. 队长 2. 队员

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318135121134.png" alt="image-20210318135121134" style="zoom:33%;" />

聚集：不是非你不可

组合：必不可少



4. 实现关系



<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210318135247940.png" alt="image-20210318135247940" style="zoom:33%;" />

不同的车从车继承下来，但是对于去都有不同的实现方法

多态（后续补充）

应该如何思考：  应该有哪些类/对象，考虑应有的方法/属性。再考虑类之间的关系



# 类的定义

![image-20210329190112717](/Users/tony/Library/Application Support/typora-user-images/image-20210329190112717.png)

![image-20210329190144825](/Users/tony/Library/Application Support/typora-user-images/image-20210329190144825.png)



## 引用

![image-20210329190214606](/Users/tony/Library/Application Support/typora-user-images/image-20210329190214606.png)

![image-20210329190358868](/Users/tony/Library/Application Support/typora-user-images/image-20210329190358868.png)



## 方法

![image-20210329190427729](/Users/tony/Library/Application Support/typora-user-images/image-20210329190427729.png)



## 构造方法

![image-20210329190526632](/Users/tony/Library/Application Support/typora-user-images/image-20210329190526632.png)