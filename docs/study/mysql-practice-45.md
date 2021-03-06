# MySQL查询语句练习题45题版

设有一数据库，包括四个表：学生表（Student）、课程表（Course）、成绩表（Score）以及教师信息表（Teacher）。表结构及数据如下，请完成题目。

表（一）Student (学生表) ：

属性名 | 数据类型 | 可否为空 | 含 义  
---|---|---|---  
Sno | varchar (20) | 否 | 学号（主码）  
Sname | varchar (20) | 否 | 学生姓名  
Ssex | varchar (20) | 否 | 学生性别  
Sbirthday | datetime | 可 | 学生出生年月  
Class | varchar (20) | 可 | 学生所在班级  

表（二）Course（课程表）：

属性名 | 数据类型 | 可否为空 | 含 义  
---|---|---|---  
Cno | varchar (20) | 否 | 课程号（主码）  
Cname | varchar (20) | 否 | 课程名称  
Tno | varchar (20) | 否 | 教工编号（外码）  

表（三）Score(成绩表)：

属性名 | 数据类型 | 可否为空 | 含 义  
---|---|---|---  
Sno | varchar (20) | 否 | 学号（外码））  
Cno | varchar (20) | 否 | 课程号（外码）  
Degree | Decimal(4,1) | 可 | 成绩  
主码：Sno+ Cno |  |  |  

表（四）Teacher(教师表)：

属性名 | 数据类型 | 可否为空 | 含 义  
---|---|---|---  
Tno | varchar (20) | 否 | 教工编号（主码）  
Tname | varchar (20) | 否 | 教工姓名  
Tsex | varchar (20) | 否 | 教工性别  
Tbirthday | datetime | 可 | 教工出生年月  
Prof | varchar (20) | 可 | 职称  
Depart | varchar (20) | 否 | 教工所在部门  

表1-2数据库中的数据：  
表（一）Student:

Sno | Sname | Ssex | Sbirthday | class  
---|---|---|---|---  
108 | 曾华 | 男 | 1977-09-01 | 95033  
105 | 匡明 | 男 | 1975-10-02 | 95031  
107 | 王丽 | 女 | 1976-01-23 | 95033  
101 | 李军 | 男 | 1976-02-20 | 95033  
109 | 王芳 | 女 | 1975-02-10 | 95031  
103 | 陆君 | 男 | 1974-06-03 | 95031  

表（二）Course:

Cno | Cname | Tno  
---|---|---  
3-105 | 计算机导论 | 825  
3-245 | 操作系统 | 804  
6-166 | 数字电路 | 856  
9-888 | 高等数学 | 831  

表（三）Score:

Sno | Cno | Degree  
---|---|---  
103 | 3-245 | 86  
105 | 3-245 | 75  
109 | 3-245 | 68  
103 | 3-105 | 92  
105 | 3-105 | 88  
109 | 3-105 | 76  
101 | 3-105 | 64  
107 | 3-105 | 91  
108 | 3-105 | 78  
101 | 6-166 | 85  
107 | 6-166 | 79  
108 | 6-166 | 81  

表（四）Teacher:

Tno | Tname | Tsex | Tbirthday | Prof | Depart  
---|---|---|---|---|---  
804 | 李诚 | 男 | 1958-12-02 | 副教授 | 计算机系  
856 | 张旭 | 男 | 1969-03-12 | 讲师 | 电子工程系  
825 | 王萍 | 女 | 1972-05-05 | 助教 | 计算机系  
831 | 刘冰 | 女 | 1977-08-14 | 助教 | 电子工程系  


```mysql
#建学生信息表student
create table student(
    sno varchar(20) not null primary key,
    sname varchar(20) not null,
    ssex varchar(20) not null,
    sbirthday datetime,
    class varchar(20)
);
#建立教师表
create table teacher
(
    tno varchar(20) not null primary key,
    tname varchar(20) not null,
    tsex varchar(20) not null,
    tbirthday datetime,
    prof varchar(20),
    depart varchar(20) not null
);
#建立课程表course
create table course
(
    cno varchar(20) not null primary key,
    cname varchar(20) not null,
    tno varchar(20) not null,
    foreign key(tno) references teacher(tno)
);
#建立成绩表
create table score
(
    sno varchar(20) not null primary key,
    foreign key(sno) references student(sno),
    cno varchar(20) not null,
    foreign key(cno) references course(cno),
    degree decimal
);

#添加学生信息
insert into student values('108','曾华','男','1977-09-01','95033');
insert into student values('105','匡明','男','1975-10-02','95031');
insert into student values('107','王丽','女','1976-01-23','95033');
insert into student values('101','李军','男','1976-02-20','95033');
insert into student values('109','王芳','女','1975-02-10','95031');
insert into student values('103','陆君','男','1974-06-03','95031');
#添加教师表
insert into teacher values('804','李诚','男','1958-12-02','副教授','计算机系');
insert into teacher values('856','张旭','男','1969-03-12','讲师','电子工程系');
insert into teacher values('825','王萍','女','1972-05-05','助教','计算机系');
insert into teacher values('831','刘冰','女','1977-08-14','助教','电子工程系');
#添加课程表
insert into course values('3-105','计算机导论','825');
insert into course values('3-245','操作系统','804');
insert into course values('6-166','数字电路','856');
insert into course values('9-888','高等数学','831');
#添加成绩表
insert into score values('103','3-245','86');
insert into score values('105','3-245','75');
insert into score values('109','3-245','68');
insert into score values('103','3-105','92');
insert into score values('105','3-105','88');
insert into score values('109','3-105','76');
insert into score values('103','3-105','64');
insert into score values('105','3-105','91');
insert into score values('109','3-105','78');
insert into score values('103','6-166','85');
insert into score values('105','6-166','79');
insert into score values('109','6-166','81');
```




## 1、 查询Student表中的所有记录的Sname、Ssex和Class列。

```mysql
#单表查询
select Sname,Ssex,Class from Student;
```




## 2、 查询教师所有的单位即不重复的Depart列。

```mysql
#单表查询
select distinct Depart from teacher;
```




## 3、 查询Student表的所有记录。

```mysql
#单表查询
select * from student
```




## 4、 查询Score表中成绩在60到80之间的所有记录。

```mysql
#单表查询
select * from Score where Degree between 60 and 80;
```




## 5、 查询Score表中成绩为85，86或88的记录。

```mysql
#单表查询
select * from Score where Degree in (85,86,88);
```




## 6、 查询Student表中“95031”班或性别为“女”的同学记录。

```mysql
#单表查询
select * from Student where Class= '95031' or Ssex='女';
```




## 7、 以Class降序查询Student表的所有记录。

```mysql
#单表查询
select * from Student order by Class desc;
```




## 8、 以Cno升序、Degree降序查询Score表的所有记录。

```mysql
#单表查询
select * from Score order by Cno asc,Degree desc;
```




## 9、 查询“95031”班的学生人数。

```mysql
#单表查询
select count(*) from Student where Class = '95031';
```




## 10、查询Score表中的最高分的学生学号和课程号。（子查询或者排序）

```mysql
#单表查询
select Sno,Cno from Score where Degree = (
select max(Degree) from Score);
select Sno,Cno from Score order by Degree desc limit 0,1;
```




## 11、 查询每门课的平均成绩。

```mysql
#单表查询
select Cno,avg(Degree) from Score group by Cno;
```




## 12、查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。

```mysql
#单表查询
select Cno,avg(Degree) from Score 
where Cno like '3%'
group by Cno 
having count(Sno) >=5;

select avg(Degree) from score where Cno like '3%' and Cno in (select Cno from score group by Cno having count(*)>=5);
```




## 13、查询分数大于70，小于90的Sno列。

```mysql
#单表查询
select Sno from Score where Degree >70 and Degree < 90;
```




## 14、查询所有学生的Sname、Cno和Degree列。

```mysql
#多表查询
#可以用where,内连接，子查询
select Sname,Cno,Degree from Student,Score where Student.Sno=Score.Sno;

select Sname,Cno,Degree from Student inner join Score
on Student.Sno=Score.Sno;
```




## 15、查询所有学生的Sno、Cname和Degree列。

```mysql
#多表查询
#同上
select Sno,Cname,Degree from Score,Course where Score.Cno=Course.Cno;
```




## 16、查询所有学生的Sname、Cname和Degree列。

```mysql
#多表查询
#同上
select Sname,Cname,Degree from student,course,score where student.Sno=score.Sno and course.Cno=score.Cno;

select Sname,Cname,Degree from Student inner join Score on Student.Sno=Score.Sno inner join 
Course on Score.Cno=Course.Cno;
```




## 17、查询“95033”班学生的平均分。

```mysql
#多表查询
#用where,内连接，子查询
select avg(Degree) from Score,Student where Student.Sno=Score.Sno and Class='95033';

select avg(Degree) from Score inner join Student on Student.Sno=Score.Sno where Class='95033';

select avg(Degree)  from Score where Sno in (select Sno from Student
where Class='95033');
```




## 18、 假设使用如下命令建立了一个grade表：

```mysql
create table grade(low  int(3),upp  int(3),rank  char(1))
insert into grade values(90,100,’A’)
insert into grade values(80,89,’B’)
insert into grade values(70,79,’C’)
insert into grade values(60,69,’D’)
insert into grade values(0,59,’E’)
```

现查询所有同学的Sno、Cno和rank列。

```mysql
#多表查询
select Sno,Cno,rank from Score,grade where Degree between low and upp;
```




## 19、查询选修“3-105”课程的成绩高于“109”号同学成绩的所有同学的记录。

```mysql
#单表查询
#109同学，选修是3-105课的
select * from score where Cno='3-105' and degree>(select max(degree ) from Score where Sno='109' and Cno='3-105' );
#109同学，没有选修3-105课
select * from Score where Cno = '3-105' and Degree > (
select max(Degree) from Score where Sno ='109')and degree<( select max(degree ) from Score where sno in (select Sno from score group by Sno having count(*)>1));
#选了多门课程并且是这个课程下不是最高分的
select * from score a where Sno in (select Sno from score group by Sno having count(*)>1) and degree<( select max(degree ) from Score b where b.cno = a.cno);
```




## 21、查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录。

```mysql
#单表查询
select * from Score where Degree >(
select Degree from Score where Cno = '109' and Course = '3-105');
```




## 22、查询和学号为108、101的同学同年出生的所有学生的Sno、Sname和Sbirthday列。

```mysql
#单表查询
select Sno,Sname,Sbirthday from Student where year(Sbirthday) in (
select year(Sbirthday) from Student where Sno='108' or '101');
```




## 23、查询“张旭“教师任课的学生成绩。

```mysql
#多表查询
#用where,内连接，子查询
select Sno,Degree from Score where Cno in (
select Cno from Course where Tno in(
select Tno from Teacher where Tname = '张旭'));

select Sno,Degree from Score,Course,Teacher where
Score.Cno=Course.Cno and Course.Tno=Teacher.Tno and Tname = '张旭' ;

select Sno,Degree from Score inner join Course on Score.Cno=Course.Cno inner join Teacher on Teacher.Tno=Course.Tno where Tname ='张旭' ;
```




## 24、查询选修某课程的同学人数多于5人的教师姓名。

```mysql
#多表查询
#用where,内连接，子查询
select Tname from Teacher where Tno in (
select Tno from Course where Cno in (
select Cno from Score group by Cno having count(Sno)>5));

select Tname from Teacher,  Course where Teacher.Tno=Course.Tno and Course.Cno =(select Cno from Score group by Cno having count(*)>5);
```




## 25、查询95033班和95031班全体学生的记录。

```mysql
#多表查询
#用where,内连接，子查询
select * from Student,Score,Course where Student.Sno=Score.Sno and Score.Cno=Course.Cno and class = '95033' or '95031';
```




## 26、 查询存在有85分以上成绩的课程Cno。

```mysql
select Cno from Score where Degree >85;
```




## 27、查询出“计算机系“教师所教课程的成绩表。

```mysql
#多表查询
#用where,内连接，子查询
select * from Score where Cno in (
select Cno from Course where Tno in (
select Tno from Teacher where Depart = '计算机系'));
#先找交集再not in
select Score.Sno,Score.Cno,Score.Degree from Score,Course,Teacher where Score.Cno=Course.Cno and Course.Tno=Teacher.Tno and Depart = '计算机系';

select Score.Sno,Score.Cno,Score.Degree from Score inner join Course on Score.Cno=Course.Cno inner join Teacher on Teacher.Tno =Course.Tno where Depart ='计算机系';
```




## 28、查询“计算 机系”与“电子工程系“不同职称的教师的Tname和Prof。

```mysql
select Tname,Prof from Teacher where Prof not in (
select Prof from Teacher where Prof in (select Prof from Teacher where Depart = '计算机系') and Depart = '电子工程系');

select Tname,Prof from Teacher where Prof not in (
select Prof from Teacher where Depart = '计算机系' or Depart ='电子工程系' group by Prof having count(*)>1);

select Tname,Prof from Teacher where Depart ='计算机系' and Prof  not in( select Prof from Teacher where Depart ='电子工程系')
union 
select Tname,Prof from Teacher where Depart ='电子工程系' and Prof  not in( select Prof from Teacher where Depart ='计算机系');
```




## 29、查询选修编号为“3-105“课程且成绩至少高于选修编号为“3-245”的同学的Cno、Sno和Degree,并按Degree从高到低次序排序。

```mysql
select Cno,Sno,Degree from Score where Cno = '3-105' and Degree >
any(select Degree from Score where Cno = '3-245') order by Degree desc;

select Cno,Sno,Degree from Score where Cno = '3-105' and Degree >
(select min(Degree) from Score where Cno = '3-245') order by Degree desc;
#大于任何一个或者大于最小的
```




## 30、查询选修编号为“3-105”且成绩高于选修编号为“3-245”课程的同学的Cno、Sno和Degree。

```mysql
select Cno,Sno,Degree from Score where Cno = '3-105' and Degree > all(select Degree from Score where Cno = '3-245');
```




## 31、 查询所有教师和同学的name、sex和birthday。

```mysql
select Sname as '学生',Ssex,Sbirthday from Student
union
select Tname as '老师',Tsex,Tbirthday from Teacher;
```




## 32、查询所有“女”教师和“女”同学的name、sex和birthday。

```mysql
select Sname,Ssex,Sbirthday from Student where Ssex = '女'
union
select Tname,Tsex,Tbirthday from Teacher where Tsex = '女';
```




## 33、 查询成绩比该课程平均成绩低的同学的成绩表。

```mysql
select * from score a  where degree < (
select avg(degree) from score b where b.cno=a.cno);
```




>这是一种特殊形式的父子表连接（自连接）SQL选择查询写法。对于这种特殊的写法，数据库引擎会以特殊的方式检索父查询表里的数据。如果搞不清楚这种特殊的检索方式，我们很难从该SQL语句的表面逻辑理出个中道理。  
>  现在我们来分拆该SQL语句里的父查询和子查询  
>  1）语句中的父查询  
>  select * from score a where degree<”子查询获得的一个数据值“  
>  2）语句中的子查询  
>  select avg(degree) from score b where a.cno=b.cno  
>  请注意这个子查询的from子句里只有一张表 b ，但是where子句里却出现了第二张表 a ，  
>
>如果单独运行这个子查询，因为子查询没有列出表a，系统会要求输入a.cno或者直接报错，反正无法顺利执行，但是表a可以在父查询里的from子句中找到，面对这种情况数据库引擎会采取逐条取主查询记录与子查询实施比对以确定是否检出该条记录，最后汇总各次检索的结果输出整个记录集。  
>  这个特殊的SQL语句检索过程大致如下：  
>
>取出首条记录的a.cno用作过滤，子查询里以avg函数得到该课程的平均分，主查询以分数比对平均分，满足条件保留否则抛弃（degree小于平均分的留下）；  
>  跟着判断父查询表下一条记录，处理过程相同，最后合并各次判断结果从而的到最终结果。  
>  这种特殊的写法可以规避输出包含非分组字段，而分组不得输出非分组字段的矛盾。

## 34、 查询所有任课教师的Tname和Depart。

```mysql
select Tname,Depart from Teacher where Tno in (
select Tno from Course);
```




## 35 、查询所有未讲课的教师的Tname和Depart。

```mysql
select Tname,Depart from Teacher where Tno not in (
select Tno from Course where Cno in (
select Cno from Score ));
```




## 36、查询至少有2名男生的班号。

```mysql
select Class from Student where Ssex = '男'
group by Class 
having count(Ssex) >1;
```




## 37、查询Student表中不姓“王”的同学记录。

```mysql
select * from Student where Sname not like '王%%';
```



## 38、查询Student表中每个学生的姓名和年龄。

```mysql
select Sname,year(now())-year(Sbirthday) from Student;
```




## 39、查询Student表中最大和最小的Sbirthday日期值。

```mysql
select max(Sbirthday),min(Sbirthday) from Student;
```




## 40、以班号和年龄从大到小的顺序查询Student表中的全部记录。

```mysql
select * from Student order by Class desc,Sbirthday;
```




## 41、查询“男”教师及其所上的课程。

```mysql
#需要两个表的信息用where或者连接查询
select Cno,Cname,Tname from Course,Teacher where Course.Tno=Teacher.Tno and Tsex='男';

select Cno,Cname,Tname from Course inner join Teacher on Course.Tno=Teacher.Tno where Tsex='男';
```




## 42、查询最高分同学的Sno、Cno和Degree列。

```mysql
select Sno,Cno,Degree from Score order by Degree desc limit 0,1;

select Sno,Cno,Degree from Score where Degree = (
select max(Degree) from Score);
```




## 43、查询和“李军”同性别的所有同学的Sname。

```mysql
select Sname from Student where Ssex = (
select Ssex from Student where Sname = '李军');
```




## 44、查询和“李军”同性别并同班的同学Sname。

```mysql
select Sname from Student where Ssex = (
select Ssex from Student where Sname = '李军')
and Class = (
select Class from Student where Sname = '李军');
```




## 45、查询所有选修“计算机导论”课程的“男”同学的成绩表。

```mysql
select * from Score where Cno = (
select Cno from Course where Cname = '计算机导论') and
Sno in (select Sno from Student where Ssex = '男');
```
