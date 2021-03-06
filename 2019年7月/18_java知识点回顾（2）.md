## javase
### collection
实用常用的数据结构,就是容器
常用子接口  
1. List   实现类：ArrayList、Vector、LinkedList  
2. Set   实现类：HashSet、TreeSet
#### List
有序（存储和取出的元素顺序一致），可重复
#### Set
无序（存储和取出顺序不一致，有可能会一致），但是元素唯一，不能重复
 ### map
 可以通过键来获取值 key-value
### 异常
并不等同于语法错误，从Throwable继承，一般视为从Exception中来
#### 处理异常
**1.捕获 （finally可以省略，catch可以有多个，捕获多个不同的异常。也可以只有try代码块）**
```java
try{ 
    //可能发生异常的代码 
} catch(异常的类型声明){ 
    //发生异常时执行的代码 
} finally{ 
    //无论是否发生异常都会执行的代码 
}
```
**2.抛出**
+ throw
抛出的是异常的对象，存在在方法体内，执行一定会抛出异常
+ throws 
抛出的是异常的类型，声明在方法体外，可能会发生异常
**3.自定义异常**
自己定义异常
### 线程
适用于：
+ 程序需要同时完成多个任务
+ 可以使用单线程完成的但是利用多线程可以更快的完成
#### 创建
1.用Thread创建线程
```java
class thread extends Thread{
    public void run() {

    }
}
public static void main(String[] args) {
    Thread t=new thread();
    t.start();
}
```
2.用Runnable创建线程
### 同步
多个线程同时操作一个可共享的资源变量时，将会导致数据不准确，加入同步锁以避免在该线程没有完成操作之前，被其他线程的调用， 从而保证了该变量的唯一性和准确性。
**方法：synchronized关键字修饰的语句块会自动被加上内置锁，从而实现同步**
同步是一种高开销的操作，因此应该尽量减少同步的内容。
### 线程池
实现了ThreadPoolExecutor类：
+ ExecutorService newCachedThreadPool()：无界线程池
+ ExecutorService newFixedThreadPool()：有界线程池
+ ExecutorService newSingleThreadExecutor()：单一线程池
### Lambda表达式
是一个匿名函数,一段可以传递的代码
操作符为 “->”,左侧指定了 Lambda 表达式需要的所有参数,右侧指定了 Lambda 体，即 Lambda 表达式要执行的功能
```java
public static void main(String[] args) {    
    Runnable r1 = () -> System.out.println("Hello Lambda!");   
    r1.run();
 }//Hello Lambda!
```
（1）语法格式一：无参，无返回值，Lambda 体只需一条语句
  示例：Runnable r1 = () -> System.out.println("Hello Lambda!");
（2）语法格式三：Lambda 只需要一个参数时
  示例：
  ```java
Consumer<String> con = x -> System.out.println(x);
con.accept("hello");// hello
```
.......
**感觉用的不多，需要的时候在查吧**
### File类
主要用于文件和目录的创建、文件的查找和文件的删除等
1.File file = new  File("D:/test.txt")或者  File  file  =new  File("D:\\test.txt")
File  file  =  new File("D:/myword","word.txt");
### io流
#### 字节流的抽象基类：
字节流：InputStream（字节输入流）、OutputStream（字节输出流）；字符流：Reader（字符输入流）、Writer（字符输出流）；
+ 字节输出流：FileOutputStream outputStream = new FileOutputStream("a.txt"); 
+ 字节输入流：FileInputStream inputStream = new FileInputStream("a.txt");
####  缓冲流、转换流、序列化流、打印流
### 网络编程
+ C/S结构 ：全称为Client/Server结构，是指客户端和服务器结构，qq
+ B/S结构 ：全称为Browser/Server结构，是指浏览器和服务器结构，谷歌浏览器
#### 网络通信协议
1.网络通信协议：通信协议是对计算机必须遵守的规则 
2.TCP/IP协议： 传输控制协议/因特网互联协议
+ TCP：面向连接的通信协议，三次握手
+ UDP：一个面向无连接的协议，不可靠，因为无连接，所以传输速度快，但是容易丢失数据。
#### TCP通信流程
1. 【服务端】启动,创建ServerSocket对象，等待连接。
2. 【客户端】启动,创建Socket对象，请求连接。
3. 【服务端】接收连接,调用accept方法，并返回一个Socket对象。
4. 【客户端】Socket对象，获取OutputStream，向服务端写出数据。
5. 【服务端】Scoket对象，获取InputStream，读取客户端发送的数据。
### Stream流
### mysql
#### 常用语句看所有数据库：show databases； 
切换（选择要操作的）数据库use ：数据库名； 
功能|语句|名字
-|-|-
创建数据库|create database 
删除数据库|drop database
修改数据库|update database 
修改数据库编码|alteb database mydbl character set utf-8
创建表|create table[if not exit ] 
查看当前所有表|show tables 
查看指定表的创建语句|show create table 
查看表结构|desc  
删除表|drop table 
修改表|前缀为alter table 
#### 事务
开启事务：start transaction； 
结束事务：commit 或 rollback
commit表示提交，即事务中的多条SQL语句所作出的影响会持久化到数据库中。
rollback表示数据回滚，即回到事务的起点，之前做的所有操作都被撤销了。
### JDBC 
数据库连接 
```java
//动态加载驱动(这里需要事先导入一个jar包) 
Class.forName(“com.mysql.jdbc.Driver”); 
//声明连接数据库的配置信息 
//连接mysql的url 
String url = “jdbc:mysql://localhost:3306/库名”; 
//登录mysql的用户名 
String user = “root”; 
//登录mysql的密码 
String pwd = “root”; 
//获得数据库连接 
Connection conn = DriverManger.getConnection(url,user.pwd);
```
### JDBC连接池
复用连接，代替传统的频繁占用系统资源和耗费时间的方式
### JDBCTemplate
spring JDBC，简化代码
### Bootstrap
html+css+js框架
### XML
配置文件，现在基本用注解了
### tomcat 
类似于IIS、Apache的Web服务端程序，Apache只支持静态网页html，tomcat支持jsp动态网页。 
### https 
https是在http协议基础上加入加密处理和认证机制以及完整性保护，即http+加密+认证+完整性保护=httpshttps并非应用层的一种新协议，只是http通信接口部分用ssl/tls协议代替而已。通常http直接和tcp通信，当使用ssl时则演变成先和ssl通信，再由ssl和tcp通信。所谓https，其实就是身披ssl协议这层外壳的http
### request
通过Request对象可以在服务器端获取客户端发送的请求数据内容
### response
通过Response对象可以生成服务器端向客户端响应的数据内容。
### cookie,session
session和cookie的作用有点类似，都是为了存储用户相关的信息。不同的是，cookie是存储在本地浏览器，而session存储在服务器
#### 生命周期
(1)cookie的生命周期是累计的，从创建时，就开始计时，20分钟后，cookie生命周期结束，(2)session的生命周期是间隔的，从创建时，开始计时如在20分钟，没有访问session，那么session生命周期被销毁但是，如果在20分钟内（如在第19分钟时）访问过session，那么，将重新计算session的生命周期
(3)关机会造成session生命周期的结束，但是对cookie没有影响
### jsp
一个简化的Servlet设计，用来创建动态网页
### el，stl
简化jsp编写
### servlet
Servlet 通常用来servlet主要负责处理请求并且应答浏览器发出的请求. 有 HttpServlet 作为实现类，根据需要覆盖 doGet 或 doPost.
### filter
过滤器，做一些拦截的任务,在Servlet接受请求之前，做一些事情，如果不满足限定，可以拒绝进入Servlet（细节到类）
+ 在创建Filter对象的时候，调用 init 方法
+ 当请求过来之后，调用 doFilter，也就是主要的业务逻辑所在了
   chain.doFilter(request, response)放行
+ 销毁Filter对象的时候，调用 destroy 方法
### listener
监听器用于监听web应用中某些对象、信息的创建、销毁、增加，修改，删除等动作的发生，然后作出相应的响应处理,监听器可以监听Application、Session、Request对象，当这些对象发生变化就会调用对应的监听方法。
> 统计网站的在线人数(session数量)
### jq
jQuery 是一个 JavaScript 库
### MVC
## javaee
+ M：Model，模型。(JavaBean)
			* 完成具体的业务操作，如：查询数据库，封装对象
+ V：View，视图。(JSP)
			* 展示数据
+ C：Controller，控制器。(Servlet)
			* 获取用户的输入
			* 调用模型
			* 将数据交给视图进行展示
### AJAX
AJAX 是与服务器交换数据的艺术，它在不重载全部页面的情况下，实现了对部分网页的更新。（一般为异步交互模型）
### json
是存储和交换文本信息的语法，类似 XML，但更小、更快，更易解析。
### redis
数据结构服务器，为数据库分压，是一种NoSQL。
+ 存储 缓存 用的数据
+ 需要高速读/写的场合使用它快速读/写
第一次使用时发现redis中没有该数据，就从数据库中取出来放到redis再使用；第二次使用就直接从redis中取出。
### maven

+ 管理jar包，第三方jar引用（自己写的模块也可作为jar）
+ 减少项目空间
+ 项目分开后更好地配合
+ 简化构建过程

