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
$ ./configure --prefix=/usr/local/httpd --sysconfdir=/etc/httpd --enable-so --enable-ssl --enable-cgi --enable-rewrite --with-zlib --with-pcre --with-included-apr --enable-modules=most --enable-mpms-shared=all --with-mpm=event
$ make -j 4 && make install
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
AddType application/font-sfnt .otf .ttf
AddType application/font-woff .woff
AddType application/font-woff2 .woff2
AddType application/vnd.ms-fontobject .eot
phpAddType application/x-httpd-php .php

6.加载rewrite
(1)去除"#LoadModule rewrite_module modules/mod_rewrite.so"的注释; 
(2)修改"AllowOverride None"为"AllowOverride all",同时最好将Options也置为"all",否则可能会出问题。 
```



##### 配置系统服务脚本

```shell

$ cp /usr/local/httpd/bin/apachectl /etc/init.d/httpd
$ vi /etc/rc.d/init.d/httpd		# 加入以下代码

#!/bin/bash  
#  
# chkconfig: 2345 85 15  
# description: The Apache HTTP Server is an efficient and extensible  \  


$ chmod +x /etc/rc.d/init.d/httpd			# 为此脚本赋予执行权限
$ chkconfig --add httpd						# 加入服务列表
$ service httpd start						# 启动Apache服务
```



##### 设置开机启动

```shell
$ vi /etc/rc.local 		# 在最后一行添加：
$ service httpd start
```
