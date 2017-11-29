[TOC]



-- MYSQL语句

#### -- 登录数据库

```mysql
mysql -h 127.0.0.1 -u root -p（密码为空可以省略-p）
```

#### -- 显示全部数据库

```mysql
SHOW DATABASES;
```

#### -- 创建数据库

```mysql
CREATE DATABASE IF NOT EXISTS `sunshine` CHARSET utf8 COLLATE utf8_bin;
```

#### -- 显示完整的创库语句

```mysql
SHOW CREATE DATABASE `sunshine`;
```

#### -- 显示MYSQL编码

```mysql
SHOW CHARSET;
```

#### -- 查看字符集

```mysql
SHOW VARIABLES LIKE '%char%';
```

#### -- 设置字符集

```mysql
CREATE DATABASE IF NOT EXISTS `sunshine` CHARSET SET = utf8;
可简写为:
CREATE DATABASE `sunshine` CHARSET utf8;
```

#### -- collation的主要作用是指定字符排序时使用哪种字符编码作为排序的依据

```mysql
SHOW COLLATION;
SHOW COLLATION WHERE Charset = 'utf8';
```

#### -- 登录数据库时选择数据库

```mysql
mysql -h localhost `sunshine` -u root -p
```

#### -- 使用数据库

```mysql
USE `sunshine`;
```

#### -- 查看全部数据表

```mysql
SHOW TABLES;
```

#### -- 创建数据表(单个字段)

```mysql
CREATE TABLE IF NOT EXISTS `tbl_user` (
	`id` int(10) NOT NULL PRIMARY KEY AUTO_INCREMENT
);
```
#### -- 创建数据表(多个字段)

```mysql
CREATE TABLE IF NOT EXISTS `tbl_collect` (
    `id` int(10) NOT NULL PRIMARY KEY AUTO_INCREMENT,
	`userid` int(10),
	`username` varchar(50) DEFAULT ''
);
```

#### -- 得到创建数据表语句

```mysql
SHOW CREATE TABLE `sunshine`;
```

#### -- 打印表结构

```mysql
DESC `tbl_user`;
```

#### -- 打印存储引擎(主要使用的是两种引擎：MyISAM和InnoDB)

```mysql
SHOW ENGINES;
```

#### -- MySQL默认的引擎是InnoDB，如果需要使用其他的引擎，需要特别指定

```mysql
CREATE TABLE IF NOT EXISTS `tbl_collect` (
	`id` int(10) NOT NULL PRIMARY KEY AUTO_INCREMENT,
	`userid` int(10),
	`username` varchar(50) DEFAULT ''
) ENGINE MyISAM;
```
#### -- 删除数据表

```mysql
DROP TABLE IF EXISTS `tbl_comment`;
```

#### -- 删除数据库中全部数据表

```MYSQL
SELECT concat('DROP TABLE IF EXISTS ', table_name, ';') FROM information_schema.tables WHERE table_schema = 'subject_3.0';
```

#### -- 修改表名

```mysql
ALTER TABLE `tbl_collect` RENAME `tbl_comment`;
```

#### -- 增加表字段

```mysql
ALTER TABLE `tbl_user` ADD `addtime` int(10);
```

#### -- 将增加的表字段放在所有字段的最前面

```mysql
ALTER TABLE `tbl_user` ADD `sorts` tinyint(1) FIRST;
```

#### -- 将增加的表字段放在任意字段后面

```mysql
ALTER TABLE `tbl_user` ADD `state` tinyint(1) AFTER `id`;
```

#### -- 删除字段

```mysql
ALTER TABLE `tbl_user` DROP `state;
```

#### -- 修改字段的属性

```mysql
ALTER TABLE `tbl_user` MODIFY `id` int(11);
```

#### -- 修改字段名和属性

```mysql
ALTER TABLE `tbl_user` CHANGE `addtime` state tinyint(1);
```

#### -- 显示警告

```json
SHOW WARNING;
```

#### -- 插入单条数据

```mysql
INSERT INTO `tbl_user` ( `id`, `sorts`, `state` ) VALUES (1,1,1);
INSERT INTO `tbl_user` VALUES (1, 2, 1);
INSERT INTO `tbl_user` SET `sorts`=1, `id`=3, `state`=1;
```

#### -- 插入多条数据

```mysql
INSERT INTO `tbl_user` VALUES (1, 4, 1),(1, 5, 1),(1, 6, 1);
```

#### -- 查询用户表中数据

```mysql
SELECT * FROM `tbl_user`;
SELECT `id`,`sorts`,`state` FROM `tbl_user`;
```

#### -- 查询和计算

```mysql
SELECT 2*3 FROM `tbl_user`;
```

#### -- 条件查询

##### -- 比较运算符 =  ,  >  ,  <   ,  <>  ,  <=  ,  >=  

```mysql
SELECT * FROM `tbl_user` WHERE `id` = 1;
SELECT * FROM `tbl_user` WHERE `id` > 1;
SELECT * FROM `tbl_user` WHERE `id` < 6;
SELECT * FROM `tbl_user` WHERE `id` <> 1;
SELECT * FROM `tbl_user` WHERE `id` <= 1;
SELECT * FROM `tbl_user` WHERE `id` >= 1;
```

##### -- 逻辑运算符  AND、OR

```mysql
SELECT * FROM `tbl_user` WHERE `id` > 1 AND `id` < 6;
SELECT * FROM `tbl_user` WHERE `id` BETWEEN 1 AND 6;
SELECT * FROM `tbl_user` WHERE `id` = 1 OR `state` = 1;
```

##### -- 字符串模糊查询 LIKE ‘%%’ NOT LIKE ‘%%’

```mysql
SELECT * FROM `tbl_user` WHERE `name` LIKE '%W%';
SELECT * FROM `tbl_user` WHERE `name` LIKE '%W';
SELECT * FROM `tbl_user` WHERE `name` LIKE 'W%';
```

##### -- 按范围查询 IN、 NOT IN

```mysql
SELECT * FROM `tbl_user` WHERE `age` IN (20,25,30);
SELECT * FROM `tbl_user` WHERE `age` NOT IN (20,25,30);
```

#### -- 对查询出来的结果进行排序

```mysql
SELECT * FROM `tbl_user` ORDER BY `id` DESC;
SELECT * FROM `tbl_user` ORDER BY `id` ASC;
```



#### -- GROUP BY搭配COUNT函数使用(譬如30个学生分别来自4个城市，按城市分组后，查询出来的记录数是4条)

```mysql
SELECT `city`,COUNT(city) AS num FROM `tbl_user` GROUP BY `city` DESC;
```

#### -- HAVING只能使用在group by之后，用来对分组的结果进行再筛选

```mysql
SELECT `city`,COUNT(city) AS num FROM `tbl_user` GROUP BY `city` DESC HAVING COUNT(city) > 4;
```

#### -- having和where的区别

```mysql
SELECT `name` FROM `cv` WHERE `age` > 20 GROUP BY `city` DESC;
SELECT `city`,COUNT(city) FROM `tbl_user` GROUP BY `city` DESC HAVING COUNT(city) > 4;
```

-- 两者的区别是先后顺序不同，where是先筛选数据，having对分组的结果进行筛选

-- 按价格和销量排序

```mysql
SELECT * FROM `product` ORDER BY `price` DESC, `sold` ASC;
```

#### -- LIMIT offset：从符合条件的所有记录的第几条开始取数据,rows：取多少条数据

```mysql
SELECT `name` FROM `tbl_user` LIMIT 0,9;
```

#### -- 修改数据

```mysql
UPDATE `tbl_user` SET `name` = 'wt', 'id'=1, 'state'=3 WHERE `userid` = 1;
```

#### -- 删除数据

```mysql
DELETE FROM `tbl_user` WHERE `userid` = 1;
```

#### -- 清空表中全部数据

```mysql
TRUNCATE TABLE `tbl_user`;
```

#### -- 数据类型

```
整型类：BIGINT、INT、MEDIUMINT、SMALLINT、TINYINT
浮点型：FLOAT、DOUBLE
定点型：DECIMAL
字符类：CHAR、VARCHAR、TEXT
日期时间类：DATE、DATETIME、TIMESTAMP
其他类型：ENUM、SET、BIT、BLOB
浮点型：FLOAT(3,2)   输入1，保存为1.00……  
FLOAT(4,2)时，输入12.4875，按保留2位进行四舍五入，得到12.49
```

-- FORMAT：保留几位小数

```mysql
SELECT FORMAT(a - b, 1) FROM `float`;
```

-- CHAR(M) 是用来保存固定字符长度的字符串，譬如身份证号、手机号之类。

-- VARCHAR(M) 是用来保存可变长度的字符串，M是最大字符数

```mysql
CREATE TABLE `enumtest` (
    test_field ENUM('one', 'two', 'three')
);
```

-- BIT类型（大部分是用来处理权限的）

```mysql
CREATE TABLE `bittest` (
    test_field BIT(4)
);

INSERT INTO `bittest` VALUES (11);
INSERT INTO `bittest` VALUES (b'11');

SELECT bin(`test_field`) FROM `bittest`; -- (查询方式不同)
```

-- SET类型与ENUM非常相似，最大的区别是，ENUM是单选，而SET是多选，SET的最多可以设置64个可选值

```mysql
CREATE TABLE `settest` (
    `test_field` SET('one', 'two', 'three')
);

INSERT INTO settest VALUES ('one,two');  
-- 插入数据时需要注意，逗号（,）后面不可以有空格。如果插入不存在的值，系统会自动截取掉多余的值
```



#### -- MYSQL函数

```mysql
-- COUNT 计数
SELECT COUNT(*) FROM tbl_name;

-- NOW 取系统当前时间
INSERT INTO tbl_name VALUES (NOW());

-- FORMAT 指定小数位数
SELECT FORMAT(col_name, n) FROM tbl_name;

-- 求余数
SELECT MOD(31, 8);

-- TRUNCATE(小数，小数点后保留位数) 直接截取不四舍五入（注意与FORMAT的区别）
SELECT TRUNCATE(col_name, 1) FROM tbl_name;

-- 返回字节个数
SELECT CHAR_LENGTH('abcdefg');

-- 合并字符串
SELECT CONCAT('hello','world');

-- 使用分隔符合并字符串
CONCAT_WS('hello','-','world');   -- hello-world

-- 清除文字两侧的空白字符
SELECT TRIM('  HELLOWORLD  ')   -- HELLOWORLD

-- 假设有字符型字段friends保存了所有好有的姓名，其中保存的值均是以逗号（,）分隔（注意无空格）
-- 譬如：tom,jack,andy 或者 andy,susan,tommy
-- 如果想要查询好友是tom的记录，使用like查询很不方便
-- 可以使用SELECT * FROM `friends` WHERE FIND_IN_SET('tom',friends);

-- 将时间转化为时间戳
SELECT UNIX_TIMESTAMP('2013-01-01 10:10:10');  -- (date 类型数据转换成 timestamp 形式整数)

-- 将时间戳转化为时间
SELECT FROM_UNIXTIME(1479313381);				2012-12-12 08:32:40
select from_unixtime(1355272360,'%Y%m%d');    	20121212

-- IF 函数(如果表达式expr是TRUE，则IF()的返回值为T；否则IF()返回F)
SELECT IF (2>1,'T','F');

-- DATABASE 用来获取当前数据库的库名
SELECT DATABASE;

-- MD5 不可逆的加密函数
SELECT MD5('root'); 
```
#### -- 将字段设置唯一
```mysql
ALTER TABLE `cv` ADD UNIQUE KEY (`id`);
建表语句中添加：
CREATE TABLE `cv` (
...
  unique (`id`)
);
```

#### -- 将字段去除唯一

```mysql
ALTER TABLE `cv` DROP INDEX `id`;
```

#### -- 设置主键

```mysql
ALTER TABLE `cv` MODIFY `id` int(10) UNSIGNED PRIMARY KEY;
```

#### -- 删除主键

```mysql
ALTER TABLE `cv` DROP PRIMARY KEY;
```

#### -- 复合主键

```mysql
ALTER TABLE `cv` ADD PRIMARY KEY (`id`,`name`);
```

#### -- 建表时设置主键

```mysql
CREATE TABLE `cv` (
	`id` int(10) PRIMRAY KEY,
  	`name` varchar(20)
)
```

#### -- 复合主键设置

```mysql
CREATE TABLE `cv2` (
	`id` int(10),
  	`name` int(10),
  	PRIMARY KEY (`id`,`name`)
);
```

#### -- 给字段设置自增长

```mysql
CREATE TABLE `cv` (
	`id` int(10) NOT NULL PRIMARY KEY AUTO_INCREMENT
);

ALTER TABLE `cv` MODIFY `id` int(10) NOT NULL PRIMARY KEY AUTO_INCREMENT;
```

#### -- 获取前一条INSERT语句或者UPDATE语句执行后，对应记录中AUTO_INCREMENT字段的值

```mysql
SELECT LAST_INSERT_ID();
```

#### -- 字段设置默认值

```mysql
ALTER TABLE `cv` MODIFY `sex` tinyint(1) NOT NULL DEFAULT 'm';
```

#### -- 设置字段不为空

```mysql
ALTER TABLE `cv` MODIFY `sex` tinyint(1) NOT NULL DEFAULT 'm';
```

#### -- NOT NULL 和DEFAULT 关系

```tex
注意：在MySQL中，null是空值，它不代表0，也不代表’’，当你忘记给一个字段设置数据时，MySQL默认会设置一个null，这时not null才会起作用;
当用户没有给字段赋值时，默认赋null
如果字段设置了not null时，系统会查找是否有设置default，如果设置了，则用default来代替null；
当设置了not null，又没有设置default时，系统会报错

```



#### -- 连表查询

##### -- LEFT JOIN

```mysql
LEFT JOIN（左连接）以edu表的记录为基础，左表的记录会全部显示出来，右表如果没有能够关联上的数组，则记录中显示NULL

SELECT t1.`id`,t1.`name`,t1.`age`,t2.`school`,t2.`grade` FROM `tbl_user` t1 LEFT JOIN `tbl_school` ON t1.`id` = t2.`userId`;
```

##### -- RIGHT JOIN

```mysql
RIGHT JOIN（右连接）与左连接相反，查询出的结果以右表为主，如果edu表中的school_id与school表中的id有对应不上的情况，school_id错误的简历信息将无法显示

SELECT t1.`id`,t1.`name`,t1.`age`,t2.`school`,t2.`grade` FROM `tbl_user` t1 RIGHT JOIN `tbl_school` ON t1.`id` = t2.`userId`;
```

##### -- INNER JOIN

```mysql
INNER JOIN（内连接）既不以左表为准，也不以右表为准，只显示两边完全符合条件的记录

SELECT t1.`id`,t1.`name`,t1.`age`,t2.`school`,t2.`grade` FROM `tbl_user` t1 INNER JOIN `tbl_school` ON t1.`id` = t2.`userId`;
```

#### -- 外键

```tex
如果一张数据表中的某个字段的值是另外一张表的主键，那么我们可以称该字段为表的外键，因此，我们可以称：cv_id是edu表的外键；school_id也是edu表的外键。外键主要是用来保证数据的完整性，表里是否设置外键不影响其使用
```

#### -- 添加外键

```mysql
如果我们给数据表添加外键，需要注意，表中的外键所保存的值，与关联表里的id是否真正的能够对应更直白的说就是，shool_id里保存的id在school表里是否存在，如果不存在，添加外键时，系统会报错，因此，在测试时，我们可以直接执行truncate来清空错误数据，但是如果是在产品线上，需要通过批处理脚本，来统一修正错误数据，然后才可以添加外键

ALTER TABLE edu ADD FOREIGN KEY(cv_id) REFERENCES cv(id) ON DELETE CASCADE;
ALTER TABLE edu ADD FOREIGN KEY(school_id) REFERENCES school(id) ON DELETE CASCADE;
```

#### -- 约束条件

```t
当主表的数据被删除时，关联表的从数据如何处理。譬如school表里的记录被删除时，edu表里保存同样school_id的数据如何被系统自动处理
1、RESTRICT：约束     如果存在从数据，不允许删除主数据。 
2、NO ACTION     如果存在从数据，不允许删除主数据。
3、CASCADE：级联     删除主数据，顺便也删掉从数据。 
4、SET NULL     删除主数据，从数据外键的值设为NULL。 
```

#### -- 创建索引（如何快速的检索数据）

```mysql
ALTER TABLE tbl_name ADD INDEX index_name (col_name);
检查select语句是否使用了正确的索引，我们一般在select之前添加explain
ALTER TABLE cv ADD INDEX idx_gender(gender);
EXPLAIN SELECT * FROM cv WHERE gender = 1;
```

#### -- 组合索引

```tex
对多个字段组合创建的索引ALTER TABLE tbl_name ADD INDEX index_name (column1, column2);当where条件里不仅按一个字段去进行检索，还使用了其他字段，如果这种检索是经常性的操作，那么我们需要创建组合索引假设有组合索引（A,B,C）三个字段，相当于创建了三个索引可以使用（以左为准）（A,B,C）（A,B）（A）所以，查询的顺序很重要
```

