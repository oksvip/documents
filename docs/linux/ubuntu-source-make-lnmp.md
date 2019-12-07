# Ubuntu18.04编译安装nginx和php

### 安装nginix

##### 下载源码包

```shell
$ wget http://mirrors.sohu.com/php/php-7.2.25.tar.gz
$ wget http://nginx.org/download/nginx-1.16.1.tar.gz
```

##### 解压文件

```shell
$ tar -xzvf nginx-1.16.1.tar.gz
$ tar -xzvf php-7.2.25.tar.gz
```

##### 安装nginx需要的拓展

```shell
$ apt-get install gcc automake autoconf make build-essential libtool libpcre3 libpcre3-dev zlib1g-dev openssl
```

##### 编译nginx

```shell
$ ./configure --prefix=/usr/local/nginx --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock --user=www --group=www

$ make && make install
```

##### 添加环境变量

```shell
$ vim /etc/profile
```

```tex
export PATH=$PATH:/usr/local/nginx/sbin
```

##### 启动 nginx 并查看进程

```shell
$ nginx -c /etc/nginx/conf/nginx.conf
$ ps -ef | grep nginx
```

##### 查看端口

```shell
$ netstat -lntp
```


##### 配置开机启动

```shell
$ vim /etc/rc.local
```

```tex
nginx -c /usr/local/nginx/conf/nginx.conf
```


### 安装php7.2

##### 安装php需要的拓展

```shell
$ sudo apt update && sudo apt install gcc make openssl libssl-dev curl libcurl4-openssl-dev libbz2-dev libxml2-dev libjpeg-dev libpng-dev libfreetype6-dev libzip-dev
```

##### 编译php

```shell
$ ./configure --prefix=/usr/local/php/7.2 --with-config-file-path=/etc/php/7.2 --enable-fpm --with-fpm-user=www --with-fpm-group=www --with-mysqli --with-pdo-mysql --with-iconv-dir --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-mbstring --enable-ftp --with-gd --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --disable-fileinfo --enable-maintainer-zts
```

##### 安装php

```shell
$ make -j4 && make install
```
