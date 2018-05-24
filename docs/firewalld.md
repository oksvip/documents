#### centos7开放端口
```shell
$ firewall-cmd --zone=public --add-port=443/tcp --permanent
```

#### centos7关闭端口
```shell
$ firewall-cmd --zone= public --remove-port=443/tcp --permanent
```

#### 开机启动/关闭开机启动
```shell
$ systemctl enable firewalld.service
$ systemctl disable firewalld.service
```

#### 查看状态
```shell
$ firewall-cmd --state
```
#### 添加规则(常用端口：80网页 3306MySQL 9000PHP-FPM 6379Redis 11211Memcached)
```shell
$ firewall-cmd --zone=public --add-port=80/tcp --permanent  
$ firewall-cmd --zone=public --add-port=80/udp --permanent  
$ firewall-cmd --zone=dmz --add-port=80/tcp --permanent  
$ firewall-cmd --zone=dmz --add-port=80/udp --permanent 
```
#### 生效
```shell
$ firewall-cmd --reload
```

#### 开启、关闭、重启防火墙
```shell
$  systemctl start/stop/restart/status firewalld.service
```

#### 查看端口被哪个程序占用

```shell
$ netstat -lnp|grep 80
```

#### 查看监听的端口
```shell
$ netstat -lntp
```
