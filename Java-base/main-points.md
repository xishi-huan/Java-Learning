1. Object类包含一个方法getClass,可返回一个实例（对象）的类型类。
类型类指的是代表一个类型的类，类型也可以认为是一个对象
表示一个特定类型的类型类也可以用“类型.class”的方式获得。
2. Java的存储空间：  
寄存器，堆，栈，静态存储区，常量存储区（常量池）、其他存储位置    
堆：存储new出来的对象，此对象由垃圾收集器收集。  
常量池：存储final static、String的常量  
栈：每调用一个方法，会创建一个栈帧，存放局部变量。
3. String使用equals 和 == 区别  
推荐>https://www.cnblogs.com/baotong-9396/p/7182906.html   
String既可以作为一个对象来使用，又可以作为一个基本类型来使用。这里指的作为一个基本类型来使用只是指使用方法上的，比如String s = "Hello"，它的使用方法如同基本类型int一样，比如int i = 1;，而作为一个对象来使用，则是指通过new关键字来创建一个新对象，比如String s = new String("Hello")。
``` 
//示例一

String a = new String("hello");  
String b = new String("hello");  
//两个new的"hello"对象，内容相同，但在堆中是两个不同的地址。
System.out.println(a == b);		//false  
System.out.println(a.equals(b)); //true

示例二

String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1 == s2); //true
System.out.println(s1.equals(s2)); //true
 
```
4. 静态Double.compare方法，如果第一个参数小于第二个参数，它会返回一个负值，如果两者相等则返回0；否则返回一个正值。
5. Object类中的equals方法用于判断两个对象是否具有相同的引用。  
```  
Objects.equals()
public static boolean equals(Object a,Object b){  
    return (a == b)||(a != null && a.equals(b));
}
```
6. 二分查找：  
```
public static int indexOf(int[] a,int key){
	int l=0;
	int h=a.length-1;
	while(l <= h)
	{
		int mid = l+(h-l)/2;
		if(key < a[mid])	h=mid-1;
		else if(key > a[mid])	l=mid+1;
		else	return mid;
	}
	return -1;
}
```
7. assert格式  
assert [boolean表达式]  
如果[boolean表达式]为true,程序继续执行。
	如果为false，则程序抛出AssertionError,并终止执行。
8. Scanner用法  
Sanner sc = new Scanner(System.in);  
+ 其hasNext、hasNextInt、hasNextDouble、hasNextLine等方法，在扫描输入中，如果没有了值，不会返回false，只会等待你输入新的值，然后继续执行，hasNextInt、hasNextDouble、hasNextFloat等方法如果扫描输入过程中遇到 非int或double或float类型的值，结束扫描，返回false，hasNext和hasNextLine会无法停止。   
解决办法：hasNext("0"),遇到0返回true  
while( ! hasNext("0") )	{ ...  }  
+ next、nextLine方法返回值为String类型  
+ next方法遇到空格停止读取，返回值。
9. 类成员默认初始化  
+ 基本数据类型int、double、boolean等，默认初始化为0、0.0、false等值
+ 引用数据类型：如String、一般对象、泛型变量，默认初始化为null
+ 数组： (1)若你不初始化容量，默认初始化为null  
(2)初始化容量后，①对于int[]等基本类型数组，不赋值或部分赋值，剩余部分默认初始化0、false等值    
②对于String[]一般数组对象、泛型数组等，不赋值或部分赋值，剩余部分默认初始化为null
10. int和char  
int c = 3 + 'a';  // c = 100  
char c = 3 + 'a';  // c = 'd'

