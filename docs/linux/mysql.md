存储数据；

配置文件：/etc/my.cnf

端口：3306

用户名：root 	密码：root

开机启动：是

#### 安装Mysql

```shell
$ cd /usr/local/soft/
$ mv mysql-5.7.17-linux-glibc2.5-i686/ mysql
$ cd mysql
$ mkdir data	# 存储mysql数据文件
```

#### 开放端口

```shell
$ vi /etc/sysconfig/iptables

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT # 添加规则允许21端口通行

$ service iptables restart		# 重启防火墙
```

##### 配置创建mysql的用户组和用户

```shell
$ groupadd mysql
$ useradd mysql -g mysql
$ chown -R mysql .
$ chgrp -R mysql .
```

##### 初始化mysql

```shell
$ cd /usr/local/mysql/bin
# 指定mysql基本目录和数据目录文件夹
$ ./mysql_install_db --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data   
2016-01-09 12:00:28 [WARNING] mysql_install_db is deprecated. Please consider switching to mysqld --initialize
2016-01-09 12:00:33 [WARNING] The bootstrap log isn't empty:
2016-01-09 12:00:33 [WARNING] 2016-01-09T04:00:29.262989Z 0 [Warning] --bootstrap is deprecated. Please consider using --initialize instead
2016-01-09T04:00:29.264643Z 0 [Warning] Changed limits: max_open_files: 1024 (requested 5000)
2016-01-09T04:00:29.264653Z 0 [Warning] Changed limits: table_open_cache: 431 (requested 2000)
```

##### 启动mysql服务

```shell
$ cd /usr/local/mysql/support-files/
$ ./mysql.server start
  Starting MySQL. SUCCESS! 
```

##### 添加Mysql系统服务

```shell
$ cp -v /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
$ chkconfig --add mysql
$ service mysql start
```

##### 添加Mysql全局环境变量

```shell
$ ln -s /usr/local/mysql/bin/mysql /bin
```

##### 查看root随机密码

```shell
$ cat /root/.mysql_secret
# Password set for user 'root@localhost' at 2016-01-09 12:00:28 
:5ul#H6dmcwX
```

##### 登录mysql

```shell
$ mysql -h localhost -u root -p
或者
$ cd /usr/local/mysql/bin
$ ./mysql -u root -p
```

##### 修改Mysql的root密码

```mysql
mysql> SET PASSWORD=PASSWORD('root');
```

##### 远程登录Mysql

```mysql
mysql> USE mysql;
mysql> SELECT Host,User FROM user;
mysql> GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
```

##### 默认utf8字符集
```shell
$ cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
$ vi /etc/my.cnf
    basedir = /usr/local/mysql
    datadir = /usr/local/mysql/data
    port = 3306
    character-set-server = utf8
```

##### 解决Navicat For Mysql插入数据后显示错误
```shell
$  sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION
```