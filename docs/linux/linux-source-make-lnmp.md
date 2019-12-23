# Ubuntu16.04安装lnmp

### 更换Ubuntu阿里云源

```shell
$ sudo cp /etc/apt/sources.list /etc/apt/source.list.bak
$ sudo vim /etc/apt/sources.list
```

```shell
:%d  // 清除所有内容
:%s/archive.ubuntu.com/mirrors.aliyun.com/g  // 替换所有的ubuntu链接为阿里云的链接
```

```tex
deb http://mirrors.aliyun.com/ubuntu xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu xenial main restricted
deb http://mirrors.aliyun.com/ubuntu xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu xenial-updates main restricted
deb http://mirrors.aliyun.com/ubuntu xenial universe
deb-src http://mirrors.aliyun.com/ubuntu xenial universe
deb http://mirrors.aliyun.com/ubuntu xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu xenial multiverse
deb-src http://mirrors.aliyun.com/ubuntu xenial multiverse
deb http://mirrors.aliyun.com/ubuntu xenial-updates multiverse
deb-src http://mirrors.aliyun.com/ubuntu xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu xenial-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu xenial-security main restricted
deb http://mirrors.aliyun.com/ubuntu xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu xenial-security universe
deb http://mirrors.aliyun.com/ubuntu xenial-security multiverse
deb-src http://mirrors.aliyun.com/ubuntu xenial-security multiverse
```

### 更新源

```shell
$ sudo apt-get update
$ sudo apt-get upgrade
```

### 下载依赖包（二选一）
```shell
# 简略版本
$ sudo apt-get -y install libpcre3 libpcre3-dev openssl libssl-dev

# 完整版本
$ sudo apt-get install gcc automake autoconf make build-essential libtool libpcre3 libpcre3-dev zlib1g-dev openssl
```

### 下载源码包

```shell
$ cd ~ && mkdir src && cd src
$ wget http://mirrors.sohu.com/php/php-7.2.25.tar.gz
$ wget http://nginx.org/download/nginx-1.16.1.tar.gz
```

### nginx 编译安装

```shell
$ tar -xzvf nginx-1.16.1.tar.gz
$ cd nginx-1.16.1

$ ./configure \
--prefix=/usr/local/nginx \
--conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--lock-path=/var/lock/nginx/nginx.lock \
--pid-path=/var/run/nginx/nginx.pid \
--sbin-path=/usr/local/nginx/sbin/nginx \
--user=www \
--group=www \
--with-debug \
--with-file-aio \
--with-google_perftools_module \
--with-http_memcached_module \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-http_gzip_static_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_image_filter_module \
--with-http_geoip_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_auth_request_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_degradation_module \
--with-http_slice_module \
--with-mail \
--with-mail_ssl_module \
--with-openssl \
--with-pcre \
--with-pcre-jit \
--with-stream \
--with-stream_ssl_module \
--with-stream_realip_module \
--with-stream_geoip_module \
--with-stream_geoip_module=dynamic \
--with-stream_ssl_preread_module \
--with-threads

$ sudo make && sudo make install
```
### 创建目录并切换用户

```shell
$ sudo mkdir /var/lock/nginx && sudo chown -R www:www /var/lock/nginx
$ sudo chown -R www:www /usr/local/nginx
$ sudo chown -R www:www /etc/nginx
$ sudo chown -R www:www /var/log/nginx
```

### 配置环境变量

```shell
$ sudo touch /etc/profile.d/nginx.sh
$ echo 'export PATH=$PATH:/usr/local/nginx/sbin' > /etc/profile.d/nginx.sh
$ sudo chmod 0777 /etc/profile.d/nginx.sh
$ source /etc/profile.d/nginx.sh
```

### 测试环境变量
```shell
$ nginx -h
```

### 原生命令启动、关闭、重启命令（推荐第二种启动方式）
```shell
# 验证nginx配置是否正确
$ nginx -t

# 启动nginx
$ nginx -c /etc/nginx/nginx.conf

# 关闭nginx（nginx在退出前完成已经接受的连接请求）
$ nginx -s quit

# 关闭nginx（强制关闭）
$ nginix -s stop

# 日志分割(重新打开日志文件)
$ nginx -s reopen

# 重启nginx
$ nginx -s reload

# 配置开机启动
$ vim /etc/rc.local

------------ 写入下面命令 ———————————————
nginx -c /usr/local/nginx/conf/nginx.conf

```

### 编写快捷启动脚本

```shell
$ sudo vim /lib/systemd/system/nginx.service

# --------------------------------------------------------------------------
# 输入以下代码
# --------------------------------------------------------------------------
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
PIDFile=/var/run/nginx/nginx.pid
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

### 设置服务

```shell
# 开机启动
$ sudo systemctl enable nginx.service

# 启动服务
$ sudo systemctl start nginx.service

# 停止服务
$ sudo systemctl stop nginx.service

# 程序状态
$ sudo systemctl status nginx.service

# 重启服务
$ sudo systemctl restart nginx.service

# 杀死进程
$ pkill -9 nginx
```

### 查看nginx进程和端口

```shell
$ ps -ef | grep nginx
$ netstat -lntp
```

### 配置NGINX
```shell
$ cd /etc/nginx
$ sudo vim nginx.conf

# --------------------------------------------------------------------------
# 修改如下代码
# --------------------------------------------------------------------------
1. #user nobody; => #user www www;  #去除前面的#号 并将nobody改为 www www
2. pid logs/nginx.pid;              #去除前面的#号
3. #去除前面的#号
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
4. gzip  on;                        #去除前面的#号
5. server_tokens off;               #在gzip on下方加下一行(隐藏版本)
6. 将index.php设为默认页面
index  index.html index.htm index.php;
7. 使WEB环境支持PHP
location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    #fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
8. 在http{} 节点末尾添加 “include sites-enabled/*.conf;” 使其方便添加虚拟多网站配制
```

### 配制多网站(虚拟主机)
```shell
$ sudo mkdir /etc/nginx/sites-available
$ mkdir /etc/nginx/sites-enabled
$ mkdir /etc/nginx/ssl

#新增网站配制www.demo.com
$ sudo vim demo.conf
#并粘贴以下代码(以下仅为示列配制，根据自身要求做修改即可)
```

```tex
server {

    listen 80;
    listen [::]:80;

    listen 443 ssl;
    listen [::]:443 ssl;

    server_name demo.com www.demo.com;

	if ($scheme = http ) {
		return 301 https://$server_name$request_uri;
	}

    root /www/demo.com/www;
    index index.php index.html index.htm;

    ssl_certificate      /etc/nginx/ssl/demo.com/www/2797287_demo.com.pem;
    ssl_certificate_key  /etc/nginx/ssl/demo.com/www/2797287_demo.com.key;
    
    location / {
         try_files $uri $uri/ /index.php?$query_string;
    }

    #解析PHP代码
    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_buffers 16 16k;
        fastcgi_buffer_size 32k;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        #fixes timeouts
        fastcgi_read_timeout 600;
        include fastcgi_params;

    }

    #静态资源缓存10天
    location ~ .*\.(gif|jpg|jpeg|png|bmp|ico|swf|js|css|eof|ttf|ttc|otf|eof|woff|woff2|svg)$ {
        expires    10d;
        access_log  off;
        add_header Access-Control-Allow-Origin *;
    }

    error_log /var/log/nginx/demo.com/www_error.log;
    access_log /var/log/nginx/demo.com/www_access.log;
}
```

## 安装php依赖包

### Ubuntu

```shell
$ sudo apt install gcc make openssl libssl-dev curl libcurl4-openssl-dev libbz2-dev libxml2-dev libjpeg-dev libpng-dev libfreetype6-dev libzip-dev
```

### Centos
```shell
$ yum -y install libxml2-devel bzip2-devel libcurl-devel libjpeg-devel libpng-devel freetype-devel gmp-devel readline-devel libxslt-devel
```

### 编译php
```shell
$ ./configure \
--prefix=/usr/local/php/7.2 \
--with-config-file-path=/etc/php/7.2 \
--with-fpm-user=www \
--with-fpm-group=www \
--with-bz2 \
--with-cdb \
--with-curl \
--with-freetype-dir \
--with-gd \
--with-gettext \
--with-gmp \
--with-iconv \
--with-jpeg-dir \
--with-libxml-dir \
--with-mhash \
--with-mysqli \
--with-onig \
--with-openssl \
--with-openssl-dir \
--with-pcre-dir \
--with-pcre-regex \
--with-pdo-mysql \
--with-pdo-sqlite \
--with-pear \
--with-png-dir \
--with-readline \
--with-xmlrpc \
--with-xsl \
--with-zlib \
--with-zlib-dir \
--enable-bcmath \
--enable-calendar \
--enable-dom \ \
--enable-debug \
--enable-fpm \
--enable-ftp \
--enable-fileinfo \
--enable-filter \
--enable-exif \
--enable-gd-jis-conv \
--enable-inline-optimization \
--enable-json \
--enable-mbregex \
--enable-mbstring \
--enable-maintainer-zts \
--enable-mbregex-backtrack \
--enable-mysqlnd-compression-support \
--enable-opcache \
--enable-pcntl \
--enable-pdo \
--enable-rpath \
--enable-shared \
--enable-soap \
--enable-sysvmsg \
--enable-sysvsem \
--enable-sysvshm \
--enable-session \
--enable-shmop \
--enable-sockets \
--enable-simplexml \
--enable-wddx \
--enable-xml \
--enable-zip

```

### 安装php

```shell
$ sudo make -j4 && sudo make install
```

### 修改用户组

```shell
$ sudo mkdir /etc/php/7.2 && sudo chown -R www:www /etc/php/7.2
$ sudo chown -R www:www /usr/local/php/7.2
```

### 配置php.ini

```shell
$ cp ~/src/php7.2.25/php.production /etc/php/7.2/php.ini
$ vim /etc/php/7.2/php.ini


# 做以下修改（内存，时区，隐藏版本号，开启opcache缓存）
# --------------------------------------------------------------------------
1.让到memory_limit = 128M                              修改为：memory_limit = 1024M
2.找到：;date.timezone =                               修改为：date.timezone = PRC
3.找到：expose_php = On                                修改为：expose_php = Off
4.找到：;opcache.enable=1                              修改为：opcache.enable=1
5.找到：;opcache.enable_cli=0                          修改为：opcache.enable_cli=1
6.在 Dynamic Extensions 代码块中添加 “zend_extension=opcache.so”
```

### 添加环境变量

```shell
$ sudo touch /etc/profile.d/php.sh
$ echo 'export PATH=$PATH:/usr/local/php/bin/' > /etc/profile.d/php.sh
$ sudo chmod 0777 /etc/profile.d/php.sh
$ source /etc/profile.d/php.sh
```

### 配置php-fpm

```shell
$ cp /usr/local/php/7.2/etc/php-fpm.conf.default /usr/local/php/7.2/etc/php-fpm.conf
$ cp /usr/local/php/7.2/etc/php-fpm.d/www.conf.default /usr/local/php/7.2/etc/php-fpm.d/www.conf

$ cp ~/src/php7.2.25/sapi/fpm/init.d.php-fpm /usr/local/php/7.2/bin/php-fpm
$ sudo chmod 0777 /usr/local/php/7.2/bin/php-fpm
```

### 创建服务

```shell
$ vim /lib/systemd/system/php7.2-fpm.service

# --------------------------------------------------------------------------
# 输入以下代码
# --------------------------------------------------------------------------
[Unit]
Description=php7.2-fpm
After=network.target

[Service]
Type=forking
PIDFile=/usr/local/php/7.2/var/run/php-fpm.pid
ExecStart=/usr/local/php/7.2/bin/php-fpm start
ExecReload=/usr/local/php/7.2/bin/php-fpm restart
ExecStop=/usr/local/php/7.2/bin/php-fpm stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

### 启停服务

```shell
# 开机启动
$ systemctl enable php7.2-fpm.service

# 启动服务
$ systemctl start php7.2-fpm.service

# 停止服务
$ systemctl stop php7.2-fpm.service

# 重启服务
$ systemctl restart php7.2-fpm.service

# 杀死进程
$ pkill -9 php7.2-fpm
```


## Centos7.4安装Mariadb

- 升级所有包同时也升级软件和系统内核(因电脑差异可能需要几分钟时间)

```shell
$ yum -y update
```

- 删除系统默认数据库配置文件 及 卸载系统自带mariadb-libs 并 重启系统

```shell
$ rpm -e mariadb-libs-* --nodeps
$ reboot
```

#### 安装MariaDB数据库

- 安装依赖库(Centos7)

```shell
$ yum -y install gcc-c++ openssl-devel ncurses-devel bison
```

- 创建用户组 及 数据库相关目录

```shell
$ groupadd -r mysql
$ useradd -r -g mysql -s /sbin/nologin -d /usr/local/mysql -M mysql
$ mkdir -p /usr/local/mysql
$ mkdir -p /data/mysql
$ chown -R mysql:mysql /data/mysql
```

- 安装cmake

```shell
$ cd /usr/local/src
$ wget https://downloads.mariadb.org/interstitial/mariadb-10.4.10/source/mariadb-10.4.10.tar.gz
$ tar -zxvf mariadb-10.4.10.tar.gz && cd mariadb-10.4.10
```

- 编译参数

```shell
$ cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DCMAKE_BUILD_TYPE=Release \
-DSYSCONFDIR=/etc \
-DMYSQL_DATADIR=/data/mysql \
-DMYSQL_UNIX_ADDR=/data/mysql/mysqld.sock \
-DWITHOUT_TOKUDB=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STPRAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DWITH_SSL=system \
-DWITH_ZLIB=system \
-DWITH_LOBWRAP=0 \
-DDEFAULT_CHARSET=utf8mb4 \
-DDEFAULT_COLLATION=utf8mb4_general_ci
```

> 如果编译失败请删除 CMakeCache.txt 让指令重新执行，否则每次读取这个文件，命令修改正确也是报错

- 开始安装
```shell
$ make && make install
```

- 导入mysql系统表

```shell
$ cd /usr/local/mysql/
$ scripts/mysql_install_db --user=mysql --datadir=/data/mysql
```

```tex
#--------------------------------------------------------------------------
#经过测试，mariadb-10.4 以上运行以上脚本会报以下错误
#--------------------------------------------------------------------------
chown: 无法访问"./lib/plugin/auth_pam_tool_dir": 没有那个文件或目录
Cannot change ownership of the './lib/plugin/auth_pam_tool_dir' directory
 to the 'mysql' user. Check that you have the necessary permissions and try again.
#--------------------------------------------------------------------------
#解决方法：
$ vi scripts/mysql_install_db
#找到483,484,493,494行 并注释掉代码即可
483   #chown $user "$pamtooldir/auth_pam_tool_dir" && \
484   #chmod 0700 "$pamtooldir/auth_pam_tool_dir"
.......
493     #chown 0 "$pamtooldir/auth_pam_tool_dir/auth_pam_tool" && \
494     #chmod 04755 "$pamtooldir/auth_pam_tool_dir/auth_pam_tool"
```

- 复制配制文件

```shell
$ cp /usr/local/mysql/support-files/wsrep.cnf /etc/my.cnf
```

- 编写服务脚本

```shell
$ hostname
xxxxxxxxxxxxxxxxxxxxxxx(此处会显示电脑名称，将此名称填写在下方PIDFile=/data/mysql/xxxxx.pid中)

$ vi /lib/systemd/system/mysql.service
```

```tex
#--------------------------------------------------------------------------
#输入以下代码
#--------------------------------------------------------------------------
[Unit]
Description=MySQL Community Server
After=network.target

[Service]
User=mysql
Group=mysql
Type=forking
PermissionsStartOnly=true
PIDFile=/data/mysql/************* YOU HOST NAME *************.pid
ExecStart=/usr/local/mysql/support-files/mysql.server start
ExecReload=/usr/local/mysql/support-files/mysql.server restart
ExecStop=/usr/local/mysql/support-files/mysql.server stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

- 设置服务

```shell
# 开机启动
$ systemctl enable mysql.service

# 启动服务
$ systemctl start mysql.service

# 查看状态
$ systemctl status mysql.service

# 停止服务
$ systemctl stop mysql.service

# 重启服务
systemctl restart mysql.service

# 杀死进程
$ pkill -9 mysql
```

- 配置环境变量(以便在任何目录下输入mysql命令)

```shell
$ touch /etc/profile.d/mysql.sh
$ echo 'export PATH=$PATH:/usr/local/mysql/bin/' > /etc/profile.d/mysql.sh
$ chmod 0777 /etc/profile.d/mysql.sh
$ source /etc/profile.d/mysql.sh
```


- 初始化MariaDB

```shell
$ ./bin/mysql_secure_installation
```

```tex
# --------------------------------------------------------------------------
# 根据相关提示进行操作
# 以下提示：
# --------------------------------------------------------------------------
Enter current password for root (enter for none):    输入当前root密码(回车)
Switch to unix_socket authentication [Y/n]           是否启用socket授权(n)
Change the root password? [Y/n]                      是否修改root用户密码?(Y)
New password:                                        输入新root密码(a123456)
Re-enter new password:                               确认输入root密码(a123456)
Remove anonymous users? [Y/n] Y                      删除匿名用户?(Y)
Disallow root login remotely? [Y/n] Y                不允许root登录远程?(Y)
Reload privilege tables now? [Y/n] Y                 现在重新加载权限表(Y)

#全部完成!如果你已经完成了以上步骤,MariaDB安装现在应该安装完成。
```

- 创建外网可访问的管理员帐号(根据需要，请尽量保证密码的复杂性避免数据库外泄)

```shell
$ mysql -uroot -p

$ GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY 'your password' WITH GRANT OPTION;
```













