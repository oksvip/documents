centos6.5安装node.js

1.下载源文件

```tex
$ wget http://nodejs.org/dist/v0.10.24/node-v0.10.24.tar.gz
$ tar zxvf node-v0.10.24.tar.gz
$ cd node-v0.10.24
$ ./configure --prefix=/usr/local/node
$ make && make install
$ echo "export PATH=$PATH:/usr/local/node/bin" >> /etc/profile
$ source /etc/profile
$ node -v
```

2.创建hello world

```javascript
var http = require('http');

http.createServer(function (request, response) {

    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```

3.执行hello world

```javascript
$ node server.js
Server running at http://127.0.0.1:8888/
```

