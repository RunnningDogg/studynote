2021年03月27日18:30:16



# 目录

异常的概念

分类

捕获处理

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327183049871.png" alt="image-20210327183049871" style="zoom:50%;" />



数组：

int [] arr={1,2,3}

相当于在内存中分了三个小格，每个小格都是int类型，有下标（从0开始）

![image-20210327184558267](/Users/tony/Library/Application Support/typora-user-images/image-20210327184558267.png)



```java
		int[] arr = {1, 2, 3};
		System.out.println(arr[2]);
		try {
			System.out.println(2/0);
		} catch (ArithmeticException e) {
			System.out.println("系统正在维护,请与管理员联系");
			e.printStackTrace();
		}
```



# 概念



定义：运行期间出现的错误，找catech的代码，有catch就跳到相关地方处理

这也是一个异常的对象，有错误，系统机会帮我初始化



# 分类

![image-20210327190101224](/Users/tony/Library/Application Support/typora-user-images/image-20210327190101224.png)

![image-20210327190406816](/Users/tony/Library/Application Support/typora-user-images/image-20210327190406816.png)

![image-20210327190501641](/Users/tony/Library/Application Support/typora-user-images/image-20210327190501641.png)

![image-20210327190653751](/Users/tony/Library/Application Support/typora-user-images/image-20210327190653751.png)

![image-20210327191241200](/Users/tony/Library/Application Support/typora-user-images/image-20210327191241200.png)

Exception：可以处理的异常



throwable：可被抛出的， error：系统的错误 exception：可以处理的错误

runtime exception：经常出现的错误；（可以不处理）

api文档的 throw出来的，必须逮住他



eg

![image-20210327191842958](/Users/tony/Library/Application Support/typora-user-images/image-20210327191842958.png)

```java
	FileInputStream in = null;
	
try {
    in = new FileInputStream("myfile.txt");
    int b;
    b = in.read();
    while (b != -1) {
        System.out.print((char) b);
        b = in.read();
    }
}  catch (IOException e) {
  System.out.println(e.getMessage());
 	
} catch (FileNotFoundException e) {
	e.printStackTrace(); 
  
} finally {
	try {
  	in.close();
  } catch (IOException e) {
  	e.printStackTrace();
  }
}
```


# 格式

![image-20210327200253324](/Users/tony/Library/Application Support/typora-user-images/image-20210327200253324.png)

![image-20210327200412083](/Users/tony/Library/Application Support/typora-user-images/image-20210327200412083.png)

![image-20210327200701662](/Users/tony/Library/Application Support/typora-user-images/image-20210327200701662.png)

![image-20210327200807925](/Users/tony/Library/Application Support/typora-user-images/image-20210327200807925.png)

![image-20210327200817367](/Users/tony/Library/Application Support/typora-user-images/image-20210327200817367.png)

```java
	FileInputStream in = null;
	
try {
    in = new FileInputStream("myfile.txt");
    int b;
    b = in.read();
    while (b != -1) {
        System.out.print((char) b);
        b = in.read();
    }
}  catch (IOException e) {
  System.out.println(e.getMessage());
 	
} catch (FileNotFoundException e) {
	e.printStackTrace(); 
  
} finally {
	try {
  	in.close();
  } catch (IOException e) {
  	e.printStackTrace();
  }
}
```


# 异常捕获和处理

![image-20210327201124081](/Users/tony/Library/Application Support/typora-user-images/image-20210327201124081.png)

 throws  往外跑异常

```java
void f() throws FileNotFoundException , IOException {
	FileInputStream in = new FileInputStream("myfile.txt");
int b;
b = in.read();
while (b != -1) {
    System.out.print((char) b);
    b = in.read();
}
}
```


常见exceptron，不要把错误吞噬掉，能处理的异常要处理，不能处理就往外抛



手动

```java
void m(int i) throws ArithmeticException {
	if(i==0) 
		throw new ArithmeticException("被除数为0");
}
```


try catch finally thorw  throws



<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210327202429291.png" alt="image-20210327202429291" style="zoom:50%;" />





# 自定义异常

![image-20210327202824824](/Users/tony/Library/Application Support/typora-user-images/image-20210327202824824.png)

![image-20210327203213625](/Users/tony/Library/Application Support/typora-user-images/image-20210327203213625.png)



# 注意

![image-20210327203312993](/Users/tony/Library/Application Support/typora-user-images/image-20210327203312993.png)



# 总结

一个图

5个关键字

先捕获小异常，再捕获大异常

异常和重写的关系