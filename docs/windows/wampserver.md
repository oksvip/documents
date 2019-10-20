# wampserver安装php7.1.6

#### 1.下载php7.1.6-ts
```tex
http://windows.php.net/download#php-7.0  // 选择 VC14 x86 Thread Safe  64位选X64 32位选x86
```

#### 2.下载VC14运行库安装
```tex
https://www.microsoft.com/en-US/download/details.aspx?id=48145
```

3.在wamp/bin/php 下建立一个php7.1.6的文件夹把php7解压进去

复制php.ini-development 重命名为 php.ini

打开php.ini 查询

;extension_dir = "ext/"

修改为

extension_dir = "F:/wamp/bin/php/php7.1.6/ext/"

开启自己需要的扩展

复制php.ini 重命名为 phpForApache.ini

4.复制 wamp/bin/php 下原版本wampserver.conf 复制到 php7.1.6文件夹下

5.打开 php7.1.6文件下的 wampserver.conf

将所有phpX_module phpXapache.dll   phpXapache2_3.dll    phpXapache2_4.dll

更换为php7_module php7apache.dll   php7apache2_3.dll    php7apache2_4.dll

重新启动wampserver 选择php php7.1.6
