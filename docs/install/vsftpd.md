通过ftp进行文件传输；

配置文件：/etc/vsftpd/vsftpd.conf

端口：21 / 30000 / 30100

用户名：sunrise 	密码：sunrise

#### 安装Vsftpd

```shell
$ yum -y install vsftpd
```

#### 开放端口

```shell
$ vi /etc/sysconfig/iptables

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT # 添加规则允许21端口通行
    -A INPUT -p tcp --dport 30000:30100 -j ACCEPT		# 防火墙配置开放

$ service iptables restart		# 重启防火墙
```

##### 配置Vsftpd

```shell
$ cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak	# 备份配置文件
$ vi /etc/vsftpd/vsftpd.conf	#添加以下内容

anonymous_enable=NO		#设定不允许匿名用户访问。

# 让FLASHFXP更好的连接服务器，得让VSFTPD支持被动模式才行,在最后加入
pasv_enable=YES
pasv_max_port=30100
pasv_min_port=30000			# 30000--30100端口号可以是其它的
```

#### 添加Ftp用户

```shell
$ adduser -d /www -g ftp -s /bin/bash sunrise		# 指定此用户的网站文件夹,并且可以登录系统
# -s /bin/bash是让其能登陆系统，-d 是指定用户目录为/www ，即该账户既能登陆ftp，又能用做登陆系统用。-g 是指定为FTP用户组
$ passwd sunrise		# 修改用户密码
$ vi /etc/sudoers		# 修改用户可以有输入命令的权限
sunrise ALL=(ALL) ALL

$ chmod -R 777 /www		# 修改根目录属性
$ chgrp -R ftp /www		# 把此目录及该目录下所有文件和子目录的组属性设置成ftp组
```

#### 限制用户目录

```shell
$ vi /etc/vsftpd/vsftpd.conf		# 去掉前面的注释
#chroot_list_enable=YES
#chroot_list_file=/etc/vsftpd/chroot_list

$ vi /etc/vsftpd/chroot_list  # 新增一个文件，设置限制的用户名
sunrise

$ service vsftpd restart
```

##### 设置开机启动

```shell
$ vi /etc/rc.local 		# 在最后一行添加：
$ service vsftpd start
```