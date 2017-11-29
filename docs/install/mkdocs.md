##### 安装python3.6.1

```shell
$ wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz
$ tar vxf Python-3.6.1.tgz
$ cd Python-3.6.1
$ ./configure --prefix=/usr/local/python3   #编译，自定义安装目录
$ make && make install  #过程很慢
$ ln -s /usr/local/python3/bin/python3 /usr/bin/  #建立软连接
$ python3 --version
$ pip --version #检查是否安装了pip
$ pip install --upgrade pip #更新pip

```

##### 安装pip 

```shell
$ wget https://pypi.python.org/packages/11/b6/abcb525026a4be042b486df43905d6893fb04f05aac21c32c638e939e447/pip-9.0.1.tar.gz#md5=35f01da33009719497f01a4ba69d63c9  #pip下载
$ tar zxf pip-9.0.1.tar.gz 
$ cd pip-9.0.1
$ python3 setup.py install

```

##### 安装mkdocs

```shell
$ pip install mkdocs
$ ln -s /usr/local/python3/bin/mkdocs /usr/bin
$ mkdocs --version
```

##### 创建项目

```shell
$ mkdocs new documents
$ cd documents && mkdocs serve #开启内置服务器
$ vi mkdocs.yml
$ site_name: Sunrise    #修改项目标题
$ theme: readthedocs    #修改文档模板
$ pages:                #添加页面
  - Home: index.md
  - About: about.md

$ mkdocs build  #生成html文档，html文件存储在site目录中
$ mkdocs build --clean  #去掉不需要的页面
```