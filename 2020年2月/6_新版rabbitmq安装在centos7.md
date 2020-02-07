# 
## 安装erlang
+ 主机安装erlang安装包：http://erlang.org/download/otp_src_22.0.tar.gz，
或者centos7上wget
+ 解压tar -zxvf otp_src_22.0.tar.gz
+ 安装构建Erlang语言的工具
yum install -y make gcc gcc-c++ m4 openssl openssl-devel ncurses-devel unixODBC unixODBC-devel java java-devel
+ 必要操作，具体路径根据实际： ./configure --prefix=/home/yanzhao/java_all/erlang/
+ 构建
make && make install
+ 配置环境
vim /etc/profile
```
export PATH=$PATH:/home/yanzhao/java_all/erlang/bin
```
+ 重载环境变量配置
source /etc/profile
+ 检查是否安装
erl
## 安装RabbitMQ
+ 下载安装包：https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.16/rabbitmq-server-generic-unix-3.7.16.tar.xz
> 最好改vpn
+ tar -xvf rabbitmq-server-generic-unix-3.7.16.tar.xz
+ vim /etc/profile
```
export PATH=$PATH:/home/yanzhao/java_all/rabbitmq/sbin
```
+ 重载环境变量配置
source /etc/profile
+ 启动rabbitmq，-detached代表后台守护进程方式启动。
rabbitmq-server -detached
+ 检查是否启动成功
rabbitmqctl status
+ 配置防火墙：linux 端口 15672 网页管理 5672 AMQP端口：

firewall-cmd --permanent --add-port=15672/tcp

firewall-cmd --permanent --add-port=5672/tcp

systemctl restart firewalld.service
> 防火墙开启service firewalld start
> 重启service firewalld restart
> 关闭service firewalld stop
+ 通过命令开启web管理插件：rabbitmq-plugins enable rabbitmq_management,
停止：abbitmqctl stop，再开：rabbitmq-server -detached
> （删除）重启：service rabbitmq-server restart
> （删除）+ 如果失败，出现：ERROR: node with name "rabbit" already running on，可以通过：ps -ef|grep rabbitmq，找出后台文件再kill
+ 输入服务器IP:15672 就可以看到RabbitMQ的WEB管理页面
> 阿里云记得设置安全组，开放端口
+ 设置开机自启动
如通过yum安装的话直接通过chkconfig rabbitmq-server on 就可以设置为开机自启动，但该方法不行，需在/etc/init.d目录下新建一个rabbitmq-server文件，vim /etc/init.d/rabbitmq-server内容如下：
```
#!/bin/sh
#
# rabbitmq-server RabbitMQ broker
#
# chkconfig: - 80 05
# description: Enable AMQP service provided by RabbitMQ
#
 
### BEGIN INIT INFO
# Provides:          rabbitmq-server
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Description:       RabbitMQ broker
# Short-Description: Enable AMQP service provided by RabbitMQ broker
### END INIT INFO
 
# Source function library.
. /etc/init.d/functions
export HOME=/root
PATH=/sbin:/usr/sbin:/bin:/usr/bin
NAME=rabbitmq-server
#DAEMON=/usr/sbin/${NAME}
#CONTROL=/usr/sbin/rabbitmqctl
DAEMON=/home/yanzhao/java_all/rabbitmq/rabbitmq_server-3.7.16/sbin/${NAME}
CONTROL=/home/yanzhao/java_all/rabbitmq/rabbitmq_server-3.7.16/sbin/rabbitmqctl
DESC=rabbitmq-server
USER=root
ROTATE_SUFFIX=
INIT_LOG_DIR=/var/log/rabbitmq
PID_FILE=/var/run/rabbitmq/pid
 
START_PROG="daemon"
LOCK_FILE=/var/lock/subsys/$NAME
 
test -x $DAEMON || exit 0
test -x $CONTROL || exit 0
 
RETVAL=0
set -e
 
[ -f /etc/default/${NAME} ] && . /etc/default/${NAME}
 
ensure_pid_dir () {
    PID_DIR=`dirname ${PID_FILE}`
    if [ ! -d ${PID_DIR} ] ; then
        mkdir -p ${PID_DIR}
        chown -R ${USER}:${USER} ${PID_DIR}
        chmod 755 ${PID_DIR}
    fi
}
 
remove_pid () {
    rm -f ${PID_FILE}
    rmdir `dirname ${PID_FILE}` || :
}
 
start_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        echo RabbitMQ is currently running
    else
        RETVAL=0
        ensure_pid_dir
        set +e
        RABBITMQ_PID_FILE=$PID_FILE $START_PROG $DAEMON \
            > "${INIT_LOG_DIR}/startup_log" \
            2> "${INIT_LOG_DIR}/startup_err" \
            0<&- &
        $CONTROL wait $PID_FILE >/dev/null 2>&1
        RETVAL=$?
        set -e
        case "$RETVAL" in
            0)
                echo SUCCESS
                if [ -n "$LOCK_FILE" ] ; then
                    touch $LOCK_FILE
                fi
                ;;
            *)
                remove_pid
                echo FAILED - check ${INIT_LOG_DIR}/startup_\{log, _err\}
                RETVAL=1
                ;;
        esac
    fi
}
 
stop_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        set +e
        $CONTROL stop ${PID_FILE} > ${INIT_LOG_DIR}/shutdown_log 2> ${INIT_LOG_DIR}/shutdown_err
        RETVAL=$?
        set -e
        if [ $RETVAL = 0 ] ; then
            remove_pid
            if [ -n "$LOCK_FILE" ] ; then
                rm -f $LOCK_FILE
            fi
        else
            echo FAILED - check ${INIT_LOG_DIR}/shutdown_log, _err
        fi
    else
        echo RabbitMQ is not running
        RETVAL=0
    fi
}
 
status_rabbitmq() {
    set +e
    if [ "$1" != "quiet" ] ; then
        $CONTROL status 2>&1
    else
        $CONTROL status > /dev/null 2>&1
    fi
    if [ $? != 0 ] ; then
        RETVAL=3
    fi
    set -e
}
 
rotate_logs_rabbitmq() {
    set +e
    $CONTROL rotate_logs ${ROTATE_SUFFIX}
    if [ $? != 0 ] ; then
        RETVAL=1
    fi
    set -e
}
 
restart_running_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        restart_rabbitmq
    else
        echo RabbitMQ is not runnning
        RETVAL=0
    fi
}
 
restart_rabbitmq() {
    stop_rabbitmq
    start_rabbitmq
}
 
case "$1" in
    start)
        echo -n "Starting $DESC: "
        start_rabbitmq
        echo "$NAME."
        ;;
    stop)
        echo -n "Stopping $DESC: "
        stop_rabbitmq
        echo "$NAME."
        ;;
    status)
        status_rabbitmq
        ;;
    rotate-logs)
        echo -n "Rotating log files for $DESC: "
        rotate_logs_rabbitmq
        ;;
    force-reload|reload|restart)
        echo -n "Restarting $DESC: "
        restart_rabbitmq
        echo "$NAME."
        ;;
    try-restart)
        echo -n "Restarting $DESC: "
        restart_running_rabbitmq
        echo "$NAME."
        ;;
    *)
        echo "Usage: $0 {start|stop|status|rotate-logs|restart|condrestart|try-restart|reload|force-reload}" >&2
        RETVAL=1
        ;;
esac
 
exit $RETVAL
```
> 注意根据实际更改DAEMON=/home/yanzhao/java_all/rabbitmq/rabbitmq_server-3.7.16/sbin/${NAME}
CONTROL=/home/yanzhao/java_all/rabbitmq/rabbitmq_server-3.7.16/sbin/rabbitmqctl
DESC=rabbitmq-server
USER=root

保存上面的文件并设置执行权限：chmod u+x /etc/init.d/rabbitmq-server。


配置为开机自启动服务：ln -s $(which erl) /usr/bin/erl && chkconfig rabbitmq-server on
## 配置访问账号密码和权限
+ 添加用户，后面两个参数分别是用户名和密码，我这都用root了。
rabbitmqctl add_user root root
+ 添加权限
rabbitmqctl set_permissions -p / root ".*" ".*" ".*"
+ 修改用户角色
rabbitmqctl set_user_tags root administrator
## 其他安装方法
https://www.jianshu.com/p/51001ef01f28
