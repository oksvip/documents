[TOC]

### 笔试题

- 列举出几种你知道的 PHP 遍历或者迭代数组的方法

  ```tex
  for, foreach, while，array_walk, array_map
  ```

- 解释什么是 `XSS`、`CSRF`、`SQL` 注入以及如何防范

  ```tex
  XSS是跨站脚本攻击，对html标签及特殊符号进行转义或者添加html标签黑名单
  CSRF是跨站请求伪造，使用请求令牌或对HTTP Refer的值进行验证
  SQL注入：通过在表单中或参数的查询字符串填写包含 SQL 关键字的数据来使数据库执行非常规代码，避免使用拼接字符串的方式生成sql语句，使用数据库参数绑定进行sql拼接
  ```

- 在浏览器中输入网址到页面显示，期间发生了哪些过程

  ```tex
  1.浏览器解析url
  2.应用层DNS解析域名
  3.应用层客户端发起HTTP请求
  4.传输层TCP传输报文
  5.网络层IP协议查询MAC地址
  6.数据到达数据链路层
  7.服务端接收数据
  8.服务器响应请求
  9.服务器返回相应文件
  10.浏览器渲染页面
  ```

- 一张采用 `Innodb` 的 `User` 表，其中 `id` 为主键，`name` 为普通索引，试从**索引的数据结构**角度分析，以下两条语句（**均返回一条记录**）在检索过程中有哪些区别

  > Sql 1：SELECT id,name,address FROM User WHERE name = 'smith';
  >
  > Sql 2：SELECT id,name,address FROM User WHERE id = 1;

  ```tex
  因为普通索引是二级索引，所以SQL1会先通过普通索引查找到该条数据的主键id，再通过主键id查询数据
  SQL2可以直接通过主键id查询出数据，不需要回表
  ```

- 现有一统计网站独立访客的需求，流量百万以上，如以 IP 为标识，可以查看当天实时或者指定某天的 IP 数（需要去重），采用 `MySQL` 来实现，那么：

  - 5.1 你会如何设计表和索引？（文字、sql 均可，方案尽可能高效）

    ```tex
    使用日期进行分表，将ip转成整型存储，设置日期和ip为联合索引
    ```

  - 5.2 数据如何入库，当天实时和某天数据该如何查询？（写出 sql 语句）

    ```tex
    利用redis队列缓存然后mysql批量入库
    select count(ip) from view_log_{yyyymm} where date={yyyy-mm-dd}
    ```



### 面试题

- 使用框架的组件用过哪些

  ```tex
  Eloquent ORM
  Predis
  Carbon
  Migration
  Factory
  Seeder
  ```

- http 3握手4挥手

  ```tex
  第一次握手：服务端确认对方发送正常
  第二次握手：服务端确认自己发送正常，客户端发送正常。
  第三次握手：服务端确认自己发送/接受正常，客户端发送接受正常。
  
  为什么3次握手而不是2次握手：
  为了实现可靠数据传输， TCP 协议的通信双方， 都必须维护一个序列号， 以标识发送出去的数据包中， 哪些是已经被对方收到的。 三次握手的过程即是通信双方相互告知序列号起始值， 并确认对方已经收到了序列号起始值的必经步骤。
  如果只是两次握手，至多只有连接发起方的起始序列号能被确认，另一方选择的序列号则得不到确认。
  ```

- header头中都包含哪些信息

  ```tex
  Accept:浏览器能够接受的内容类型
  Accept-Charset:浏览器能够显示的字符集
  Accept-Encoding：浏览器能够处理的压缩编码
  Accept-Language：浏览器当前设置的语言
  Connection：浏览器与服务器之间连接的类型
  Cookie：当前页面设置的任何Cookie
  Host：发出请求的页面所在的域
  Referer：发出请求的页面的URL
  User-Agent：浏览器的用户代理字符串
  ```

- redis在项目中用来做什么了

  ```tex
  1.项目缓存
  2.异步队列
  ```

- 常用的数据结构和算法都有哪些

  ```tex
  数据结构：数组、栈、队列、链表、树、散列表、堆、图
  算法：冒泡排序、快排
  
  https://blog.csdn.net/weixin_42288131/article/details/81458963
  一、线性表
    1.数组实现
    2.链表
  二、栈与队列
  三、树与二叉树
    1.树
    2.二叉树基本概念
    3.二叉查找树
    4.平衡二叉树
    5.红黑树
  四、图
  ```

- InnoDB引擎为什么采用的是B+树而不是其他树

  ```tex
  1、InnoDB存储引擎的最小存储单元是页，页可以用于存放数据也可以用于存放键值+指针，在B+树中叶子节点存放数据，非叶子节点存放键值+指针。
  
  2、索引组织表通过非叶子节点的二分查找法以及指针确定数据在哪个页中，进而在去数据页中查找到需要的数据；
  
  因为B树不管叶子节点还是非叶子节点，都会保存数据，这样导致在非叶子节点中能保存的指针数量变少。指针少的情况下要保存大量数据，只能增加树的高度，导致IO操作变多，查询性能变低；
  ```

- InnoDB的一棵B+树可以存放多少行数据？

  ```tex
  约2千万
  ```

- 有个搜索功能，怎么设置表结构或索引

- 非sql数据库用过哪些，有啥区别

  ```tex
  redis和memcache
  区别：
  1、存储方式： 
  memecache 把数据全部存在内存之中，断电后会挂掉，数据不能超过内存大小 
  redis有部份存在硬盘上，这样能保证数据的持久性，支持数据的持久化。 
  2、数据支持类型： 
  redis在数据支持上要比memecache多的多。 
  3、使用底层模型不同： 
  新版本的redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。 
  4、运行环境不同： 
  redis目前官方只支持LINUX 上运行，从而省去了对于其它系统的支持，这样的话可以更好的把精力用于本系统 环境上的优化，虽然后来微软有一个小组为其写了补丁。但是没有放到主干上
  ```

- redis 集群添加新节点需要做哪些操作

  ```tex
  1. 启动集群服务（向集群添加新节点，则说明，集群是已知的）。
  2. 搭建将要添加到集群的节点
  　（1）以集群的方式对新添加的节点进行配置：redis.conf.
  　（2）启动节点实例服务.
  3.集群管理
  ```

- mysql 隔离级别，分别是什么，有没有实操过

  ```tex
  读已提交
  读未提交
  可重复读（Mysql默认的，解决了不可重复读和幻读的问题）
  串行化
  ```

- mysql事务的基本要素有哪些

  ```tex
  1.原子性（Atomicity）：事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。
  2.一致性（Consistency）：事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。
  3.隔离性（Isolation）：同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。
  4.持久性（Durability）：事务完成后，事务对数据库的所有更新将被保存到数据库，不能回滚。
  ```

- http2.0与http1.X的区别

  ```tex
  Http1.x：
  缺陷：线程阻塞，在同一时间，同一域名的请求有一定数量限制，超过限制数目的请求会被阻塞。
  
  Http/2.0：
  1.采用二进制格式而非文本格式；
  2.完全多路复用，而非有序并阻塞的、只需一个连接即可实现并行；（解决了线头阻塞的问题，与Http1最重要的区别）
  3.使用报头压缩，降低开销
  4.服务器推送
  ```

- nginx和apache的区别，为什么选择用nginx

```tex
轻量级，同样起web 服务，比apache 占用更少的内存及资源 
抗并发，nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能 
高度模块化的设计，编写模块相对简单 
社区活跃，各种高性能模块出品迅速啊 
apache 相对于nginx 的优点： 
rewrite ，比nginx 的rewrite 强大 
模块超多，基本想到的都可以找到 
少bug ，nginx 的bug 相对较多 
```





#### 一个16年毕业生所经历的php面试


时间点：2017-11


## 一、什么是面试

说到面试，还是先说你为什么要离职，

- [什么情况下你会毫不犹豫地辞职？][1]浏览数已经达到51826523次了，5000w+，o_o！

关键字：成长、发展、委屈、领导、钱（工资）`突发感想是不是可以抓取下然后分析关键字，哈哈哈`

还有

- [「宁愿花 11K 重新招人，也不愿意花 9K 留住老员工」的现象是否属实？为什么会出现该现象？][2]

还想看吗

- [离职前一定要找好下家吗？公司招聘时一般如何看待裸辞和骑驴找马？][3]

我的理解：面试不是高考，高考只有一次（不说补习），面试可以有n次，只要有面试机会，你就可以一直去面，面到吐为止都没关系，不要怕失败，
公司没选择你不是你不优秀，而是你不符合他们的要求，回家思考下面试不足的地方，调整下心态，准备下个面试才是你正确的做法。

## 二、面试准备

准备是多方面的，俗话说：成功只留给有准备的人

### 1. 问：什么时候开始准备？

你是牛人吗 `?` 不用准备，等猎头挖 `:` 老老实实的随时准备好

### 2. 问：怎么准备？

- 项目经验是一部分：面试是离不开的，充分理解自己所做的那部分，能在面试中清楚的表述出自己做了什么，充当什么样的角色
- 自己分享知识的地方：比如博客/github，自己总结的会比去看别人的总结效果好不只一丁半点
- 基础知识：字符串操作、数组操作、文件操作、正则操作
- 进阶知识：面向对象、数据结构和算法、设计模式、mysql索引、mysql引擎、mysql事务、mysql锁
- 高阶知识：linux+nginx+mysql+php+redis **优化**，只会操作没用，谁都会，高并发、分布式系统、负载均衡、分库分表、消息队列

## 三、面试干货

### 1、某教育机构`两面`

#### mysql事务是什么

```php
我说了mysql的四个特性，原子性、一致性、隔离性、持久性，事务可以理解成一次操作要不完成要不失败。
面试官问了隔离性和锁的问题，有点忘了，这个真没答上来
-------------
可以参考下《Innodb中的事务隔离级别和锁的关系》https://tech.meituan.com/innodb-lock.html
```

#### php代码解释过程

```php
我说了通过zend引擎解析成opcode，然后转化成机器识别的代码。
平时确实没有去关注这个，失败失败，查了资料
鸟哥写的《深入理解PHP原理之Opcodes》http://www.laruence.com/2008/06/18/221.html
```

#### 百度统计的实现原理

```php
我没答上来，自己后面查的
-------------
参考：http://blog.csdn.net/iqzq123/article/details/8877645
```

#### 如何共享session

```php
我说了通过redis存储session，以达到共享目的，后面查了下方案，还挺多的
-------------
参考：http://www.cnblogs.com/wangtao_20/p/3395518.html#commentform
```

#### git分支管理策略

```php
我写了master作为主分支，dev作为开发分支，bug_fix分支作为bug分支
面试官说有个临时bug需要改，而我们在dev上已经开发了很多内容了，我答的是用bug_fix分支拉master代码，
再合回去，面试官说dev怎么办，改了相同的模块会冲突想了下，确实会，然后求教面试官，
面试官说可以用git rebase变基实现，自己见过这个命令，但是没用过，尴尬
```

#### restful设计

```php
我说了get、post、put、patch、delete，面试官问他们分别怎么用，毕竟自己做api的
就回答了get是从服务器取资源、post是新建资源、put是
更新完整资源、patch更新部分资源、delete是删除资源，就过了
```

#### 附加题：论坛的表设计

```php
有点忘了，我答的不是很好，问的是有以下几个需求，请问需要建几张表，为什么？主要考的如何合理的设计表，
比如用户登录信息表可以怎样设计？发帖表和回帖表需要怎样设计？内容字段比较大，怎样设计更好？哪些字段需要加索引？
```

#### 情景题：一个登录系统的改进

```php
用伪代码实现
第一步：PC端只有两个表单框和注册按钮，后端接收参数，再存入数据库
第二步：添加移动端，需要发送短信
第三步：加入第三方登陆方式，需要发送邮件
第四步：有个兄弟公司，给了我们一张execl表，里面是用户信息，需要后台注册这些用户，如何修改现有的代码
第四步：有点忘了，好像是如何进一步优化

这里第三步就答不好了，考察的是逻辑能力和代码组织能力，设计模式的重要性。
```

#### 自己最满意的代码

```php
自己自由发挥，项目中遇到的应该是最好答的
```

#### 了解哪些设计模式，实际用例

```php
我的回答
工厂模式：定义一个标准，用到的类可以按这个标准实现相应功能
单例模式：防止重复实例化，减少资源调用
数据映射：数据库ORM应用
适配器模式：兼容老数据，多态的应用

面试管问了怎么兼容老数据，考察codereview能力
```

困了，先到这里

---

### 2、某电商公司`三人面`

笔试题挺有意思的，比较能考察出我这样的面试者水平

笔试题的解法见php/php7.php文件中的24、25、26

#### 将一个二维数组的值首字母大写

```php
题目意思大概是这样的，印象不深很清了
当时做的时候好像做错了，现在想了下，思路是获取键值和值，然后双重循环转大写即可。
```

#### 使用正则获取html里的href属性的值和a标签内的值，并以href值为key，a标签内的值为value存入二维数组中

大概是这样的结构，我简化了

```php
<ul class="attr">
    <li>
        <a href="www.baidu.com">百度baidu</a>
        <a href='www.tecent.com'>腾讯tengxun</a>
        <a href="www.alibaba.com">阿里巴巴alibaba</a>
    </li>
</ul>

考察正则表达式的运用和数组的拼装，我当时做的稀烂，回去稍微看了下正则，就很容易了。
```

#### 优化语句查询

test表中数据有500w，字段有id/t_id/type_id/plat_id，语句为`select max(t_id) from test where type_id=1 and plat_id=1`

```php
考察mysql语句优化，这里主要是优化max函数，max函数会导致全表扫描，效率会很低，可以使用order by加limit进行优化

我是这样答的
select t_id from test where type_id=1 and plat_id=1 order by t_id desc limit 1;
当然还可以使用加索引进一步优化速度，这里可以加上(type_id/plat_id)联合索引。
-------------
正确做法是给`t_id`加索引，还有(type_id/plat_id)联合索引，order by 并不能避免全表扫描。
```

#### 找出N个数中的第M大的数

```php
考察算法，貌似是查找？
我当时的想法是先对数组排序，然后索引的N-M的数就是M大的数
```

#### Ajax跨域请求时，会出现什么问题？如何解决

```php
我的回答：稀烂，跨域出现问题会出现请求拒绝，是出于安全起见，设置Access-Control-Allow-Origin为*即可。哈哈哈naive
-------------
回去搜了下，方案还挺多的。
参考https://dailc.github.io/2017/03/22/ajaxCrossDomainSolution.html
```

#### 如何优化一个CPU运算密集的网站

```php
没有实践过对吧，没关系，会google吗？会背吗？脑子能留个大概的原理吗？
```

#### 自己最满意的 代码

```php
自己自由发挥，项目中遇到的应该是最好答的
```

#### 其它

简历上的项目询问，项目中最有难度的地方？如何解决的？

困了，睡

---

### 3、某民宿杭州分公司`一面`

没有笔试题，纯面试题，很注重基础，比如操作系统

#### phpunit的用法

```php
自己平时没注意这个东西，所以答的稀烂，问到有没有连数据库测，我说有，只是用预期数据与数据库查出的数据对比测试
面试官问有没有用过mock
懵逼了，这个概念确实听过，没去做过，意思是造假的意思，通过模拟数据库操作来达到测试目的，还有stub站桩，需要多实践啊骚年 o_o
-------------
参考：https://phpunit.de/manual/current/zh_cn/test-doubles.html
```

#### redis异步队列实现细节

看我简历中写用过

```php
我当时用的是laravel中的队列机制，通过dispatch()触发任务，php artisan queue:work 开启后台进程监视队列并完成任务
面试官不满足，问我他的原理，怎样实现的？
我说用的是list，通过触发lpush入队，然后依次rpop出队处理任务
面试官说这是阻塞的，能不能有高效的做法，我说不清楚，面试官好像说可以把一个list的数据放到多个list中并行处理，zzz。
```

#### redis中zest如何根据两个属性排序，比如id和age

```php
不知道，只知道根据score分值排序，有知道的同学可以留言
请自行google   redis zset 多字段排序
-------------
查了下，有说先自行排序后存入redis
https://segmentfault.com/q/1010000004669705
```

#### 你对多进程和多线程还有协程的理解

作为cs专业的phper，大学学的都交给老师了，zzz

```php
进程可以有多个线程，在php中yield可以实现协程，面试官问swoole新版本中自带协程，你怎么理解？
没研究过zzz
-------------
进程是正在运行的程序的实例
进程是内核分配资源的最基本的单元
线程是内核执行的最基本的单元
进程内可以包含多个线程
协程的话相当于语言自己实现一个函数调度
参考：http://www.cnblogs.com/lxmhhy/p/6041001.html
```

#### 说说怎么理解现在前端框架中的组件化和模块化

看到我会react.js问的

```php
我说就比如react里的组件可以理解为一个class类，模块的意思是一个文件就是一个模块，有单独的作用域
-------------
参考：http://xiaodongtongxue.top/2016/05/20/浅谈前端自动化%20工程化%20组件化%20模块化/
```

#### http协议中get和post的区别，怎么实现的

```php
get是获取资源，post创建资源
get数据长度有限制，post无限制
get数据在url中安全性差,post不显示在url中更安全
怎么实现的，懵逼zzz
-------------
参考：https://zhuanlan.zhihu.com/p/22536382
```

#### 最近在看什么书

```php
送命题，没看过就老实说没看过好了，不然会xxxx，比如我说了看了《图解HTTP》，首先问了我上面那道题，然后说你看的书不够深入，我。。。。
```

### 4、某众筹杭州分公司`三面`

#### php使用什么mysql连接？

```php
还以为很简单呢，直接说了mysqli和pdo，mysql弃用了
面试官来了句不是问这个，是问连接池
我对这个概念很懵，就回了是持久连接吗，可以用mysql_connect()
面试官再问比如laravel默认使用什么mysql连接的
我猜的是pdo吧
他说再想想
我xxxx，就这样过了
-------------
回去很郁闷的查了下，默认PDO，我还以为是想考察关于php编译时mysqlnd这样的呢
All database work in Laravel is done through the PHP PDO facilities
so make sure you have the driver for your particular database of
choice installed on your machine before you begin development.
```

#### 场景题：索引的建立规则和explain

有这样一张表 自增id、名字、昵称、年龄、客户类型、创建时间

哪些字段需要建立索引？为什么？

```php
我说年龄、创建时间、客户类型需要建立索引，where条件经常用到的字段
面试官说客户类型用的enum枚举呢，是不是也需要建立索引
我说不清楚
面试官说其实是不需要的，枚举类型只有几种，mysql查询时会分组查，一般写sql前可以用explain观察sql语句
```

#### 你用的是php版本是哪个？为什么？php7有什么更新？

```php
很幸运，自己一直用的是php7，因为php7速度有很大提升，也有很多新特性，比如标量类型声明、返回类型声明
命名空间、Trait、自动加载都是现代php所需要的
面试官问有自己发不过composer包吗？他的自动加载原理是怎么样的？
自己创建过composer包，没发布，自动加载是通过sql_autoload_register()实现的
面试官说之前都是include引入的对吧，你应该在往前想下
我xxxx
面试官说是当这个文件使用的时候，才会加载进来，以达到自动加载目的。
```

#### nginx相关

简历写了了解nginx负载均衡和反向代理，你是怎么做的

php-fpm能代理其他端口吗？除了默认9000

nginx配置php-fpm的时候走的是什么协议？还可以走其他协议吗？

```php
自己搭建的vps，负载均衡是通过多个端口实现的，反向代理是代理到apache
可以走其他端口，需要配置ini文件，走的http协议，其它协议，不知道
面试官说可以走tcp协议，比如socket
```

#### 看你用过laravel和tp，比较下两款框架

```php
tp是国人写的，理念比较陈旧过时
laravel是现在最火的php框架，开源社区活跃，工具也最多，运用了面向对象的思想和很多设计模式，是值得学习和运用的选择
laravel的源码看过吗？
看过，laravel的container容器，还有ioc控制反转、di依赖注入，通过server provider服务提供者bind绑定实例放入到容器内，
然后通过make解析容器中的某个实例，可以通过facade门面直接静态调用。
面试官说怎么兼容之前的版本，比如像app那样，既发布了新的版本，老版本也需要兼容。
我没做过，我的想法是一通过适配器模式，达到兼容两者的需求，二可以使用dingo/api多版本控制
```

#### 你未来的发展规划，1年的，3年的

```php
自由发挥，其实也蛮重要的，往积极方面答总没错，比如深入php、扩展技术栈
```

#### 其他

简历上的项目询问，项目中最有难度的地方？如何解决的？

### 5、某旅游公司`两面`

套路真多，这面试官很有代入感

#### 为什么离职

机智点，按你填表里写的离职理由答就行，别露馅了，哈哈哈。

#### 如何选择PHP的？关于目前流行的Java比较？

这里机智点答就行，比较的话讲两者的使用场景，比如php适合web开发，java有多种选择，web和安卓

#### 有没有系统的培训过？有没有看php相关的网络课程和书籍？

```php
我以为是说我有没有去培训过，我当然说没了
我说的是慕课网的优秀课程和《Modern PHP》《PHP核心技术和最佳实践》《PHP the right way》
```

#### 看你用过laravel和tp， 比较下两款框架

```php
tp是国人写的，理念比较陈旧过时
laravel是现在最火的php框架，开源社区活跃，工具也最多，运用了面向对象的思想和很多设计模式，是值得学习和运用的选择
laravel的源码看过吗？
看过，laravel的container容器，还有ioc控制反转、di依赖注入，通过server provider服务提供者bind绑定实例放入到容器内，
然后通过make解析容器中的某个实例，可以通过facade门面直接静态调用。
```

#### php基础知识**

```php
php面向对象说下
封装、继承、多态
多态描述下
当时这里说成了重载了，真是扇自己一巴掌，其实多态是一套接口下面的实现类，注入的是接口类，使用的是实现类，从而实现多态
php的类型有哪些
还真的忘了，掌嘴，没说完整，说了array数组、string字符串、object对象、resource资源、NULL
```

#### laravel包**

```php
dingo/api和jwt-auth是自己搭的吗？有没有遇到坑？
当时项目赶，用的集成的，自己搭也是没问题的
entrust怎么用的？有哪些表？
user用户表、role角色表、perm权限表、role-user用户角色关联表、role-perm角色权限关联表
我们还做了扩展，menu菜单表、menu-perm菜单权限关联表
```

#### 你未来的发展规划， 1年的，3年的

```php
自由发挥，其实也蛮重要的，往积极方面答总没错，比如深入php、扩展技术栈
```

#### 你可以问我两个问题？

```php
当场黑人问号，为什么是两个问题？
面试官：我想看你的关注点
我说了这边技术团队是怎样的+有没有技术分享+我加入公司将做什么
别问我为什么问了三个，因为我get到了面试官的点，哈哈哈
```

#### 其 他

```php
简历上的项目询问，为什么离职，之前收获了什么，你期待的公司是怎样的etc
```

```php
为什么说套路呢，不然发现这个面试官所有准备的题都是有针对性的，后面他就指出我的基础不够扎实，并举例说一个基础扎实的人和
一个基础不扎实的人做同一个东西，基础扎实的可能很快就会做完且不会出错，而不扎实的老是需要google且做出的东西会出错，
也不知道哪里错了，因为是拿来主义并没有转为自己的东西。然后指出我没有系统学习过php，因为php容易入门，
但是学会它需要花很大功夫。最后说我的规划还不错，是加分项，哈哈哈。
```

## 四、面试总结

面了5家公司，拿了3份offer，自我感觉还良好吧，哈哈哈。最主要的还是心态、面试准备、面试总结、睡眠质量、水

写的挺流水化的，很多都有点遗忘，毕竟2星期后了，只能记住有意义的题目了。

有心的同学应该会看到我这个[noteBook][4]下面的其它知识，希望对你们有些许帮助。

后语：有收获的话请**加颗小星星**，没有收获的话可以 **反对 没有帮助 举报**三连

[1]:https://www.zhihu.com/question/50056807
[2]:https://www.zhihu.com/question/63878469
[3]:https://www.zhihu.com/question/20029279
[4]:https://github.com/OMGZui/noteBook
[5]:https://github.com/OMGZui/noteBook/blob/master/level.md




#### 1、双引号和单引号的区别
```tex
- 双引号解释变量，单引号不解释变量
- 双引号里插入单引号，其中单引号里如果有变量的话，变量解释
- 双引号的变量名后面必须要有一个非数字、字母、下划线的特殊字符，或者用{}讲变量括起来，否则会将变量名后面的部分当做一个整体，引起语法错误
- 双引号解释转义字符，单引号不解释转义字符，但是解释'\和\\
- 能使单引号字符尽量使用单引号，单引号的效率比双引号要高（因为双引号要先遍历一遍，判断里面有没有变量，然后再进行操作，而单引号则不需要判断）

#### 2、常用的超全局变量(8个)

- $_GET ----->get传送方式

- $POST ----->post传送方式

- $REQUEST ----->可以接收到get和post两种方式的值

  $REQUEST ----->可以接收到get和post两种方式的值
  ***

- $GLOBALS ----->所有的变量都放在里面

- $FILE ----->上传文件使用

- $SERVER ----->系统环境变量

  $SERVER ----->系统环境变量
  \* **

- $SESSION ----->会话控制的时候会用到

- $COOKIE ----->会话控制的时候会用到

#### 3、HTTP中POST、GET、PUT、DELETE方式的区别

> HTTP定义了与服务器交互的不同的方法，最基本的是POST、GET、PUT、DELETE，与其比不可少的URL的全称是资源描述符，我们可以这样理解：url描述了一个网络上资源，而post、get、put、delegate就是对这个资源进行增、删、改、查的操作！

##### 3.1表单中get和post提交方式的区别

- get是把参数数据队列加到提交表单的action属性所指的url中，值和表单内各个字段一一对应，从url中可以看到；post是通过HTTPPOST机制，将表单内各个字段与其内容防止在HTML的head中一起传送到action属性所指的url地址，用户看不到这个过程
- 对于get方式，服务器端用Request.QueryString获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据
- get传送的数据量较小，post传送的数据量较大，一般被默认不受限制，但在理论上，IIS4中最大量为80kb，IIS5中为1000k，get安全性非常低，post安全性较高

##### 3.2

- GET请求会向数据库发索取数据的请求，从而来获取信息，该请求就像数据库的select操作一样，只是用来查询一下数据，不会修改、增加数据，不会影响资源的内容，即该请求不会产生副作用。无论进行多少次操作，结果都是一样的。
- 与GET不同的是，PUT请求是向服务器端发送数据的，从而改变信息，该请求就像数据库的update操作一样，用来修改数据的内容，但是不会增加数据的种类等，也就是说无论进行多少次PUT操作，其结果并没有不同。
- POST请求同PUT请求类似，都是向服务器端发送数据的，但是该请求会改变数据的种类等资源，就像数据库的insert操作一样，会创建新的内容。几乎目前所有的提交操作都是用POST请求的。
- DELETE请求顾名思义，就是用来删除某一个资源的，该请求就像数据库的delete操作。

#### 4、PHP介绍

Hypertext Preprocessor----超文本预处理器

Personal Home Page 原始名称

目标用途: 允许web开发人员快速编写动态生成的web页面，与其他页面相比，PHP是将程序嵌入到HTML文档中去执行，效率比完全生成HTML编辑的CGI高很多

HTML: Hypertext Markup Language

创始人: 拉姆斯勒·勒多夫Rasmus Lerdorf，1968年生，加拿大滑铁卢大学

勒多夫最开始是为了维护个人网页，用prel语言写了维护程序，之后又用c进行了重写，最终衍生出php/fi

时间轴:

- 1995.06.08将PHP/FI公开释出
- 1995 php2.0，加入了对MySQL的支持
- 1997 php3.0
- 2000 php4.0
- 2008 php5.0
- 由于php6.0没有完全解决Unicode编码，所以基本没有生产线上的应用，基本只是一款概念产品，很多功能已经在php5.3.3和php5.3.4上实现

常见的IDE(Intergrated Development Environment): 集成开发环境

- [Coda（mac）](http://panic.com/coda/)
- [PHPStrom](http://www.jetbrains.com/phpstorm/)
- [Adobe Dreamweaver](http://www.adobe.com/)
- [NetBeans](http://netbeans.org/)

常见文本编辑器，具备代码高亮：

- [NodePad++](https://notepad-plus-plus.org/)
- [SublimeText](http://www.sublimetext.com/)

#### PHP优势

PHP特性:

- php独特混合了C,Java,Prel以及PHP自创的语法
- 可以比CGI或者Prel更快速去执行动态网页，与其他变成语言相比，PHP是讲程序嵌入到HTML文档中去执行，执行效率比完全生成HTML编辑的CGI要高很多，所有的CGI都能实现
- 支持几乎所有流行的数据库以及操作系统
- PHP可以使用C,C++进行程序的扩展

PHP优势:

- 开放源代码
- 免费性
- 快捷性
- 跨平台强
- 效率高
- 图形处理
- 面向对象
- 专业专注

PHP技术应用:

- 静态页面生成
- 数据库缓存
- 过程缓存
- div+css w3c标准
- 大负荷
- 分布式
- flex
- 支持MVC
- Smarty模块引擎

#### PHP认证级别

- 初级 IFE:Index Front Engineer 前端工程师
- 中级 IPE:Index PHP Engineer PHP工程师
- 高级 IAE:Index Architecture Engineer 架构工程师

#### 6、echo、print_r、print、var_dump之间的区别

```
* echo、print是php语句，var_dump和print_r是函数
* echo 输出一个或多个字符串，中间以逗号隔开，没有返回值是语言结构而不是真正的函数，因此不能作为表达式的一部分使用
* print也是php的一个关键字，有返回值 只能打印出简单类型变量的值(如int，string)，如果字符串显示成功则返回true，否则返回false
* print_r 可以打印出复杂类型变量的值(如数组、对象）以列表的形式显示，并以array、object开头，但print_r输出布尔值和NULL的结果没有意义，因为都是打印"\n"，因此var_dump()函数更适合调试
* var_dump() 判断一个变量的类型和长度，并输出变量的数值
```

#### 7、HTTP状态码

[点击这儿查看HTTP状态码详解](http://www.jianshu.com/p/9ecfda409eeb)

常见的HTTP状态码：

- 200 - 请求成功
- 301 - 资源(网页等)被永久转义到其他URL
- 404 - 请求的资源(网页等)不存在
- 505 - 内部服务器错误

HTTP状态码分类:

- 1** - 信息，服务器收到的请求，需要请求者继续执行操作

- 2** - 成功，操作被成功接收并处理

- 3** - 重定向，需要进一步的操作以完成请求

- 4** - 客户端错误，请求包含语法错误或者无法完成请求

- 5** 服务器错误，服务器在处理请求的过程

  5** 服务器错误，服务器在处理请求的过程
  中发生了错误

#### 8、什么是魔术引号

魔术引号是一个将自动将进入PHP脚本的数据进行转义的过程，最好在编码时不要转义而在运行时根据需要而转义

#### 9、如何获取客户端的ip(要求取得一个int)和服务器ip的代码

客户端：`$_SERVER["REMOTE_ADDR"];或者getenv('REMOTE_ADDR')`

客户端：`$_SERVER["REMOTE_ADDR"];或者getenv('REMOTE_ADDR')`
`ip2long进行转换`

客户端：`$_SERVER["REMOTE_ADDR"];或者getenv('REMOTE_ADDR')`
`ip2long进行转换`
服务器端：`gethostbyname('www.baidu.com')`

##### 10、使用那些工具进行版本控制

cvs、svn、vss、git

#### 11、优化数据库的方法

###### [MySQL数据库优化的八大方式（经典必看）点击获取](http://www.jianshu.com/p/dac715a88b44)

- 选取最适用的字段属性，尽可能减少定义字段宽度，尽量把字段设置NOTNULL，例如'省份'、'性别'最好适用ENUM
- 使用连接(JOIN)来代替子查询
- 适用联合(UNION)来代替手动创建的临时表
- 事务处理
- 锁定表、优化事务处理
- 适用外键，优化锁定表
- 建立索引
- 优化查询语句

#### 12、是否使用过模板引擎？使用的模板引擎的名字是？

[Smarty:](http://www.smarty.net/)Smarty算是一种很老的PHP模板引擎了，它曾是我使用这门语言模板的最初选择。虽然它的更新已经不算频繁了，并且缺少新一代模板引擎所具有的部分特性，但是它仍然值得一看。

#### 13、对于大流量网站，采用什么方法来解决访问量的问题

- 确认服务器硬件是否能够支持当前的流量
- 数据库读写分离，优化数据表
- 程序功能规则，禁止外部的盗链
- 控制大文件的下载
- 使用不同主机分流主要流量

#### 14、语句include和require的区别是什么？为避免多次包含同一文件，可以用(?)语句代替他们

- require是无条件包含，也就是如果一个流程里加入require，无论条件成立与否都会先执行require，当文件不存在或者无法打开的时候，会提示错误，并且会终止程序执行
- include有返回值，而require没有(可能因为如此require的速度比include快)，如果被包含的文件不存在的化，那么会提示一个错误，但是程序会继续执行下去

注意:包含文件不存在或者语法错误的时候require是致命的，而include不是

- require_once表示了只包含一次，避免了重复包含

#### 15、谈谈mvc的认识

由模型、视图、控制器完成的应用程序，由模型发出要实现的功能到控制器，控制器接收组织功能传递给视图

#### 16、 说明php中传值与传引用的区别，并说明传值什么时候传引用？

变量默认总是传值赋值，那也就是说，当将一个表达式的值赋予一个变量时，整个表达式的值被赋值到目标变量，这意味着：当一个变量的赋予另外一个变量时，改变其中一个变量的值，将不会影响到另外一个变量

php也提供了另外一种方式给变量赋值：引用赋值。这意味着新的变量简单的__引用__(换言之，成为了其别名或者指向)了原始变量。改动的新的变量将影响到原始变量，反之亦然。使用引用赋值，简单地将一个&符号加到将要赋值的变量前(源变量)

对象默认是传引用

对于较大是的数据，传引用比较好，这样可以节省内存的开销









#### 基础题:

1.表单中 get与post提交方法的区别?

答:get是发送请求HTTP协议通过url参数传递进行接收,而post是实体数据,可以通过表单提交大量信息.

 GET安全性比较差 POST安全性比较好

 GET传输数据较小 POST传输数据较多

2.session与cookie的区别?

答:session:储存用户访问的全局唯一变量,存储在服务器上的php指定的目录中的（session_dir）的位置进行的存放

   cookie:用来存储连续訪問一个頁面时所使用，是存储在客户端，对于Cookie来说是存储在用户WIN的Temp目录中的。

   两者都可通过时间来设置时间长短

 

3.数据库中的事务是什么?

答:事务（transaction）是作为一个单元的一组有序的数据库操作。如果组中的所有操作都成功，则认为事务成功，即使只有一个操作失败，事务也不成功。如果所有操作完成，

 

事务则提交，其修改将作用于所有其他数据库进程。如果一个操作失败，则事务将回滚，该事务所有操作的影响都将取消。

 

简述题:

1、用PHP打印出前一天的时间格式是2006-5-1022:21:21(2分)

答:echo date('Y-m-d H:i:s', strtotime('-1 days'));

 

2、echo(),print(),print_r()的区别(3分)

答:echo是PHP语句, print和print_r是函数,语句没有返回值,函数可以有返回值(即便没有用) 

   print（）    只能打印出简单类型变量的值(如int,string) 

   print_r（）可以打印出复杂类型变量的值(如数组,对象) 

   echo    输出一个或者多个字符串

 

3、能够使HTML和PHP分离开使用的模板(1分)

答:Smarty,               Dwoo,TinyButStrong,Template Lite,Savant,phemplate,XTemplate

 

5、使用哪些工具进行版本控制?(1分)

答:svn,         

 

6、如何实现字符串翻转?(3分)

答:echo strrev($a);

 

7、优化MYSQL数据库的方法。(4分，多写多得)

答:

1、选取最适用的字段属性,尽可能减少定义字段长度,尽量把字段设置NOT NULL,例如'省份,性别',最好设置为ENUM

2、使用连接（JOIN）来代替子查询:

   a.删除没有任何订单客户:DELETE FROMcustomerinfo WHERE customerid NOT in(SELECT customerid FROM orderinfo)

   b.提取所有没有订单客户:SELECT FROMcustomerinfo WHERE customerid NOT in(SELECT customerid FROM orderinfo)

   c.提高b的速度优化:SELECTFROM customerinfo LEFT JOIN orderidcustomerinfo.customerid=orderinfo.customerid

   WHERE orderinfo.customerid IS NULL

3、使用联合(UNION)来代替手动创建的临时表

   a.创建临时表:SELECT name FROM`nametest` UNION SELECT username FROM `nametest2`

4、事务处理:

   a.保证数据完整性,例如添加和修改同时,两者成立则都执行,一者失败都失败

   mysql_query("BEGIN");

   mysql_query("INSERT INTO customerinfo(name) VALUES ('$name1')";

   mysql_query("SELECT * FROM `orderinfo`where customerid=".$id");

   mysql_query("COMMIT");

5、锁定表,优化事务处理:

   a.我们用一个 SELECT 语句取出初始数据，通过一些计算，用 UPDATE 语句将新值更新到表中。

     包含有 WRITE 关键字的LOCK TABLE 语句可以保证在 UNLOCK TABLES 命令被执行之前，

     不会有其它的访问来对 inventory 进行插入、更新或者删除的操作

   mysql_query("LOCK TABLE customerinfoREAD, orderinfo WRITE");

   mysql_query("SELECT customerid FROM`customerinfo` where id=".$id);

   mysql_query("UPDATE `orderinfo` SETordertitle='$title' where customerid=".$id);

   mysql_query("UNLOCK TABLES");

6、使用外键,优化锁定表

   a.把customerinfo里的customerid映射到orderinfo里的customerid,

     任何一条没有合法的customerid的记录不会写到orderinfo里

   CREATE TABLE customerinfo

   (

     customerid INT NOT NULL,

     PRIMARY KEY(customerid) 

   )TYPE = INNODB;

   CREATE TABLE orderinfo

   (

     orderid INT NOT NULL,

     customerid INT NOT NULL,

     PRIMARY KEY(customerid,orderid),

     FOREIGN KEY (customerid) REFERENCEScustomerinfo

     (customerid) ON DELETE CASCADE  

   )TYPE = INNODB;

   注意:'ON DELETE CASCADE',该参数保证当customerinfo表中的一条记录删除的话同时也会删除order

         表中的该用户的所有记录,注意使用外键要定义事务安全类型为INNODB;

7、建立索引:

   a.格式:

   (普通索引)->

   创建:CREATE INDEX <索引名>ON tablename (索引字段)

   修改:ALTER TABLE tablenameADD INDEX [索引名] (索引字段)

   创表指定索引:CREATE TABLEtablename([...],INDEX[索引名](索引字段))

   (唯一索引)->

   创建:CREATE UNIQUE <索引名>ON tablename (索引字段)

   修改:ALTER TABLE tablenameADD UNIQUE [索引名] (索引字段)

   创表指定索引:CREATE TABLEtablename([...],UNIQUE[索引名](索引字段))

   (主键)->

   它是唯一索引,一般在创建表是建立,格式为:

   CREATA TABLE tablename ([...],PRIMARY KEY[索引字段])

8、优化查询语句

   a.最好在相同字段进行比较操作,在建立好的索引字段上尽量减少函数操作

   例子1:

   SELECT * FROM order WHEREYEAR(orderDate)<2008;(慢)

   SELECT * FROM order WHEREorderDate<"2008-01-01";(快)

   例子2:

   SELECT * FROM order WHERE addtime/7<24;(慢)

   SELECT * FROM order WHERE addtime<24*7;(快)

   例子3:

   SELECT * FROM order WHERE title like"%good%";

   SELECT * FROM order WHEREtitle>="good" and name<"good";

 

8、PHP的意思(送1分)

答:PHP是一个基于服务端来创建动态网站的脚本语言，您可以用PHP和HTML生成网站主页

 

9、MYSQL取得当前时间的函数是?，格式化日期的函数是(2分)

答:now(),date()

 

10、实现中文字串截取无乱码的方法。(3分)

答:function GBsubstr($string, $start, $length) {

    if(strlen($string)>$length){

     $str=null;

     $len=$start+$length;

     for($i=$start;$i<$len;$i++){

    if(ord(substr($string,$i,1))>0xa0){

     $str.=substr($string,$i,2);

     $i++;

    }else{

     $str.=substr($string,$i,1);

     }

    }

   return $str.'...';

    }else{

   return $string;

   }

}

 

11、您是否用过版本控制软件? 如果有您用的版本控制软件的名字是?(1分)

是 svn

12、您是否用过模板引擎? 如果有您用的模板引擎的名字是?(1分)

答:用过,smarty

 

13、请简单阐述您最得意的开发之作(4分)

答:信息分类(根据自己的情况写)

 

14、对于大流量的网站,您采用什么样的方法来解决访问量问题?(4分)

答:确认服务器硬件是否足够支持当前的流量,数据库读写分离,优化数据表,

   程序功能规则,禁止外部的盗链,控制大文件的下载,使用不同主机分流主要流量

 

15、用PHP写出显示客户端IP与服务器IP的代码1分)

答:打印客户端IP:echo $_SERVER[‘REMOTE_ADDR’]; 或者:getenv('REMOTE_ADDR');

 

16、语句include和require的区别是什么?为避免多次包含同一文件，可用(?)语句代替它们? (2分)

答:require是无条件包含也就是如果一个流程里加入require,无论条件成立与否都会先执行require

  include有返回值，而require没有

  包含文件不存在或者语法错误的时候require是致命的,include不是

 Include_once require_once

17、如何修改SESSION的生存时间(1分).

答:方法1:将php.ini中的session.gc_maxlifetime设置为9999重启apache

   方法2:$savePath ="./session_save_dir/";

         $lifeTime = 小时 * 秒;

         session_save_path($savePath);

         session_set_cookie_params($lifeTime);

         session_start();

   方法3:setcookie() andsession_set_cookie_params($lifeTime);

 

18、有一个网页地址, 比如PHP开发资源网主页:http://www.phpres.com/index.html,如何得到它的内容?($1分)

答:方法1(对于PHP5及更高版本):

   $readcontents =fopen("http://www.phpres.com/index.html", "rb");

   $contents = stream_get_contents($readcontents);

   fclose($readcontents);

   echo $contents;

   方法2:

   echofile_get_contents("http://www.phpres.com/index.html");

 

19、在HTTP 1.0中，状态码401的含义是(?);如果返回“找不到文件”的提示，则可用 header 函数，其语句为(?);(2分)

答:状态401代表未被授权,header("Location:www.xxx.php");

 

12、在PHP中，heredoc是一种特殊的字符串，它的结束标志必须?(1分)

答:heredoc的语法是用"<<<"加上自己定义成对的标签，在标签范围內的文字视为一个字符串

   例子:

   $str = <<<SHOW

   my name is Jiang Qihui!

   SHOW;

 

13、谈谈asp,php,jsp的优缺点(1分)

答:ASP全名Active Server Pages，是一个WEB服务器端的开发环境，利用它可以产生和运

行动态的、交互的、高性能的WEB服务应用程序。ASP采用脚本语言VB Script（Java script

）作为自己的开发语言。

PHP是一种跨平台的服务器端的嵌入式脚本语言. 它大量地借用C,Java和Perl语言的语法

, 并耦合PHP自己的特性,使WEB开发者能够快速地写出动态生成页面.它支持目前绝大多数数

据库。还有一点，PHP是完全免费的，不用花钱，你可以从PHP官方站点(http://www.php.ne

t)自由下载。而且你可以不受限制地获得源码，甚至可以从中加进你自己需要的特色。

JSP 是Sun公司推出的新一代站点开发语言，他完全解决了目前ASP,PHP的一个通病－－

脚本级执行（据说PHP4 也已经在Zend 的支持下，实现编译运行）.Sun 公司借助自己在Jav

a 上的不凡造诣，将Java从Java 应用程序和 Java Applet 之外，又有新的硕果，就是Js

p－－JavaServer Page。Jsp 可以在Serverlet和JavaBean的支持下，完成功能强大的站点

程序。

　　三者都提供在 HTML 代码中混合某种程序代码、由语言引擎解释执行程序代码的能力。

但JSP代码被编译成 Servlet 并由 Java 虚拟机解释执行，这种编译操作仅在对 JSP 页面的

第一次请求时发生。在 ASP 、PHP、JSP 环境下，HTML 代码主要负责描述信息的显示样式

，而程序代码则用来描述处理逻辑。普通的 HTML 页面只依赖于 Web 服务器，而ASP 、PH

P、JSP 页面需要附加的语言引擎分析和执行程序代码。程序代码的执行结果被重新嵌入到

HTML 代码中，然后一起发送给浏览器。 ASP 、PHP、 JSP三者都是面向 Web 服务器的技术

，客户端浏览器不需要任何附加的软件支持。

 

14、谈谈对mvc的认识(1分)

答:由模型(model),视图(view),控制器(controller)完成的应用程序

   由模型发出要实现的功能到控制器,控制器接收组织功能传递给视图;

 

15、写出发贴数最多的十个人名字的SQL，利用下表：members(id,username,posts,pass,email)(2分)

答:SELECT * FROM `members` ORDER BY posts DESC limit 0,10;

 

\16. 请说明php中传值与传引用的区别。什么时候传值什么时候传引用?(2分)

答:按值传递：函数范围内对值的任何改变在函数外部都会被忽略

   按引用传递：函数范围内对值的任何改变在函数外部也能反映出这些修改

   优缺点：按值传递时，php必须复制值。特别是对于大型的字符串和对象来说，这将会是一个代价很大的操作。

   按引用传递则不需要复制值，对于性能提高很有好处。

 

\17. 在PHP中error_reporting这个函数有什么作用? (1分)

答:设置错误级别与错误信息回报

 

\18. 请写一个函数验证电子邮件的格式是否正确 (2分)

答:function checkEmail($email)

  {

    $pregEmail ="/([a-z0-9]*[-_\.]?[a-z0-9]+)*@([a-z0-9]*[-_]?[a-z0-9]+)+[\.][a-z]{2,3}([\.][a-z]{2})?/i";

    return preg_match($pregEmail,$email); 

  }

 

\19. 简述如何得到当前执行脚本路径，包括所得到参数。(2分)

答:$script_name = basename(__file__); print_r($script_name);

 

21、JS表单弹出对话框函数是?获得输入焦点函数是? (2分)

答:弹出对话框: alert(),prompt(),confirm()

   获得输入焦点 focus()

 

22、JS的转向函数是?怎么引入一个外部JS文件?(2分)

答:window.location      <scripttype="text/javascript"src="js/js_function.js"></script>

 

23、foo()和@foo()之间有什么区别?(1分)

答:@foo()控制错误输出

 

24、如何声明一个名为”myclass”的没有方法和属性的类? (1分)

答:class myclass{ }

 

25、如何实例化一个名为”myclass”的对象?(1分)

答:new myclass()

 

26、你如何访问和设置一个类的属性? (2分)

答:$object = new myclass();

   $newstr = $object->test;

   $object->test = "info";

 

27、mysql_fetch_row() 和mysql_fetch_array之间有什么区别? (1分)

答:mysql_fetch_row是从结果集取出1行数组,作为枚举

   mysql_fetch_array是从结果集取出一行数组作为关联数组,或数字数组,两者兼得

 

28、GD库是做什么用的? (1分)

答:gd库提供了一系列用来处理图片的API，使用GD库可以处理图片，或者生成图片。

   在网站上GD库通常用来生成缩略图或者用来对图片加水印或者对网站数据生成报表。

 

29、指出一些在PHP输入一段HTML代码的办法。(1分)

答:echo "<a href='index.php'>aaa</a>";

 

30、下面哪个函数可以打开一个文件，以对文件进行读和写操作?(1分)

   (a) fget() (b) file_open() (c) fopen() (d)open_file()  



31、下面哪个选项没有将 john 添加到users 数组中? (1分)

　　(a) $users[]= ‘john’;

　(b) array_add($users,’john’);

　　(c)array_push($users,‘john’);

　　(d) $users ||= ‘john’; 

 

32、下面的程序会输入是否?(1分)

　　$num = 10;

　　functionmultiply(){

　　$num = $num* 10;

　　}

　　multiply();

　　echo $num;

　　?>

    输出:10

 正确

33、使用php写一段简单查询，查出所有姓名为“张三”的内容并打印出来 (2分)

　　表名User

　　Name TelContent Date

　　张三13333663366 大专毕业2006-10-11

　　张三13612312331 本科毕业2006-10-15

　　张四021-55665566 中专毕业2006-10-15

　　请根据上面的题目完成代码：

　　$mysql_db=mysql_connect("local","root","pass");

@mysql_select_db("DB",$mysql_db);

    $result = mysql_query("SELECT * FROM`user` WHERE name='张三'");

    while($rs = mysql_fetch_array($result)){

      echo$rs["tel"].$rs["content"].$rs["date"];

    }   

 

34、如何使用下面的类,并解释下面什么意思?(3)

　　class test{

    function Get_test($num){

　　    $num=md5(md5($num)."En");

　　    return $num;

　　 }

　　}

答:$testnum = "123";

   $object = new test();

   $encrypt = $object->Get_test($testnum);

   echo $encrypt;

   类test里面包含Get_test方法,实例化类调用方法多字符串加密

 

35、写出 SQL语句的格式 : 插入 ，更新 ，删除 (4分)

　　表名User

　　Name TelContent Date

　　张三13333663366 大专毕业2006-10-11

　　张三13612312331 本科毕业2006-10-15

　　张四021-55665566 中专毕业2006-10-15

　　(a) 有一新记录(小王 13254748547 高中毕业 2007-05-06)请用SQL语句新增至表中

    mysql_query("INSERT INTO `user` (name,tel,content,date)VALUES

    ('小王','13254748547','高中毕业','2007-05-06')")

 

　　(b) 请用sql语句把张三的时间更新成为当前系统时间

    $nowDate = date("Ymd");

    mysql_query("UPDATE `user` SETdate='".$nowDate."' WHERE name='张山'");

 

　　(c) 请写出删除名为张四的全部记录

    mysql_query("DELETE FROM `user` WHERE name='张四'");

 

36、请写出数据类型(int char varchar datetime text)的意思; 请问varchar和char有什么区别(2分)

答:int是数字类型,char固定长度字符串,varchar实际长度字符串,datetime日期时间型,text文本字符串

   char的场地固定为创建表设置的长度,varchar为可变长度的字符

 

38、写出以下程序的输出结果 (1分)

　　$b=201;

　　$c=40;

   $a=$b>$c?4:5;

　　echo $a;

　　?>

答:4

 

39、检测一个变量是否有设置的函数是否?是否为空的函数是?(2分)

答:isset($str),empty($str);

 

40、取得查询结果集总数的函数是?(1分)

答:mysql_num_rows($result);

 

41、$arr = array('james', 'tom', 'symfony'); 请打印出第一个元素的值 (1分)

答:echo $array[0];

 

42、请将41题的数组的值用','号分隔并合并成字串输出(1分)

答:for($i=0;$i<count($array);$i++){ echo$array[$i].",";}

 

43、$a = 'abcdef'; 请取出$a的值并打印出第一个字母(1分)

答:echo $a{0} 或 echo substr($a,0,1)

 

44、PHP可以和sqlserver/oracle等数据库连接吗?(1分)

答:当然可以

 

45、请写出PHP5权限控制修饰符(3分)

答:public(公共),private(私用),protected(继承)

 

46、请写出php5的构造函数和析构函数(2分)

答:__construct , __destruct

 

47、完成以下:

   (一)创建新闻发布系统，表名为message有如下字段 (3分)

　　id 文章id

　　title 文章标题

　　content 文章内容

　　category_id 文章分类id

   hits 点击量

答:CREATE TABLE 'message'(

   'id' int(10) NOT NULL auto_increment,

   'title' varchar(200) default NULL,

   'content' text,

   'category_id' int(10) NOT NULL,

   'hits' int(20),

   PRIMARY KEY('id');

   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

 

    (二)同样上述新闻发布系统：表comment记录用户回复内容，字段如下 (4分)

　　comment_id 回复id

　　id 文章id，关联message表中的id

　　comment_content回复内容

　　现通过查询数据库需要得到以下格式的文章标题列表,并按照回复数量排序，回复最高的排在最前面

　　文章id 文章标题 点击量 回复数量

　　用一个SQL语句完成上述查询，如果文章没有回复则回复数量显示为0

答:SELECT message.id id,message.title title,IF(message.`hits`IS NULL,0,message.`hits`) hits,

   IF(comment.`id` is NULL,0,count(*)) numberFROM message LEFT JOIN

   comment ON message.id=comment.id GROUP BYmessage.`id`;

 

　　(三)上述内容管理系统，表category保存分类信息，字段如下 (3分)

　　category_idint(4) not null auto_increment;

　　categroy_namevarchar(40) not null;

　　用户输入文章时，通过选择下拉菜单选定文章分类

　　写出如何实现这个下拉菜单

答:function categoryList()

{

    $result=mysql_query("selectcategory_id,categroy_name from category")

            or die("Invalid query: ". mysql_error());

    print("<select name='category'value=''>\n");

    while($rowArray=mysql_fetch_array($result))

    {

       print("<optionvalue='".$rowArray['category_id']."'>".$rowArray['categroy_name']."</option>\n");

    }

    print("</select>");

}





编程题:

\1. 写一个函数，尽可能高效的，从一个标准 url 里取出文件的扩展名

   例如:http://www.sina.com.cn/abc/de/fg.php?id=1 需要取出 php 或 .php

答案1:

   function getExt($url){

   $arr = parse_url($url);

  

   $file = basename($arr['path']);

   $ext = explode(".",$file);

   return $ext[1];

}

答案2:

    function getExt($url) {

    $url = basename($url);

    $pos1 = strpos($url,".");

    $pos2 = strpos($url,"?");

    if(strstr($url,"?")){

         return substr($url,$pos1 + 1,$pos2 -$pos1 - 1);

    } else {

      return substr($url,$pos1);

    }

}

 

 

\2. 在 HTML 语言中，页面头部的 meta标记可以用来输出文件的编码格式，以下是一个标准的 meta 语句

　　请使用 PHP 语言写一个函数，把一个标准 HTML 页面中的类似 meta 标记中的 charset 部分值改为 big5

　　请注意:

　　1. 需要处理完整的 html 页面，即不光此 meta 语句

　　2. 忽略大小写

   3. ' 和 " 在此处是可以互换的

   4. 'Content-Type' 两侧的引号是可以忽略的，但 'text/html; charset=gbk' 两侧的不行

\5. 注意处理多余空格

$str=File_get_contents(‘xxx.php’);

 

 Preg_replace(‘/<meta http-equiv=‘content-type’content=[‘”]text/html;charset=.*[‘”]>/’,‘/<meta ‘content-type’ content=‘text/html;charset=utf-8’>/’,$str）

 

\3. 写一个函数，算出两个文件的相对路径

　　如 $a ='/a/b/c/d/e.php';

　　$b ='/a/b/12/34/c.php';

　　计算出 $b 相对于 $a 的相对路径应该是 ../../c/d将()添上

答:function getRelativePath($a, $b) {  

    $returnPath = array(dirname($b));  

    $arrA = explode('/', $a);  

    $arrB = explode('/', $returnPath[0]);  

    for ($n = 1, $len = count($arrB); $n <$len; $n++) {  

        if ($arrA[$n] != $arrB[$n]) {  

            break;  

        }   

    }  

    if ($len - $n > 0) {  

        $returnPath = array_merge($returnPath,array_fill(1, $len - $n, '..'));  

    }  

      

    $returnPath = array_merge($returnPath,array_slice($arrA, $n));  

    return implode('/', $returnPath);  

   }  

   echo getRelativePath($a, $b); 

 

填空题:

1.在PHP中，当前脚本的名称(不包括路径和查询字符串)记录在预定义变量__$_SERVER['PHP_SELF']__中;而链接到当前页面的URL记录在预定义变量__$_SERVER['HTTP_REFERER']__

 

中

 

2.执行程序段<?php echo 8%(-2) ?>将输出__0__。

 

3.在HTTP 1.0中，状态码 401 的含义是__未被授权__;如果返回“找不到文件”的提示，则可用 header 函数，其语句为__header(‘location:xxx.php’)__。

 

4.数组函数 arsort 的作用是__对数组进行逆向排序并保持索引关系__;语句error_reporting(2047)的作用是__报告所有错误和警告__。

 

5.PEAR中的数据库连接字符串格式是__。

 

6.写出一个正则表达式，过虑网页上的所有JS/VBS脚本(即把scrīpt标记及其内容都去掉):preg_replace("/<script[^>].*?>.*?</script>/si","newinfo", $script);

 

7.以Apache模块的方式安装PHP，在文件http.conf中首先要用语句____动态装载PHP模块，然后再用语句____使得Apache把所有扩展名为php的文件都作为PHP脚本处理。

  LoadModule php5_module "c:/php/php5apache2.dll" ,

  AddTypeapplication/x-httpd-php .php,

 

8.语句 include 和 require 都能把另外一个文件包含到当前文件中，它们的区别是____;为了避免多次包含同一文件，可以用语句__require_once||include_once__来代替它们。

 

9.类的属性可以序列化后保存到 session 中，从而以后可以恢复整个类，这要用到的函数是__unserialize__。

 

10.一个函数的参数不能是对变量的引用，除非在php.ini中把__allow_call_time_pass_reference boolean__设为on.

 

11.SQL中LEFT JOIN的含义是__自然左外链接__。如果 tbl_user记录了学生的姓名(name)和学号(ID)，tbl_score记录了学生(有的学生考试以后被开除了，没有其记录)的学号(ID)

 

和考试成绩(score)以及考试科目(subject)，要想打印出各个学生姓名及对应的的各科总成绩，则可以用SQL语句__select  *  fromtbl_user left jion tbl_score on tbl_user.id=tbl_score.uid__。

 

12.在PHP中，heredoc是一种特殊的字符串，它的结束标志必须____。

 <<<EOT

Sdashkdhklahdklh

EOT

编程题:

13.写一个函数，能够遍历一个文件夹下的所有文件和子文件夹。

答:

function my_scandir($dir)

{

     $files = array();

     if ( $handle = opendir($dir) ) {

         while ( ($file = readdir($handle)) !==false ) {

$file=$dir.’/’.$file

             if ( $file != ".."&& $file != "." ) {

                 if ( is_dir($dir ."/" . $file) ) {

                     $files[$file] =scandir($dir . "/" . $file);

                 }else {

                     $files[] = $file;

                 }

             }

         }

         closedir($handle);

         return $files;

     }

}

 

14.简述论坛中无限分类的实现原理。

答:

<?php

/*

数据表结构如下:

CREATE TABLE `category` (

 `categoryID` smallint(5) unsigned NOT NULLauto_increment,

 `categoryParentID` smallint(5) unsigned NOTNULL default '0',

 `categoryName` varchar(50) NOT NULL default'',

 PRIMARY KEY (`categoryID`)

) ENGINE=MyISAM DEFAULTCHARSET=gbk;

 

INSERT INTO `category` (`categoryParentID`, `categoryName`) VALUES

(0, '一级类别'),

(1, '二级类别'),

(1, '二级类别'),

(1, '二级类别'),

(2, '三级类别'),

(2, '333332'),

(2, '234234'),

(3, 'aqqqqqd'),

(4, '哈哈'),

(5, '66333666');

 

*/

 

//指定分类id变量$category_id,然后返回该分类的所有子类

//$default_category为默认的选中的分类

functionGet_Category($category_id = 0,$level = 0, $default_category = 0)

{

 global $DB;

 $sql = "SELECT * FROM category ORDER BYcategoryID DESC";

 $result = $DB->query( $sql );

 while ($rows = $DB->fetch_array($result))

 {

 $category_array[$rows[categoryParentID]][$rows[categoryID]]= array('id' => $rows[categoryID], 'parent' => $rows[categoryParentID],'name' => $rows

 

[categoryName]);

 }

 if (!isset($category_array[$category_id]))

 {

 return "";

 }

 foreach($category_array[$category_id] AS $key=> $category)

 {

 if ($category['id'] == $default_category)

 {

 echo "<option selectedvalue=".$category['id']."";

 }else

 {

 echo "<optionvalue=".$category['id']."";

 }

 

 if ($level > 0)

 {

 echo ">" . str_repeat( "", $level ) . " " . $category['name'] ."</option>\n";

 }

 else

 {

 echo ">" . $category['name'] ."</option>\n";

 }

 Get_Category($key, $level + 1,$default_category);

 }

 unset($category_array[$category_id]);

}

 

/*

函数返回的数组格式如下所示:

Array

(

 [1] => Array ( [id] => 1 [name] => 一级类别[level] => 0 [ParentID] => 0 )

 [4] => Array ( [id] => 4 [name] => 二级类别[level] => 1 [ParentID] => 1 )

 [9] => Array ( [id] => 9 [name] => 哈哈[level] => 2 [ParentID] => 4 )

 [3] => Array ( [id] => 3 [name] => 二级类别[level] => 1 [ParentID] => 1 )

 [8] => Array ( [id] => 8 [name] =>aqqqqqd [level] => 2 [ParentID] => 3 )

 [2] => Array ( [id] => 2 [name] => 二级类别[level] => 1 [ParentID] => 1 )

 [7] => Array ( [id] => 7 [name] =>234234 [level] => 2 [ParentID] => 2 )

 [6] => Array ( [id] => 6 [name] =>333332 [level] => 2 [ParentID] => 2 )

 [5] => Array ( [id] => 5 [name] => 三级类别[level] => 2 [ParentID] => 2 )

 [10] => Array ( [id] => 10 [name] =>66333666 [level] => 3 [ParentID] => 5 )

)

*/

//指定分类id,然后返回数组

functionCategory_array($category_id = 0,$level=0)

{

 global $DB;

 $sql = "SELECT * FROM category ORDER BYcategoryID DESC";

 $result = $DB->query($sql);

 while ($rows = $DB->fetch_array($result))

 {

 $category_array[$rows['categoryParentID']][$rows['categoryID']]= $rows;

 }

 

 foreach ($category_array AS $key=>$val)

 {

 if ($key == $category_id)

 {

 foreach ($val AS $k=> $v)

 {

 $options[$k] =

 array(

 'id' => $v['categoryID'], 'name' =>$v['categoryName'], 'level' => $level, 'ParentID'=>$v['categoryParentID']

 );

 

 $children = Category_array($k, $level+1);

 

 if (count($children) > 0)

 {

 $options = $options + $children;

 }

 }

 }

 }

 unset($category_array[$category_id]);

 return $options;

}

 

?>

 

<?php

 

class cate

{

 

        function Get_Category($category_id =0,$level = 0, $default_category = 0)

        {

             echo $category_id;

             $arr = array(

              '0' => array(

                             '1' =>array('id' => 1, 'parent' => 0, 'name' => '1111'),

                             '2' =>array('id' => 2, 'parent' => 0, 'name' => '2222'),

                            '4' =>array('id' => 4, 'parent' => 0, 'name' => '4444')   

                          ),

              '1' => array(

                              '3' =>array('id' => 3, 'parent' => 1, 'name' => '333333'),

                            '5' =>array('id' => 5, 'parent' => 1, 'name' => '555555')    

                            ),

                         

              '3' => array(

                            '6' =>array('id' => 6, 'parent' => 3, 'name' => '66666'),

                            '7' =>array('id' => 7, 'parent' => 3, 'name' => '77777')

                            ),

              '4' => array(

                            '8' =>array('id' => 8, 'parent' => 4, 'name' => '8888'),

                            '9' =>array('id' => 9, 'parent' => 4, 'name' => '9999')

                            )   

             );

 

             if (!isset($arr[$category_id]))

             {

                return "";

             }

   

             foreach($arr[$category_id] AS $key=> $cate)

             {

                 if ($cate['id'] ==$default_category)

                 {

                     $txt = "<optionselected value=".$cate['id']."";

                 }else{

                     $txt = "<optionvalue=".$cate['id']."";

                 }

           

                 if ($level > 0)

                 {

                    $txt1 = ">" .str_repeat( "-", $level ) . " " . $cate['name'] ."</option>\n";

                 }else{

                     $txt1 = ">" .$cate['name'] . "</option>\n";

                 }

                 $val = $txt.$txt1;

                 echo $val;

                 self::Get_Category($key,$level + 1, $default_category);

             }

           

        }

       

       

        function getFlush($category_id =0,$level = 0, $default_category = 0)

        {

           

            ob_start();

 

            self::Get_Category($category_id,$level, $default_category);

 

            $out = ob_get_contents();

 

            ob_end_clean();

            return $out;

        }   

}

$id =$_GET['id'];

echo"<select>";

$c = new cate();

//$c->Get_Category();

$ttt=  $c->getFlush($id,'0','3');

echo $ttt;

echo"</select>";

?>





1.烧一根不均匀的绳，从头烧到尾总共需要1个小时。现在有若干条材质相同的绳子，问如何用烧绳的方法来计时一个小时十五分钟呢？（这道题我当初想了一个小时）

先用两条绳子，从分别从两边点，对齐，当火相遇时，就是半个小时的时间的程度，同样在计算出一半，即15分钟的长度，在加上一条整跟的，就是1小时15分

 

2.你有一桶果冻，其中有黄色、绿色、红色三种，闭上眼睛抓取同种颜色的两个。抓取多少个就可以确定你肯定有两个同一颜色的果冻？（5秒-1分钟）

4个

 

3.如果你有无穷多的水，一个3公升的提捅，一个5公升的提捅，两只提捅形状上下都不均匀，问你如何才能准确称出4公升的水？（40秒-3分钟）

第一步，5－3＝2；

  第二步，把3倒掉，把2倒进3的桶；

  第三步，用5把3填满，剩4。

 

4.一个岔路口分别通向诚实国和说谎国。来了两个人，已知一个是诚实国的，另一个是说谎国的。诚实国永远说实话，说谎国永远说谎话。现在你要去说谎国，但不知道应该走哪条路，需要问这两个人。请问应该怎么问？（20秒-2分钟）

 

问诚实国的，说谎国怎么走，他指的路是对的

 

5.12个球一个天平，现知道只有一个和其它的重量不同，问怎样称才能用三次就找到那个球。13个呢？（注意此题并未说明那个球的重量是轻是重，所以需要仔细考虑）（5分钟-1小时）

为便于区分，我们先将12个球标上号码，分别为1——12号，再将天平分为左右两边。

第一次：将1、3、5、7号球放在天平的左边，将2、4、6、8号球放在天平右边。

将会出现三种可能：

1、天平平衡，证明有问题的球在9、10、11、12号球中；

2、左侧下沉，说明有问题的球在1、2、3、4、5、6、7、8号球中；

3、右侧下沉，说明有问题的球在1、2、3、4、5、6、7、8号球中，如果将太平扭过来，与左侧下沉相同。

第二次：天平左侧放1、3、4号球，右侧放2、5、9号球，也有三种可能：

1、天平平衡，证明有问题的球在6、7、8号球中（第三次A）；

2、左侧下沉，说明有问题的球在1、2、3号球中（第三次B）；

3、右侧下沉，说明有问题的球在4、5号球中（第三次C）。

第三次A：分别将6、8号球放在天平的两侧，如果平衡，7号球有问题；如果不平衡，轻的那个球有问题。

第三次B：分别将1、3号球放在天平的两侧，如果平衡，2号球有问题；如果不平衡，重的那个球有问题。

第三次C：分别将4、9号球放在天平的两侧，如果平衡，5号球有问题；如果不平衡，4号球有问题。

 

 

 

 

6.在9个点上画10条直线，要求每条直线上至少有三个点？（3分钟-20分钟）

五角星去掉一个点（10分钟）

7.在一天的24小时之中，时钟的时针、分针和秒针完全重合在一起的时候有几次？都分别是什么时间？你怎样算出来的？（5分钟-15分钟）

24次，00:00:00,01:05:05,02:11:11,03:16:16,04:22:22,05:27:27,06:33:33,07:38:38,08:44:44,09:49:49,10:55:55,11:59:59,12:00:00,13:05:05,14:11:11,15:16:16,16:22:22,17:27:27,18:33:33,19:38:38,20:44:44,21:49:49,22:55:55,23:59:59



    echo '</div>';


```
