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
$ echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
$ source /etc/bashrc
```

##### 4）查看版本号

```tex
$ git --version
git version 2.1.2
```