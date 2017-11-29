[TOC]

## Centos6.5安装php开发环境

### 一、准备篇

#### 1、下载安装包

```shell
LINUX
Apache2.4.25：
最新地址：http://httpd.apache.org/download.cgi
下载地址：http://apache.fayea.com/httpd/httpd-2.4.25.tar.gz

Mysql5.7.17：
最新地址：https://dev.mysql.com/downloads/mysql/
下载地址：https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.17-linux-glibc2.5-x86_64.tar.gz

php7.1：
最新地址：http://php.net/downloads.php
下载地址：http://cn2.php.net/distributions/php-7.1.2.tar.gz

phpMyAdmin4.7.0：
最新地址：https://www.phpmyadmin.net/
下载地址：https://files.phpmyadmin.net/phpMyAdmin/4.7.0-rc1/phpMyAdmin-4.7.0-rc1-all-languages.zip

Git
最新地址：https://github.com/git/git
下载链接：https://github.com/git/git/archive/master.zip
```

#### 2、安装依赖包

```shell
# yum groupinstall "Development tools"
# yum install -y make cmake gcc gcc-c++ autoconf automake libpng-devel libjpeg-devel zlib libxml2-devel ncurses-devel bison libtool-ltdl-devel libiconv libmcrypt mhash mcrypt pcre-devel libaio pcre python python-devel openssl-devel kernel-devel freetype-devel libcurl-devel libxslt-devel wget unzip lrzsz vsftpd curl curl-devel perl cpio expat-devel gettext-devel  zlib-devel perl-ExtUtils-MakeMaker tcl
```
#### 3、删除自带apache

```shell
# rpm -qa httpd
# rpm -qa | grep httpd
httpd-2.2.15-29.el6.centos.i686
# rpm -e --nodeps httpd-2.2.15-29.el6.centos.i686
```

#### 4、卸载自带的mysql

```shell
# rpm -qa mysql
# rpm -qa | grep mysql
mysql-libs-5.1.71-1.el6.i686
# rpm -e --nodeps mysql-libs-5.1.71-1.el6.i686
```

#### 5、开启80端口、3306端口

```shell
在22端口这条规则的下面加上
# vi /etc/sysconfig/iptables
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
# /etc/init.d/iptables restart 或者 # service iptables restart
```

#### 6、关闭SELINUX

```shell
# vi /etc/selinux/config
#SELINUX=enforcing #注释掉
#SELINUXTYPE=targeted #注释掉
SELINUX=disabled #增加
# shutdown -r now#重启系统
```

#### 7、解压压缩包

```shell
# cd /usr/local/soft
# for tar in *.tar.gz;  do tar xvf $tar; done
# for tar in *.tar.bz2; do tar xvf $tar; done
```

#### 9、安装vim并设置代码高亮

##### 1）下载vim

```shell
# yum -y install vim-*
```

##### 2）设置vim编程风格

```shell
# vim /etc/vimrc
set nu
set showmode
set autoindent
set ts=4
set mouse=a
set expandtab
syntax on

ln -sf /usr/bin/vim /bin/vi

//设置vi命令高亮

对于已保存的文件，可以使用下面的方法进行空格和TAB的替换：
TAB替换为空格：
:set ts=4
:set expandtab
:%retab!

空格替换为TAB：
:set ts=4
:set noexpandtab
:%retab!
加!是用于处理非空白字符之后的TAB，即所有的TAB，若不加!，则只处理行首的TAB。
```

#### 10、安装sshd服务配置secureCRT远程登录

```shell
# yum install openssh-server
# service sshd start
# service sshd status
```

#### 11、centos6.5修改主机名

```shell
# hostname	//显示当前主机名
# vi /etc/sysconfig/network
HOSTNAME=centos
# vi /etc/hosts
在第一行127.0.0.1 centos localhost localhost.localdomain localhost4 localhost4.localdomain4
# hostname centos
# exit
```

#### 12、CentOS6 配置FTP服务器

##### 2）开放21端口

```shell
# service iptables stop
# vi /etc/sysconfig/iptables
# 添加规则允许21端口通行
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
# 防火墙配置开放:
-A INPUT -p tcp --dport 30000:30100 -j ACCEPT
# service iptables restart
```

##### 3）编辑vsftpd.config文件

```shell
# 备份 etc/vsftpd/vsftpd.conf
# cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
# vi /etc/vsftpd/vsftpd.conf
anonymous_enable=NO		//设定不允许匿名用户访问。
# 为了让FLASHFXP之类的软件更好的连接服务器，得让VSFTPD支持被动模式才行,上面已经开通相应端口防火墙，在最后加入
pasv_enable=YES
pasv_max_port=30100
pasv_min_port=30000
(上面的30000--30100端口号可以是其它的,在此举例)

# 指定此用户的网站文件夹
# adduser -d /www -g ftp -s /bin/bash sunrise
//-s /bin/bash是让其能登陆系统，-d 是指定用户目录为/www ，即该账户既能登陆ftp，又能用做登陆系统用。-g 是指定为FTP用户组
# passwd sunrise
# vi /etc/sudoers
添加sunrise ALL=(ALL) ALL

Changing password for user beinan.//接下来会出现让你设置新的密码
有必要的话  设置www目录权限
修改根目录属性：
# chmod -R 777 /www
//递归地给此目录下所有文件和子目录的读、写、执行权限
# chgrp -R ftp /www
//递归地把此目录及该目录下所有文件和子目录的组属性设置成ftp组
```

##### 4）限制用户目录，不得改变目录到上级

```shell
# vi /etc/vsftpd/vsftpd.conf 
将这两行
#chroot_list_enable=YES
#chroot_list_file=/etc/vsftpd/chroot_list
注释去掉

新增一个文件: 
# vi /etc/vsftpd/chroot_list 
内容写需要限制的用户名：
sunrise
重新启动vsftpd
# service vsftpd restart
```

##### 5）设置开机启动

```shell
# vi /etc/rc.local
# 在最后一行添加：
# service vsftpd start
```



### 二、安装篇：

#### 1、编译安装apache

原文：[http://jingyan.baidu.com/article/d5c4b52bec7d6bda560dc5fb.html](http://jingyan.baidu.com/article/d5c4b52bec7d6bda560dc5fb.html)

##### 1）安装apr

```shell
# cd apr-1.5.2
# ./configure --prefix=/usr/local/apr
# make && make install
```

##### 2）安装apr-util

```shell
# cd ../apr-util-1.5.4
# ./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
# make && make install
```

##### 3）安装apache

```shell
# cd ../httpd-2.4.25
# ./configure --prefix=/usr/local/httpd --sysconfdir=/usr/local/httpd/conf --enable-so --enable-ssl --enable-cgi --enable-rewrite --with-zlib --with-pcre --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util --enable-modules=most --enable-mpms-shared=all --with-mpm=event
# make && make install
```

##### 4）备份httpd.conf

```shell
# cd /usr/local/httpd/conf
# cp httpd.conf httpd.conf.bak
```

##### 5）修改httpd的主配置文件

```shell
# vi /usr/local/httpd/conf/httpd.conf
将ServerName www.example.com:80修改为localhost:80
局域网访问：
Order allow,deny
Allow from all
根目录：
DocumentRoot "/www"

DirectoryIndex index.php index.html

加载ttf字体
AddType application/font-sfnt            .otf .ttf
AddType application/font-woff            .woff
AddType application/font-woff2           .woff2
AddType application/vnd.ms-fontobject    .eot

加载rewrite

加载php
AddType application/x-httpd-php .php
```

##### 6）提供SysV服务脚本/etc/rc.d/init.d/httpd

```shell
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
```

##### 7）为此脚本赋予执行权限：

```shell
# chmod +x /etc/rc.d/init.d/httpd
```

##### 8）加入服务列表

```shell
# chkconfig --add httpd
```

##### 9）启动Apache服务

```shell
# service httpd start
# curl http://127.0.0.1
```

#### 2、编译安装mysql

原文：[http://lavasoft.blog.51cto.com/blog/62575/1733207](http://lavasoft.blog.51cto.com/blog/62575/1733207)

##### 1）解压缩到/usr/local/下面，mysql的主目录命名为mysql

```shell
# cd /usr/local/soft/
# tar -zvxf mysql-5.7.17-linux-glibc2.5-i686.tar.gz  /usr/local
# cd ../
# mv mysql-5.7.17-linux-glibc2.5-i686/ mysql
# cd mysql
```

##### 2）在mysql下面创建data数据库文件目录

```shell
# mkdir data
```

##### 3）创建mysql的用户组和用户，并对mysql目录设置用户组和用户

```shell
# groupadd mysql
# useradd mysql -g mysql
# chown -R mysql .
# chgrp -R mysql .
```

##### 4）初始化mysql并启动mysql服务

```shell
# cd /usr/local/mysql/bin
# ./mysql_install_db --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data   //指定mysql基本目录和数据目录文件夹
2016-01-09 12:00:28 [WARNING] mysql_install_db is deprecated. Please consider switching to mysqld --initialize
2016-01-09 12:00:33 [WARNING] The bootstrap log isn't empty:
2016-01-09 12:00:33 [WARNING] 2016-01-09T04:00:29.262989Z 0 [Warning] --bootstrap is deprecated. Please consider using --initialize instead
2016-01-09T04:00:29.264643Z 0 [Warning] Changed limits: max_open_files: 1024 (requested 5000)
2016-01-09T04:00:29.264653Z 0 [Warning] Changed limits: table_open_cache: 431 (requested 2000)
```

##### 5）启动mysql服务

```shell
# cd /usr/local/mysql/support-files/
# ./mysql.server start
Starting MySQL. SUCCESS! 
```

##### 6）将mysql服务添加至系统服务中

```shell
# cp -v /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
# chkconfig --add mysql
# service mysql {start|stop|restart|reload|force-reload|status}
```

##### 7）将mysql命令加入linux环境变量

```shell
# ln -s /usr/local/mysql/bin/mysql /bin
```

##### 8）查看root随机密码

```shell
# cat /root/.mysql_secret
# Password set for user 'root@localhost' at 2016-01-09 12:00:28 
:5ul#H6dmcwX
```

##### 9）登录mysql

```shell
# mysql -h localhost -u root -p
或者
# cd /usr/local/mysql/bin
# ./mysql -u root -p
```

##### 10）改mysql的root用户密码为root

```shell
mysql> SET PASSWORD=PASSWORD('root');
```

##### 11）设定远程登录mysql

```mysql
mysql> USE mysql;
mysql> SELECT Host,User FROM user;
mysql> GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
```

##### 12）设定默认utf8字符集
```shell
# cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
# vi /etc/my.cnf
修改
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
port = 3306
character-set-server = utf8
```

##### 13）解决navicat for mysql插入数据后显示错误
```shell
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```

#### 3、编译安装php

##### 1）编译安装php

```shell
# tar xf php-7.1.10.tar.gz
# cd php-7.1.10

Orgin:
# ./configure --prefix=/usr/local/php --with-libxml-dir=/usr/include/libxml2 --with-mysqli --with-zlib-dir=/usr/src/kernels/2.6.32-431.el6.i686/include/config/zlib --with-apxs2=/usr/local/httpd/bin/apxs --with-config-file-path=/usr/local/php

New:
./configure --prefix=/usr/local/php \
    --with-apxs2=/usr/local/httpd/bin/apxs \
    --with-config-file-path=/usr/local/php \
    --with-curl \
    --with-freetype-dir \
    --with-gd \
    --with-gettext \
    --with-iconv-dir \
    --with-kerberos \
    --with-libdir=lib64 \
    --with-libxml-dir \
    --with-mysqli \
    --with-openssl\
    --with-pcre-regex \
    --with-pdo-mysql \
    --with-pdo-sqlite \
    --with-pear \
    --with-png-dir \
    --with-xmlrpc \
    --with-xsl \
    --with-zlib \
    --enable-fpm \
    --enable-bcmath \
    --enable-libxml \
    --enable-inline-optimization \
    --enable-gd-native-ttf \
    --enable-mbregex \
    --enable-mbstring \
    --enable-opcache \
    --enable-pcntl \
    --enable-shmop \
    --enable-soap \
    --enable-sockets \
    --enable-sysvsem \
    --enable-xml \
    --enable-zip

# make && make install
# cp /usr/local/soft/php-7.1.1/php.ini-development /usr/local/php/php.ini
##将安装目录下的php.ini-production复制到/usr/local/php/conf下作为配置文件
```

##### 2）备份php.ini

```shell
# cd /usr/local/php
# cp php.ini php.ini.bak
```

##### 3）配置Apache，使其和Php结合

```shell
# vi /usr/local/httpd/conf/httpd.conf
#添加index.php网页为默认访问页：
DirectoryIndex index.php index.html
#使apache与扩展名为.php的文件类型相关联
在/usr/local/httpd/conf/httpd.conf文件中添加一句：
AddType application/x-httpd-php .php
```

##### 4）将php命令添加至环境变量
```shell
# export PATH=$PATH:/usr/local/php/bin
```

##### 5）重启Apache服务，并添加php和mysql测试网页

```php
# service httpd restart
# cd /www
# vi index.php
#写如下代码
$link = mysqli_connect('localhost', 'root', 'root');
if ($link == true) {
    echo 'ok';
} else {
    echo 'no';
}
phpinfo();
```



#### 4、安装GIT

##### 2）卸载Centos自带的git1.7.1

```shell
# rpm -qa git
git-1.7.1-4.el6_7.1.i686
# rpm -e --nodeps git-1.7.1-4.el6_7.1.i686
或者
# yum remove git
```

##### 3）安装git并将git添加到环境变量中

```shell
# cd /usr/local/soft
# wget https://github.com/git/git/archive/v2.12.0-rc1.tar.gz
# tar -xzvf git-2.1.2.tar.gz
# cd git-2.1.2
# make prefix=/usr/local/git all
# make prefix=/usr/local/git install
# echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
# source /etc/bashrc
```

##### 4）查看版本号

```shell
# git --version
git version 2.1.2
```



#### 5、安装redis和redis拓展

```shell
php拓展安装教程： http://www.cnblogs.com/myright/articles/5408276.html
php全部拓展下载： http://pecl.php.net/package-stats.php
redis下载地址：   https://github.com/MSOpenTech/redis
php7拓展下载地址：http://pecl.php.net/package/redis/3.1.1/windows
```

原文：[http://blog.csdn.net/ludonqin/article/details/47211109](http://blog.csdn.net/ludonqin/article/details/47211109)

##### 1）安装redis依赖文件

```shell
# yum install tcl
```

##### 2）下载redis安装包并解压

```shell
# tar -xzvf redis-4.0-rc3.tar.gz
# cd redis-4.0-rc3
# make && make install
```

##### 3）成功后/usr/local/bin下已有这些文件

```shell
redis-benchmark  redis-check-rdb  redis-sentinel
redis-check-aof  redis-cli        redis-server
# redis-server –v (查看版本命令)
```

##### 4）修改配置文件redis.conf

创建配置文件目录，dump file 目录，进程pid目录，log目录等

```shell
# cd /usr/local/
# mkdir redis
# cd redis
# mkdir conf data log run
# cp /usr/local/soft/redis/redis.conf /usr/local/redis/conf/redis_6379.conf
# cd /usr/local/redis/conf
# cp redis_6379.conf redis_6379.conf.bak
```

##### 5）修改Redis配置

```shell
# vi redis_6379.conf
pidfile /usr/local/redis/run/redis_6379.pid	//pid目录
dir /usr/local/redis/data	        //dump目录
logfile /usr/local/redis/log/redis.log	//log存储目录
daemonize no改为yes	//修改配置文件使得redis在background运行
持久化	//默认rdb，可选择是否开启aof，若开启，修改配置文件appendonly
```

##### 6）启动redis，查看各目录下文件

```shell
# redis-server /usr/local/redis/conf/redis_6379.conf
```

##### 7）客户端连接redis

```shell
# redis-cli
```

##### 8）服务及开机自启动

复制启动脚本
```shell
# cp /usr/local/soft/redis/utils/redis_init_script /etc/init.d/redis
# cd /etc/init.d
# vi /etc/init.d/redis
PIDFILE=/usr/local/redis/run/redis_${REDISPORT}.pid
CONF="/usr/local/redis/conf/redis_${REDISPORT}.conf"
```

```shell
// 给reids服务设置优先级（在开始位置添加）
#!/bin/sh
#
# added by wt on 20170312
# chkconfig: 2345 90 10
#
# description: Redis is a persistent key-value database
# added by wt on 20170312



# chmod +x /etc/init.d/redis	//给启动脚本添加权限
# chmod –x /etc/init.d/redis	//删除权限

//添加开机启动服务
# chkconfig redis on
```

##### 9）启动、关闭redis

```shell
# service redis start
# service redis stop
```

##### 10) 安装php拓展

[http://www.cnblogs.com/GaZeon/p/5422078.html](http://www.cnblogs.com/GaZeon/p/5422078.html)

```shell
# wget https://codeload.github.com/phpredis/phpredis/zip/php7
# unzip phpredis-php7.zip
# cd phpredis-php7
# /usr/local/php/bin/phpize
# ./configure --with-php-config=/usr/local/php/bin/php-config
# make && make install
```
##### 11） 将extension=redis.so加入到php.ini
```shell
# vim /usr/local/php/php.ini
extension=redis.so
# service httpd restart
```

#####  12) 测试redis是否成功
```php
<?php
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$redis->set('name', 'wt');
echo $redis->get('name');
```

### Centos6.5_64安装memcached服务端和php_memcacahed拓展

#### 1.下载

```shell
# wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz

# wget http://www.memcached.org/files/memcached-1.4.36.tar.gz
# wget https://github.com/websupport-sk/pecl-memcache/archive/php7.zip
```

#### 2.安装libevent

```shell
# tar -xzvf libevent-2.1.8-stable.tar.gz
# cd libevent-2.1.8-stable
# ./configure --prefix=/usr/local/libevent
# make && make install
```

#### 3.安装memcached

```shell
tar -xzvf memcached-1.4.36.tar.gz
cd memcached-1.4.36
# ./configure --prefix=/usr/local/memcached --with-libevent=/usr/local/libevent
# make && make install
```

#### 4.启动memcached

```
# /usr/local/memcached/bin/memcached -d -m 128 -l 127.0.0.1 -p 11211 -u root
(128为内存, 11211为端口,root为用户组)
```

#### 5.开机启动，编辑 /etc/rc.d/rc.local 文件，添加以下内容。

```
/usr/local/memcached/bin/memcached -d -m 128 -l 127.0.0.1 -p 11211 -u root
```

#### 6.查看是否启动成功

```shell
# ps aux|grep memcached
```

#### 7.php_memcache扩展安装

```shell
# unzip pecl-memcache-php7.zip
# cd pecl-memcache-php7
# /usr/local/php/bin/phpize
# ./configure --with-php-config=/usr/local/php/bin/php-config
# make && make install
```

#### 8.修改php.ini 加载Memcache组件

```
[memcache]
extension = memcache.so
```

#### 9.重启 httpd

```
# service httpd restart
```

#### 10.测试memcache是否可用

```php
$memcache   =   new Memcache();
$memcache->connect('127.0.0.1', 11211);
$memcache->set('name', 'hello memcache');
echo $memcache->get('name');die;
```

## Centos6.5安装mongodb
http://www.cnblogs.com/luyucheng/p/6085882.html

### 一、安装MongoDB

1.创建mongodb用户组和用户

```shell
$ groupadd mongodb
$ useradd -r -g mongodb -s /sbin/nologin -M mongodb
```

2.下载mongodb源码包
下载页面：[https://www.mongodb.com/download-center?jmp=nav](https://www.mongodb.com/download-center?jmp=nav)
这里用的是 mongodb-linux-x86_64-rhel62-3.2.10.tgz
下载地址：[https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-3.2.10.tgz](https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-3.2.10.tgz)

 4.创建mongodb文件目录

```shell
$ mkdir -p /usr/local/mongodb/data
$ mkdir -p /usr/local/mongodb/conf
$ mkdir -p /usr/local/mongodb/run
$ mkdir -p /usr/local/mongodb/log
```

6.将文件复制到mongodb/目录

```shell
$ mv mongodb-linux-i686-2.4.9 /usr/local/mongodb
```

 7.创建mongodb配置文件mongodb.conf

```
$ cd /usr/local/mongodb/conf
$ vim mongodb.conf
```

8.添加下面内容，保存退出

```shell
dbpath=/usr/local/mongodb/data #数据目录存在位置
logpath=/usr/local/mongodb/log/mongodb.log #日志文件存放目录
logappend=true #写日志的模式:设置为true为追加
fork=true  #以守护程序的方式启用，即在后台运行
verbose=true
vvvv=true #启动verbose冗长信息，它的级别有 vv~vvvvv，v越多级别越高，在日志文件中记录的信息越详细
maxConns=20000 #默认值：取决于系统（即的ulimit和文件描述符）限制。MongoDB中不会限制其自身的连接
pidfilepath=/usr/local/mongodb/run/mongodb.pid
directoryperdb=true #数据目录存储模式,如果直接修改原来的数据会不见了
profile=0 #数据库分析等级设置,0 关 2 开。包括所有操作。 1 开。仅包括慢操作
slowms=200 #记录profile分析的慢查询的时间，默认是100毫秒
quiet=true
syncdelay=60 #刷写数据到日志的频率，通过fsync操作数据。默认60秒
#port=27017  #端口
#bind_ip = 10.1.146.163 #IP
#auth=true  #开始认证
#nohttpinterface=false #28017 端口开启的服务。默认false，支持
#notablescan=false#不禁止表扫描操作
#cpu=true #设置为true会强制mongodb每4s报告cpu利用率和io等待，把日志信息写到标准输出或日志文件
```

9.修改mongodb目录权限

```shell
$ chown -R mongodb:mongodb /usr/local/mongodb
$ chown -R mongodb:mongodb usr/local/mongodb/run
$ chown -R mongodb:mongodb usr/local/mongodb/log
```

10.将mongodb命令加入环境变量，修改profile文件

```shell
$ echo "export PATH=$PATH:/usr/local/mongodb/bin" >> /etc/profile
$ source /etc/profile
```

13.将mongodb服务脚本加入到init.d/目录，创建mongod文件

```
vim /etc/init.d/mongodb
```

14.加入下面内容，保存退出

```shell
#!/bin/sh  
# chkconfig: 2345 93 18
# description:MongoDB  

#默认参数设置
#mongodb 家目录
MONGODB_HOME=/usr/local/mongodb

#mongodb 启动命令
MONGODB_BIN=$MONGODB_HOME/bin/mongod

#mongodb 配置文件
MONGODB_CONF=$MONGODB_HOME/conf/mongodb.conf

MONGODB_PID=$MONGODB_HOME/run/mongodb.pid

#最大文件打开数量限制
SYSTEM_MAXFD=65535

#mongodb 名字  
MONGODB_NAME="mongodb"
. /etc/rc.d/init.d/functions

if [ ! -f $MONGODB_BIN ]
then
    echo "$MONGODB_NAME startup: $MONGODB_BIN not exists! "  
    exit
fi

start(){
    ulimit -HSn $SYSTEM_MAXFD
    $MONGODB_BIN --config="$MONGODB_CONF"  --journal
    ret=$?
    if [ $ret -eq 0 ]; then
        action $"Starting $MONGODB_NAME: " /bin/true
    else
        action $"Starting $MONGODB_NAME: " /bin/false
    fi
}

stop(){
    PID=$(ps aux |grep "$MONGODB_NAME" |grep "$MONGODB_CONF" |grep -v grep |wc -l) 
    if [[ $PID -eq 0  ]];then
        action $"Stopping $MONGODB_NAME: " /bin/false
        exit
    fi
    kill -HUP `cat $MONGODB_PID`
    ret=$?
    if [ $ret -eq 0 ]; then
        action $"Stopping $MONGODB_NAME: " /bin/true
        rm -f $MONGODB_PID
    else   
        action $"Stopping $MONGODB_NAME: " /bin/false
    fi
}

restart(){
    stop
    sleep 2
    start
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    status)
    status $prog
        ;;
    restart)
        restart
        ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart}"
esac
```

15.为mongod添加可执行权限

```
$ chmod +x /etc/init.d/mongodb
```

16.将mongodb加入系统服务

```
chkconfig --add mongodb
```

17.修改服务的默认启动等级

```
chkconfig mongodb on
```

18.启动mongodb

```
service mongodb start
```

### 二、PHP7安装MongoDB拓展

1.下载php7 mongodb拓展包，并将源码包放到/usr/local/src/目录下

下载页面：[http://pecl.php.net/package/mongodb](http://pecl.php.net/package/mongodb)
这里用的是 mongodb-1.1.9.tgz
下载地址：[http://pecl.php.net/get/mongodb-1.1.9.tgz](http://pecl.php.net/get/mongodb-1.1.9.tgz)

3.解压拓展包

```shell
$ cd /usr/local/soft
$ tar -zxf mongodb-1.1.9.tgz
```

4.进入mongodb拓展目录，编译安装拓展

```
$ cd mongodb-1.1.9/
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```

5.修改php.ini文件

```
vim /usr/local/php/php.ini
```

6.添加mongodb.so扩展配置，保存退出

```
extension=mongodb.so
```

7.重启Apache或php-fpm

```
$ service httpd restart
```

8.测试是否安装成功

```php
<?php
$manager = new MongoDB\Driver\Manager("mongodb://127.0.0.1:27017");
$bulk = new MongoDB\Driver\BulkWrite;
$bulk->insert(['x' => 1, 'class'=>'toefl', 'num' => '18']);
$bulk->insert(['x' => 2, 'class'=>'ielts', 'num' => '26']);
$bulk->insert(['x' => 3, 'class'=>'sat', 'num' => '35']);
$manager->executeBulkWrite('test.log', $bulk);
$filter = ['x' => ['$gt' => 1]];
$options = [
    'projection' => ['_id' => 0],
    'sort' => ['x' => -1],
];
$query = new MongoDB\Driver\Query($filter, $options);
$cursor = $manager->executeQuery('test.log', $query);
foreach ($cursor as $document) {
    print_r($document);
}
```

## 安装composer

```shell
//下载composer
wget https://dl.laravel-china.org/composer.phar -O /usr/local/bin/composer

// 添加权限
chmod a+x /usr/local/bin/composer

//切换国内源
composer config -g repo.packagist composer https://packagist.laravel-china.org
```

## 安装mkdocs

## 一.安装python3.6.1

```shell
$ wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz
$ tar vxf Python-3.6.1.tgz
$ cd Python-3.6.1
$ ./configure --prefix=/usr/local/python3   #编译，自定义安装目录
$  make && make install  #过程很慢
$ ln -s /usr/local/python3/bin/python3 /usr/bin/  #建立软连接
$ python3 --version
$ pip --version #检查是否安装了pip
$ pip install --upgrade pip #更新pip
```

## 二、安装pip（安装之前先检查是否安装了）

```shell
$ wget https://pypi.python.org/packages/11/b6/abcb525026a4be042b486df43905d6893fb04f05aac21c32c638e939e447/pip-9.0.1.tar.gz#md5=35f01da33009719497f01a4ba69d63c9  #pip下载
$ tar zxf pip-9.0.1.tar.gz 
$ cd pip-9.0.1
$ python3 setup.py install
```

## 三、安装mkdocs

```shell
$ pip install mkdocs
$ ln -s /usr/local/python3/bin/mkdocs /usr/bin
$ mkdocs --version
```

## 四、创建项目

```shell
$ mkdocs new documents
$ cd documents && mkdocs serve #开启内置服务器
$ vi mkdocs.yml
$ site_name: Sunrise	#修改项目标题
$ theme: readthedocs	#修改文档模板
$ pages: {				#添加页面
  - Home: index.md
  - About: about.md
}
$ mkdocs build	#生成html文档，html文件存储在site目录中
$ mkdocs build --clean	#去掉不需要的页面
```