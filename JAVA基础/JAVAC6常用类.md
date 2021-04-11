

# 常用类列表

字符串相关类

基本数据类型包装类

Marh类

FIle类

枚举类





# String

![image-20210328221737434](/Users/tony/Library/Application Support/typora-user-images/image-20210328221737434.png)

[String (Java 2 Platform SE v1.5.0) (washington.edu)](https://nick-lab.gs.washington.edu/java/jdk1.5b/api/index.html)

字节数组变成 字符串

deprecated 过时，废弃

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328222401383.png" alt="image-20210328222401383" style="zoom:50%;" />

**这里是true的原因，不是new出来的，字符串常量在data segment，这里的s1和s3都是指向s3**



<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328222537606.png" alt="image-20210328222537606" style="zoom:50%;" />

false，不是指定同一个东西 （==比较的是是不是指向同一个东西，但是equals默认是==，可以重写）

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328222647395.png" alt="image-20210328222647395" style="zoom:50%;" />

c：字符数组，看构造方法



## 常用方法

![image-20210328222833714](/Users/tony/Library/Application Support/typora-user-images/image-20210328222833714.png)

length是字符的长度



eg![image-20210328223049758](/Users/tony/Library/Application Support/typora-user-images/image-20210328223049758.png)



### 静态重载方法

![image-20210328223338516](/Users/tony/Library/Application Support/typora-user-images/image-20210328223338516.png)

![image-20210328223553905](/Users/tony/Library/Application Support/typora-user-images/image-20210328223553905.png)

这里隐含了一点，这个ovject会调用自身的 tostring方法



### 练习题

注意：字符比较的时候比较的是 asc码

思路：取出每一个字符，作比较，然后写判断语句（可以用asc码来判断，也可以用 indexof来判断）

还可以参考， Character 这个类

（疑惑点：字符有基础类型包装类！基础类型·包装类的意义是什么）



![image-20210328230234305](/Users/tony/Library/Application Support/typora-user-images/image-20210328230234305.png)





## String Buffer

![image-20210328230356200](/Users/tony/Library/Application Support/typora-user-images/image-20210328230356200.png)



eg: 意义 说明 helloworld这个拼接，以及不可变字符串的意义

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328230803853.png" alt="image-20210328230803853" style="zoom:50%;" />

![image-20210328231033068](/Users/tony/Library/Application Support/typora-user-images/image-20210328231033068.png)

![image-20210328231129365](/Users/tony/Library/Application Support/typora-user-images/image-20210328231129365.png)

![image-20210328231231869](/Users/tony/Library/Application Support/typora-user-images/image-20210328231231869.png)

![image-20210328231424510](/Users/tony/Library/Application Support/typora-user-images/image-20210328231424510.png)



# 基础数据类型包装类

![image-20210328231635078](/Users/tony/Library/Application Support/typora-user-images/image-20210328231635078.png)



一般基本数据类型在栈上，如果要分配到堆上面，就要使用基础数据类型包装类



[Double (Java 2 Platform SE v1.5.0) (washington.edu)](https://nick-lab.gs.washington.edu/java/jdk1.5b/api/index.html)

![image-20210328232049811](/Users/tony/Library/Application Support/typora-user-images/image-20210328232049811.png)

![image-20210328232116537](/Users/tony/Library/Application Support/typora-user-images/image-20210328232116537.png)



联系

![image-20210328232441765](/Users/tony/Library/Application Support/typora-user-images/image-20210328232441765.png)

```java
public class Test329{
	public static void main(String[] args){
		//Test329 test= new Test329();
		String s1="1,2;3,4,5;6,7,8";
		double [][] s2= erwei(s1);

		for (int i=0;i<s2.length;i++){
			//System.out.println(s2[i]);
			for (int j=0;j<s2[i].length;j++){
				System.out.print(s2[i][j]+" ");
			}
			System.out.print("\n");
		}		
		
	}
	


//
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

	}
// //double[][]
// 	public static void  erwei(String s1){

// 		String[] s2=s1.split(";");

// 		int length_1= s2.length;

// 		double a[][]= new double [length_1][];

// 		for (int i=0;i<length_1;i++){
// 		String[] temp=s2[i].split(",");
// 		//这里获取了每个二维数组里面的字符串

// 		for (int j=0;j<temp.length;j++){

// 			double d1=Double.valueOf(temp[j]);
// 			//a[i][j]=new double d1;
// 			System.out.print(d1+" ");
// 		}
// 		//System.out.print("\n");
// 		}
// 		//return a;

// 	}
	
} 
```

写程序循序渐进

总结： double [][] [] [] d 这里只是声明了这个空间，里面都是 null

![image-20210329183327598](/Users/tony/Library/Application Support/typora-user-images/image-20210329183327598.png)



# Math

![image-20210329183720803](/Users/tony/Library/Application Support/typora-user-images/image-20210329183720803.png)



# File(只是文件名)

![image-20210329184503871](/Users/tony/Library/Application Support/typora-user-images/image-20210329184503871.png)

目录/文件

![image-20210329185533153](/Users/tony/Library/Application Support/typora-user-images/image-20210329185533153.png)



当类在包里面的时候，上层路径要先找最上层的包，再找路径

file既可以代表路径，也可以代表子文件

getparent file 的方法



联练习：不会（调出子目录结构）

```java
import java.io.*;
public class Test329{
	public static void main(String[] args){
		//Test329 test= new Test329();
		// String s1="1,2;3,4,5;6,7,8;9,10;11,12,31";
		// double [][] s2= erwei(s1);

		// for (int i=0;i<s2.length;i++){
		// 	//System.out.println(s2[i]);
		// 	for (int j=0;j<s2[i].length;j++){
		// 		System.out.print(s2[i][j]+" ");
		// 	}
		// 	System.out.print("\n");
		// }		

		//System.out.println(File.separator);
		String directory = "mydir1";
		File f = new File(directory);
		tree(f,0);
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
	
```



# 枚举类型



![image-20210329220018591](/Users/tony/Library/Application Support/typora-user-images/image-20210329220018591.png)

![image-20210329220410418](/Users/tony/Library/Application Support/typora-user-images/image-20210329220410418.png)

只能写预先定义好的纸

# 总结

![image-20210329221415033](/Users/tony/Library/Application Support/typora-user-images/image-20210329221415033.png)