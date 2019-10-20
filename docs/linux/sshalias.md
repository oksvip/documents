# ssh和alias快速登录远程机器

#### 在local机器下的~/.ssh文件夹生成公钥和密钥:
```shell
$ ssh-keygen -t rsa -P ''
```

#### 首先确保remote机器中存在~/.ssh文件夹,不存在创建即可。创建好.ssh文件夹后,将local的公钥文件id_rsa.pub通过scp拷贝到远程机器remote中
```shell
$ scp .ssh/id_rsa.pub username@ip:/home/username/.ssh/id_rsa.pub
```

#### 登录到remote机器,进入~/.ssh文件夹,将从local机器复制来的的公钥文件id_rsa.pub追加到.ssh文件夹的authorized_keys文件中
```shell
$ cat id_rsa.pub >> authorized_keys
```

#### 保证authorized_keys文件权限对本用户是可读写的:
```shell
$ chmod u=rw authorized_keys
```

#### 退出remote机器,在local机器中重命名登录remote机器的命令,编辑当前shell的配置文件(bash的配置文件是~/.bashrc),并使alias生效
```shell
$ echo 'alias remote="ssh username@ip"' >> ~/.bashrc
$ source ~/.bashrc
```


# 或者使用ssh machine命令登录

#### 在.ssh/config文件里写入
```text
Host machine
HostName 192.168.1.1
User root
IdentitiesOnly yes
```

- 使用命令remote即可登录远程机器(第一次进入需要密码)
