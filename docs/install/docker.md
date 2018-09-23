# Centos7.4安装Docker

### 卸载旧版本Docker    

```shell           
$ sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
```


### 安装依赖库

```shell           
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2             
```


### 查看Docker版本

```shell
$ yum list docker-ce --showduplicates | sort -r
```

### 启动Docker并加入开机启动

```shell
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

### 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)

```shell
$ docker version
```


# 安装docker-compose

### 安装epel源
```shell
$ yum install -y epel-release
```

### 安装docker-compose
```shell
$ yum install -y docker-compose
```

# 使用docker-compose编排容器

### 创建docker-compose.yaml文件--example

```shell
version: '2'
services:
 zbx-app:
  image: ivixq/alpine-s6-edge-zabbix
  container_name: zbx-app
  ports:
   - 162:162/udp
   - 10051:10051/tcp
   - 10052:10052/tcp
   - 8081:80/tcp
  volumes:
 #  - /data/zbx.cfg/alertscripts:/etc/zabbix/alertscripts
   - /data/zbx.cfg/externalscripts:/etc/zabbix/externalscripts
  environment:
   - DEBUG_MODE=true
   - HTTP_FQDN=your ip
   - SMTP_SERVER=your smtp server
   - SENDER_MAIL_ADDR=your email address
   - EMAIL_PASS=email password
  restart: always
  networks:
   - zabbix-net
 
 zbx-db:
  image: ivixq/alpine-s6-edge-mariadb
  container_name: zbx-db
  volumes:
   - /var/lib/docker/data1/mysql/zabbix:/var/lib/mysql
  environment:
   - DEBUG_MODE=true
   - MYSQL_ROOT_PASSWORD=root password
   - MYSQL_USER=zabbix
   - MYSQL_DATABASE=zabbix
   - MYSQL_PASSWORD=zbxpass
  restart: always
  networks:
   - zabbix-net
 
networks:
  zabbix-net:
```

### 运行

```shell
$ docker-compose up -d nginx mysql
```
