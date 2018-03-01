# 如何用 gitbook 结合 Markdown 写一本开源书籍

直接进入正题。

1. 安装 gitbook 并写书#

首先，安装。
```shell
$ npm install -g gitbook-cli
```
如果你没有 nodejs 环境，你就去官网下载安装一下，安装完就有 npm 命令了。

新建一个目录放书的内容，比如叫 webpack，这个目录里是放书的内容。
```shell
$ mkdir webpack
$ cd webpack
```
然后初始化生成文件。
```shell
$ gitbook init
```
生成了下面两个文件。
```text
.
├── README.md
└── SUMMARY.md
```
其中 README.md 是放书的说明，SUMMARY.md 是放书的目录。

看了我的内容，你应该就会明白如何写的。

```tex
README.md

# webpack 3 零基础入门教程

最详细，最简单的零基础 webpack 3 入门教程，人人都能学会。

原文发布于我的个人博客：https://www.rails365.net

源码位于：https://github.com/yinsigan/webpack-tutorial

电子版: [PDF](https://www.gitbook.com/download/pdf/book/yinsigan/webpack-3) [Mobi](https://www.gitbook.com/download/mobi/book/yinsigan/webpack-3) [ePbu](https://www.gitbook.com/download/epub/book/yinsigan/webpack-3)

```

```tex
# Summary

* [0. 开始](README.md)
* [1. 介绍](chapters/1.md)
* [2. 安装](chapters/2.md)
* [3. 实现 hello world](chapters/3.md)
* [4. webpack 的配置文件 webpack.config.js](chapters/4.md)
* [5. 使用第一个 webpack 插件 html-webpack-plugin](chapters/5.md)
* [6. 使用 loader 处理 CSS 和 Sass](chapters/6.md)
* [7. 初识 webpack-dev-server](chapters/7.md)

```
目录结构：
```text
.
├── README.md
├── SUMMARY.md
└── chapters
    ├── 1.md
    ├── 2.md
    ├── 3.md
    ├── 4.md
    ├── 5.md
    ├── 6.md
    └── 7.md
```
在终端运行：
```shell
$ gitbook serve
```

这样，可以用浏览器打开 http://localhost:4000，一边写书，一边看效果。

2. 把书的源码放到 github 上#

现在我们准备把写好的书，做成一个 git 仓库，再放到 github 上。
```shell
$ git init
$ git add .
$ git commit -m "first commit"
```
新建一个文件 .gitignore。

内容如下：
```tex
# Node rules:
## Grunt intermediate storage (http://gruntjs.com/creating-plugins#storing-task-files)
.grunt

## Dependency directory
## Commenting this out is preferred by some people, see
## https://docs.npmjs.com/misc/faq#should-i-check-my-node_modules-folder-into-git
node_modules

# Book build output
_book

# eBook build output
*.epub
*.mobi
*.pdf
```
这是为了防止提交一些不必要的内容。

在 github 上新建一个仓库叫 webpack-tutorial。
```shell
$ git remote add origin git@github.com:yinsigan/webpack-tutorial.git
$ git push origin master
```
3. gitbook 新建书#
进入 https://www.gitbook.com/ 网站。
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/468/2017/24b1cc70c94e9195fb175d85ba1626d4.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/470/2017/5af03ab00272a0c0d5d0ec90c192d93e.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/471/2017/c9d8aa5c7deb3a226e66160b984e7015.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/473/2017/d0eebeeff005439c6545b720b57e2cbe.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/474/2017/366286fab917322f0819d585ad88be67.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/475/2017/e313a3086ecc35a5ee47ace0f306c6b5.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/476/2017/2acfd1447d1c3cf7ed05f21975cbff2b.png)
现在可以在线浏览了。

网址为：https://www.gitbook.com/read/book/yinsigan/webpack-3

4. 绑定个性域名#
你如果有你自己的域名也是可以绑定过来的。
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/477/2017/b7296c8b29758fb5a53f7607dfbafe3c.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/478/2017/b0d8b0be48ecfe7967c7f0ef86143774.png)
![enter image description here](https://rails365.oss-cn-shenzhen.aliyuncs.com/uploads/photo/image/479/2017/5076310c653081f343db1a92536b7b33.png)


