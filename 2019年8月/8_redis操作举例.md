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
## redis字典操作举例
+增
hset myhash field1 "s"    
若字段field1不存在,创建该键及与其关联的Hashes, Hashes中,key为field1 ,并设value为s ，若存在会覆盖原value
hsetnx myhash field1 s    
若字段field1不存在,创建该键及与其关联的Hashes, Hashes中,key为field1 ,并设value为s， 若字段field1存在,则无效
hmset myhash field1 "hello" field2 "world       一次性设置多个字段
+ 删
hdel myhash field1                      删除 myhash 键中字段名为 field1 的字段
del myhash                          删除键
+ 改  
hincrby myhash field 1                  给field的值加1
+ 查
hget myhash field1                      获取键值为 myhash,字段为 field1 的值
hlen myhash                         获取myhash键的字段数量
hexists myhash field1                   判断 myhash 键中是否存在字段名为 field1 的字段
hmget myhash field1 field2 field3           一次性获取多个字段
hgetall myhash                          返回 myhash 键的所有字段及其值
hkeys myhash                        获取myhash 键中所有字段的名字
hvals myhash                        获取 myhash 键中所有字段的值
## redis列表操作举例
+ 增 
lpush mykey a b             若key不存在,创建该键及与其关联的List,依次插入a ,b， 若List类型的key存在,则插入value中
lpushx mykey2 e             若key不存在,此命令无效， 若key存在,则插入value中
linsert mykey before a a1   在 a 的前面插入新元素 a1
linsert mykey after e e2    在e 的后面插入新元素 e2
rpush mykey a b             在链表尾部先插入b,在插入a
rpushx mykey e              若key存在,在尾部插入e, 若key不存在,则无效
rpoplpush mykey mykey2      将mykey的尾部元素弹出,再插入到mykey2 的头部(原子性的操作)
+ 删
del mykey               删除已有键 
lrem mykey 2 a              从头部开始找,按先后顺序,值为a的元素,删除数量为2个,若存在第3个,则不删除
ltrim mykey 0 2             从头开始,索引为0,1,2的3个元素,其余全部删除
+ 改
lset mykey 1 e              从头开始, 将索引为1的元素值,设置为新值 e,若索引越界,则返回错误信息
rpoplpush mykey mykey       将 mykey 中的尾部元素移到其头部
+ 查
lrange mykey 0 -1       取链表中的全部元素，其中0表示第一个元素,-1表示最后一个元素。
lrange mykey 0 2            从头开始,取索引为0,1,2的元素
lrange mykey 0 0            从头开始,取第一个元素,从第0个开始,到第0个结束
lpop mykey                  获取头部元素,并且弹出头部元素,出栈
lindex mykey 6              从头开始,获取索引为6的元素 若下标越界,则返回nil
## redis集合操作举例
+ 增
sadd myset a b c  
若key不存在,创建该键及与其关联的set,依次插入a ,b,若key存在,则插入value中,若a 在myset中已经存在,则插入了 d 和 e 两个新成员。
+ 删
spop myset              尾部的b被移出,事实上b并不是之前插入的第一个或最后一个成员
srem myset a d f        若f不存在, 移出 a、d ,并返回2
+ 改
smove myset myset2 a        将a从 myset 移到 myset2，
+ 查
sismember myset a           判断 a 是否已经存在，返回值为 1 表示存在。
smembers myset          查看set中的内容
scard myset             获取Set 集合中元素的数量
srandmember myset       随机的返回某一成员
sdiff myset1 myset2 myset3      1和2得到一个结果,拿这个集合和3比较,获得每个独有的值
sdiffstore diffkey myset myset2 myset3      3个集和比较,获取独有的元素,并存入diffkey 关联的Set中
sinter myset myset2 myset3              获得3个集合中都有的元素
sinterstore interkey myset myset2 myset3  把交集存入interkey 关联的Set中
sunion myset myset2 myset3              获取3个集合中的成员的并集
sunionstore unionkey myset myset2 myset3  把并集存入unionkey 关联的Set中
## redis有序操作举例
+ 增
zadd myzset 2 "two" 3 "three"           添加两个分数分别是 2 和 3 的两个成员
+ 删
zrem myzset one two                 删除多个成员变量,返回删除的数量
+ 改
zincrby myzset 2 one                将成员 one 的分数增加 2，并返回该成员更新后的分数
+ 查 
zrange myzset 0 -1 WITHSCORES       返回所有成员和分数,不加WITHSCORES,只返回成员
zrank myzset one                获取成员one在Sorted-Set中的位置索引值。0表示第一个位置
zcard myzset                        获取 myzset 键中成员的数量
zcount myzset 1 2                   获取分数满足表达式 1 <= score <= 2 的成员的数量
zscore myzset three                 获取成员 three 的分数
zrangebyscore myzset  1 2               获取分数满足表达式 1 < score <= 2 的成员
#-inf 表示第一个成员，+inf最后一个成员
#limit限制关键字
#2  3  是索引号
zrangebyscore myzset -inf +inf limit 2 3  返回索引是2和3的成员
zremrangebyscore myzset 1 2         删除分数 1<= score <= 2 的成员，并返回实际删除的数量
zremrangebyrank myzset 0 1              删除位置索引满足表达式 0 <= rank <= 1 的成员
zrevrange myzset 0 -1 WITHSCORES        按位置索引从高到低,获取所有成员和分数
#原始成员:位置索引从小到大
      one  0  
      two  1
#执行顺序:把索引反转
      位置索引:从大到小
      one 1
      two 0
#输出结果: two  
       one
zrevrange myzset 1 3                获取位置索引,为1,2,3的成员
#相反的顺序:从高到低的顺序
zrevrangebyscore myzset 3 0             获取分数 3>=score>=0的成员并以相反的顺序输出
zrevrangebyscore myzset 4 0 limit 1 2    获取索引是1和2的成员,并反转位置索引
