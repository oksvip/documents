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
