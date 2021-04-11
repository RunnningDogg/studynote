# 目录

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405232606085.png" alt="image-20210405232606085" style="zoom:50%;" />

2021年04月05日23:27:03

# 定义：流

![image-20210405232726679](/Users/tony/Library/Application Support/typora-user-images/image-20210405232726679.png)

BG：文件是桶：常见方法弄管道

这一章讲的是不同的管道



# 分类

![image-20210405233305274](/Users/tony/Library/Application Support/typora-user-images/image-20210405233305274.png)

常用思想：用接口来组织抽象的东西，这里是抽象类（4根管道）

默认程序视角

输入流：读；

输出流：写

字节流：原始流（0，1）8位01

字符流：一个字符一个字符往外读（字符是2个字节 UTF16）

## 节点流

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405233502126.png" alt="image-20210405233502126" style="zoom:50%;" />



## InputStream

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405234301744.png" alt="image-20210405234301744" style="zoom:50%;" />



基本方法：

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405234409936.png" alt="image-20210405234409936" style="zoom:50%;" />

buffer是缓冲区，字节数组



## OutputStream

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405234819838.png" alt="image-20210405234819838" style="zoom:50%;" />



基本方法

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405234904270.png" alt="image-20210405234904270" style="zoom:50%;" />



BG：因为硬盘反复读写很浪费时间，可以空出一块内存放这个



## Reader

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235114258.png" alt="image-20210405235114258" style="zoom:50%;" />

BG：中文 2个字符

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235209583.png" alt="image-20210405235209583" style="zoom:50%;" />



## Writer

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235241382.png" alt="image-20210405235241382" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235255935.png" alt="image-20210405235255935" style="zoom:50%;" />



# 具体分类

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235444892.png" alt="image-20210405235444892" style="zoom:50%;" />

管道 线程之间的通讯

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210405235611587.png" alt="image-20210405235611587" style="zoom:50%;" />

![image-20210406000646766](/Users/tony/Library/Application Support/typora-user-images/image-20210406000646766.png)

因为中文两个字符

需要复习的东西：第三章 引用类型是不是都要new一个东西？？？？



用filereader writer 复制一个文件



# 处理流

包在别的管道上的管道！

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406112505921.png" alt="image-20210406112505921" style="zoom:50%;" />

## 缓冲流

不用反复读写

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406112529426.png" alt="image-20210406112529426" style="zoom:50%;" />



BufferedInputStream

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406112823593.png" alt="image-20210406112823593" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406113141680.png" alt="image-20210406113141680" style="zoom: 50%;" />

bufferreader 有一个好用的点 readline 直接读一行

注意输入是分配到程序的内存区域上

## 转换流

可以把字节流转换成字符流

```java
import java.io.*;
public class TestTransForm1 {
  public static void main(String[] args) {
    try {
      OutputStreamWriter osw = new OutputStreamWriter(
           new FileOutputStream("d:\\bak\\char.txt")); //这里是因为 OutPutStreamWriter可以直接写字符进去，
      osw.write("mircosoftibmsunapplehp");
      System.out.println(osw.getEncoding());
      osw.close();
      osw = new OutputStreamWriter(
      								new FileOutputStream("d:\\bak\\char.txt", true),
      								"ISO8859_1"); // latin-1
      osw.write("mircosoftibmsunapplehp");
      System.out.println(osw.getEncoding());
      osw.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

![image-20210406124828606](/Users/tony/Library/Application Support/typora-user-images/image-20210406124828606.png)

这里是父类引用指向子类对象

```java
import java.io.*;
public class TestTransForm2 {
  public static void main(String args[]) {
    InputStreamReader isr = 
            new InputStreamReader(System.in);
    BufferedReader br = new BufferedReader(isr);
    String s = null;
    try {
      s = br.readLine();
      while(s!=null){
        if(s.equalsIgnoreCase("exit")) break;
        System.out.println(s.toUpperCase());
        s = br.readLine();
      }
      br.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
} //阻塞
```

input stream

  in 已经是一个窗口

这种方法是阻塞式的方法



## 数据流

BG：写一个long的数，先转成字符串，再转成字节数组？

![image-20210406133756849](/Users/tony/Library/Application Support/typora-user-images/image-20210406133756849.png)

一个long占8个字节？  float 4个 double 8个

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406133924333.png" alt="image-20210406133924333" style="zoom:50%;" />

```java
import java.io.*;
public class TestDataStream {
  public static void main(String[] args) {
    ByteArrayOutputStream baos = 
                        new ByteArrayOutputStream(); 
    DataOutputStream dos = 
                        new DataOutputStream(baos);
    try {
      dos.writeDouble(Math.random());
      dos.writeBoolean(true);
      ByteArrayInputStream bais = 
          new ByteArrayInputStream(baos.toByteArray());
      System.out.println(bais.available());
      DataInputStream dis = new DataInputStream(bais);
      System.out.println(dis.readDouble());
      System.out.println(dis.readBoolean());
      dos.close();  dis.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

ByteArrayOutputStream();  先在内存分配一个字节数组，然后再插一个管道

![image-20210406135415016](/Users/tony/Library/Application Support/typora-user-images/image-20210406135415016.png)

先写的先读



## Print流

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406143053277.png" alt="image-20210406143053277" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406143604877.png" alt="image-20210406143604877" style="zoom:50%;" />

原本out的输出是在命令行，setout改变了

2021年04月06日15:05:33

## Object流

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406152011114.png" alt="image-20210406152011114" style="zoom:50%;" />

序列化（对象转换成字节流）

把整个object写进去

Serializable 标记化接口

```java
class T 
	implements Serializable
{
	int i = 10;
	int j = 9;
	double d = 2.3;
	transient int k = 15;
}
```

transient 透明的  这个值是不写 在编译的时候

![image-20210406152144953](/Users/tony/Library/Application Support/typora-user-images/image-20210406152144953.png)

子接口：控制自己的序列化过程



# 总结

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210406152258654.png" alt="image-20210406152258654" style="zoom:50%;" />

输入输出 字节字符 节点处理



拿一张纸写下来名字（晚上）