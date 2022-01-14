## Day 02 复习内容

### 一、JavaSE  部分

1. "=="和equals方法区别

   ==是操作符，equals是方法。显然对于基本数据类型，只能是用==，因为不涉及方法。用于比较两个变量分别对应的内存中所存储的**值**是否相等。对于引用数据类型，如果一个变量指向的数据是对象，那么此时涉及两块内存。对象本身占用一块内存（堆空间中），变量也占用 一块内存（栈空间中）。

   ```Java
   Object obj = new Object();
   ```

   obj 所对应内存中 存的是 对象占用的那块内存的首地址。因此如果是要比较两个变量是否指向了同一个对象，也就是看地址是不是相同，那需要用==操作符；如果是想要比较两个独立对象的内容是否相同，那需要用equals. 

   ```java
   String a = new String("foo");
   Stirng b = new String("foo");
   ```

   ```Java
   a == b; //false
   a.equals(b);//true
   ```

   字符串的比较一般都是用equals方法。

   如果一个类没有重写equals方法，那么它将继承Object类的equals方法

   ```java
   boolean equals(Object o){
       return this == o;
   }
   ```

   也就是说默认的equals方法就是使用==操作符，也就是在比较两个变量指向的对象是否为同一个对象，此时使用equals和==是一样的结果。比较的是两个独立对象的话，总是返回false.

   

   感觉还是比较乱…重新总结下

   - 关于 ==

     基本类型中用于比较值是否相等；在一般的引用类型中，比如User对象，比较地址是否相等

   - 关于equals

     equals方法在Object中，如上所示，源码中用的也是 ==,也就是说equals默认比较的还是地址。所以作用一致。

     ```Java
     public class StringDemo{
         public static void main(String args[]){
             int a = 1;
             int b = 1;
             System.out.println(a == b);//基本，true
             User user1 = new User();
             User user2 = new User();
             System.out.println(user1 == user2 );//普通对象， flase
             System.out.println(user1.equals(user2));//普通对象 继承Object false
         }
     }
     ```

     那么看起来equals和==无区别啊，问题在于一些重写了equals方法的类。比如最常用的String类。

   - 较为特殊的String类

     Object对象里面的==和equals没有啥区别，但是String 在 Object的基础上重写了equals方法，于是功能被改变了。可以在String源码看到重写的方法:

     ```Java
     public boolean equals(Object anObject){
         if(this == anObject){
             return true;
         }
         if(anObject instanceof String){
             String anotherString = (String)anObject;
             int n = value.length;
             if(n==anotherString.value.length){
                 char v1[] = value;
                 char v2[] = anotherString.value;
                 int i = 0;
                 while(n-- != 0){
                     if(v1[i] != v2[i]){
                         return false;
                     i++ ;
                     }
                     return true;
                 }
             }
         }
         return false;
     }
     ```

     也就是说String中的equals方法其实比较的是字符串的**内容**是否一样。如果是String、Date等重写过equals的类，使用的时候就和Object不一样。

   - 测试String

     ```Java
     public class StringDemo{
         public static void main(String args[]){
             String str1 = "Hello";
             String str2 = new String("Hello");
             String str3 = str2;//这里是引用传递
             System.out.println(str1 == str2);//false
             System.out.println(str1 == str3);//false
             System.out.println(str2 == str3);//true
             System.out.println(str1.equals(str2));//true
             System.out.println(str1.equals(str3));//true
             System.out.println(str2.equals(str3));//true
         }
     }
     ```

     结果应该从内存角度去分析：

     ![image-20220112135503606](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220112135508011-650446721.png)

   - 总结

     - 基础类型

       使用 == 比较值是否相等

     - 引用类型

       - 重写了equals方法，比如String

         * 使用==比较的是String的引用是否指向同一内存，即比较地址值
         * 使用equals比较的是String的引用的对象内容是否相同，即比较存储的数值

       - 没有重写equals方法，比如自定义类

         此时是继承自Object类，内部实现默认了 == 和equals方法一样，都是比较引用是否指向同一内存，即地址值

2. 静态变量和实例变量的区别

   - 首先是语法上的区别

     静态变量要加static关键字，而实例对象不用加

   - 其实是在程序运行时的区别

     静态对象和实例对象都属于**成员变量**，静态对象又叫做**类变量**, 是属于类的，是独立于方法之外的变量。而实例对象同样也是独立于方法之外的，但是是属于某个对象的属性。

     对于静态变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以通过**类名**被使用了；对于实例变量，必须创建了该类的实例对象，实例变量才会被分配空间，才能使用这个实例变量。也就说和对象关联在一起，在创建的对象上进行方法调用，每个对象的实例变量彼此之间是**独立的**.

     ```java
     public class StaticTest{
         private static int staticInt = 2; //静态变量、类变量
         private int random = 2;//实例变量
         
         public StaticTest(){
             staticInt++;
             random++;
             System.out.println("staticInt = "+ staticInt+ "random = " + random);
         }
         
         public static void main(String[] args){
             StaticTest test = new StaticTest();//对象1
             StaticTest test2 = new StaticTest();//对象2
         }
     }
     ```

     ```java
     staticInt = 3 random = 3
     staticInt = 4 random = 3
     ```

     每创建一个实例对象，就会分配一个random,每个random各自的值只加一次；但无论创建了多少个实例对象，永远都只分配一个staticInt变量，并且每创建一个实例对象，它的值就+1.

3. 能否从一个static方法内部发出对非static方法的调用

   不可以。

   因为非static方法是要和对象关联的。必须创建 一个对象之后，才能在该对象上进行方法 的调用。而static方法调用时不用创建对象，可以直接调用。那么，当一个static方法被调用时，可能还**没有创建任何实例对象**，如果从一个static方法内部调用非static方法，那么这个非static方法到底是关联到哪个对象呢？逻辑无法成立所以不可以。

4. Integer和int的区别

   int 是Java八大数据类型之一。而Java为每个原始类型都提供了**封装类**，Integer就是对应int的封装类。int默认值为0，而Integer的默认值为null(因为类是属于引用类型的，引用类型只能放地址或者null). 这样的话，Integer其实是可以区分出**未赋值**和**赋值0**的区别的。

   总结：一共有5点不同

   - 数据类型不同 

     基本数据类型 VS　包装类数据类型

   - 默认值不同

     0 VS null

   - 内存存储方式不同

     数据值 VS 对象引用 （new一个Integer时，实际上是生成了一个指针指向此对象）

   - 实例化方式不同

     不需要实例化 VS 必须实例化才能使用

   - 比较方式不同（对于值的比较）

     可以使用==比较 VS 必须使用equals比较

![image-20220112145826078](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220112145829691-1740861787.png)

补充： 为什么要有包装类型

Java中，new一个对象存储在堆里，通过栈里的引用来使用这些对象。对于常用的类型，比如int,通过new存在堆里，用指针指过去不够高效。所以我们有了基本类型，直接将变量值存在栈里。

但是基本类型并不具备对象的性质。Java面向对象，为了让基本类型也有对象的特征，就出现了包装类。相当于把基本类型包装起来，使他具备对象的性质，添加属性和方法，达到丰富基本类型操作的目的。在集合类型Collection中，就必须使用包装类型而不是基本类型。

二者区别主要在于 声明方式不同、存储位置和方式不同、初始值不同、使用方式不同。可以相互**转化**

- int --- > Integer

  ```Java
  int i = 0;
  Integer i1 = new Integer(i);
  ```

- Integer --- > int

  ```Java
  Integer i1 = new Integer(0);
  int i = i1.intValue();
  ```

5. Math.round(11.5)？ Math.round(-11.5)?

   round是个取整方法。Math类提供三个和取整有关的方法：ceil ,floor, round. ceil(天花板)所以是**向上取整**，Math.ceil(11.3) 应该是12，Math.ceil(-11.3)应该是-11；floor(地板)所以是向下取整，Math.floor(11.6)应该是11，Math.floor(-11.6)应该是-12；而round是表示**四舍五入**, 算法其实是 Math.floor(x + 0.5), 也就说说原数据加上0.5再向下取。Math.round(11.5) 应该是12，Math.round(-11.5)应该是 -11.

### 二、计网部分

1. 传输层作用

   传输层主要是解决**进程之间基于网络通信的问题**。比如如何区分是哪个进程？传输出现错误时如何处理？这里的通信指的是逻辑通信。传输层向高层用户屏蔽了下面网络层的核心细节，使应用程序看起来像是在两个传输层实体之间有一条端对端的逻辑通信信道

2. 会话层作用

   建立会话：比如身份认证，权限鉴定等

   保持会话：对该会话进行维护，在会话期间收发双方可以随时使用这条会话传输数据

   断开会话：当应用程序或者应用层规定的超时时间到了以后，OSI会话层就会释放这条会话

3. 表示层作用

   表示层对数据格式进行编译，对收发的数据根据应用层的特征进行处理。比如处理为文字、图片、音频、视频、文档等，还可以对压缩文件进行解压缩，对加密文件解密等

4. 应用层作用

   应用层主要解决**通过应用进程的交互来实现特定网络应用的问题**，提供应用层协议。比如HTTP、FTP协议，方便应用程序之间通信。

5. TCP和UDP区别

   - TCP是传输控制协议，UDP是用户数据包协议

   - TCP可以看作是打电话，双方必须建立一条不间断的通路。数据不到达对方，对方就一直在等待。除非是对方挂电话了。而且先说话的先到，后说后到，遵守顺序。

     UDP可以看作写信。发信者只管发，不管收，但是必须写明地址。双方没有通路，靠邮电局联系。收到时可能过了很久，可能根本没收到。先发的未必先到。

   - 抽象出来就是，TCP提供**面向连接**、**可靠**的**字节流**服务。客户和服务器彼此交换数据之前，必须建立起一个TCP连接，之后才能开始传输数据。TCP提供**超时重发**，丢弃重复数据，检验数据，流量控制等功能，会保证数据能从一端到另一端。而是一定是端到端的服务。

     UDP是一个面向数据报的传输层协议，**不可靠**，只是负责把应用程序传给IP层的数据报发出去，不保证能到达。在传输数据报之前**不会在客户和服务器之间建立一个连接，而且没有超时重发机制**，所以**速度是很快的**。可以是一对一、一对多、多对多等。

     关于二者选择，要看重视的是可靠还是迅速。

   - 总结

     - 有连接 VS 无连接
     - 流模式 VS 数据报模式
     - 可靠性 （到达且准确 无差错 不丢失 不重复）VS 不可靠性（可能丢包 尽最大努力交付）
     - 数据顺序 VS 乱序
     - 端对端  VS　一对多一对一多对一多对多交互通信
     - 系统资源
       - TCP首部开销20字节
       - UDP首部开销小，只有8字节

