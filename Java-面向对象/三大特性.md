1. 封装（数据（类的实例域）隐藏）
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
    测试：`if(Tom.equals(boss)) ...`  
    总结:
        1. 某一类的方法可以直接访问该类的任何一个对象的私有域。  
        2. 某一类的方法可以间接访问（传入对象参数）其他类中定义为public属性的数据和方法。        
    3. 静态域：用static修饰，该类的所有对象共享这一数据域。  
    4. 静态方法：用static修饰  
        1. 属于类方法，该类的所有对象共有。
        2. 只能访问自身类的静态域和静态方法。  
        3. 可以传入其他类的对象，调用这一对象的非静态方法。  
        问题：什么时候使用静态方法？  
        1、不需要访问对象实例域（状态）  
        2、只需要访问类的静态域。  
        常见用途：个别类使用静态工厂方法来构造对象，生成不同风格的格式化对象。
        `NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();`
        `NumberFormat percentFormatter = NumberFormat.getPercentInstance();`  
    5. this作用：
        1、表示隐式参数	向类中定义的方法传递的参数，代表本类的对象。   
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
        2、在构造器中调用同一类的另一个构造器。
        ```
            public Employee(double s)
            {
	            this("Employee#"+nextId,s);
	            nextId++;
            }
        ```

2. 继承
    1. super作用：
        1. 调用超类的方法  super.方法  
	    2. 调用超类的构造器  super（参数1,2，...）
    2. 多态:    向上转型  
        三个必要条件：继承、方法重写、向上转型1  
        继承:在多态中必须存在有继承关系的超类和子类。  
        重写:子类对超类中某些方法重新定义。  
        向上转型：父类引用指向子类对象  
        总结:  
        1. 指向子类的父类引用由于向上转型，它只能访问父类中拥有的方法和属性，不能访问子类 中扩展的方法和重载了父类的方法
        2. 对于子类重写父类的方法，当调用这些方法时，调用子类中重写的方法。
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
        `public Employee getBuddy()	{...}`
        `public Manager getBuddy()	{...}`
    3.  ......