# Java 面向对象 

2021年03月23日20:36:03

## 命名规则

1. 类名的首字母大写
2. 变量名和方法名的首字母小写
3. 驼峰，意思是如果有几个单词构成，这几个单词都应该是大写  HelloWorld



## 内存解析

```java
class BirthDate {
    private int day;
    private int month;
    private int year;
    
    public BirthDate(int d, int m, int y) {
        day = d; 
        month = m; 
        year = y;
    }
    
    public void setDay(int d) {
    	day = d;
  	}
  	
    public void setMonth(int m) {
    	month = m;
    }
    
    public void setYear(int y) {
    	year = y;
    }
    
    public int getDay() {
    	return day;
    }
    
    public int getMonth() {
    	return month;
    }
    
    public int getYear() {
    	return year;
    }
    
    public void display() {
    	System.out.println
        (day + " - " + month + " - " + year);
    }
}


public class Test{
    public static void main(String args[]){
        Test test = new Test();
        int date = 9;
        BirthDate d1= new BirthDate(7,7,1970);
        BirthDate d2= new BirthDate(1,1,2000);    
        test.change1(date);
        test.change2(d1);
        test.change3(d2);
        System.out.println("date=" + date);
        d1.display();
        d2.display();
    }
    
    public void change1(int i){
    	i = 1234;
    }
    
    public void change2(BirthDate b) {
    	b = new BirthDate(22,2,2004);
    }
    
    public void change3(BirthDate b) {
    	b.setDay(22);
    }
}

```



方法调用是值传递，方法执行完毕分配的局部变量内存全部消失

Test test = new Test();  定义了一个test，局部变量

这里直接写 change1不行（目前） 要先new对象，针对某个对象调用方法



注意：每次有局部变量 就会在 stack 创造一块内存创造，然后执行完就销毁



# 练习

```java
class Point{
    private int x;
    private int y;
    private int z;
// 构造函数
    public Point(int d ,int m, int y){
        x=d;y=m;z=y;
    }
    public void setx (int d){
        x=d;
    }
    public void sety (int m){
        y=m;
    }

    public void setz (int y){
        z=y;
    }

    public int getx(){
        return x;
    }
    public int gety(){
        return y;
    }
    public int getz(){
        return z;
    }

    public void display() {
        System.out.println(x+"-"+y+"-"+z);
    }
    public double Getdistance(){
        double i = Math.sqrt(x*x+y*y+z*z);
        return i;
    }
}

public class TestVar2 {
    public static void main(String[] args){
    /*   boolean b=true;
        int x,y=9;
        double d =3.1415;
        char c1,c2;
        c1='\u534e';c2='c';
        x=12;
        System.out.println("b=" +b );
        System.out.println("x=" +x +"y=" +y );
        System.out.println("d=" +d );
        System.out.println("c1=" +c1 );
        System.out.println("c2=" +c2);
    */
        Point a= new Point(2,4,5);
        a.setx(10);
        System.out.println(a.Getdistance());
        // a.display();        
        }
    }




```

注意：main方法结束以后，相关变量就消失，当没有变量指向new的东西的时候，过一会垃圾收集器就把气手机





# 方法重载(Overload)

定义：类里面定义有相同的名字，但是参数不同的多个方法，调用的时候根据不同的参数选择对应的方法

```java
void info(){
  System.out.println("test")
}

void info( String t ){
  System.out.println("test"+t)
}
```

判断方法参数个数和参数类型任意一个不一样就可以



构造方法也可以重载

```java
class Point{
    private int x;
    private int y;
    private int z;
// 构造函数
    public Point(int d ,int m, int y){
        x=d;y=m;z=y;
    }
      public Point(){
        x=1;y=2;z=3;
    }
}
```



复习：

对象的创建和使用：

1. new关键字来创建对象
2. 对象引用.成员变量 来引用相关的成员变量  person.age
3. 对象引用,方法   person.setage()
4. 同一个类的每个对象有不同成员变量的存储空间
5. 同个类的每个对象共享该类的方法（非静态方法针对每个对象进行调用）



TestCircle 内存分析：

```java
public class TestCircle {
    public static void main(String args[]) {
        Circle c1 = new Circle(new Point(1.0,2.0), 2.0); 
      //临时变量1.0,2.0 生成 heap的 空间 Circle 也在 heap空间，C1 在占 指向他
        Circle c2 = new Circle(5.0);
      //方法重载，point 0.0 ，C2 在占 指向他
        System.out.println("c1:("+c1.getO().getX()+","
            +c1.getO().getY()+"),"+c1.getRadius());
      // c1.get0  返回0的值 临时在栈,
        System.out.println("c2:("+c2.getO().getX()
            +","+c2.getO().getY()+"),"+c2.getRadius());
        System.out.println("c1 area = "+c1.area());
      //
        System.out.println("c1 area = "+c2.area());
        c1.setO(5,6);
        c2.setRadius(9.0);
        System.out.println("c1:("+c1.getO().getX()+","
            +c1.getO().getY()+"),"+c1.getRadius());
        System.out.println("c2:("+c2.getO().getX()+","
            +c2.getO().getY()+"),"+c2.getRadius());
        System.out.println("c1 area = "+c1.area());
        System.out.println("c1 area = "+c2.area());
        
        Point p1 = new Point(5.2, 6.3);
        System.out.println(c1.contains(p1));
        System.out.println(c1.contains(new Point(10.0,9.0)));
        
    }
}
```



不确定的地方，return内存是怎么样

判断错了

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210324005156994.png" alt="image-20210324005156994" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210324005632286.png" alt="image-20210324005632286" style="zoom:50%;" />



方法是在某个具体的对象上实现的！![image-20210324012316957](/Users/tony/Library/Application Support/typora-user-images/image-20210324012316957.png)

局部变量： 方法体内部声明（包括形参）

成员变量：类体里面声明的变量









