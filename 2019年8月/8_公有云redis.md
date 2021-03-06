## 公有云
环境搭建好，java代码能连上后，开始下一步构思，看看大佬的博客
https://blog.51cto.com/13447608/2159501
## 开放6973端口
> 为了方便，直接开饭6379给所有ip
+ 开启防火墙 sudo systemctl start firewalld
+ 命令firewall-cmd --zone=public --add-port=80/tcp --permanent
+ 重启firewall-cmd --reload
+ 查看开放端口firewall-cmd --zone=public --list-ports
## RDB
可以在指定的时间间隔生成数据集的时间点快照（point-in-time-snapshot）；相当于在一定时间内把当前redis缓存数据库里面的数据拍个照片，
存放到磁盘上的永久化文件上（dbfilename），性能比AOF持久化高，突然宕机可能会照成少量的数据丢失
## AOF
记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集。AOF文件中命令全部以Redis协议的格式来保存，
新命令会被追加到文件末尾。因为每执行一条写操作，都要对磁盘上写一次，所以性能比较低，安全性最好，实时记录
## RDB和AOF的选择
一般来说,如果想达到足以媲美 PostgreSQL 的数据安全性， 你应该同时使用两种持久化功能。
如果你非常关心你的数据,但仍然可以承受数分钟以内的数据丢失， 那么你可以只使用 RDB 持久化。
有很多用户单独使用AOF，但是我们并不鼓励这样，因为时常进行RDB快照非常方便于数据库备份，启动速度也较之快，还避免了AOF引擎的bug。
## 快照
+ 默认情况下，Redis保存数据集快照到磁盘，名为dump.rdb的二进制文件。
你可以设置让Redis在N秒内至少有M次数据集改动时保存数据集，或者你也可以手动调用SAVE或者BGSAVE命令。
+ 例如，这个配置会让Redis在每个60秒内至少有1000次键改动时自动转储数据集到磁盘：
在redis.conf配置文件里面加上save 60 1000
具体参考以下两个网址
> https://blog.51cto.com/13447608/2159501
https://www.cnblogs.com/itdragon/p/7906481.html
## rdb配置
在redis.conf在文件中配置：
```redis
#60S内有1000次改写就拍照
save 60 1000 
高级配置
stop-writes-on-bgsave-error yes
rdbcompression yes 
rdbchecksum yes
dbfilename dump.rdb
dir ./ 
以上配置分别表示：
后台备份进程出错时,主进程停不停止写入? 主进程不停止容易造成数据不一致 
导出的rdb文件是否压缩 如果rdb的大小很大的话建议这么做
导入rbd恢复时数据时,要不要检验rdb的完整性 验证版本是不是一致
导出来的rdb文件名
rdb的放置路径
```
## AOF持久化配置
```redis
#基本配置
```redis
appendonly yes/no
appendfsync always
appendfsync everysec
appendfsync no
```
配置分别表示：
是否打开aof日志功能
每1个命令,都立即同步到aof 
每秒写1次
写入工作交给操作系统,由操作系统判断缓冲区大小,统一写入到aof.
#高级配置
```redis
no-appendfsync-on-rewrite yes/no
auto-aof-rewrite-percentage 100 
auto-aof-rewrite-min-size 64mb
```
配置分别表示：
正在导出rdb快照的过程中,要不要停止同步aof
aof文件大小比起上次重写时的大小,增长率100%时重写,缺点:业务开始的时候，会重复重写多次。
aof文件,至少超过64M时,重写
```
## 消息模式
Redis发布消息通常有两种模式：
1.队列模式（queuing）
任务队列：顾名思义，就是“传递消息的队列”。与任务队列进行交互的实体有两类，一类是生产者（producer），另一类则是消费者（consumer）。  
生产者将需要处理的任务放入任务队列中，而消费者则不断地从任务独立中读入任务信息并执行。
任务队列的好处：
• 松耦合。
生产者和消费者只需按照约定的任务描述格式，进行编写代码。
• 易于扩展。
多消费者模式下，消费者可以分布在多个不同的服务器中，由此降低单台服务器的负载。
2.发布-订阅模式(publish-subscribe)
一个发布者发布消息，多个订阅者进行消息的订阅，目的是为了消息的传送
##  redis中事物锁机制
+ 悲观锁
概念：12306买票，我选择了票，不管有没有付钱这张票都是我的，我把它锁上，别人就看不到了
+ 乐观锁
概念：类似于商品秒杀，你选择之后，别人还是能看到，被人还是能付钱，谁先付钱是谁的
## redis的主从复制
• 使用异步复制。
• 一个主服务器可以有多个从服务器。
• 从服务器也可以有自己的从服务器。
• 复制功能不会阻塞主服务器。
• 可以通过复制功能来让主服务器免于执行持久化操作，由从服务器去执行持久化操作即可。
> 无论何时，数据安全都是极其重要的，所以应该禁止主服务器关闭持久化的同时自动拉起。
