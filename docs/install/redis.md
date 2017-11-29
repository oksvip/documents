#### 安装redis和redis拓展

```tex
php拓展安装教程： http://www.cnblogs.com/myright/articles/5408276.html
php全部拓展下载： http://pecl.php.net/package-stats.php
redis下载地址：   https://github.com/MSOpenTech/redis
php7拓展下载地址：http://pecl.php.net/package/redis/3.1.1/windows
原文：http://blog.csdn.net/ludonqin/article/details/47211109
```

##### 安装redis依赖文件

```shell
$ yum install tcl
```

##### 下载redis安装包并解压

```shell
$ tar -xzvf redis-4.0-rc3.tar.gz
$ cd redis-4.0-rc3
$ make && make install
```

##### 成功后/usr/local/bin下已有这些文件

```shell
redis-benchmark  redis-check-rdb  redis-sentinel
redis-check-aof  redis-cli        redis-server
$ redis-server –v (查看版本命令)
```

##### 修改配置文件redis.conf

```shell
$ 创建配置文件目录，dump file 目录，进程pid目录，log目录等
$ cd /usr/local/
$ mkdir redis
$ cd redis
$ mkdir conf data log run
$ cp /usr/local/soft/redis/redis.conf /usr/local/redis/conf/redis_6379.conf
$ cd /usr/local/redis/conf
$ cp redis_6379.conf redis_6379.conf.bak
```

##### 修改Redis配置

```shell
$ vi redis_6379.conf
pidfile /usr/local/redis/run/redis_6379.pid
dir /usr/local/redis/data
logfile /usr/local/redis/log/redis.log
daemonize no改为yes					# 修改配置文件使得redis在background运行
```

##### 启动redis，查看各目录下文件

```shell
$ redis-server /usr/local/redis/conf/redis_6379.conf
```

##### 客户端连接redis

```shell
$ redis-cli
```

##### 服务及开机自启动

```shell
# 给reids服务设置优先级（在开始位置添加）
#!/bin/sh
#
# added by wt on 20170312
# chkconfig: 2345 90 10
#
# description: Redis is a persistent key-value database
# added by wt on 20170312


$ cp /usr/local/soft/redis/utils/redis_init_script /etc/init.d/redis
$ cd /etc/init.d
$ vi /etc/init.d/redis
PIDFILE=/usr/local/redis/run/redis_${REDISPORT}.pid
CONF="/usr/local/redis/conf/redis_${REDISPORT}.conf"

$ chmod +x /etc/init.d/redis	# 给启动脚本添加权限
$ chkconfig redis on			# 添加开机启动服务
```

##### 启动、关闭redis

```shell
$ service redis start
$ service redis stop
```

##### 安装php拓展

[http://www.cnblogs.com/GaZeon/p/5422078.html](http://www.cnblogs.com/GaZeon/p/5422078.html)

```shell
$ wget https://codeload.github.com/phpredis/phpredis/zip/php7
$ unzip phpredis-php7.zip
$ cd phpredis-php7
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```

##### 将extension=redis.so加入到php.ini

```shell
$ vim /usr/local/php/php.ini
extension=redis.so
$ service httpd restart
```

##### 测试redis是否成功

```php
<?php
$redis = new Redis();
$redis->connect('127.0.0.1', 6379);
$redis->set('name', 'wt');
echo $redis->get('name');
```