



![image-20210327234004535](/Users/tony/Library/Application Support/typora-user-images/image-20210327234004535.png)

![image-20210327234032819](/Users/tony/Library/Application Support/typora-user-images/image-20210327234032819.png)



# 一维数组的声明

![image-20210327234154331](/Users/tony/Library/Application Support/typora-user-images/image-20210327234154331.png)

注意这里 a1 装的是 string对象的引用（复习：除了基础类型以外（4类8种）其他都是引用类型）![image-20210327234541097](/Users/tony/Library/Application Support/typora-user-images/image-20210327234541097.png)

![image-20210327234816132](/Users/tony/Library/Application Support/typora-user-images/image-20210327234816132.png)

![image-20210327235349276](/Users/tony/Library/Application Support/typora-user-images/image-20210327235349276.png)



元素为引用类型的数组：

![image-20210327235526529](/Users/tony/Library/Application Support/typora-user-images/image-20210327235526529.png)

这里days装的是date的引用

![image-20210327235646678](/Users/tony/Library/Application Support/typora-user-images/image-20210327235646678.png)

![image-20210327235722473](/Users/tony/Library/Application Support/typora-user-images/image-20210327235722473.png)



# 初始化

1. 动态初始化  先分配空间然后赋值
2. 静态初始化

![image-20210328002346986](/Users/tony/Library/Application Support/typora-user-images/image-20210328002346986.png)



静态初始化：定义数组的同事为数组元素分配空间并赋值

![image-20210328002445309](/Users/tony/Library/Application Support/typora-user-images/image-20210328002445309.png)



## 默认初始化

![image-20210328002526964](/Users/tony/Library/Application Support/typora-user-images/image-20210328002526964.png)



## 数组元素的引用

![image-20210328002617194](/Users/tony/Library/Application Support/typora-user-images/image-20210328002617194.png)

a.length是属性

```java
public class Test328{
	public static void main(String[] args){
		int [] a ={2,4,6,7,3,5,1,9,8};
		// for (int i =0;i<a.length;i++){
		// 	System.out.print(a[i]+" ");
		// }
		for (int i =0;i<args.length;i++){
			System.out.print(args[i]+" ");
		}
	}
}
```

这里可以解释 String[]是什么意思了，这是接受命令行参数，放到一个字符串的数组里面



一般来说是在栈空间上（4类8种），但是后面可以强制改成在堆空间上

eg  基础类型的包装类

![image-20210328005505204](/Users/tony/Library/Application Support/typora-user-images/image-20210328005505204.png)

![image-20210328005701487](/Users/tony/Library/Application Support/typora-user-images/image-20210328005701487.png)

还是静态方法，所以可以直接用类名

```java
public class TestArgs {
	public static void main(String[] args) {
		/*
		for(int i=0; i<args.length; i++) {
			System.out.println(args[i]);
		}
		
		System.out.println( 
              "Usage: java Test \"n1\" \"op\" \"n2\"");
              */
    if(args.length<3){
            System.out.println( 
              "Usage: java Test \"n1\" \"op\" \"n2\"");
            System.exit(-1);
    } 
    double d1 = Double.parseDouble(args[0]);//强制转换类型
    double d2 = Double.parseDouble(args[2]);
    double d = 0;
    if(args[1].equals("+")) d = d1+d2;
    else if(args[1].equals("-")) d = d1-d2;
    else if(args[1].equals("x")) d = d1*d2;
    else if(args[1].equals("/")) d = d1/d2;
    else{
        System.out.println("Error operator!"); 
        System.exit(-1);
    }   
    System.out.println(d);
	}
}
```





自己实现的排序方法

```java
public class Test328{
	public static void main(String[] args){
		//int [] a ={2,4,6,7,3,5,1,9,8};
		// for (int i =0;i<a.length;i++){
		// 	System.out.print(a[i]+" ");
		// }
		int [] a;
		a= new int [9];
		for (int i =0;i<args.length;i++){
			a[i]=Integer.parseInt(args[i]);
		}

		// for (int i =0;i<a.length;i++){
		// 	System.out.print(a[i]+" ");
		// }

		for (int i=0;i<a.length;i++){
			//int a_1=a[i];
			for (int j=i;j<a.length;j++){
				if (a[j]<a[i]){
					int k=a[i];
					a[i]=a[j];
					a[j]=k;

				}
			}
			System.out.println(a[i]);
		}		
		for (int i =0;i<a.length;i++){
			System.out.print(a[i]+" ");
		}

	}
}
```

算法分析思路：白纸写循环，画图

![image-20210328014946307](/Users/tony/Library/Application Support/typora-user-images/image-20210328014946307.png)

## 选择排序改进

改进：只做一次交换

![image-20210328015141733](/Users/tony/Library/Application Support/typora-user-images/image-20210328015141733.png)

![image-20210328015732373](/Users/tony/Library/Application Support/typora-user-images/image-20210328015732373.png)





Q：什么是返回数组的引用

![image-20210328183157131](/Users/tony/Library/Application Support/typora-user-images/image-20210328183157131.png)

重点：比较引用类型的大小（前面是基本类型）

冒泡排序法： 

```java
public class TestDateSort {
	public static void main(String[] args) {
		Date[] days = new Date[5];
		days[0] = new Date(2006, 5, 4);
		days[1] = new Date(2006, 7, 4);
		days[2] = new Date(2008, 5, 4);
		days[3] = new Date(2004, 5, 9);
		days[4] = new Date(2004, 5, 4);
		
		Date d = new Date(2006, 7, 4);
		String str = String.valueOf(d);
		//str = d.toString();
		bubbleSort(days);
		
		for(int i=0; i<days.length; i++) {
			System.out.println(days[i]);
		}
		
		System.out.println(binarySearch(days, d));
	}
	
	 public static Date[] bubbleSort(Date[] a){
        int len = a.length;
        for(int i = len-1;i>=1;i--){
            for(int j = 0;j<=i-1;j++){
                if(a[j].compare(a[j+1]) > 0){
                    Date temp = a[j]; 
                    a[j]=a[j+1];
                    a[j+1]=temp;
                }
            }
        }
        return a;
    }
    
    public static int binarySearch(Date[] days, Date d) {
    	if (days.length==0) return -1;
    
	    int startPos = 0; 
	    int endPos = days.length-1;
	    int m = (startPos + endPos) / 2;
	    while(startPos <= endPos){
	      if(d.compare(days[m]) == 0) return m;
	      if(d.compare(days[m]) > 0) {
	      	startPos = m + 1;
	      }
	      if(d.compare(days[m]) < 0) {
	      	endPos = m -1;
	      }
	      m = (startPos + endPos) / 2;
	    }
	    return -1;
    }
}

class Date {
  int year, month, day;
  
  Date(int y, int m, int d) {
    year = y; month = m; day = d;
  }
  
  public int compare(Date date) {
    return year > date.year ? 1
           : year < date.year ? -1
           : month > date.month ? 1
           : month < date.month ? -1
           : day > date.day ? 1
           : day < date.day ? -1 : 0;
  }
  
  public String toString() {
  	return "Year:Month:Day -- " + year + "-" + month + "-" + day;
  }
}
```



# 搜索算法

搜索旺旺建立在排好序的基础上



# 二维数组

![image-20210328193823605](/Users/tony/Library/Application Support/typora-user-images/image-20210328193823605.png)

![image-20210328194039708](/Users/tony/Library/Application Support/typora-user-images/image-20210328194039708.png)



![image-20210328195540901](/Users/tony/Library/Application Support/typora-user-images/image-20210328195540901.png)



多维数组相当于数组的数组】

内存复习

![image-20210328201313556](/Users/tony/Library/Application Support/typora-user-images/image-20210328201313556.png)





# 数组拷贝

![image-20210328212813849](/Users/tony/Library/Application Support/typora-user-images/image-20210328212813849.png)

数组的内存地址通常是连续的






    
```java
public class TestArrayCopy {
  public static void main(String args[]) {
    String[] s = 
            {"Mircosoft","IBM","Sun","Oracle","Apple"};
    String[] sBak = new String[6];
    System.arraycopy(s,0,sBak,0,s.length);
    
    for(int i=0;i<sBak.length;i++){
      System.out.print(sBak[i]+" ");
    }
    
    System.out.println();
    int[][] intArray = {{1,2},{1,2,3},{3,4}};
    int[][] intArrayBak = new int[3][];
    System.arraycopy
            (intArray,0,intArrayBak,0,intArray.length);
    intArrayBak[2][1] = 100;
    
    for(int i = 0;i<intArray.length;i++){
        for(int j =0;j<intArray[i].length;j++){
            System.out.print(intArray[i][j]+"  "); 
        }
        System.out.println();
    }
  }
}

```


注意区分字符和字符串，字符串也是类，所以也是引用

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328213902219.png" alt="image-20210328213902219" style="zoom:30%;" />

<img src="/Users/tony/Library/Application Support/typora-user-images/image-20210328214115668.png" alt="image-20210328214115668" style="zoom:33%;" />





# 总结：

内存布局（数组是引用类型）

常见算法