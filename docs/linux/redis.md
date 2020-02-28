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
$ make && make PREFIX=/usr/local/redis install #安装到指定目录中
```

##### 成功后/usr/local/redis/bin下已有这些文件

```shell
redis-benchmark  redis-check-rdb  redis-sentinel
redis-check-aof  redis-cli        redis-server
$ redis-server –v (查看版本命令)
```

##### 修改配置文件redis.conf

```shell
$ 创建配置文件目录，dump file 目录，进程pid目录，log目录等
$ cd /usr/local/redis
$ mkdir /etc/redis
$ cp ~/lamp/redis-4.0.9/redis.conf /etc/redis/redis.conf
$ cd /etc/redis
$ cp redis.conf redis.conf.bak
```

##### 修改Redis配置

```shell
$ vim redis.conf
dir /data/redis
logfile /var/log/redis.log
daemonize no改为yes					# 修改配置文件使得redis在background运行
maxmemory maxmemory 2147483648      # 2G，根据服务器配制适当填写
requirepass password                # 设置redis连接密码
```

##### 启动redis，查看各目录下文件

```shell
$ redis-server /etc/redis/redis.conf
```

##### 创建服务
```shell
$ vim /lib/systemd/system/redis.service

# --------------------------------------------------------------------------
# 输入以下代码并保存
# --------------------------------------------------------------------------
[Unit]
Description=The redis-server Process Manager
After=syslog.target network.target
[Service]
Type=forking
PIDFile=/var/run/redis_6379.pid
ExecStart=/usr/local/redis/bin/redis-server /etc/redis/redis.conf         
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID
[Install]
WantedBy=multi-user.target
```

##### 启停服务
```shell
# 开机启动
[redis-5.0.5] systemctl enable redis.service

# 启动服务
[redis-5.0.5] systemctl start redis.service

# 停止服务
[redis-5.0.5] systemctl stop redis.service

# 重启服务
[redis-5.0.5] systemctl restart redis.service

# 强杀进程
[redis-5.0.5] pkill -9 redis

```

##### 客户端连接redis

```shell
$ redis-cli
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
