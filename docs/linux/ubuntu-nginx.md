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
$ apt-get install gcc automake autoconf make build-essential libtool libpcre3 libpcre3-dev zlib1g-dev openssl
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

$ sudo ./configure --prefix=/usr/local/nginx --sbin-path=/usr/local/nginx/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx/nginx.lock --user=www --group=www --without-http_memcached_module --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module

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

## 安装php

```shell
$ sudo apt install gcc make openssl libssl-dev curl libcurl4-openssl-dev libbz2-dev libxml2-dev libjpeg-dev libpng-dev libfreetype6-dev libzip-dev
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
















