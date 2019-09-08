## 公有云
环境搭建好，java代码能连上后，开始下一步构思，看看大佬的博客
https://blog.51cto.com/13447608/2159501
## 开放6973端口
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

## redis全局keys举例
+ 删 
flushdb             清空当前选择的数据库 
del mykey mykey2        删除了两个 Keys 
+ 改 
move mysetkey 1         将当前数据库中的 mysetkey 键移入到 ID 为 1 的数据库中 
rename mykey mykey1     将 mykey 改名为 mykey1 
renamenx oldkey newkey  如果 newkey 已经存在，则无效 
expire mykey 100        将该键的超时设置为 100 秒
persist mykey       将该 Key 的超时去掉,变成持久化的键 
+ 查 
keys my*            获取当前数据库中所有以my开头的key 
exists mykey        若不存在,返回0;存在返回1 
select 0            打开 ID 为 0 的数据库 
ttl mykey           查看存货时间还剩下多少秒
type mykey          返回mykey对应的值的类型 
randomkey           返回数据库中的任意键
## redis字符串操作举例
+ 增
set mykey "test"            为键设置新值，并覆盖原有值
getset mycounter 0              设置值,取值同时进行
setex mykey 10 "hello"          设置指定 Key 的过期时间为10秒,在存活时间可以获取value
setnx mykey "hello"             若该键不存在，则为键设置新值
mset key3  "zyx"  key4 "xyz"    批量设置键
+ 删
del mykey                   删除已有键
+ 改
append mykey "hello"            若该键并不存在,返回当前 Value 的长度
                            该键已经存在，返回追加后 Value的长度
incr mykey                  值增加1,若该key不存在,创建key,初始值设为0,增加后结果为1
decrby  mykey  5            值减少5
setrange mykey 20 dd            把第21和22个字节,替换为dd, 超过value长度,自动补0
+ 查  
exists mykey                    判断该键是否存在，存在返回 1，否则返回0
get mykey                   获取Key对应的value
strlen mykey                获取指定 Key 的字符长度
ttl mykey                   查看一下指定 Key 的剩余存活时间(秒数)
getrange mykey 1 20             获取第2到第20个字节,若20超过value长度,则截取第2个和后面所有的
mget key3 key4                  批量获取键
