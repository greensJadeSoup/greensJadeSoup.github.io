## @Async
满足计算机CPU处理能力的情况下，使用多线程可以明显提高程序运行效率，缩短大数据处理的能力.
spring中有@Async，可将一个方法设置为异步方法
这里缺了调用方法参考：https://www.jianshu.com/p/67052c477dbf
这里补充：https://blog.csdn.net/weixin_39528789/article/details/80769112
还有：https://www.jianshu.com/p/2d4b89c7a3f1
https://www.cnblogs.com/onlymate/p/9686740.html
## udp客户端思维
一个多线程开启监听udp服务器一切发送协议，接受后解析，再调用不同方法，例如存到redis缓存，显示到页面，再存到数据库
页面点击后，后端生成协议发送给服务器
## PostgreSQL
+ https://www.jianshu.com/p/b46efc72454e
+ 导入参考数据库
https://blog.csdn.net/qq_28289405/article/details/80349582
## udp测试
https://blog.csdn.net/moshowgame/article/details/88420880
https://blog.csdn.net/qq_39402590/article/details/90473261
https://www.cnblogs.com/singleprogramdog/p/11095619.html
https://blog.csdn.net/qq_39335514/article/details/79045496
这几个接不到数据，应该是没有传参本地port
## 遇到问题
+ 服务器被写好，只能返回数据到他接受到数据的原ip与端口，而客户端发送与接收在同一个端口会冲突
