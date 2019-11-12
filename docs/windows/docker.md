## windows 下使用docker

#### 下载[Docker-toolbox](http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/DockerToolbox-18.03.0-ce.exe)

#### 点击桌面Docker Quickstart Terminal进行初始化

#### 测试是否安装成功

```shell
$ docker --version
$ docker-compose --version
```

#### docker toolbox镜像加速

```shell
$ docker-machine ssh default
```

> 执行如下命令，将镜像地址替换为 https://3ulaxpq7.mirror.aliyuncs.com

```shell
$ sudo sed -i "s|EXTRA_ARGS='|EXTRA_ARGS='--registry-mirror=https://3ulaxpq7.mirror.aliyuncs.com |g" /var/lib/boot2docker/profile
```

#### 退出服务器容器

```shell
$ exit
```

#### 重启容器后再尝试下载镜像就很快了

```shell
$ docker-machine restart default
$ git clone https://github.com/laradock/laradock.git
$ cd laradock
$ docker-compose up -d nginx mysql
```




