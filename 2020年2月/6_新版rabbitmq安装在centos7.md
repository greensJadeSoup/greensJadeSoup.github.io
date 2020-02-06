# 
## 安装erlang
1.主机安装erlang安装包：http://erlang.org/download/otp_src_22.0.tar.gz，
或者centos7上wget
2.解压tar -zxvf otp_src_22.0.tar.gz
3.安装构建Erlang语言的工具
yum install -y make gcc gcc-c++ m4 openssl openssl-devel ncurses-devel unixODBC unixODBC-devel java java-devel
4.操作，具体路径根据自己
./configure --prefix=/home/yanzhao/java_all/erlang/
5.构建
make && make install
6.配置环境
vim /etc/profile
```
export PATH=$PATH:/home/yanzhao/java_all/erlang/bin
```
7.重载环境变量配置
source /etc/profile
8.检查是否安装
erl
## 安装RabbitMQ
1、下载安装包：https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.16/rabbitmq-server-generic-unix-3.7.16.tar.xz
+最好改vpn
2.tar -xvf rabbitmq-server-generic-unix-3.7.16.tar.xz
3.
vim /etc/profile
```
export PATH=$PATH:/home/yanzhao/java_all/rabbitmq/sbin
```
4.重载环境变量配置
source /etc/profile
5.启动rabbitmq，-detached代表后台守护进程方式启动。
rabbitmq-server -detached
6.检查是否启动成功
rabbitmqctl status
7.配置防火墙：linux 端口 15672 网页管理 5672 AMQP端口：
firewall-cmd --permanent --add-port=15672/tcp
firewall-cmd --permanent --add-port=5672/tcp
systemctl restart firewalld.service
+ 防火墙开启service firewalld start
+ 重启service firewalld restart
+ 关闭service firewalld stop
8.通过命令开启web管理插件：rabbitmq-plugins enable rabbitmq_management,
停止：abbitmqctl stop，再开：rabbitmq-server -detached
（删除）重启：service rabbitmq-server restart
（删除）+ 如果失败，出现：ERROR: node with name "rabbit" already running on，可以通过：ps -ef|grep rabbitmq，找出后台文件再kill
9.输入服务器IP:15672 就可以看到RabbitMQ的WEB管理页面
+阿里云记得设置安全组，开放端口
10.配置访问账号密码和权限
## 其他安装方法
https://www.jianshu.com/p/51001ef01f28
