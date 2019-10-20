## Centos6.5_x86安装LNMP环境（Nginx1.14.0+Mysql5.7+PHP7.2）

#### 修改yum源
```shell
$ cd /etc/yum.repos.d/
$ vim nginx.repo
```
#### 写入源内容

```tex
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
```

#### 更新yum源

```shell
$ yum update
```

#### 使用yum 安装nginx
```shell
$ yum install nginx -y
$ nginx -v
```

#### 打开nginx自启动
```shell
$ chkconfig nginx on
```

#### 安装mysql 5.7

##### 更新及安装mysql的yum 源

##### 官网下载源码包

```shell
$ wget http://dev.mysql.com/get/mysql57-community-release-el6-7.noarch.rpm
```

#### rpm 安装mysql的yum源

```shell
$ rpm -Uvh mysql57-community-release-el6-7.noarch.rpm
```

#### 打开 mysql-community.repo 看关于mysql的内空，确定mysql57的enable是打开的

```shell
$ vim /etc/yum.repos.d/mysql-community.repo
```

#### 安装mysql服务

##### 执行安装mysql命令

```shell
$ yum install mysql-community-server
$ service mysqld start
```

#### 启动后，查看安装后自动生成的密码
```shell
$ grep "password" /var/log/mysqld.log
```

#### 修改初始化密码

```shell
$ mysql_secure_installation
```

#### 打开mysql自启动

```shell
$ chkconfig mysqld on
```

#### 安装PHP7.2

#### 如果之前已经安装过php的话执行
```shell
$ yum remove php* php-common
```

#### 安装php7的yum源

##### remi 源需要 epel-release
```shell
$ rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
$ rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
$ rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
```

#### 安装remi

```shell
$ rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
$ rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-remi
```

#### 关于设置级别事情 肯定要安装 priorities

```shell
$ yum install yum-priorities
```

#### 修改yum源（将[remi]段中的enabled=0改为enabled=1）

```shell
$ vim /etc/yum.repos.d/remi.repo
$ vim /etc/yum.repos.d/remi-php72.repo // 与remi.repo类似，将[remi-php72]段中的enabled=0改为enabled=1
```

#### 扫行命令查看版本如果显示的是7.x的话 那就没问题，当然也可以直接使用yum install php70 进行安装

```shell
$ yum list php
```

#### yum 安装PHP7.2

```shell
$ yum install php php-fpm php-cli php-pdo php-mysql php-gd php-bcmath php-xml php-mbstring php-mcrypt php-redis
$ php -V
$ php -m
```

#### php的php.ini配制一般在/etc/php.ini简单的修改一些配制:

```tex
vim /etc/php.ini

date.timezone = Asia/Shanghai
upload_max_filesize = 20M
post_max_size = 20M
display_errors = Off // 生产环境半掉就好了

# 使HTTP Header中不显示PHP信息把
expose_php = On 
修改为
expose_php = Off
```
#### 重启php: 
```shell
$ service php-fpm restart
```

#### 打开php自启动
```shell
$ chkconfig php-fpm on
```

#### 配置nginx与php

```tex
安装好nginx之后，nginx默认的网站根目录应该是在/usr/share/nginx/html/
虚拟主机的配制在 /etc/nginx/conf.d 如果要配制新的域名在这里就可以了。
默认有一个default.conf的配制，如果不需要的话可以删除。
```

#### 创建一个新的配置，比如我创建一个 laravel.work 的网站;在/etc/nginx/conf.d/ 创建文件laravel.work.conf 内容如下
```tex
server {
    listen       80;
    server_name  laravel.work;

    client_max_body_size 10m;

    root /www/laravel/public;

    access_log  /var/log/nginx/laravel.work.access.log  main;
    error_log /var/log/nginx/laravel.work.error.log;

    location / {
        try_files $uri /index.php$is_args$args;
        index index.php index.html index.htm;
    }

    error_page  404              /404.html;
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html; 
    }

    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        include        fastcgi_params;
    }
}

```

#### 创建日志文件及分组

```shell
$ touch /var/log/nginx/laravel.work.access.log
$ chown -R nginx.nginx /var/log/nginx/laravel.work.access.log
```

#### 安装composer

```shell
$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
$ php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified';  } else { echo 'Installer corrupt'; unlink('composer-setup.php');  } echo PHP_EOL;"
$ php composer-setup.php
$ php -r "unlink('composer-setup.php');"
```

#### 设置国内镜像

```shell
$ composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

















