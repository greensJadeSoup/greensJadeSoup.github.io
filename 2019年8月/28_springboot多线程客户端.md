## @Async
满足计算机CPU处理能力的情况下，使用多线程可以明显提高程序运行效率，缩短大数据处理的能力.
spring中有@Async，可将一个方法设置为异步方法
这里缺了调用方法参考：https://www.jianshu.com/p/67052c477dbf
这里补充：https://blog.csdn.net/weixin_39528789/article/details/80769112
## udp客户端思维
一个多线程开启监听udp服务器一切发送协议，接受后解析，再调用不同方法，例如存到redis缓存，显示到页面，再存到数据库
页面点击后，后端生成协议发送给服务器
## PostgreSQL
+ https://www.jianshu.com/p/b46efc72454e
+ 导入参考数据库
