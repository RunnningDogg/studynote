# 目录

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406164258451.png" alt="image-20210406164258451" style="zoom:50%;" />



# 基本概念

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406164319747.png" alt="image-20210406164319747" style="zoom:50%;" />

> 1. 线程：程序里面不同的执行路径；（进程里面不同的执行路径？）
>
> 2. 进程：静态概念， ；进程执行：进程里面主线程的执行
>
> main方法：主分支，主线程
>
> 3. 在一个时间点上，cpu只执行一个线程，但是因为速度太快（反复横跳？）

![image-20210406165600991](/Users/tony/Library/Application Support/typora-user-images/image-20210406165600991.png)



# 线程的创建和启动

![image-20210406165621755](/Users/tony/Library/Application Support/typora-user-images/image-20210406165621755.png)

实现了 runnable 接口以后，就是线程类

```java
public class TestThread1 {
	public static void main(String args[]) {
		Runner1 r = new Runner1();
		r.start();
		//r.run();
		//Thread t = new Thread(r);
		//t.start();
		
		for(int i=0; i<100; i++) {
			System.out.println("Main Thread:------" + i);
		}
	}
}

//class Runner1 implements Runnable {
class Runner1 extends Thread {
	public void run() {
		for(int i=0; i<100; i++) {	
			System.out.println("Runner1 :" + i);
		}
	}
}

```





注意区分：方法调用 vs 线程启动

2种实现方式，extend thread；implements Runnable

start会 产生另一个分支，（并行执行）

直接调用run方法，这是方法调用，就是<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406170516618.png" alt="image-20210406170516618" style="zoom:33%;" />

# 线程状态

![image-20210406181753488](/Users/tony/Library/Application Support/typora-user-images/image-20210406181753488.png)

new出来是一个对象

就绪不是立即执行

![image-20210406182530065](/Users/tony/Library/Application Support/typora-user-images/image-20210406182530065.png)



## 基本方法

![image-20210406183018616](/Users/tony/Library/Application Support/typora-user-images/image-20210406183018616.png)

重写的方法不同抛出不同的异常

因为是静

```java
import java.util.*;
public class TestInterrupt {
  public static void main(String[] args) {
    MyThread thread = new MyThread();
    thread.start();
    try {Thread.sleep(10000);}
    catch (InterruptedException e) {}
    thread.interrupt();
  }
}

class MyThread extends Thread {
	boolean flag = true;
  public void run(){
    while(flag){
      System.out.println("==="+new Date()+"===");
      try {
        sleep(1000);
      } catch (InterruptedException e) {
        return;
      }
    }
  }
}
```



join方法 示例



<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406191840666.png" alt="image-20210406191840666" style="zoom:50%;" />

相当于方法调用



# 线程优先级

 ![image-20210406192717682](/Users/tony/Library/Application Support/typora-user-images/image-20210406192717682.png)

```java
public class TestPriority {
	public static void main(String[] args) {
		Thread t1 = new Thread(new T1());
		Thread t2 = new Thread(new T2());
		t1.setPriority(Thread.NORM_PRIORITY + 3);
		t1.start();
		t2.start();
	}
}

class T1 implements Runnable {
	public void run() {
		for(int i=0; i<1000; i++) {
			System.out.println("T1: " + i);
		}
	}
}

class T2 implements Runnable {
	public void run() {
		for(int i=0; i<1000; i++) {
			System.out.println("------T2: " + i);
		}
	}
}
```

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406192959318.png" alt="image-20210406192959318" style="zoom:50%;" />



# 线程同步

访问多个资源进行协调

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406211638767.png" alt="image-20210406211638767" style="zoom:50%;" />

Timer里面如果没有 synchronized add的话，结果是2个2，因为 t1访问加1，睡一秒

```java
public class TestSync implements Runnable {
  Timer timer = new Timer();
  public static void main(String[] args) {
    TestSync test = new TestSync();
    Thread t1 = new Thread(test);
    Thread t2 = new Thread(test);
    t1.setName("t1"); 
    t2.setName("t2");
    t1.start(); 
    t2.start();
  }
  public void run(){
    timer.add(Thread.currentThread().getName());
  }
}

class Timer{
  private static int num = 0;
  public synchronized void add(String name){ 
  	//synchronized (this) {
	    num ++;
	    try {Thread.sleep(1);} 
	    catch (InterruptedException e) {}
	    System.out.println(name+", 你是第"+num+"个使用timer的线程");
	  //}
  }
}
```

执行方法的过程中，当前对象被锁定！





## 死锁

![image-20210406213409797](/Users/tony/Library/Application Support/typora-user-images/image-20210406213409797.png)

线程执行过程中锁定某一个对象，但是他需要锁定另外一个对象才能完成，但是这个对象已经被其他进程锁定，并且需要该进程锁定的对象才能完成

![image-20210406213712543](/Users/tony/Library/Application Support/typora-user-images/image-20210406213712543.png)



解决思路：把锁的粒度变粗一点

## 面试题

```java
public class TT implements Runnable {
	int b = 100;
	
	public synchronized void m1() throws Exception{
		//Thread.sleep(2000);
		b = 1000;
		Thread.sleep(5000);
		System.out.println("b = " + b);
	}
	
	public synchronized void m2() throws Exception {
		System.out.println(b);
	}
	
	public void run() {
		try {
			m1();
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
	
	public static void main(String[] args) throws Exception {
		TT tt = new TT();
		Thread t = new Thread(tt);
		t.start();
		Thread.sleep(1000);
		tt.m2();
		// System.out.println(tt.b);
	}
}
```

start -> 调用线程，执行run方法，即执行m1 ，

变量：先声明，赋值，在使用

syn m1 只能保证只有一个线程访问这个方法，不能保证多个线程访问别的方法，所以是锁方法

这里同步了m1 和m2 就不能访问了



# 生产者消费者问题

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406234354531.png" alt="image-20210406234354531" style="zoom:50%;" />

```java

public class ProducerCon{
	public static void main (String[] args){
		SyncStack ss= new SyncStack();
		Producer p=new Producer(ss);
		Consumer c= new Consumer(ss);
		new Thread(p).start();
		new Thread(c).start();
	}
}

class Wotou{
	int id;
	Wotou(int id){
		this.id=id;
	}
	public String toString(){
		return "wotou"+id;
	}
}

class SyncStack{
	int index=0;
	Wotou[] arrWT= new Wotou [6];

	public  synchronized  void push(Wotou wt){
	 while (index==arrWT.length){
			try{
				this.wait(); //object的wait,当前正在访问这个对象的线程的wait,wait的时候锁就不归所有
			}
			catch (InterruptedException  e ){
  			e.printStackTrace();}
		} 
		this.notify();
		arrWT[index]=wt;
		index++; //这里如果有多线程调用，这两句话有可能被打断，所以需要不能被打断
	}

  public synchronized  Wotou pop(){
  	while (index==0){  //while vs if
  		try{
  			this.wait();
  		} catch (InterruptedException  e ){
  			e.printStackTrace();
  		}
  		
  	}
  	this.notify(); //叫醒在wait这个对象的线程？
  	index --;
  	return arrWT[index];
  }

}

class Producer implements Runnable {
	SyncStack ss =null;
	Producer (SyncStack ss ){
		this.ss=ss;
	}

	public void run (){
		for (int i=0;i<20;i++){
			Wotou wt= new Wotou(i);
			ss.push(wt);
			System.out.println("生产了"+wt);
			try{
				Thread.sleep((int)Math.random() * 200);
			}
			catch (InterruptedException e){
				e.printStackTrace();
			}
		}
	}

}

class Consumer implements Runnable {
	SyncStack ss =null;
	Consumer (SyncStack ss ){
		this.ss=ss;
	}

	public void run (){
		for (int i=0;i<20;i++){
			Wotou wt=ss.pop();
			System.out.println("消费了"+wt);
		}
			try{
				Thread.sleep((int)Math.random() * 1000);
			}
			catch (InterruptedException e){
				e.printStackTrace();
			}
	}

}
```

notify 叫醒等在这个对象的的线程

wait使用情况：堵塞事件，什么时候醒过来，另一个方法调用notify



![image-20210407005255656](/Users/tony/Library/Application Support/typora-user-images/image-20210407005255656.png)



# 总结

![image-20210407005415069](/Users/tony/Library/Application Support/typora-user-images/image-20210407005415069.png)

不同执行路径，进程 class文件，静态



2021年04月07日00:55:31