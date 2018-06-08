版本控制软件；

##### 卸载Centos自带的git1.7.1

```tex
$ rpm -qa git
  git-1.7.1-4.el6_7.1.i686
$ rpm -e --nodeps git-1.7.1-4.el6_7.1.i686
$ yum remove git
```

##### 安装git并将git添加到环境变量中

```tex
$ cd /usr/local/soft
$ wget https://github.com/git/git/archive/v2.12.0-rc1.tar.gz
$ tar -xzvf git-2.1.2.tar.gz
$ cd git-2.1.2
$ make prefix=/usr/local/git all
$ make prefix=/usr/local/git install
$ ln -sf /usr/local/git/bin/git /usr/local/bin
```

##### 4）查看版本号

```tex
$ git --version
git version 2.1.2
```

##### 5) 设置提交不需输入帐号密码

```tex
$ git config --global credential.helper store
$ git push // 输入帐号密码，以后就不用输入了
```


