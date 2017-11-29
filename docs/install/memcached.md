##### 下载

```shell
$ wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
$ wget http://www.memcached.org/files/memcached-1.4.36.tar.gz
$ wget https://github.com/websupport-sk/pecl-memcache/archive/php7.zip
```

#### 安装libevent

```shell
$ tar -xzvf libevent-2.1.8-stable.tar.gz
$ cd libevent-2.1.8-stable
$ ./configure --prefix=/usr/local/libevent
$ make && make install
```

#### 安装memcached

```shell
$ tar -xzvf memcached-1.4.36.tar.gz
$ cd memcached-1.4.36
$ ./configure --prefix=/usr/local/memcached --with-libevent=/usr/local/libevent
$ make && make install
```

#### 启动memcached

```shell
$ /usr/local/memcached/bin/memcached -d -m 128 -l 127.0.0.1 -p 11211 -u root
# (128为内存, 11211为端口,root为用户组)
```

#### 开机启动

```shell
$ vi /etc/rc.d/rc.local 添加以下内容
# /usr/local/memcached/bin/memcached -d -m 128 -l 127.0.0.1 -p 11211 -u root
```

#### 查看是否启动成功

```shell
$ ps aux|grep memcached
```

#### php_memcache扩展安装

```shell
$ unzip pecl-memcache-php7.zip
$ cd pecl-memcache-php7
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```

#### 修改php.ini 加载Memcache组件

```shell
[memcache]
extension = memcache.so
```

#### 重启 httpd

```shell
$ service httpd restart
```

#### 测试memcache是否可用

```php
<?php
$memcache = new Memcache();
$memcache->connect('127.0.0.1', 11211);
$memcache->set('name', 'hello memcache');
echo $memcache->get('name');die;
```

