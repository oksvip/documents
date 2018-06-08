# Centos软件安装配置信息

Linux版本为centos6.5.

#### 安装依赖包

```shell
$ yum install -y make cmake gcc gcc-c++ autoconf automake libpng-devel libjpeg-devel zlib libxml2-devel ncurses-devel bison libtool-ltdl-devel libiconv libmcrypt mhash mcrypt pcre-devel libaio pcre python python-devel openssl-devel kernel-devel freetype-devel libcurl-devel libxslt-devel wget unzip lrzsz curl curl-devel perl cpio expat-devel gettext-devel  zlib-devel perl-ExtUtils-MakeMaker tcl
```
#### 开启网络
```shell
$ vi /etc/sysconfig/network-scripts/ifcfg-eth0
修改：ONBOOT=yes
$ service network restart
```

#### 关闭SELINUX

```shell
$ vi /etc/selinux/config
  #SELINUX=enforcing 		# 注释掉
  #SELINUXTYPE=targeted 	# 注释掉
  SELINUX=disabled 		# 增加
$ reboot				# 重启生效
```

#### 在根目录下创建~/lamp文件夹并上传文件至服务器

```tex
apr-1.6.3.tar.gz
apr-util-1.6.1.tar.gz
git-v2.12.0-rc1.tar.gz
httpd-2.4.33.tar.bz2
php-7.1.16.tar.xz
phpredis-php7.zip
redis-4.0.9.tar.gz
mariadb-10.3.7.tar.gz
```

#### 解压文件

```shell
$ cd ~/lamp/
$ for tar in *.tar.gz;  do tar xvf $tar; done
$ for tar in *.tar.bz2; do tar xvf $tar; done
```

#### 移动apr、apr-util到httpd文件夹中

```shell
$ mv apr-1.6.3 httpd-2.4.33/srclib/apr
$ mv apr-util-1.6.1 httpd-2.4.33/srclib/apr-util
```
