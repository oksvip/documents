[TOC]

# Ubuntu16.04 install Lamp Enviroment

#### 1.Update the system

```shell

$ sudo apt update

```
#### 2.Change root password

```shell

$ su passwd

```

#### 3.Install Apache2.4.18

```shell

$ sudo apt install apache2

```


#### 4.Install PHP7.0.22

```shell

$ sudo apt install php
$ sudo apt-get install libapache2-mod-php

```
#### 5.Install PHP7.1.11

```shell
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install php7.1
$ sudo apt-get install php7.1-mbstring
$ sudo apt-get install php7.1-gd
$ sudo apt-get install php7.1-dom
$ sudo apt-get install php7.1-mysql
```

#### 6.Change Apache and terminal PHP Version

```shell
Change Version From 7.0 TO PHP 7.1：
Apache：
$ sudo a2dismod php7.0
$ sudo a2enmod php7.1
$ sudo service apache2 restart

Terminal：
$ sudo update-alternatives --set php /usr/bin/php7.1
```

#### 7.Install Mysql5.7.20

```shell

$ sudo apt install mysql-server php7.0-mysql
$ sudo apt-get install mysql-client
$ mysql_secure_installation

```

#### 8.Install phpMyAdmin4.5.4

```shell

$ sudo apt-get install phpmyadmin
$ sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

```

#### 9.Config phpMyAdmin

```shell

$ nano /etc/php/7.0/apache2/php.ini

update the text:
	display_errors = On
	extension=php_mbstring.dll

```

#### 10.Change mysql datadir

```shell

$ sudo service mysql stop
$ sudo mkdir -p /data/mysql
$ sudo chown -vR mysql:mysql /data/mysql
$ sudo chmod -vR 700 /data/mysql
$ su root
$ cp -av /var/lib/mysql/* /data/mysql
$ exit
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

update follow config:
    datadir = /data/mysql


$ sudo vi /etc/apparmor.d/usr.sbin.mysqld

update follow config:
    # /var/lib/mysql/ r,
    # /var/lib/mysql/** rwk,
to:
    /data/mysql/ r,
    /data/mysql/** rwk,

$ sudo service apparmor reload
$ sudo service mysql restart

```

#### 11.Config apache documentroot route

```shell

$ sudo mkdir /home/sunrise/www
$ cd /etc/apache2/sites-available/
$ sudo vi 000-default.conf

update DocumentRoot /home/sunrise/www

$ sudo vi /etc/apache2/apache2.conf

update: AllowOverride None
To: AllowOverride All
add: 
Order deny,allow
Allow from all

$ sudo service apache2 restart

```

#### 12.Config apache domain request

```shell

$ cd /etc/apache2/sites-available/
$ sudo cp 000-default.conf phpmyadmin.conf
$ sudo vi phpmyadmin.conf

update follow:
    ServerName phpmyadmin.dev
    ServerAdmin webmaster@localhost
    DocumentRoot /home/sunrise/www/phpmyadmin

$ cd ../sites-enabled/
$ sudo ln -s ../sites-available/phpmyadmin.conf
$ sudo service apache2 restart

```

#### 13.Start Mysql remote access

```shell

$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

update follow:
    # bind-address = 127.0.0.1

http://phpmyadmin.dev
Add User:
    root root
Permission:
    数据/结构

```



[TOC]
# ubuntu php多版本共存切换，为每个站点设置不同的php版本
> 做开发时，由于本机开发的php版本跟线上发布的php版本不一致，很容易在上线后，发现因版本的影响导致一些bug，但又不想重新去换本机的php版本，那么多版本共存就很方便了！有必要时，切换到指定版本测试下，没问题再上线就OK了！

LMAP环境安装记录如下：
#### 安装apache2
```shell
$ sudo apt-get install -y apache2
```

#### 安装：mysql5.7（ubuntu16.04自带）
```shell
$ sudo apt instal -y mysql-server mysql-client libmysqlclient-dev mysql-workbench
```

#### 安装：php5.6
```shell
$ sudo add-apt-repository ppa:ondrej/php
$ sudo apt-get update
$ sudo apt-get install -y php5.6-common php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-xml php5.6-gd php5.6-curl php5.6-json php5.6-fpm php5.6-zip php5.6-mcrypt libapache2-mod-php5.6
```

#### 安装：php7.0
```shell
$ sudo apt-get install -y php7.0-common php7.0-mbstring php7.0-mcrypt php7.0-mysql php7.0-xml php7.0-gd php7.0-curl php7.0-json php7.0-fpm php7.0-zip php7.0-mcrypt libapache2-mod-php7.0
```

#### 安装：php7.1
```sheel
$ sudo apt-get install -y php7.1-common php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-xml php7.1-gd php7.1-curl php7.1-json php7.1-fpm php7.1-zip php7.1-mcrypt libapache2-mod-php7.1
```

#### 开启重写转向
```shell
$ sudo a2enmod rewrite
$ sudo a2enmod headers
$ sudo service apache2 restart
```

```shell
$ sudo vi .bashrc
```
#### 编辑bashrc加入自定义命令，为方便在不同php版本间切换
```txt
alias php56='sudo a2dismod php7.0 && sudo a2dismod php7.1 && sudo a2enmod php5.6 && sudo service apache2 restart'
alias php70='sudo a2dismod php5.6 && sudo a2dismod php7.1 && sudo a2enmod php7.0 && sudo service apache2 restart'
alias php71='sudo a2dismod php5.6 && sudo a2dismod php7.0 && sudo a2enmod php7.1 && sudo service apache2 restart'
```

安装完成后，重启电脑，默认是运行5.6版本的php（命令行下php-v 是显示最高版本7.1）
在/etc/php目录下有对应版本号的文件夹，编辑相应的php.ini可配置相应的php版本，命令行下php70可切换到php7版本，其它类同
网上搜索一片，很多都是用nginx来处理多站点用不同php版本，但我真不想那么麻烦再装个nginx，基本上都是用代理转发，apache本身就有proxy，所以用apache本身的也就好
个人开发习惯上是用虚拟主机，所以，配置了一下虚拟主机的目录

```tex
# 以下配置放到文件最后面，在此配置后面的网站，也会引用此php版本
<VirtualHost *:80>
        ServerName my.xxx.com
        DocumentRoot /var/www/html/xxx
        // 加载不同的php版本
        <FilesMatch \.php$>
            SetHandler "proxy:unix:/run/php/php7.0-fpm.sock|fcgi://localhost"
        </FilesMatch>
        // 加载不同的php版本
</VirtualHost>
```

#### 启用以下两个模块
```shell
$ sudo a2enmod proxy proxy_fcgi
$ sudo service apache2 restart
```
注意apahce版本为2.4.10版本以上才有效，本人ubuntu14.04系统的apache为2.4.7版本，所以升级到最新版本
```shell
$ sudo add-apt-repository ppa:ondrej/apache2
$ sudo apt-get update
$ sudo apt-get dist-upgrade
```
