# 目录

![image-20210407165309393](/Users/tony/Library/Application Support/typora-user-images/image-20210407165309393.png)

网络编程 != 网站编程（网页，动态网站）



# 基础概念

![image-20210407165508879](/Users/tony/Library/Application Support/typora-user-images/image-20210407165508879.png)



计算机网络：一堆计算机连着别的计算机

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210407165750561.png" alt="image-20210407165750561" style="zoom:50%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210407165952132.png" alt="image-20210407165952132" style="zoom:50%;" />

分层思想！ 效率可能会稍微低一点，第五层和第四层打交道

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210407171129240.png" alt="image-20210407171129240" style="zoom:50%;" />

网络：ip  传输：tcp/udp

## 数据封装和数据拆封

![image-20210407171312858](/Users/tony/Library/Application Support/typora-user-images/image-20210407171312858.png)

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210407171455359.png" alt="image-20210407171455359" style="zoom:50%;" />

>  推荐书籍: TCP/IP详解

# IP层

![image-20210407171643101](/Users/tony/Library/Application Support/typora-user-images/image-20210407171643101.png)

提供了独一无二的IP地址，4个字节 32位： 网络ID（占一个字节是A类网）+主机ID

子网掩码：凡是1的都是网络ID，凡是0的是主机ID

网关：2块网卡

公网IP：



# TCP/UDP

![image-20210407172929669](/Users/tony/Library/Application Support/typora-user-images/image-20210407172929669.png)

三次捂手

TCP首先要建立连接

tcp：慢，但是可靠 udp:快，但是不可靠



### 技术/管理/沟通



# Socket 插座

![image-20210407182936864](/Users/tony/Library/Application Support/typora-user-images/image-20210407182936864.png)

Socket 用在 client端的插座

ServerSocket  服务器端的插座

端口号： 两个字节，（一个应用程序能用多个端口号） 作用：用来区分 同一台机器的不同应用程序 1024以下的可能是系统端口号



端口号分为 TCP端口和  UDP端口

Server client一起写

![image-20210407190234713](/Users/tony/Library/Application Support/typora-user-images/image-20210407190234713.png)

？监听

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210407201447811.png" alt="image-20210407201447811" style="zoom: 67%;" />

通过管道/流 来说话

！异步式！

```java
import java.net.*;
import java.io.*;
public class TCPServer{
	public static void main(String[] args) throws Exception{
		ServerSocket ss =new ServerSocket(6666);
		while (true){
			Socket s =ss.accept(); //阻塞式 跟System.in 一样
			//Listens for a connection to be made to this socket and accepts it.
			DataInputStream dis = new DataInputStream(s.getInputStream());
			System.out.println(dis.readUTF());//阻塞式
			//System.out.println("测试端口");
			dis.close();
			s.close();
		}

	}
}

// client
import java.net.*;
import java.io.*;
public class TCPClient{
	public static void main(String[] args) throws Exception{
		Socket ss =new Socket("127.0.0.01",6666);
		OutputStream os=ss.getOutputStream();
		DataOutputStream dos= new DataOutputStream(os);
		dos.writeUTF("hello server");
		dos.flush();
		dos.close();
		ss.close();
	}
}

```



###  ！实现聊天的功能



## UDP

DatagramPacket  存在这里

DatagramSocket

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210408001836706.png" alt="image-20210408001836706" style="zoom:50%;" />

```java
import java.net.*;
import java.io.*;

public class TestUDPServer
{
	public static void main(String args[]) throws Exception
	{
		byte buf[] = new byte[1024];
		DatagramPacket dp = new DatagramPacket(buf, buf.length);
		DatagramSocket ds = new DatagramSocket(5678);
		while(true)
		{
			ds.receive(dp);
			ByteArrayInputStream bais = new ByteArrayInputStream(buf);
			DataInputStream dis = new DataInputStream(bais);
			System.out.println(dis.readLong());
		}
	}
}



import java.net.*;
import java.io.*;

public class TestUDPClient
{
	public static void main(String args[]) throws Exception
	{
		long n = 10000L;
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		DataOutputStream dos = new DataOutputStream(baos);
		dos.writeLong(n);
		
		byte[] buf = baos.toByteArray();
System.out.println(buf.length);
		
		DatagramPacket dp = new DatagramPacket(buf, buf.length, 
											   new InetSocketAddress("127.0.0.1", 5678) //这里本来穿的是SocketAddress 父类引用指向子类对象
											   );
		DatagramSocket ds = new DatagramSocket(9999);
		ds.send(dp);
		ds.close();
		
	}
}
```

![image-20210408002252074](/Users/tony/Library/Application Support/typora-user-images/image-20210408002252074.png)



# 总结

![image-20210408003150863](/Users/tony/Library/Application Support/typora-user-images/image-20210408003150863.png)

