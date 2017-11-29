Apache服务器；

配置文件：/usr/local/httpd/conf/httpd.conf

根目录：/www

端口：80

开机启动：是

#### 安装Apr

```shell
$ cd apr-1.5.2
$ ./configure --prefix=/usr/local/apr
$ make && make install
```



#### 安装Apr-Util

```shell
$ cd ../apr-util-1.5.4
$ ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
$ make && make install
```



#### 安装Apache

```shell
$ cd ../httpd-2.4.27
$ ./configure --prefix=/usr/local/httpd --sysconfdir=/usr/local/httpd/conf --enable-so --enable-ssl --enable-cgi --enable-rewrite --with-zlib --with-pcre --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-modules=most --enable-mpms-shared=all --with-mpm=event
$ make && make install
```



#### 修改配置

```shell
$ cd /usr/local/httpd/conf
$ cp httpd.conf httpd.conf.bak
$ vi /usr/local/httpd/conf/httpd.conf

1.修改ServerName www.example.com:80为localhost:80

2.局域网访问：
Order allow,deny
Allow from all

3.根目录：
DocumentRoot "/www"

4.主文件顺序
DirectoryIndex index.php index.html

5.加载ttf字体
AddType application/font-sfnt            .otf .ttf
AddType application/font-woff            .woff
AddType application/font-woff2           .woff2
AddType application/vnd.ms-fontobject    .eot
phpAddType application/x-httpd-php .php

6.加载rewrite
(1)去除"#LoadModule rewrite_module modules/mod_rewrite.so"的注释; 
(2)修改"AllowOverride None"为"AllowOverride all",同时最好将Options也置为"all",否则可能会出问题。 
```



##### 配置系统服务脚本

```shell
$ vi /etc/rc.d/init.d/httpd		# 加入以下代码

#!/bin/bash  
#  
# httpd        Startup script for the Apache HTTP Server  
#  
# chkconfig: - 85 15  
# description: The Apache HTTP Server is an efficient and extensible  \  
#          server implementing the current HTTP standards.  
# processname: httpd  
# config: /usr/local/httpd/conf/httpd.conf  
# config: /etc/sysconfig/httpd  
# pidfile: /var/run/httpd/httpd.pid  
#  
### BEGIN INIT INFO  
# Provides: httpd  
# Required-Start: $local_fs $remote_fs $network $named  
# Required-Stop: $local_fs $remote_fs $network  
# Should-Start: distcache  
# Short-Description: start and stop Apache HTTP Server  
# Description: The Apache HTTP Server is an extensible server   
#  implementing the current HTTP standards.  
### END INIT INFO  
  
# Source function library.  
. /etc/rc.d/init.d/functions  
  
if [ -f /etc/sysconfig/httpd ]; then  
        . /etc/sysconfig/httpd  
fi  
  
# Start httpd in the C locale by default.  
HTTPD_LANG=${HTTPD_LANG-"C"}  
  
# This will prevent initlog from swallowing up a pass-phrase prompt if  
# mod_ssl needs a pass-phrase from the user.  
INITLOG_ARGS=""  
  
# Set HTTPD=/usr/sbin/httpd.worker in /etc/sysconfig/httpd to use a server  
# with the thread-based "worker" MPM; BE WARNED that some modules may not  
# work correctly with a thread-based MPM; notably PHP will refuse to start.  
  
# Path to the apachectl script, server binary, and short-form for messages.  
apachectl=/usr/local/httpd/bin/apachectl  
httpd=${HTTPD-/usr/local/httpd/bin/httpd}  
prog=httpd  
pidfile=${PIDFILE-/usr/local/httpd/logs/httpd.pid}  
lockfile=${LOCKFILE-/var/lock/subsys/httpd}  
RETVAL=0  
STOP_TIMEOUT=${STOP_TIMEOUT-10}  
  
# The semantics of these two functions differ from the way apachectl does  
# things -- attempting to start while running is a failure, and shutdown  
# when not running is also a failure.  So we just do it the way init scripts  
# are expected to behave here.  
start() {  
        echo -n $"Starting $prog: "  
        LANG=$HTTPD_LANG daemon --pidfile=${pidfile} $httpd $OPTIONS  
        RETVAL=$?  
        echo  
        [ $RETVAL = 0 ] && touch ${lockfile}  
        return $RETVAL  
}  
  
# When stopping httpd, a delay (of default 10 second) is required  
# before SIGKILLing the httpd parent; this gives enough time for the  
# httpd parent to SIGKILL any errant children.  
stop() {  
    echo -n $"Stopping $prog: "  
    killproc -p ${pidfile} -d ${STOP_TIMEOUT} $httpd  
    RETVAL=$?  
    echo  
    [ $RETVAL = 0 ] && rm -f ${lockfile} ${pidfile}  
}  
reload() {  
    echo -n $"Reloading $prog: "  
    if ! LANG=$HTTPD_LANG $httpd $OPTIONS -t >&/dev/null; then  
        RETVAL=6  
        echo $"not reloading due to configuration syntax error"  
        failure $"not reloading $httpd due to configuration syntax error"  
    else  
        # Force LSB behaviour from killproc  
        LSB=1 killproc -p ${pidfile} $httpd -HUP  
        RETVAL=$?  
        if [ $RETVAL -eq 7 ]; then  
            failure $"httpd shutdown"  
        fi  
    fi  
    echo  
}  
  
# See how we were called.  
case "$1" in  
  start)  
    start  
    ;;  
  stop)  
    stop  
    ;;  
  status)  
        status -p ${pidfile} $httpd  
    RETVAL=$?  
    ;;  
  restart)  
    stop  
    start  
    ;;  
  condrestart|try-restart)  
    if status -p ${pidfile} $httpd >&/dev/null; then  
        stop  
        start  
    fi  
    ;;  
  force-reload|reload)  
        reload  
    ;;  
  graceful|help|configtest|fullstatus)  
    $apachectl $@  
    RETVAL=$?  
    ;;  
  *)  
    echo $"Usage: $prog {start|stop|restart|condrestart|try-restart|force-reload|reload|status|fullstatus|graceful|help|configtest}"  
    RETVAL=2  
esac  
exit $RETVAL


$ chmod +x /etc/rc.d/init.d/httpd			# 为此脚本赋予执行权限
$ chkconfig --add httpd						# 加入服务列表
$ service httpd start						# 启动Apache服务
```



##### 设置开机启动

```shell
$ vi /etc/rc.local 		# 在最后一行添加：
$ service httpd start
```