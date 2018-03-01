#### 安装codeigniter框架

```shell
$ composer create-project codeigniter/framework
```

#### 去掉index.php

```shell
$ vim .htaccess

添加内容：
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]

```
