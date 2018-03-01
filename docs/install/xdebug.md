通过Xdebug配合phpstorm断点调试；

#### 安装Xdebug

```shell
$ wget https://xdebug.org/files/xdebug-2.6.0.tgz
$ tar -xzvf xdebug-2.6.0.tgz
$ cd xdebug-2.6.0
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config --enable-xdebug
$ make && make install
```

#### 修改php配置文件

```shell
$ vi /usr/local/php/php.ini
添加如下配置：
zend_extension=xdebug.so

[xdebug]
xdebug.profiler_enable=on
xdebug.trace_output_dir="/usr/local/php/xdebug"
xdebug.profiler_output_dir="/usr/local/php/xdebug"
xdebug.show_exception_trace=On
xdebug.max_nesting_level=200
xdebug.var_display_max_depth=5
xdebug.idekey=PHPSTORM
xdebug.remote_enable=1
xdebug.remote_host=localhost
xdebug.remote_port=10000
xdebug.profiler_enable=1

 service httpd restart
```

#### phpstorm配置
- [使用 PHPStorm 与 Xdebug 调试 Laravel (一)](https://www.imooc.com/article/13589)
- [使用 PHPStorm 与 Xdebug 调试 Laravel (二)](https://www.imooc.com/article/13590)
- [浏览器安装调试开关](https://confluence.jetbrains.com/display/PhpStorm/Browser+Debugging+Extensions)
