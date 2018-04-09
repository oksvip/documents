解析php文件；

配置文件：/usr/local/php/php.ini

##### 编译安装php

```shell
$ tar xf php-7.1.9.tar.gz
$ cd php-7.1.9
$ ./configure --prefix=/usr/local/php \
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

$ ./configure \
        --prefix=/usr/local/php \
        --with-apxs2=/usr/local/httpd/bin/apxs \
        --with-config-file-path=/etc/php \
        --with-config-file-scan-dir=/etc/php.d \
        --with-libxml-dir=/usr \
        --with-libdir=lib64 \
        --with-curl \
        --with-mysqli \
        --with-openssl \
        --with-pdo-mysql \
        --with-pdo-sqlite \
        --with-pcre-regex \
        --with-pear \
        --with-xmlrpc \
        --with-gd \
        --enable-gd-native-ttf \
        --with-jpeg-dir \
        --with-freetype-dir \
        --with-png-dir \
        --with-gettext \
        --with-iconv-dir \
        --with-kerberos \
        --with-zlib \
        --with-xsl \
        --with-mcrypt \
        --enable-inline-optimization \
        --enable-mbregex \
        --enable-mbstring \
        --enable-libxml \
        --enable-opcache \
        --enable-pcntl \
        --enable-xml \
        --enable-mysqlnd \
        --enable-fpm \
        --enable-bcmath \
        --enable-shmop \
        --enable-soap \
        --enable-sockets \
        --enable-sysvsem \
        --enable-zip \
        --enable-mbstring \
        --enable-maintainer-zts \
        --enable-fileinfo

$ make && make install
```

##### 备份php.ini

```shell
# 将安装目录下的php.ini-production复制到/usr/local/php/conf下作为配置文件
$ cp /usr/local/soft/php-7.1.9/php.ini-development /usr/local/php/php.ini
$ cp /usr/local/php/php.ini php.ini.bak
```

##### Apache整合Php

```shell
$ vi /usr/local/httpd/conf/httpd.conf
# 添加index.php网页为默认访问页：
DirectoryIndex index.php index.html
# 使apache与扩展名为.php的文件类型相关联，在/usr/local/httpd/conf/httpd.conf文件中添加一句：
AddType application/x-httpd-php .php
```

##### 将php命令添加至环境变量
```tex
$ export PATH=$PATH:/usr/local/php/bin
$ php --version
```

##### phpinfo()信息

```shell
$ service httpd restart
$ cd /www
$ vi index.php

<?php
$link = mysqli_connect('localhost', 'root', 'root');
if ($link == true) {
    echo 'ok';
} else {
    echo 'no';
}
phpinfo();
```
