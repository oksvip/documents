让vim编辑器更适合编程

#### 下载vim

```shell
$ yum -y install vim-*
```

#### vim代码高亮

```bash
$ vim /etc/vimrc	# 编辑vim配置文件，添加以下配置
set nu
set showmode
set autoindent
set ts=4
set mouse=a
set expandtab
syntax on
```
#### 链接vi

```shell
$ ln -sf /usr/bin/vim /bin/vi
```

#### TAB切换四个空格

```shell
TAB替换为空格：
:set ts=4
:set expandtab
:%retab!

空格替换为TAB：
:set ts=4
:set noexpandtab
:%retab!
加!是用于处理非空白字符之后的TAB，即所有的TAB，若不加!，则只处理行首的TAB。
```

