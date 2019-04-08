## 1.封装（数据隐藏） 
   1. 关键在于不能让类中的方法直接访问其他类的实例域。对象数据（即类的实例域）仅能被（所属类的所有）对象方法访问与获取。  
    ```
        public String getName()  //域访问器
        {
	        return name;
        }
    ```
   2. 基于类的访问权限：一个方法可以访问调用它的对象的私有数据域，**还可以访问自身类的所有对象的私有域。  

    ```
        class Employee
        {
	        ...
	        public boolean equals(Employee other)
	        {
		        return name.equals(other.name);
	        }
        }
    ```  
    测试： 
        `if(Tom.equals(boss)) ...`    
    总结: 
       * 某一类的方法可以直接访问本身类的任何一个对象的私有域。  
       * 某一类的方法可以间接访问（传入对象参数）其他类中定义为public属性的数据和方法。        
   3. 静态域：用static修饰，该类的所有对象共享这一数据域。  
   4. 静态方法：用static修饰  
        * 属于类方法，该类的所有对象共有。
        * 只能访问自身类的静态域和静态方法。  
        * 可以传入其他类的对象，调用这一对象的非静态方法。  

        1. 问题：什么时候使用静态方法？  
            * 不需要访问对象实例域（状态）  
            * 只需要访问类的静态域。  
        2. 常见用途：个别类使用静态工厂方法来构造对象，生成不同风格的格式化对象。  
        `NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();`
        `NumberFormat percentFormatter = NumberFormat.getPercentInstance();`  
   5. this作用：
        1. 表示隐式参数	向类中定义的方法传递的参数，代表本类的对象。   
        ```  
            class Employee
            {
	            private String name;
	            private double salary;
                
	            public Employee(String name,double salary)
	            {
		            this.name = name;
		            this.salary = salary;
	            }
            }
        ```
        2. 在构造器中调用同一类的另一个构造器。
        ```
            public Employee(double s)
            {
	            this("Employee#"+nextId,s);
	            nextId++;
            }
        ```

## 2.继承
   1. super作用：
       * 调用超类的方法  super.方法  
	   * 调用超类的构造器  super（参数1,2，...）
   2. 多态:    向上转型  
        * 三个必要条件：继承、方法重写、向上转型1  
            * 继承:在多态中必须存在有继承关系的超类和子类。  
            * 重写:子类对超类中某些方法重新定义。  
            * 向上转型：父类引用指向子类对象  
        * 总结:  
            * 指向子类的父类引用由于向上转型，它只能访问父类中拥有的方法和属性，不能访问子类 中扩展的方法和重载了父类的方法
            * 对于子类重写父类的方法，当调用这些方法时，调用子类中重写的方法。
            ```
                public class Test3 {
                    public static void main(String[] args) {
                        Wine a = new Beer();
                        a.f1();
                    }
                }
                class Wine {
                    public void f1()
                    {
                        System.out.println("这是第一瓶酒");
                        f2();
                    }           
                    public void f2()
                    {
                        System.out.println("这是第二瓶酒");
                    }

                }
                class Beer extends Wine{
                    public void f1(String str)
                    {
                        System.out.println("这是第一瓶啤酒");
                        f2();
                    }
                    public void f2()
                    {
                        System.out.println("这是第二瓶啤酒");
                    }
                }

                运行结果:
                    这是第一瓶酒
                    这是第二瓶啤酒
            ```
        * 接口引用指向实现类的对象（和父类引用指向子类对象类似），实现多态,例：    
        `List<?> list= new ArrayList<?>         //list只是ArrayList的接口不是它的父类 ，不是父类引用指向子类对象`  
        `Map<?,?> map = new  HashMap<?,?>`  
        注意：list只能使用ArrayList中已经实现了的List接口中的方法，ArrayList中那些自己的、没有在List接口定义的方法是不可以被访问到的 ,如list.add()其实是List接口的方法,但是调用ArrayList的方法如 clone()方法是调用不到的   
    3. 抽象类与接口：  
        **抽象类**：抽象类是用来捕捉子类的通用特性的，用来创建继承层级中子类的模板。    
        * 不能被实例化
        * 继承抽象类的子类，如果不是抽象类，需要实现父类中所有的抽象方法（这样才能被实例化）；如果子类不实现父类中的所有抽象方法，那么子类也是抽象类，就需要子类的子类实现剩余的抽象方法，这样才能构造对象，才有意义。  
        ```
            //自己编造的例子，有点繁杂冗余，试了试抽象类
            public class Test3{
                public static void main(String[] args)
                {
                    Vehicle v = new Car("宝马730系",new Engine(8),130);
                    System.out.println(v.getName()+" V"+v.getCylinder()+"式发动机");
                    v.alarming();
                }
            }
            abstract class Vehicle
            {
                private String name;
                private Engine engine;

                public Vehicle(String name,Engine engine) {
                this.name = name;
                this.engine = engine;
                }
                public abstract void alarming();
                public String getName() {return name;}
                public int getCylinder()
                {
                    return engine.getCylinderNumber();
                }
            }
            class Engine
            {
                private int cylinderNumber;

                public Engine(int cylinderNumber)
                {
                    this.cylinderNumber = cylinderNumber;
                }
                public int getCylinderNumber()  {return cylinderNumber;}
            }
            class Car extends Vehicle
            {
                private double speed;
                public Car(String name,Engine engine,double aSpeed)
                {
                    super(name,engine);
                    speed = aSpeed;
                }
                public void alarming(){
                    if(speed >= 120.0)
                    System.out.println("超速，发出警报！");
                }
            }
        ```  
            输出结果：  
        ```
            宝马730系 V8式发动机
            超速，发出警报！
        ```  
        **接口**：  
           * 实现接口是必须实现接口中的所有方法，但抽象类不需要实现所有的接口方法。  
        ```
            abstract class X implements Y{          //X类必须声明为abstract，因为没有完整实现接口Y
                //implements part of method of Y
            }
            class XX extends X{
                //implements the remaining method in Y
            }
        ```  
           * 接口中所有方法自动定义为public,接口引用指向实现类的对象实现多态(具体见上面多态第三条)。
           * 重写接口:（在旧接口中添加新方法）通过新接口继承旧接口的方式,为旧接口增加功能，那些依赖原接口的程序无需修改。  
        ```
            public interface DoIt{
                fun1();
                fun2();
            }
            public interface DoItPlus extends DoIt{
                fun3();
            }
        ```
    继承的几点注意：
    1. 方法调用：  例：调用x.f(args):  x为类C的一个对象  
            过程：第一步：编译器查看对象的声明类型（属于哪个类）和方法名。编译器一一列举所有C类中名为f的方法和其超类中属性为public且名为f的方法（超类的私有方法不可访问）。  
            第二步：编译器根据参数类型匹配方法（重载解析）  
            第三步：当调用private方法、static方法、final方法或者构造器时，编译器调用哪个方法依赖于显示参数的类型，这种调用方式为静态绑定。调用的方法依赖于隐式参数的实际类型，在运行时实现动态绑定。  
            第四步：动态绑定过程：虚拟机预先建立每个类的方法表  
		        例调用e.f()  e声明为A类  
		        （1）提取A类和A类所有子类的方法表  
		        （2）搜索定义f方法签名的类，然后调用。   
    2. 子类覆盖超类中的方法时，如果超类方法时public，子类方法也要声明为public，其返回类型可以是原返回类型的子类型 
    ```
        public Employee getBuddy()	{...}
        public Manager getBuddy()	{...}
    ```
    3.  ......
