# Centos软件安装配置信息

Linux版本为centos6.5.

#### 安装依赖包

```shell
$ yum install -y make cmake gcc gcc-c++ autoconf automake libpng-devel libjpeg-devel zlib libxml2-devel ncurses-devel bison libtool-ltdl-devel libiconv libmcrypt mhash mcrypt pcre-devel libaio pcre python python-devel openssl-devel kernel-devel freetype-devel libcurl-devel libxslt-devel wget unzip lrzsz curl curl-devel perl cpio expat-devel gettext-devel  zlib-devel perl-ExtUtils-MakeMaker tcl
```

#### 关闭SELINUX

    $ vi /etc/selinux/config
    #SELINUX=enforcing 		# 注释掉
    #SELINUXTYPE=targeted 	# 注释掉
    SELINUX=disabled 		# 增加
    $ reboot				# 重启生效
