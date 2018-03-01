远程连接Windows桌面；

##### 安装Freerdp

```shell
$ yum -y install freerdp
```

##### 自定义桌面大小连接远程桌面(使用快捷键ctrl+alt+enter切换全屏和半屏)

```shell
$ xfreerdp -g 1366x768 -u administrator 192.168.0.1
```

##### 全屏连接远程桌面

```shell
$ xfreerdp -f -u administrator 192.168.0.1
```
#### xfreerdp 提供了很多插件

cliprdr（开启粘贴功能）

--plugin cliprdr

```shell
$ xfreerdp -f --plugin cliprdr -u administrator 192.168.1.10
```

#### rdpdr（开启本地文件夹映射为远程服务器磁盘功能）

--plugin rdpdr --data disk:<Name>:<Path> --

例如将本地 /home/username/share 作为共享文件夹：
```shell
xfreerdp -f --plugin rdpdr --data disk:data:~/share -- -u administrator 192.168.0.1
```

