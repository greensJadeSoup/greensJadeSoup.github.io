## redis在centos7搭建
https://blog.csdn.net/zhaoxiang10052111/article/details/80253604
+ redis安装包路径home/yanzhao/redis-4.0.9
redis.conf路径：/root/usr/local/redis
+ 启动redis服务器: ./redis-server redis.conf
在后台运行./redis-server redis.conf & 
+ 启动本地redis客户端: ./redis-cli 
+ 查看redis状态:pstree
+ 端口被占用
ps -ef | grep redis 查看
kill -9 xxxx杀死
+ 关闭redis：redis-cli shutdown
+ 在配置文件里面加上
监听的地址，要加上127.0.0.1，否则从本地不能登录,或者直接注释掉
bind 10.0.0.200 127.0.0.1
取消保护模式，是否只允许本地登录
protected no
增加密码
requirepass {password}
## java代码连接redis
+ pom文件添加
```java
<dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.1.0</version>
        </dependency>
    </dependencies>
```
调用函数demo
```java
Jedis jedis=new Jedis("192.168.1.128",6379);
        jedis.auth("991030");
        System.out.println("连接成功");
        // 获取数据并输出
        Set<String> keys = jedis.keys("*");
        Iterator<String> it=keys.iterator() ;
        while(it.hasNext()){
            String key = it.next();
            System.out.println(key);
        }
```
