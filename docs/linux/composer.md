## Linux下安装composer

#### 下载composer
```shell
$ wget https://mirrors.aliyun.com/composer/composer.phar -O /usr/local/bin/composer
```

#### 添加权限
```shell
$ chmod a+x /usr/local/bin/composer
```

## Windows下安装composer

#### 直接下载[composer.phar](https://mirrors.aliyun.com/composer/composer.phar)文件

#### 把下载的 composer.phar 放到 PHP 安装目录

#### 在composer.phar文件夹下新建 composer.bat, 添加如下内容，并运行：
```text
@php "%~dp0composer.phar" %*
```

#### 测试安装是否成功
```shell
$ composer -V
```

#### 升级版本
```shell
$ composer selfupdate
```

- 注意 selfupdate 升级命令会连接官方服务器，速度很慢。建议直接下载我们的 composer.phar 镜像，每天都会更新到最新。

#### 取消配置
```shell
$ composer config -g --unset repos.packagist
```

#### 切换国内源
```shell
$ composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```