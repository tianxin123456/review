## Day 05复习

### 一、JavaSE 部分

1. 什么内部类？Static Nested Class 和 Inner Class的不同

   - 内部类就是一个在类的内部定义的类
   - 内部类中不能定义静态成员（因为没有意义，全局变量只是需要在类中找到一个安放的位置，没必要放到类的类里面）
   - 内部类可以直接访问外部类中的成员变量
   - 内部类可以定义在外部类的方法外面，也可以定义在外部类的方法体中

   ```Java
   public class Outside{
       int out_x = 0;
       
       public void method(){
           //在方法体内部定义的内部类
           class Inner2{
               public void method(){
                   out_x = 3;
               }
           }
           
           Inner2 inner2 = new Inner2();
       }
       //在方法体外面定义的内部类
       public class Inner1{
           
       }
   }
   ```

   - Inner Class(内部类)   一般是Java说法
   -  Static   Nested Class(静态嵌套类) 一般是C++说法
   - 他们的最大不同就在于**是否有 指向外部的引用 **
   
   在方法外部定义的内部类前面可以加上static关键字，从而成为Static Nested Class,它不再具有内部类的特性。或者可以说，不是内部类。
   
   Static Nested Class 和普通类在运行时的功能没有什么区别，只是在引用时的语法上有点区别。**它可以定义为public,protected,default,private等类型，但是普通类只能用public或者缺省修饰**。在外面不需要创建外部类的实例对象就可以直接创建Static Nested Class.
   
   ```Java
   Outer.Inner inner = new Outer.Inner();
   ```
   
   由于static Nested Class不依赖于外部类的实例变量，所以Static Nested Class能够访问外部类的非static的成员变量。
   
2. super.getClass()方法调用

   程序输出结果判断

   ```Java
   import java.util.Date;
   
   public class Test extends Date {
       public static void main(String[] args) {
           new Test().test();
       }
       
       public void test() {
           System.out.println(super.getClass().getName());
       }
   }
   ```

   结果是Test.而不是父类名Date.这是因为在test()方法中调用了getClass().getName()方法.而getClass()在Object类中定义成了final,子类不能覆盖该方法。在test中调用super.getClass().getName()其实就是在调用从父类继承的getClass()方法。

   如果希望得到父类的名称，应该写成：

   ```Java
   getClass().getSuperClass().getName();
   ```

3. String s = new String("xyz"); 创建了几个 String Object? 二者之间有什么区别

   两个或者一个。

   Java为了避免产生大量的String对象，设计了一个**字符串常量池**。String 类的工作原理是：当创建一个字符串时，JVM首先会检查字符串常量池中是否有值相等的字符串。如果有，那么不会再创建，直接就返回**该字符串的引用地址**；如果没有，那就创建，然后放到字符串常量池中，并且返回**新创建的字符串的引用地址**。

   所以 String s = new String("xyz")创建几个对象，是取决于之前有没有用过"xyz",在字符串常量池中能否找到这个相同值的。

   "xyz"对应着一个对象，这个对象放在**字符串常量缓冲区**。常量"xyz"不管出现多少遍，都是缓冲区中的那一个。new String每new一次，就创建一个新的对象。如果之前创建过了，就不需要创建"xyz"字符串常量，只会产生一个每次new出来的对象，并且让它引用到"xyz"（直接从常量池拿）.如果没有，那就是创建两个对象。

   ```Java
   public class StringTest{
       public static void main(String[] args){
           String s1 = "Hello";
           String s2 = "Hello";
           String s3 = new String("Hello");
           
           System.out.println("s1和s2引用地址是否相同" + (s1 == s2));
          System.out.println("s1和s2值是否相同" + s1.equals(s2)); 
           System.out.println("s1和s3引用地址是否相同" + (s1 == s3));
           System.out.println("s1和s3值是否相同" + s1.equals(s3)); 
       }
   }
   ```

   ```Java
   s1和s2 引用地址是否相同：true
   s1和s2 值是否相同：true
   s1和s3 引用地址是否相同：false
   s1和s3 值是否相同：true
   ```

    == 判断两个对象引用的地址是否相同，也就是判断是否为同一对象；

   equals判断值是否相同。说明s1和s2引用同一对象的地址，s3则和其他两个引用不是同一对象地址。

   new出来的对象都在堆里，局部变量在栈中，String字符串在常量池中。内存结构如下。

   ![image-20220120231758496](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220120231759893-1357382602.png)

s1和s2都指向常量池中的字符串常量，所以比较的是同一块内存地址。而s3指的是堆里面的一块内存（堆里面的Eden区域），所以不是同一块内存。

4. 下面这条语句创建了多少个对象？

```Java
String s = 'a' + 'b' + 'c' + 'd';
```

只有一个对象，'a' 'b' 'c' 'd'都是常量，对于常量，编译时就直接存储他们的值而不是引用，在编译阶段就直接将他们连接的结果提取出来变成了"abcd", 在class文件中就相当于:

```Java
String s = "abcd";
```

当JVM执行到这一句时，就在String常量池中找，如果没有这个字符串，就会产生一个"abcd"对象。注意s只是引用变量不是对象，String s = null; 不生成任何对象 。

可以从下面这段栗子看出javac编译可以对字符串常量直接相加的表达式进行优化直接变成常量相连接的效果，而不必等到运行时再做加法处理。

```Java
String s1 = "a";
String s2 = s1 + "b";
String s3 = "a" + "b";
System.out.println( s2 == "ab");//false
System.out.println(s3 == "ab");//true
```

 "+"运算符其实是通过StringBuilder的append方法进行拼接，然后通过StringBulider的toString方法返回。

5. String 和 StringBuffer的区别

   Java提供了这两个类来存储和操作字符串。String类提供了**数值不可改变的字符串**。而StringBuffer类**提供的字符串可以进行修改**。典型地可以使用StringBuffer来动态构造字符数据。另外，**String类实现了equals方法，但是StringBuffer类没有实现**。所以new String("abc").equals(new String("abc")) 结果为true, 但是 new StringBuffer("abc").equals(new StringBuffer("abc")) 的结果为false. (用了Object类的equals默认是==)

   一个栗子：把1到100的所有数字拼起来组成一个串

   ```Java
   StringBuffer sbf = new StringBuffer();
   for(int i = 0; i<100; i++){
       sbf.append(i);
   }
   ```

   这么写效率很高，因为只创建了一个StringBuffer对象

   ```Java
   String str = new String();
   for(int i = 0; i<100; i++){
       str = str + i;
   }
   ```

   这个效率比较低，因为创建了100个对象。

   另外，**String覆盖了hashCode方法，但是StringBuffer没有覆盖**，所以将StringBuffer对象存储进Java集合类时会出现问题。

## 二、计网部分

1. 为什么TCP挥手需要4次

   主要原因是当服务器收到客户端的FIN数据包之后，只是表明客户端这边没有要发送的数据了。但是服务端自身可能还有数据没有发完，并不会立即就确认关掉。所以服务端只是会先将ACK发过去告诉客户端我收到你的断开请求了。但是你还需要给我一点时间，这段时间我用来发送剩下的数据报文，发完之后服务端会将FIN包发给客户端表示完事了现在可以断开了。客户端收到FIN包之后也要发送ACK给客户端确认断开信息。

2. 为什么四次挥手释放连接时需要等待2MSL

   MSL: Maximum Segment LifeTime 最长报文段寿命

   指的是任何报文在网络上存在的最长的时间，超过这个时间报文将会被丢弃。设置等待时间为2MSL主要是为了解决两个问题：

   - 为了保证客户端能把最后一个ACK报文段发送到服务端，**让服务端正常进入Closed状态**

     这是因为最后这个ACK报文段有可能会丢失，这样服务端收不到ACK还处在Last_ACK状态。服务器端就要**超时重传**FIN + ACK报文段，而A能在2MSL之内收到这个重传的报文段。这样，双方都进入了closed状态。 如果客户端在Time_wait状态不等待一段时间，而是在发送完ACK之后立马就释放连接，那就没法收到服务端重传的FIN+ACK报文段，也不会再给服务端发一次确认报文段，那么服务端就没法正常进入closed状态

   - 为了保证上一次连接的报文已经在网络中消失，在新的连接中不会出现旧的连接请求报文段这种**报文冲突**的情况

     假如客户端发送的第一个请求连接报文段丢失没有收到确认，会重传一次连接请求。后来服务端收到了确认，就建立连接。这时候客户端一共发了两个连接请求报文，第一个丢失，第二个到达。如果第一个报文段不是丢失，而是在某些网络节点长时间逗留导致延误，到达服务端的时候可能上一次连接已经释放了。这本来是已经失效的报文段，但是服务端不知道，会再次建立连接。这样新的连接中有旧的连接请求报文段。2MSL等待时间就是为了让本次连接持续时间里产生的所有报文段都从网络消失，避免报文冲突。

3. 简述DNS协议

   DNS协议是基于UDP的应用层协议，域名解析协议(Domain Name System)。它的功能是根据用户输入的域名，解析出该域名对应的IP地址。

4. 简述DNS协议的解析过程

   - 客户端由浏览器发出查询请求，先在本地计算机缓存里查找。如果没有找到，就会将请求发送给根服务器查询。根服务器中记录的都是各个顶级域所在的服务器的位置，比如向根请求zdns.cn，根服务器通过解析根域部分就会返回.cn服务器的位置信息
   - 递归服务器拿到.cn的权威服务器地址之后，就会寻问cn的权威服务器zdns.cn的位置。这时候cn权威服务器就会查找并返回zdns.cn服务器的地址
   - 继续向zdns.cn的权威服务器查询这个地址，得到地址：202.173.11.10
   - 最终进行http的连接访问网站

   一旦递归服务器拿到解析记录以后，就会在本地缓存，下次客户端再次请求本地的递归域名服务器相同的域名时，就不要一层一层去查找了，直接把本地缓存的记录返回给客户端就可以了。

   ![image-20220119230531873](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220119230534876-1608521206.png)

5. 简述HTTP协议

   http协议是基于TCP协议的应用层传输协议，超文本传输协议(Hyper Test Transfer Protocol)。是让客户端和服务器端进行数据传输的一种规则。不涉及数据包传输，主要是规定了客户端和服务器之间的通信格式，默认使用80端口。有以下特点：
   
   - 简单快速
   
     客户端向服务器请求服务时，只需要传送请求方法和路径
   
     请求方法常用的有GET HEAD PUT DELETE POST
   
     每种方法规定了客户和服务器联系的类型不同
   
     由于HTTP协议比较简单，所以通信速度很快
   
   - 灵活
   
     能够传输任意类型的数据对象
   
   - 无连接
   
     无连接指的是限制每次连接只处理一个请求。服务器处理完客户请求，并且收到客户应答之后，就会断开连接。这种方式能节省传输时间
   
   - 无状态
   
     该协议本身是一种无状态的协议。也就是说协议自身不会对请求和响应之间的通信状态进行保存。任何两次请求之间都没有依赖关系。每个请求都是独立的。这么做是为了更快地处理大量事物。
   
     ![image-20220119231440249](https://img2020.cnblogs.com/blog/2537691/202201/2537691-20220119231443039-2040786431.png)

