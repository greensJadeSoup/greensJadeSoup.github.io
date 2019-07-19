## javaee
### linux
现在很多程序，数据库都是跑在上面，没深入学，只是了解了一些原理和常用命令，知道怎么在上面部署项目，呃我只是配置好环境之后把war包丢进去，命令的话用的时候要再查吧
### Hibernate(持久化层)
作用类似于SSM中的mybatis，用来操作数据库
### Struts2(控制层)
作用类似于SSM中的SpringMVC
### Spring(容器)
全家桶
### SpringMVC(控制层)
基于MVC设计模型的轻量级WEB框架，spring全家桶之一
现在基本上代替Struts2了，ssh学完没做多少东西，感觉快忘了，这个阶段学完做个小项目回顾下吧
### MyBatis(持久化层)
iBatis的优化版
使开发者只需要关注 sql 语句本身，而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。基本上已经代替Hibernate
>以上具体实现就不写了，网上一大堆，我的理解也没别人好
### oracle（比mysql贵多了）
本来想和mysql一样深入学的，可是虚拟机装了之后安装就出现各种问题，不管了，工作用到再说，现在学习就用mysql吧
### SSM整合
不多说，学习最多的，以后工作也用这个吧
### Spring Boot
Spring全家桶之一，感觉最重要的是优化配置，还有集成各种优秀框架，现在只是会简单使用
### Spring Cloud
#### Eureka
+ Eureka Server：主要提供存放注册的信息，它也提供了web界面可以查看有哪些服务，他的可用性通过复制来实现，可以通过keeplived来实现高可用
+ Eureka Client：是一个Java客户端，放在各个服务中，用于跟server端进行通信，将信息注册到服务端，同时发送心跳给server端，它本身也有缓存机制（缓存了各个服务的信息），用于防止所有的server端挂掉导致服务中断的情况。
#### Spring RestTemplate
传统情况下在java代码里访问restful服务，一般使用Apache的HttpClient。不过此种方法使用起来太过繁琐。spring提供了一种简单便捷的模板类来进行操作，这就是RestTemplate
#### Feign
一个声明式的REST客户端，它的目的就是让REST调用更加简单。Feign可以与Eureka和Ribbon组合使用以支持负载均衡
#### Ribbon
负载均衡
#### Hystrix
保护机制，熔断器
#### Zuul
网关
#### nginx
在这里的作用是反向代理和负载均衡到Zuul

