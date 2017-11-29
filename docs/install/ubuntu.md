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

#### 5.Install Mysql5.7.20

```shell

$ sudo apt install mysql-server php7.0-mysql
$ sudo apt-get install mysql-client
$ mysql_secure_installation

```

#### 6.Install phpMyAdmin4.5.4

```shell

$ sudo apt-get install phpmyadmin
$ sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

```

#### 7.Config phpMyAdmin

```shell

$ nano /etc/php/7.0/apache2/php.ini

update the text:
	display_errors = On
	extension=php_mbstring.dll

```

#### 8.Change mysql datadir

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

#### 9.Config apache documentroot route

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

#### 10.Config apache domain request

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

#### 11.Start Mysql remote access

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
