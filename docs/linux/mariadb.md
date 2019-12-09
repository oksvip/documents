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

- 安装依赖库

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

> #--------------------------------------------------------------------------
> #经过测试，mariadb-10.4 以上运行以上脚本会报以下错误
> #--------------------------------------------------------------------------
> chown: 无法访问"./lib/plugin/auth_pam_tool_dir": 没有那个文件或目录
> Cannot change ownership of the './lib/plugin/auth_pam_tool_dir' directory
>  to the 'mysql' user. Check that you have the necessary permissions and try again.
> #--------------------------------------------------------------------------
> #解决方法：
> [mysql]➔ vi scripts/mysql_install_db
> #找到483,484,493,494行 并注释掉代码即可
> 483   #chown $user "$pamtooldir/auth_pam_tool_dir" && \
> 484   #chmod 0700 "$pamtooldir/auth_pam_tool_dir"
> .......
> 493     #chown 0 "$pamtooldir/auth_pam_tool_dir/auth_pam_tool" && \
> 494     #chmod 04755 "$pamtooldir/auth_pam_tool_dir/auth_pam_tool"

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
















