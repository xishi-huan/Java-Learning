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
推荐>https://blog.csdn.net/qq_33417486/article/details/82787598    
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
11. String与int、double类型转换：  
+ Integer.parseInt(str);  
+ Double.parseDouble(str);
12. finally块：  
一般情况下无论try代码块是否抛出异常，finally块都会执行。
13. Mysql锁：
+ 共享锁（读锁）：允许事务读一行数据
+ 互斥锁（写锁）：允许事务删除或更新一行数据  
14. synchronized：  
+ 在并发编程中存在线程安全问题，主要原因有：1.存在共享数据 2.多线程共同操作共享数据。
+ 关键字synchronized可以保证在同一时刻，只有一个线程可以执行某个方法或某个代码块，同时synchronized可以保证一个线程的变化可见（可见性），即可以代替volatile。原理：synchronized可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时它还可以保证共享变量的内存可见性
15. finalize：  
finalize方法是Object提供的的实例方法，使用规则如下：
+ 当对象不再被任何对象引用时，GC会调用该对象的finalize()方法
+ finalize()是Object的方法，子类可以覆盖这个方法来做一些系统资源的释放或者数据的清理
+ 可以在finalize()让这个对象再次被引用，避免被GC回收；但是最常用的目的还是做cleanup
+ Java不保证这个finalize()一定被执行；但是保证调用finalize的线程没有持有任何user-visible同步锁。
+ 在finalize里面抛出的异常会被忽略，同时方法终止。
+ 当finalize被调用之后，JVM会再一次检测这个对象是否能被存活的线程访问得到，如果不是，则清除该对象。也就是finalize只能被调用一次；也就是说，覆盖了finalize方法的对象需要经过两个GC周期才能被清除
16. TCP握手的意义（目的）：
三次握手的目的是同步连接双方的序列号和确认号并交换 TCP 窗口大小信息。
17. volatile的特性：  
+ 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。（实现可见性）
+ 禁止进行指令重排序。（实现有序性）
+ volatile 只能保证对单次读/写的原子性。i++ 这种操作不能保证原子性。
18. 类加载过程：JVM把class文件加载到内存，并对数据进行校验、准备、解析、初始化，最终形成JVM可以直接使用的Java类型的过程。  
程序初始化顺序：  
+ 父类静态变量、父类静态块（按代码先后顺序）
+ 子类静态变量、子类静态块（顺序执行）
+ 父类变量、父类块（顺序执行）
+ 父类构造函数
+ 子类变量、子类块（顺序执行）
+ 子类构造函数
+ 另：类的静态方法、方法在调用时才会执行
```
public class TestDemo {

    public static void main(String[] args) {
        new Son().staF();
    }
}

class Father{
    private static int i = initI();                  //1


    static{
        System.out.println("父类静态块");            //2
    }
    public static void staF(){
        System.out.println("父类静态方法");
    }

    public Father(){
        System.out.println("父类构造函数");              //7
    }
    {
        System.out.println("父类块");                  //5
    }
    private int j = initJ();                            //6
    public void f(){
        System.out.println("父类方法");
    }



    private static int initI(){
        System.out.println("父类静态变量");
        return 3;
    }
    private int initJ(){
        System.out.println("父类变量");
        return 4;
    }
}


class Son extends Father{
    private static int m = initM();                 //3
    private int n = initN();                        //8

    static{
        System.out.println("子类静态块");            //4
    }
    public static void staF(){
        System.out.println("子类静态方法");
    }


    {
        System.out.println("子类块");                  //9
    }
    public void f(){
        System.out.println("子类方法");
    }
    public Son(){
        System.out.println("子类构造函数");              //10
    }


    private static int initM(){
        System.out.println("子类静态变量");
        return 3;
    }
    private int initN(){
        System.out.println("子类变量");
        return 4;
    }
}
```