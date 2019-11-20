## springboot打包
https://blog.csdn.net/m0_37063257/article/details/78300877
## 注意点
参考网址步骤中，生成的jar包没有MANIFEST.MF文件，将你项目中src\main\java\META-INF下的MANIFEST.MF放到xxx.jar新建的META-INF文件夹中，再运行
## 运行方法
+ 前台运行java -jar XXX.jar
+ 后台运行java -jar XXX.jar &
+ 一直运行nohup java -jar XXX.jar &
参考网址https://blog.csdn.net/tang9140/article/details/38899345
## 暂停方法
netstat -lnp|grep 8080   #8080为你那jar包运行的端口
 kill -9 9932        #杀掉编号为9932的进程（请根据实际情况输入）
## 位置
jar包在炼章师兄公有云的/home/yanZhao/tcp_multithreading_jar/中，日志在/logs
## springsecurity跨域问题
网页中出现No 'Access-Control-Allow-Origin'  
项目中已经完成，却因为跨域导致出错  
老版方法，已被代替：https://blog.csdn.net/qq_19419233/article/details/99645813
新版，亲测可用：https://blog.csdn.net/tiangongkaiwu152368/article/details/81099169
