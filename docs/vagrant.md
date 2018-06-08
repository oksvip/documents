#### Windows安装Vagrant

##### 安装Vagrant和Virtualbox（windows下对软件版本有要求，搭配不好有坑）

```tex
1、vagrant_1.8.1.msi
2、VirtualBox-4.3.12-93733-Win.exe
3、centos-6.5_chef_32.box
```

##### 修改VirtualBox的镜像文件存放位置

```tex
1、打开VirtualBox，打开管理-> 全局设置 （快捷键是 Ctrl-G ）
2、选择 常规 里的 默认虚拟电脑位置(M)
3、设置为非系统盘的位置
```

##### 添加box到本地仓库

```shell
$ vagrant box add 自定义你的box名称  box路径

1、使用http远程添加
$ vagrant box add my_first_box  https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box

2、使用本地box文件
$ vagrant box add my_first_box D:/centos-7.0-x86_64.box

3、使用中央仓库名称
$ vagrant box add my_first_box hashicorp/precise64
```

##### 查看已添加的box

```shell
$ vagrant box list
```

##### 删除box

```shell
$ vagrant box remove your_box_name
```

##### 初始化虚拟机

```shell
$ vagrant init [box_name]

1、使用http绝对地址远程初始化
$ vagrant init https://github.com/tommy-muehle/puppet-vagrant-boxes/releases/download/1.1.0/centos-7.0-x86_64.box

2、使用本地box
$ vagrant init my_first_box

3、使用Vagrantfile文件
$ git clone https://github.com/coreos/coreos-vagrant.git

```

##### 启动虚拟机

```shell
$ vagrant up [box_name]

注： 
1、在vmware上：
$ vagrant up  –provider=vmware_fusion [vm_name]

2、在AWS上：
$  vagrant up  –provider=aws [vm_name]
```


##### 查看虚拟机状态

```shell
$ vagrant status
```

##### 登录虚拟机

```shell
$ vagrant ssh [vm_name]
```

##### 内置账户

```tex
username:vagrant 
password:vagrant

username:root 
password:vagrant
```

##### 虚拟机挂起

```shell
$ vagrant suspend [vm_name]
```

##### 关闭虚拟机

```shell
$ vagrant halt [vm_name]
```

#### 重启虚拟机

```shell
$ vagrant reload [vm_name]
```

##### 查看已创建的所有虚拟机

```shell
$ vagrant global-status
```


##### 销毁虚拟机

```shell
$ vagrant destory [vm_id]

注：1 . 虚拟机删除后，所有在虚拟机中做的改动都不再存在，慎用。 
　　2 .执行销毁虚拟机命令后，box文件仍然是存在的，彻底解放硬盘空间可以执行 
$ vagrant box remove your_box_name删除box文件
```

##### 新建快照

```shell
$ vagrant snapshot save your_snapshot_name
```

##### 查看快照

```shell
$ vagrant snapshot list
```

##### 恢复快照

```shell
$ vagrant snapshot restore your_snapshot_name
```

##### 删除快照

```shell
$ vagrant snapshot delete your_snapshot_name
```

##### 配置共享文件夹

```tex
config.vm.synced_folder "D:/www","/www",disabled:true //禁用vagrant的默认共享目录
```


##### 配置网络

```shell
$ config.vm.network "forwarded_port", guest: 80, host: 8080

config.vm.network "private_network", ip: "192.168.10.10"
```

##### 导出虚拟机

```shell
$ vagrant package --base 要导出的虚拟机id也就是名称 --out xxx.box(这里是导出的文件名)
```
