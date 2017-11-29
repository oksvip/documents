http://www.cnblogs.com/luyucheng/p/6085882.html

##### 创建mongodb用户组和用户

```shell
$ groupadd mongodb
$ useradd -r -g mongodb -s /sbin/nologin -M mongodb
```

##### 下载mongodb源码包

```tex
下载页面：https://www.mongodb.com/download-center?jmp=nav
这里用的是 mongodb-linux-x86_64-rhel62-3.2.10.tgz
下载地址：https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-3.2.10.tgz
```

##### 创建mongodb文件目录

```shell
$ mkdir -p /usr/local/mongodb/data
$ mkdir -p /usr/local/mongodb/conf
$ mkdir -p /usr/local/mongodb/run
$ mkdir -p /usr/local/mongodb/log
```

##### 将文件复制到mongodb/目录

```shell
$ mv mongodb-linux-i686-2.4.9 /usr/local/mongodb
```

##### 创建mongodb配置文件mongodb.conf

```shell
$ cd /usr/local/mongodb/conf
$ vim mongodb.conf
```

##### 添加下面内容，保存退出

```tex
dbpath=/usr/local/mongodb/data #数据目录存在位置
logpath=/usr/local/mongodb/log/mongodb.log #日志文件存放目录
logappend=true #写日志的模式:设置为true为追加
fork=true  #以守护程序的方式启用，即在后台运行
verbose=true
vvvv=true #启动verbose冗长信息，它的级别有 vv~vvvvv，v越多级别越高，在日志文件中记录的信息越详细
maxConns=20000 #默认值：取决于系统（即的ulimit和文件描述符）限制。MongoDB中不会限制其自身的连接
pidfilepath=/usr/local/mongodb/run/mongodb.pid
directoryperdb=true #数据目录存储模式,如果直接修改原来的数据会不见了
profile=0 #数据库分析等级设置,0 关 2 开。包括所有操作。 1 开。仅包括慢操作
slowms=200 #记录profile分析的慢查询的时间，默认是100毫秒
quiet=true
syncdelay=60 #刷写数据到日志的频率，通过fsync操作数据。默认60秒
#port=27017  #端口
#bind_ip = 10.1.146.163 #IP
#auth=true  #开始认证
#nohttpinterface=false #28017 端口开启的服务。默认false，支持
#notablescan=false#不禁止表扫描操作
#cpu=true #设置为true会强制mongodb每4s报告cpu利用率和io等待，把日志信息写到标准输出或日志文件
```

##### 修改mongodb目录权限

```shell
$ chown -R mongodb:mongodb /usr/local/mongodb
$ chown -R mongodb:mongodb usr/local/mongodb/run
$ chown -R mongodb:mongodb usr/local/mongodb/log
```

##### 将mongodb命令加入环境变量，修改profile文件

```shell
$ echo "export PATH=$PATH:/usr/local/mongodb/bin" >> /etc/profile
$ source /etc/profile
```

##### 将mongodb服务脚本加入到init.d/目录，创建mongod文件

```shell
$ vim /etc/init.d/mongodb
```

##### 加入下面内容，保存退出

```c
#!/bin/sh  
# chkconfig: 2345 93 18
# description:MongoDB  

#默认参数设置
#mongodb 家目录
MONGODB_HOME=/usr/local/mongodb

#mongodb 启动命令
MONGODB_BIN=$MONGODB_HOME/bin/mongod

#mongodb 配置文件
MONGODB_CONF=$MONGODB_HOME/conf/mongodb.conf

MONGODB_PID=$MONGODB_HOME/run/mongodb.pid

#最大文件打开数量限制
SYSTEM_MAXFD=65535

#mongodb 名字  
MONGODB_NAME="mongodb"
. /etc/rc.d/init.d/functions

if [ ! -f $MONGODB_BIN ]
then
    echo "$MONGODB_NAME startup: $MONGODB_BIN not exists! "  
    exit
fi

start(){
    ulimit -HSn $SYSTEM_MAXFD
    $MONGODB_BIN --config="$MONGODB_CONF"  --journal
    ret=$?
    if [ $ret -eq 0 ]; then
        action $"Starting $MONGODB_NAME: " /bin/true
    else
        action $"Starting $MONGODB_NAME: " /bin/false
    fi
}

stop(){
    PID=$(ps aux |grep "$MONGODB_NAME" |grep "$MONGODB_CONF" |grep -v grep |wc -l) 
    if [[ $PID -eq 0  ]];then
        action $"Stopping $MONGODB_NAME: " /bin/false
        exit
    fi
    kill -HUP `cat $MONGODB_PID`
    ret=$?
    if [ $ret -eq 0 ]; then
        action $"Stopping $MONGODB_NAME: " /bin/true
        rm -f $MONGODB_PID
    else   
        action $"Stopping $MONGODB_NAME: " /bin/false
    fi
}

restart(){
    stop
    sleep 2
    start
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    status)
    status $prog
        ;;
    restart)
        restart
        ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart}"
esac
```

##### 为mongod添加可执行权限

```shell
$ chmod +x /etc/init.d/mongodb
```

##### 将mongodb加入系统服务

```shell
$ chkconfig --add mongodb
```

##### 修改服务的默认启动等级

```shell
$ chkconfig mongodb on
```

##### 启动mongodb

```shell
$ service mongodb start
```

### 二、PHP7安装MongoDB拓展

##### 下载php7 mongodb拓展包

```tex
下载页面：http://pecl.php.net/package/mongodb
这里用的是 mongodb-1.1.9.tgz
下载地址：http://pecl.php.net/get/mongodb-1.1.9.tgz
```

##### 解压拓展包

```shell
$ cd /usr/local/soft
$ tar -zxf mongodb-1.1.9.tgz
```

##### 进入mongodb拓展目录，编译安装拓展

```shell
$ cd mongodb-1.1.9/
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```

##### 修改php.ini文件

```shell
$ vim /usr/local/php/php.ini
```

##### 添加mongodb.so扩展配置，保存退出

```shell
$ extension=mongodb.so
```

##### 重启Apache或php-fpm

```shell
$ service httpd restart
```

##### 测试是否安装成功

```php
<?php
$manager = new MongoDB\Driver\Manager("mongodb://127.0.0.1:27017");
$bulk = new MongoDB\Driver\BulkWrite;
$bulk->insert(['x' => 1, 'class'=>'toefl', 'num' => '18']);
$bulk->insert(['x' => 2, 'class'=>'ielts', 'num' => '26']);
$bulk->insert(['x' => 3, 'class'=>'sat', 'num' => '35']);
$manager->executeBulkWrite('test.log', $bulk);
$filter = ['x' => ['$gt' => 1]];
$options = [
    'projection' => ['_id' => 0],
    'sort' => ['x' => -1],
];
$query = new MongoDB\Driver\Query($filter, $options);
$cursor = $manager->executeQuery('test.log', $query);
foreach ($cursor as $document) {
    print_r($document);
}
```