## V2Ray搭建

[教程来源](https://www.stackcc.com/2019/04/02/v2raysetup/)

```shell
$ sudo yum -y install wget zip unzip -y && wget https://install.direct/go.sh && sudo bash go.sh
```

## 启动/停止/重启命令

```shell
$ sudo systemctl start v2ray  启动
$ sudo systemctl status v2ray 查看启动状态
$ sudo systemctl stop v2ray	  关闭
```

## 配置

```shell
$ sudo vim /etc/v2ray/config.json
```

## 关闭防火墙

```shell
$ systemctl stop firewalld  关闭
$ firewall-cmd --reload		重启
```
