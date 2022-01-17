## Day 04 复习

### 一、JavaSE 部分

1. 面向对象的特征

   **封装、继承、多态（还可以加上抽象）**

   - 封装

     封装可以**隐藏实现细节，只提供对外接口**，提高安全性。同时可以实现高内聚、低耦合，防止程序相互依赖性带来的变动影响。最常见的是实现属性的封装。把一个对象的属性私有化，同时提供一些可以让外部访问属性的方法。通过调用对象的get、set方法等来实现对于属性的操作。

   - 继承

     在定义和实现一个类时，可以在一个已经存在的类的基础上进行。在这个定义的基础上加入若干新内容或者修改原来的方法满足需求，就是继承。继承是**子类共享父类数据和方法的机制**，是类之间的关系，目的是提高代码的可复用性。

   - 多态

     引用变量 所指向的 具体类型 和通过该引用变量发出的方法调用在编程时不确实，在程序运行时才确定。也就是说，运行时才知道一个引用变量到底会指向哪个类的实例对象，该引用变量调用的方法到底是哪个类中实现的方法，这样可以让引用变量绑定到各种不同的类实现上，从而导致该引用调用的方法也随之改变，让程序可以选择多个运行状态。
     
     多态一般都要和继承结合起来说，本质上是**子类通过覆盖或者重载父类的方法，对同一类对象同一方法 的调用产生不同的结果**
     
     ![img](https://pic3.zhimg.com/80/v2-b00c31051b1fe1215762b3c066255c1e_720w.jpg)

2. Java中实现多态的机制

   依靠的是**父类或者接口的引用指向子类或者实现类**，父类的引用是在运行时动态指向具体的事例，调用该引用的方法时，不是根据引用变量类型中定义的方法而是具体的实例的方法。

3. abstract class 和 interface有何区别
   
   用abstract修饰的类是抽象类。用abstract修饰的方法是抽象方法。抽象方法只有方法声明，没有方法体。
   
   >关于为何要有抽象类的一些描述
   >
   >面向对象中，所有的对象都是通过类来描绘，但不是所有的类都用来描绘对象。当一个类中没有足够的信息来描绘一个具体的对象时，这就是个抽象类。
   >
   >比如:图形编辑软件的开发，存在着圆、三角形、正方形这样一些具体的概念。他们是不同的，但本质上有一些共同点。都属于形状这个概念。而形状这个概念在问题领域上是不存的，形状就是圆、三角形等的抽象类。
   >
   >也就说抽象类是对一系列看上去不同，但本质上又相同的具体的概念的抽象。
   >
   >正是因为抽象的概念在问题领域没有对应着的具体概念，所以是不能实例化的。
   >
   >那为何要有抽象类呢？
   >
   >抽象类是为了继承而存在的，对于一个父类如果他的某个方法在父类中没有具体的体现，必须要根据子类的实际需求来进行不同的实现，那么可以把这个方法声明为abstract方法，这个类就成为了abstract类。这样就构造出一个固定的一组行为的抽象描述，但是这一组行为能有任意个可能的具体实现方式，算是实现了多态。同时这样父类只是知道子类应该包含什么方法，但是不知道具体实现，隐藏了一些细节。还有就是从很多个具有相同特征的类中抽象出一个抽象类，让这个抽象类作为子类的模板，避免子类设计的随意性。
   
   抽象类的特点有：
   
   - 抽象类不能被实例化 不能用new创建对象 只能被继承
   - 包含抽象方法的一定是抽象类，但是抽象类不一定有抽象方法（可以有普通方法、变量）
   - 抽象类中的抽象方法的修饰符只能用public或者protected, 默认是public
   - 一个子类继承一个抽象类，那么子类必须实现所有父类的抽象方法。如果没有实现所有的抽象方法，仍然存在着抽象方法，那么子类也必须定义为抽象类
   - 抽象类可以包含属性、方法、构造器。但构造器不能直接用于实例化，而是用来被子类调用来完成抽象类的初始化。（抽象类要通过子类进行实例化）  
   
   一个栗子：
   
   有图形类Shape, 它有两个抽象方法area() 和 perimeter() 获取图形面积和周长。另有矩形类Rectangle和圆类Cicle,继承抽象类Shape,各自实现area()方法和perimeter()方法，因为他们的计算方式是不一样的。
   
   ```Java
   public abstract class Shape{
       public abstract double area();//抽象方法1
       public abstract double perimeter();//抽象方法2 无方法体 无大括号 以分号结束 
   }
   public class Rectangle extends Shape{
       private double length;//属性的封装
       private double width;
       
       public double getLength(){
           return this.length; //get方法
       }
       
       public void setLength(double length){//set方法
           this.length = lenght;
       }
       
       public double getWidth(){
           return this.width;
       }
       public void setWidth(double width){
           this.width = width;
       }
       
       @Override
       public double area(){//实现父类的抽象方法
           return getLength()*getWidth();
       }
       @Override
       public double perimeter(){
           return (getLength() + getWidth())*2;
       }
   }
   
   
   public class Circle extends Shape{
       private double diameter;
       
       public double getDiameter(){
           return diameter;
       }
       
       public void setDiameter(double diameter){
           this.diameter = diameter;
       }
       @Override
       public double area(){
           return Math.PI * Math.pow(getDiameter/2, 2);
       }
       @Override
       public double perimeter(){
           return Math.PI*getDiameter();
       }
   }
   
   public static void main(String[] args){
       //造对象 调方法给属性赋值
       Rectangle rectangle = new Rectangle();
       rectangle.setLength(10);
       rectangle.setWidth(5);
       
       double rectangleArea = rectangle.area(); //调矩形对象的area()方法并赋值
       double rectanglePerimeter = rectangle.perimeter();
       System.out.println("矩形面积是" + rectangleArea + ",周长是" + rectanglePerimeter);
       
       Circle circle = new Circle();
       circle.setDiameter(10);
       double circleArea = circle.perimeter();
       double circlePerimeter = circle.perimeter();
       System.out.pritln("圆形面积是" + circleArea + ",周长是" + circlePerimeter);
   }
   ```
   
   ![image-20220117204847745](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220117204850340-1363844306.png)
   
   
   
   接口可以看成一种特殊的类，只能用interface修饰。
   
   >如果把抽象类看作是对事物本质的抽象，那么接口可以看作是对行为动作的抽象。抽象类表示的是这个对象是什么。比如男人，女人这两个类他们的抽象类是人。有种潜在的is a 的关系。接口则是表示这个对象能做什么。比如人可以吃东西，狗也可以吃东西，那么就可以把吃东西定义成一个接口，然后让人类和狗类分别去实现它的细节。一个类只能继承一个抽象类（不能既是…又是）但是可以实现很多接口（吃饭接口 走路接口等）
   >
   >
   
   接口的特点有：
   
   - 接口中可以包含变量和方法，变量被隐式指定为 public static final 方法被隐式指定为public abstract
   - 接口支持多继承，一个接口可以继承多个接口
   - 一个类可以实现多个接口，一个类实现某个接口就必须实现该接口中所有的抽象方法，否则该类就必须被定义为抽象类（抽象类的话他的子类必须实现）
   
   ```Java
   public interface Alram{
       void alarm();//接口中方法没有具体的实现，会被隐式指定为 public abstract方法
       //既然是抽象方法，具体实现就要由 实现接口的 类 来实现
       //一个非抽象类实现了某个接口，就必须实现该接口中所有的抽象方法。
       //一个抽象类实现了某个接口，自身可以不实现该接口中的方法，但是他的子类必须实现。
   }
   ```
   
   **抽象类 和 接口的比较**
   
   -  抽象类和接口都不能实例化，如果要实例化，抽象类必须是通过继承的方式让抽象类的对象指向实现了所有抽象方法的子类对象。而接口则是通过类实现接口的方式，让接口变量指向实现了所有接口方法的类对象。
   - 抽象类要被子类继承，接口要被类实现
   - 抽象类的抽象方法必须全部被子类实现，否则子类也只能是抽象类。同样实现接口的时候，也必须实现所有的接口方法，否则实现的这个类也只能是抽象类
   - 接口只能做方法声明，不能具体实现。但是抽象类可以做方法声明，也可以实现
   - 接口里定义的变量只能是公共静态的public static 抽象类的变量是普通变量
   - 接口可以继承接口并且支持**多继承**，但是类只能单继承（一个类只能继承一个抽象类但可以实现多个接口）
   - 抽象类里面还可以没有抽象方法
   - 抽象类是对整个类的整体进行抽象，包括属性和行为，但是接口只是对类局部（行为）抽象。继承是 是不是 的关系， 而接口是 有没有 的关系。
   - 抽象类作为很多子类的父类，是一种模板设计，而接口更像是行为规范。对于抽象类，要添加新方法，可以直接在抽象类中加入具体的方法实现，子类不动。但是接口不行，接口变动了，则所有实现这个接口的类都要改动。
   
   又是一个栗子：门和报警
   
   所有的门都有open()和close()动作，这既可以抽象出 抽象类门，又可以抽象出具有开关行为的接口。
   
   ```Java
   public abstract class Door{
       public abstract void open(); //抽象方法 具体怎么开不实现
       public abstract void close();
   }
   ```
   
   ```Java
   public interfact Door{
       void open();
       void close();//隐式的 public abstract方法
   }
   ```
   
   现在需要门有警报alarm功能。如何设计？
   
   - 在抽象类中加一个alarm()方法
   
     这么做会让所有继承这个抽象类的子类都有报警功能。但现实是不是所有的门都能报警
   
   - 在接口中加一个alarm()方法
   
     这么做会导致所有需要用到报警功能的类都要实现接口中的所有抽象方法，包括open()close(),但是开闭是门的固定行为。有的类，比如火灾报警器就没有这个行为。
   
   - 所以最好是单独把报警设计成一个接口Alarm,包含alarm()行为。Door设计成抽象类，包含open()close()行为，再设计一个报警门作为子类继承Door类并且实现Alarm接口。
   
   ```Java
   public abstract class Door{
       public abstract void open();
       public abstract void close();
   }
   public interface Alarm{
       void alarm();
   }
   public class AlarmDoor extends Door implements Alarm{
       @Override
       public void alarm(){
           //这里是实现Alarm接口抽象方法的具体代码
       }
       @Override 
       public void open(){
           //这里是继承Door类实现open()抽象方法的具体代码
       }
       @Override
       public void close(){
           //这里是继承Door类实现close()抽象方法的具体代码
       }
   }
   ```
   
   4. abstract 的method是否可同时是static , 是否可以同时是native, 是否可以同时是synchronized
   
      - abstract method是抽象方法只有声明没有实现，实现放到继承的子类或者实现接口中
   
      - static是静态的，是一种属于类而不属于对象的属性或者方法
   
      - <u>synchronized是同步，是一种相对线程的锁（这里还不知道）</u>
   
      - native是本地方法，和抽象方法比较像，也是只有方法声明没有方法实现，但是和抽象方法不同的是它会把具体的实现交给 本地系统的函数库 而不通过虚拟机，是Java和其他语言沟通的一种机制
   
      - 以上关键字都是不能和abstract同时使用的
   
        - abstract方法所在的类是抽象类，抽象类不能实例化，需要子类来重写该抽象方法。同时用abstract和static, 说明是属于类而不是对象的方法，用类名调一个抽象方法必然不行
        - synchronized是同步，同步需要有具体的操作。abstract只有方法声明，那同步什么东西成了问题。当然在抽象方法被子类继承之后，可以添加同步
        - native,本身就和abstract冲突。他们都是方法声明，只是一个把方法移交给子类，一个移交给本地的操作系统。同时出现，就根本不知道交给谁来具体实现了。
   
      - 补充：不能放一起的修饰符
   
        final+abstract     private+abstract	static+abstract
   
        因为abstract必须在子类实现，才能以多态方式调用。final不可以覆盖子类只有调用的权利没有修改的权利，private不能继承，更别说覆盖。static可以覆盖（静态方法可以被类以及对象调用，这是整个类的公共方法），但是调用时会调用编译时类型的方法，调的是父类的方法，而父类方法又是抽象的方法，不能调用。所以不能放到一起。
   
   5. String是基本数据类型吗
   
      基本数据只有八大数据类型： byte short int long char float double boolean
   
      String属于引用数据类型，是final修饰的Java类。引用数据类型还包括了 类 类型、接口 类型、数组类型、枚举类型、注解类型等 只要是引用数据类型变量，具体内容都是存放在堆里的，栈中放的只是存具体内容所在地的内存地址
   
      （关于String类所在的包java.lang.String  该类是final类型的，因此不可以继承这个类、不可以修改这个类。所以如果要修改的话，我们应该使用StringBuffer类）
   
   
   
   ### 二、计网部分
   
   1. TCP三次握手过程
   
      - 第一次握手
   
        客户端给服务器将报文段的标志位SYN置1，产生一个随机的序列号seq = x(sequence number),把该TCP数据包(包括seq）发给服务器，请求建立联接。然后客户端就进入了syn_sent状态，等待服务器确认。
   
      - 第二次握手
   
        服务器收到数据包之后由标志位SYN=1知道客户端请求建立连接，服务器将标志位SYN和ACK都置1，ack = x + 1, 随机产生一个值seq = y. 然后将该数据包发给客户端确认连接请求。此时服务器进入syn_rcvd状态。
   
      - 第三次握手
   
        客户端收到确认后进行检查。如果正确那么将标志位ACK置1， ack = y + 1,并把数据包发给服务器。服务器进行检查如果正确就说明连接成功建立，客户端和服务器都进入了established状态，完成三次握手之后，双方可以传输数据了。
   
      ```
      第一次握手：client SYN=1, Sequence number=2322326583 —> server
      第二次握手：server SYN=1,Sequence number=3573692787; ACK=1, Acknowledgment number=2322326583 + 1 —> client
      第三次握手：client ACK=1, Acknowledgment number=3573692787 + 1 -->server
      ```
   
      ![image-20220117222348291](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220117222351144-862977953.png)
   
   

2. TCP为什么需要三次握手，两次行不行

   因为TCP连接是全双工的，保证双向连接，三次是最少的。TCP进行可靠传输的关键就在于维护一个序列号，三次握手的过程就是双方相互告知序列号起始值，并且确认对方已经收到了序列号起始值。如果只有两次握手，最多只有客户端的起始序列号能被确认，服务器端的序列号得不到确认。

3. 简述半连接队列

   TCP握手中，当服务器处在SYN_RCVD状态，服务器就会把此种状态下的请求连接都放到一个队列里。这个队列就叫做半连接队列。

   ![image-20220117223007774](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220117223010602-955728298.png)

4. SYN攻击

   SYN攻击就是利用TCP协议缺陷，通过**发送大量的半连接请求，占用半连接队列**，来耗费CPU和内存资源。

   优化方式：

   - 缩短SYN Timeout时间
   - 记录IP 如果连续受到某个IP的重复SYN报文，那么将来自这个IP地址的包都丢弃

5. TCP四次挥手过程

   四次挥手终止TCP连接

   - 第一次挥手

     客户端发送一个FIN，用来关闭客户端到服务器的数据传送，发完之后客户端就进入了fin_wait_1状态

   - 第二次挥手

     服务端收到FIN之后，发送一个ACK给客户端。确认序号是收到的序号+1，服务端就进入了Colse_wait状态。TCP连接处于**半关闭**状态，客户端已经没有要发送的数据了，但是如果服务器还要发送数据，客户端仍然要接收。此时客户端进入了fin_wait_2状态，在等待服务器的FIN报文。

   - 第三次挥手

     服务器确定数据只发送完成之后，发送一个FIN,用来关闭服务器到客户端的数据传输，发完之后服务器进入了Last_ack状态

   - 第四次挥手

     客户端收到FIN之后，接着发送一个ACK给服务端来确认是否关闭，客户端进入了Time_wait状态。确认收到后服务端就进入了Closed状态（如果没有收到ACK是可以重传的），客户端等待了2MSL之后没有收到回复说明服务器已经正常关闭，自己也会关闭连接。这样就完成了四次挥手。

     ![image-20220117224509931](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220117224512651-1591619803.png)

     

     

     

     ```
     补充：TCP的６个标志位
     
     SYN(synchronous) --- 建立连接
     
     ACK(acknowledgement) --- 确认
     
     PSH(push) --- 传输
     
     FIN(finish) --- 结束
     
     RST(reset) --- 重置
     
     URG(urgent) --- 紧急
     
     ```

     

