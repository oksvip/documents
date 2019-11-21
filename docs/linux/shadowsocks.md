# 一、安装Shadowsocks服务

#### 方案一：

搬瓦工VPS为我们准备了Shadowsocks的一键安装，直接在控制面板中有，非常方便。

1.只需在左边最下面选择Shadowsocks Server
2.然后选择Install Shadowsocks Server
3.等待安装完成后选择Go Back
4.现在可以看到加密协议默认aes-256-cfb,端口默认443 ,密码随机

如果是自己用，到这里就可以使用了，直接在客户端填好这些配置信息就好了。


#### 方案二：

如果不在控制面板上安装或者是在其他没有一键安装的VPS上，可以使用命令安装。

Debian/Ubuntu:

```shell
$ apt-get install python-pip
$ pip install shadowsocks 
```
CentOS:

```shell
$ yum install python-setuptools && easy_install pip
$ pip install shadowsocks 
```

#### 优化Shadowsocks性能

1.在终端通过ssh连上vps（Windows可以用putty连，Mac直接在终端就可以了）
2.在终端输入vi /etc/sysctl.d/local.conf
3.创建配置文件
4.按i插入
5.插入以下内容

```tex
# max open files
fs.file-max = 1024000
# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
net.ipv4.tcp_max_tw_buckets = 5000
# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1

# for high-latency network
net.ipv4.tcp_congestion_control = hybla
# forward ivp4
net.ipv4.ip_forward = 1
```

5.然后按Esc退出编辑，按shift+:，输入wq，回车，就保存退出了。

6.配置生效：

```shell
$ sysctl --system
```

# 配置多用户

如果想多用户使用的话就需要更改配置。

1.首先通过ssh连上vps
2.在终端输入vi /etc/shadowsocks.json创建配置文件
3.按i插入
4.插入以下内容（用户数任意，注意最后一个用户密码后面没有逗号）

```tex
{
 	"server":"my_server_ip",  #填入你的IP地址
	"local_address": "127.0.0.1",
	"local_port":1080,
	"port_password": {
  	    "8381": "foobar1",    #端口号，密码
  	    "8382": "foobar2",
   		"8383": "foobar3",
    	"8384": "foobar4"
	},
 	"timeout":300,
	"method":"aes-256-cfb",
 	"fast_open": false
}
```
上面开了四个端口（用户）为8381-8384，密码分别为foobar1-foobar4，你还需要填入你的IP地址。

下面是详细配置说明：

name | 价格
-|-
server | 服务器地址，填ip或域名 |
local_address | 本地地址 |
local_port | 本地端口，一般1080，可任意 |
server_port | 服务器对外开的端口 |
password | 密码，可以每个服务器端口设置不同密码 |
port_password | server_port + password ，服务器端口加密码的组合 |
timeout | 超时重连 |
method | 默认: “aes-256-cfb”，见 Encryption |
fast_open | 开启或关闭 TCP_FASTOPEN, 填true / false，需要服务端支持 |


1.然后按Esc退出编辑，按shift+:，输入wq，回车，就保存退出了。
2.建议使用后端启动

```shell
- 前端启动：ssserver -c /etc/shadowsocks.json
- 后端启动：ssserver -c /etc/shadowsocks.json -d start
- 停止：ssserver -c /etc/shadowsocks.json -d stop
- 重启(修改配置要重启才生效)：ssserver -c /etc/shadowsocks.json -d restart
```

3.设置开机启动
```shell
- 在终端输入vi /etc/rc.local，
- 把里面最后的带有ssserver的一大段默认的代码删除掉，
- 再把ssserver -c /etc/shadowsocks.json -d start加进去，
- 按wq保存退出。
```
4.到此就配置好了，试试多用户运行吧！


#### 附上单用户的配置信息：/etc/shadowsocks.json

```tex
{
    "server":"my_server_ip",  
    "server_port":8381,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"foobar1",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```


# 二、安装Shadowsocks客户端

### 安装epel源、安装pip包管理
```shell
$ sudo yum -y install epel-release
$ sudo yum -y install python-pip
```

### 安装Shadowsocks客户端
```shell
$ sudo pip install shadowsocks
``````

# 配置Shadowsocks连接
### 新建配置文件、默认不存在
```shell
$ sudo mkdir /etc/shadowsocks
$ sudo vi /etc/shadowsocks/shadowsocks.json
```

### 添加配置信息：前提是需要有ss服务器的地址、端口等信息
```tex
{
    "server":"xxx.xxx.xxx.xxx",
    "server_port":2018,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"***********",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
```

### 配置自启动 
### 新建启动脚本文件/etc/systemd/system/shadowsocks.service，内容如下：
```tex
[Unit]
Description=Shadowsocks
[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/shadowsocks.json
[Install]
WantedBy=multi-user.target
```

### 启动Shadowsocks服务
```
systemctl enable shadowsocks.service
systemctl start shadowsocks.service
systemctl status shadowsocks.service
```

### 验证Shadowsocks客户端服务是否正常运行
```shell
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
```

### Shadowsock客户端服务已正常运行，则结果如下：
```tex
{
  "origin": "x.x.x.x"       #你的Shadowsock服务器IP
}
```

### 安装配置privoxy
### 安装privoxy
```shell
yum install privoxy -y
systemctl enable privoxy
systemctl start privoxy
systemctl status privoxy
```

### 配置privoxy
### 修改配置文件/etc/privoxy/config
```text
listen-address 127.0.0.1:8118 # 8118 是默认端口，不用改
forward-socks5t / 127.0.0.1:1080 . #转发到本地端口，注意最后有个点
```

### 设置http、https代理
# vi /etc/profile 在最后添加如下信息
```tex
PROXY_HOST=127.0.0.1
export all_proxy=http://$PROXY_HOST:8118
export ftp_proxy=http://$PROXY_HOST:8118
export http_proxy=http://$PROXY_HOST:8118
export https_proxy=http://$PROXY_HOST:8118
export no_proxy=localhost,172.16.0.0/16,192.168.0.0/16.,127.0.0.1,10.10.0.0/16
```

### 重载环境变量
```shell
source /etc/profile
```

### 测试代理
```shell
curl -I www.google.com
```

```tex
HTTP/1.1 200 OK
Date: Fri, 26 Jan 2018 05:32:37 GMT
Expires: -1
Cache-Control: private, max-age=0
Content-Type: text/html; charset=ISO-8859-1
P3P: CP="This is not a P3P policy! See g.co/p3phelp for more info."
Server: gws
X-XSS-Protection: 1; mode=block
X-Frame-Options: SAMEORIGIN
Set-Cookie: 1P_JAR=2018-01-26-05; expires=Sun, 25-Feb-2018 05:32:37 GMT; path=/; domain=.google.com
Set-Cookie: NID=122=PIiGck3gwvrrJSaiwkSKJ5UrfO4WtAO80T4yipOx4R4O0zcgOEdvsKRePWN1DFM66g8PPF4aouhY4JIs7tENdRm7H9hkq5xm4y1yNJ-sZzwVJCLY_OK37sfI5LnSBtb7; expires=Sat, 28-Jul-2018 05:32:37 GMT; path=/; domain=.google.com; HttpOnly
Transfer-Encoding: chunked
Accept-Ranges: none
Vary: Accept-Encoding
Proxy-Connection: keep-alive
```

### 取消使用代理
```shell
while read var; do unset $var; done < <(env | grep -i proxy | awk -F= '{print $1}')
```


# 三、一键安装SSR脚本


### 下载一键安装包
```shell
$ wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh && chmod +x shadowsocksR.sh && ./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

### 加密方式选择aes-256-cfb


### 开启BBR加速

```shell
$ wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```

### 重启后查看是否启动了BBR
```shell
$ lsmod | grep bbr
```
