## 异常分类
+ Exception包括RuntimeException和IOException，程序错误导致的异常属于RuntimeException，I/O错误这类问题导致的异常属于IOException
+ 派生于Error类或RuntimeException类的异常称为非受查异常，所有其他异常称为受查异常。
- 派生于RuntimeException的异常包括：  
    错误的类型转换、数组访问越界、访问null指针
- 派生于IOException的异常有：  
    试图在文件尾部后面读取数据、试图打开一个不存在的文件  
## 异常的几点说明
* 异常是就方法而言的
* 异常可由JVM虚拟机自动产生，也可手动抛出throw new Exception()。  
* 手动抛出的异常大多数情况下都是自己创建的异常类示例。
* 捕获处理异常，要用try、catch语句块。
## 异常处理
### 五个关键字：try,catch,throw,throws,finally
* 方法一:捕获异常
  
    - try代码块进行异常检测，若有异常发生，从try代码块抛出（throw）该异常，并被catch语句捕获。注意:此时程序执行转移到catch语句，若try代码块没有抛出异常，则不执行任何catch语句，然后继续执行catch后面的语句。
   ```
   例：
    public class Test4{
        public static void main(String[] args)
        {
            int num[] = new int[4];
            try{
                System.out.println("Before exception is generated.");
                num[10] = 7;                                            //抛出异常
                System.out.println("this won't be displayed");        //不执行
            }
            catch(ArrayIndexOutOfBoundsException exc){                //捕获数组越界错误
                System.out.println("Index out of bounds!");
            }
            System.out.println("After catch statement.");
        }
    }
    输出结果：
    Before exception is generated.
    Index out of bounds!
    After catch statement.
    ```  
    - try、catch的几点用法：
      
        * 使用多个catch语句，捕获多种异常（try、catch语句块嵌套在循环中）
        * 捕获子类异常：
        ```
            try{
                ....
            }
            catch(ArrayIndexOutOfBoundsException exc){
                ...
            }
            catch(Throwable exc){
                ...
            }
        ```  
        本例中除了ArrayIndexOutOfBoundsException异常外，所有的异常都由catch（Throwable）捕获。  
        * 手动抛出异常
        ```
            try{
                ...
            throw new ArithmeticException();
            }catch(ArithmeticException exc){
                ...
            }
        ```  
        * 重新抛出异常
          
* 方法二：声明异常
  
    + 一个方法产生自己不做处理的异常，将异常传递给方法调用者，让它去处理（处理方式try、catch），必须用throws语句声明该异常，这样层层递交，最后连main方法都throws Exception，那就交由虚拟机处理。
    + 一个方法必须声明所有可能抛出的受查异常（自己不做处理），非受查异常要么不可控制（Error），要么就应该避免（RuntimeException）,例：  
    `public FileInputStream(String name) throws FileNotFoundException`  
## 常见异常类型
  
* 除以0，会产生ArithmeticException异常
* 数组访问越界，产生ArrayIndexOutOfBoundsException异常
* 要读取的的文件可能不存在，产生FileNotFoundException异常
* 在输入过程中，遇到一个未预期的EOF信号，异常结束，为EOFException异常


    
