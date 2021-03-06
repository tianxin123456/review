

## Day 06 复习内容

### 一、Java SE 部分

1. 什么是JVM

   JVM(Java Virtual Machine) Java虚拟机

   JVM是Java程序运行的底层平台，和Java支持库一起构成了Java程序的运行环境。分为JVM规范和JVM实现两个部分。

   Java虚拟机就是能执行标准Java字节码的虚拟计算机

2. JDK、JRE、JVM区别

   首先， **JDK包含JRE,JRE包含JVM**。一个Java程序从编写到运行需要经过 编写代码（可能要用一些库）----将代码编译成.class文件----解释并执行类文件。从这个过程可以看出三者各自用途。

   ![image-20220122213935540](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122213936151-1262267169.png)

   - JVM

     虚拟机其实起了一个映射作用，实现Java的跨平台特性。不同的平台系统底层的执行代码可能不能，所有的java程序会首先被编译为.class的类文件，这种类文件可 以在虚拟机上执行，也就是说class并不直接与机器的操作系统相对应，而是经过虚拟机间接与操作系统交互，由虚拟机将程序解 释给本地系统执行。JVM的作用就是把**类文件**解释为Mac, Windows能够理解执行的代码。不同系统对应的JVM不同，同一个类文件.class要想在不同平台运行，需要经过不同的JVM解释。

   - JRE

     JRE是Java程序的运行环境，准备来说是 类文件的运行环境。其作用就是解释并执行类文件。

     ![image-20220122214329346](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122214329884-1988307388.png)

     其中包含了bin和lib.可以把bin看作是JVM,用来解释执行类文件，但是在解释类文件的过程可能要用到一些库。这些库是JVM没有的，而是在lib中。所以 JRE = JVM + lib.

   - JDK

     Java开发工具包。执行一个Java程序只需要JRE,但开发程序需要JDK.

     ![image-20220122214546241](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122214546803-1437542664.png)

     JDK是包括JRE的，因为开发过程也需要运行程序。此外，JDK中还有一些工具和编码是可能会用到的库。比如读取本地文件使用IO库等。

     - src 类库源码压缩包
     - bin 最主要的是编译器 javac.exe
     - include Java和JVM交互的头文件
     - lib 类库
     - jre Java运行环境
     - 其他几个声明文件

   因为eclipse idea等IDE有自己的编译器而不用JDK的bin目录自带的，所以安装时只要求选jre路径就行了。

   **总体来说就是，我们利用JDK（调用JAVA API）开发了属于我们自己的JAVA程序后，通过JDK中的编译程序（javac）将我们的文本java文件编译成JAVA字节码，在JRE上运行这些JAVA字节码，JVM解析这些字节码，映射到CPU指令集或OS的系统调用。**

3. 数组有没有length()方法，String有没有length()方法

   数组没有length方法，数组只有length属性。使用形式是arr.length, 而String是字符串类，有length()方法。另外，集合也没有length()方法，求长度用的是size()方法。

4. try{}里有个return语句，那么紧跟在try之后的finally{}里代码会不会被执行，什么时候被执行，在return之前还是之后

   ```Java
   try{
       for(int i = 0; i < 3; i++){
           for(int j = 0; j < 3; j++){
               if(i==2){
                   System.out.println("return 要执行了");
                   return;
               }
               System.out.println(i + "====" + j);
           }
       }
   }
   finally{
       System.out.println("finally执行")；
   }
   ```

   会在return中间执行。try中的return语句调用的函数先于finally中调用的函数执行。也就是说return 语句先执行，finally语句后执行，但是return并不是让函数马上返回，而是return 执行后，将返回结果放到函数栈中。执行finally之后才真正开始返回。此时两种情况：

   - finally中也有return , 会直接返回并终止程序，函数中的return不会被执行
   - finally中没有return ,在执行完finally中的代码之后，会将函数栈中的try中的return内容返回并终止程序。

   ![image-20220122212338394](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122212338916-807538802.png)

5. final, finally, finalize的区别

   - final 用于声明属性、方法和类，分别表示属性不可变、方法不可覆盖、类不可继承。内部类要访问局部变量，局部变量必须定义为final类型。
   - finally是异常处理语句结构的一部分，表示总是会执行
   - fianlize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法来提供垃圾收集时的其他资源回收。比如关闭文件等，JVM不保证此方法总被调用。

## 二、计网部分

1. 简述cookie

   cookie是在http协议下，服务器或者脚本维护客户工作站上信息的一种方式，存储在**本地终端**、通常经过加密的数据。http协议本身是无状态的，为了让其能处理更加复杂的逻辑。http/1.1引入cookie来**保存状态信息**。cookie是由服务端产生的，再发送给客户端保存。当客户端再次访问的时候，服务器可根据cookie辨识客户端是哪个，以此做个性化推送，免账号密码等。这样可以减轻服务器验证工作的负担以及提高用户的体验。

   ![image-20220122192615952](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122192617157-1877204455.png)

2. 简述session

   session是服务器端的一个map集合，用于**标记特定客户端的信息**。

   一般客户端带cookie对服务器进行访问，可通过cookie中的session id从整个session中查询到服务器记录的关于客户端的信息。

   当我们的Servlet需要使用session时，执行下面代码：

   ```Java
   HttpSession session = request.getSession();//取出session
   session.setAttribute("goods1","Scoat");//session存数据
   session.getAttribute("goods1");//session取数据
   ```

   第一次取session，服务器会创建一个session对象，并且存入服务器的session集合以sessioId为键，根据sessionId就可以取到对应的session引用。同时服务器会在响应http时把这个sessionId以cookie的形式发给客户端。浏览器下一次再请求这个网站时，会把之前保存的 Session ID 再重新发给服务端。 这时候服务端就会用这个 Session ID 和它之前建立的存储结构中进行匹配，如果这个 Session ID 是之前合法创建的，那么就可以从服务端存储中得到用户之前的状态了，比如登录用户名之类的。

   session的有效时间一般是30分钟。

   过程分析：

   - 浏览器第一次请求网站，服务端生成Session ID
   - 把生成的Session ID 保存到服务端存储
   - 把生成的Session ID 返回给浏览器，通过set-cookie
   - 浏览器收到Session ID, 在下一次请求时带上Session ID
   - 服务端收到浏览器发来的Session ID,从Session 存储中找到用户状态数据，会话建立。
   - 此后请求都会交换这个Session ID,进行有状态的会话
   
   ![image-20220122192639212](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122192639644-327617606.png)

3. cookie和session的区别

   cookie 和 session 都是用来跟踪浏览器用户身份的会话方式

   - 存储位置不同

     cookie数据存放在客户的浏览器上，session数据放在服务器上

   - 安全性

     cookie不是很安全，可以查看本地的cookie。session是安全的
   
   - session会在一段时间内保存在服务器上，当访问增多，会比较占用服务器资源。而cookie保存的数据大小有限制，单个cookie保存的数据不能超过4k,很多浏览器会限制一个站点最多能保存20个cookie.
   
   结合使用：
   
   - 通过cookie存储一个sessionID,然后具体数据保存在session中。server side session
   - 将session数据加密，然后存储在cookie中.client side session.

4. 简述http状态码和对应的信息

   负责表示客户端http请求的返回结果，标记服务器处理是否正常，并且通知出现的错误等。

   1××: 接收的信息正在处理   临时的响应

   2××：请求正常处理完毕   成功

   3××：重定向  客户端浏览器必须采取更多操作来实现请求

   4××：客户端错误   比如客户端没有提供有效的身份验证信息

   5××：服务端错误   服务器遇到错误不能完成请求

   一些常见的错误码：

   301：永久重定向

   ![image-20220122194227851](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194228414-703807373.png)

   302：临时重定向

   ![image-20220122194202176](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194202722-787967810.png)

   304：资源没修改，用之前缓存就行 

   ![image-20220122194324746](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194325306-41825507.png)

   400：客户端请求的报文有错误

   ![image-20220122194409659](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194410237-546573674.png)

   403：服务器禁止访问资源

   ![image-20220122194445641](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194446184-377055178.png)

   404：请求的资源在服务器上不存在或者找不到

   ![image-20220122194513060](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122194513583-1148561834.png)

5. 转发和重定向的区别

- 请求次数不同
  
     重定向可以理解为客户端的行为，至少需要两次请求。客户端发起一次请求，服务端给出一次响应。但这个响应包含着下一次客户端需要访问的url,客户端再次发起请求，将会得到处理结果。 
     
      发送请求-->服务器运行来响应请求，返回一个新的url和响应码-->浏览器根据响应码判断为重定向，发送一个新的请求给服务器-->服务器运行响应请求
     
     ![image-20220122203749223](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122203749700-459360838.png)
     
     而转发可以理解为服务端的行为。客户端发起一次请求，这个请求在整个服务器端可以被多次传递，但都是服务器端的处理程序传递给另一个处理程序，客户端不需要发起第二次请求。无论这个请求经历过多少个处理程序，始终都是同一个请求。也就意味着这个请求中的数据经历过的每一个处理程序都可以使用。

  发送请求-->服务器运行-->进行请求的重新设置，比如通过request.setAttribute(name, value)-->根据转发的地址，获取该地址的网页-->响应请求给浏览器
  
  ![image-20220122203807143](https://img2022.cnblogs.com/blog/2537691/202201/2537691-20220122203807600-112201794.png)
  
- 路径栏的区别

  使用重定向跳转页面后，在客户端路径栏显示的应该是其重定向之后的路径，客户端是可以看到页面地址改变的。

  转发，中间传递的是自己容器的request,客户端路径栏显示的还是第一次访问的路径。客户端是感受不到服务器做了转发的。

- 重定向的网址可以是任何网址，转发的网址必须是本站点的网址

  重定向时以前request中存放的变量全部失效，进入一个新的request作用域；转发时request中存放的变量不会失效，就像把两个页面拼到一起。

