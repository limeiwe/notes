## 1.对数据库常用命令

1.连接数据库

*mysql -u用户名 -p密码*

2.显示已有数据库

*show databases;*

3.创建数据库

*create database sqlname;*

4.选择数据库

*use database sqlname;*

5.显示数据库中的表（先选择数据库）

*show tables;*

6.显示当前数据库的版本信息以及连接用户名

*select version(),user();*

7.删除数据库(删除时没有提示直接删除)

*drop database sqlname;*

## 2.数据库中对表的命令

1.创建表

(1)语法：

*create table tablename(*

字段1 数据类型 字段属性

…

字段n

);

(2)注意：

1.创建表时为了防止与保留字冲突，用’'括起来

2.单行注释：#…

多行注释：/*…*/

3.创建表时多字段中间用英文逗号隔开，最后一行不用逗号。

(3) 字段约束和属性

1.非空约束*not null(字段不允许为空)*

2.默认约束*default(设置默认值)*

3.唯一约束*unique key(uk)*(设置字段的值是唯一的，可为空，但只能有一个空值)

4.主键约束*primary key(pk)(作为表记录的唯一标识)*

5.外键约束*foreign key(fk)*(用于两个表之间建立关系，需要指定引用主表的哪一字段。在数据库的存储引擎中InnoDB支持外键，MyISAM不支持外键。

作为外键的字段要求是主表中的主键（单字段主键）)

添加外键约束：

***CONSTRAINT FK_外键名 FOREIGN KEY(字表中外键字段)REFERENCES 关联表名 (关联字段)。***

grandid作为字表的外键

![grandid](https://img-blog.csdn.net/20181007171300164?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![在这里插入图片描述](https://img-blog.csdn.net/20181007171156133?atermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1.*设置自增auto_increment=n,从n开始。*

2.*设置自增set @@ auto_increment_increment=m,步长为m。*![在这里插入图片描述](https://img-blog.csdn.net/20181007173641842?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

3.多字段设置主键：*primary key(字段1，字段2…字段n)*

4.表中的注释/说明性文字：)*comment=“说明文字”;*

5.设置字符集：)*charset=“字符集”;*

6.查看表的结构:*describe’表名’/desc 表名*

7.查看数据库定义：*show create database sqlname;*

8.查看数据表定义：*show create table tablename;*

9.查看默认存储引擎：*show variables like’storage_engine%’;*

11.指定表的存储引擎：)*engine=存储引擎;*

10.删除表：*drop table ‘tablename’;*

11.获取当前日期：*now();*

12.修改表:

(1)修改表名：*alter table 旧表名 rename 新表名；*

(2)添加字段：*alter table 表名 add 字段名 数据类型…;(添加新的字段)*

(3)修改字段：*alter table 表名 change 原字段名 新字段名 数据类型…;*

(4)删除字段：*alter table 表名 drop 字段名;*

(5)在创建完表以后添加主键约束：

**alter table 表名 add constraint 主键名 primary key 表名(主键字段);**

(6)创建完表以后添加外键约束(作为外键的字段要求是主表中的主键（单字段主键)）:

**alter table 表名 add constraint 外键名 foreign key(外键字段) references 关联表名 (关联字段);**

## 插入数据

1.插入单行数据：

**insert into 表名 (字段名列表(逗号隔开)) values(值列表(逗号隔开));**

2.插入多行数据 ：

**insert into 表名(字段名列表) values (值列表1), … ,(值列表n);**

3.将查询结果插入到新表中：

**create table 新表(select 字段1, … ,from 原表);**

```pgsql
查询student表中的id，name，sex，phone数据插入到newstudent表中：

CREATE TABLE newstudent(SELECT id,`name`,sex,phone FROM student);
```

3.更新数据（修改数据）：
**update 表名 set 列名=更新值 where 更新条件;**

```routeros
修改newstudent表中id=1001的数据名字为tom：

UPDATE newstudent SET `name`='tom' WHERE id=1001;
```

4.删除数据

(1)**delete from 表名 where 删除条件;**

delete 删除的是整条数据，不会只删除单个列。

```haxe
删除newstudent表中名字为tom的数据：

DELETE FROM newstudent WHERE `name`='tom';
```

(2)truncate table 删除数据：

truncate table 删除的是表中所有的行，但表的结构，列，约束，索引等不会改变。不能用于有外键约束的表。删除数据不能恢复。

**truncate table 表名 where 删除条件;**

## 数据查询

**1.使用select查询**

select 列名/表达式/函数/常量 from 表名 where 查询条件 order by 排序的列名asc/desc;

(1)查询所有的数据行和列：

*select \* from 表名；*

(2)查询部分行和列：

*select 列名… from 表名 where 查询条件；*

(3)在查询中使用列的别名：

*select 列名 **AS** 新列名 form 表名 where 查询条件；*

计算，合并得到新的列名：

*select **列名1+’.’+列名2 AS** **新列名** from 表名；*

(4)查询空值：

通过****\*is null\***** 或者 ***is not null*** 判断列值是否为空

```n1ql
查询student表中Email为空的学生姓名：

SELECT `name` FROM student WHERE Email IS NULL;
```

**2.分组查询**![在这里插入图片描述](https://img-blog.csdn.net/20181010190210526?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```n1ql
#查询不同课程的平均分，最低分，最高分,并查询出平均分大于80分的课程
SELECT r.subjectno,sub.`SubjectName` 课程名称,AVG(StudentResult) 平均分,
MAX(StudentResult) 最高分,MIN(StudentResult) 最低分
FROM result r INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo` 
GROUP BY r.subjectno
#where AVG(StudentResult)>=80出现错误，
#分组查询group by 在where语句后，
#group by 约束条件使用having语句
HAVING AVG(StudentResult)>=80;
```

![在这里插入图片描述](https://img-blog.csdn.net/20181010192451417?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 常用函数

1.聚合函数：

(1）AVG (平均值):select avg(列名)from 表名

假设列名为成绩 则查询到的是表中所有成绩的平均值。

（2）count 返回某字段的行数

（3）max 返回某字段的最大数

（4）min 返回某字段的最小值

（5）sum 返回某字段的和。

2.字符串函数：

（1）concat() 连接字符串s1,s2…sn为一个完整的字符串。

（2）insert(s1,p1,n,news)将字符串s1从p1位置开始，n个字符长的字串替换为字符串news。

（3）lower(s)将字符串s中的所有字符改为小写。

（4）upper(s)将字符串s中的所有字符改为大写。

（5）substring(s,num,len)返回字符串s的第num个位置开始长度为len的子字符串。

3.时间日期函数：

（1）获取当前日期：curdate();

（2）获取当前时间：curtime();

（3）获取当前日期和时间：now();

（4）返回日期date为一年中的第几周：week(date);

（5）返回日期date的年份：year(date);

（6）返回时间time的小时值：hour(time);

（7）返回时间time的分钟值：minute(time);

（8）返回日期参数（date1和date2之间相隔的天数）:datediff(date1,date2);

（9）计算日期参数date加上n天后的日期：adddate(date,n);

4.数学函数

（1）返回大于或等于数值x的最小整数：ceil(x);

（2）返回小于或等于数值x的最大整数：floor(x)；

（3）返回0~1之间的随机数：rand();

***order by 子句***

order by子句按照一定的顺序排列查询结果，asc升序排列，desc降序排列。

***limit子句***

显示**指定位置***指定行数*的记录。

select 字段名列表 form 表名 where 约束条件 group by分组的字段名 order by 排序列名 limit 位置偏移量,行数;

**#查询学生信息里gid=1按学号升序排列前四条记录**![在这里插入图片描述](https://img-blog.csdn.net/20181008175815967?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```n1ql
#查询学生信息里gid=1按学号升序排列前四条记录（步长）
SELECT id,`name` FROM `student1` WHERE gid=1 ORDER BY id LIMIT 4;
（查询表里全部信息中gid=1的前四个学生）
```

查询结果：
![在这里插入图片描述](https://img-blog.csdn.net/2018100817574089?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```n1ql
#查询学生信息里gid=1按学号升序排列前四条记录（位置偏移量，步长）
SELECT id,`name` FROM `student1` WHERE gid=1 ORDER BY id LIMIT 4,4;
（查询表中全部信息gid=1前四条以后的全部信息中的前四条学生信息）
```

查询结果：
![在这里插入图片描述](https://img-blog.csdn.net/20181008175903151?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 模糊查询

***in子查询\**\**\**not in 子查询***

使用in关键字可以使父查询匹配子查询返回的多个单字段值。

解决使用比较运算符（=,>等），子查询返回值不唯一错误信息。

***like模糊查询***

LIKE语句语法格式：select * from 表名 where 字段名 like 对应值（子串）。

它主要是针对字符型字段的，它的作用是在一个字符型字段列中检索包含对应子串的。

***A:%*** ***包含零个或多个字符的任意字符串***： 1、LIKE’Mc%’ 将搜索以字母 Mc 开头的所有字符串（如 McBadden）。

2、LIKE’%inger’ 将搜索以字母 inger 结尾的所有字符串（如 Ringer、Stringer）。

3、LIKE’%en%’ 将搜索在任何位置包含字母 en 的所有字符串（如 Bennet、Green、McBadden）。

***B:_（下划线） 任何单个字符***：LIKE’_heryl’ 将搜索以字母 heryl 结尾的所有六个字母的名称（如 Cheryl、Sheryl）。

***C：[ ] 指定范围 ([a-f]) 或集合 ([abcdef]) 中的任何单个字符***：、

1，LIKE’[CK]ars[eo]n’ 将搜索下列字符串：Carsen、Karsen、Carson 和 Karson（如 Carson）。

2、LIKE’[M-Z]inger’ 将搜索以字符串 inger 结尾、以从 M 到 Z 的任何单个字母开头的所有名称（如 Ringer）

***D：[^] 不属于指定范围 ([a-f]) 或集合 ([abcdef]) 的任何单个字符：***LIKE’M[^c]%’ 将搜索以字母 M 开头，并且第二个字母不是 c 的所有名称（如MacFeather）。
　　***E：\* 它同于DOS命令中的通配符，代表多个字符***：c\*c代表cc,cBc,cbc,cabdfec等多个字符。
　　***F：？同于DOS命令中的？通配符***，代表单个字符 :b?b代表brb,bFb等
　　***G：# 大致同上，不同的是代只能代表单个数字***。k#k代表k1k,k8k,k0k 。
　　***F：[!] 排除 它只代表单个字符***
　　下面我们来举例说明一下：
　　例1，查询name字段中包含有“明”字的。
　　select \* from table1 where name like ‘%明%’
　　例2，查询name字段中以“李”字开头。
　　select \* from table1 where name like '李*’

例3，查询name字段中含有数字的。

select * from table1 where name like ‘%[0-9]%’

例4，查询name字段中含有小写字母的。

select * from table1 where name like ‘%[a-z]%’

例5，查询name字段中不含有数字的。

select * from table1 where name like ‘%[!0-9]%’

***可以自定义转移符----》escape’自定义转移符’*** 　***distinct------》去除重复项*** 　 　***between\**\*and\**模糊查询***

操作符 BETWEEN … AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

***null ，not null查询***

```pgsql
-- 查询手机号不为null的用户数据
SELECT * from user where phone is not null;
 
-- 查询手机号为null的用户数据
SELECT * from user where phone is null;
```

***exists 子查询 not exists子查询***

exists子查询用来确认后边的查询是否继续进行

drop table if exists test—>判断是否存在表test，如果存在就删除。

not exists实现取反操作。对不存在对应查询条件的记录。

## 多表连接查询

多表连接查询是通过各个表之间共同列的关联性来查询数据。

**1.内连接查询**

内连接查询根据表中共同的列进行匹配。取两个的表的交集。两个表存在主外键关系是通常使用内连接查询。

内连接使用inner join…on 关键字或者where子句来进行表之间的关联。

inner 可省略 on 用来设置条件。

（1）在where子句中指定连接条件

（2）在from中使用inner join…on关键字

```n1ql
#查询学生姓名和成绩
SELECT studentname,studentresult FROM student s,result r
WHERE s.`StudentNo`=r.`StudentNo`
#在from中使用inner join....on关键字
SELECT s.`StudentName`,r.`StudentResult` ,r.`SubjectNo`FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
```

两种方法查询结果相同。

**2.外连接查询**

外连接查询中参与连接的表有主从之分，已主表的每行数据匹配从表的数据列，将符合连接条件的数据直接返回到结果集中，对不符合连接条件的列，将被填上null值再返回到结果集中。

*（1）左外连接查询* **left join…on** 或者**left outer join…on**关键字进行表之间的关联。

```autohotkey
SELECT s.`StudentName`,r.`StudentResult` ,r.`SubjectNo`FROM student s
LEFT JOIN result r ON s.`StudentNo`=r.`StudentNo`
```

将没有成绩的学生成绩查出。

*（2）右外连接查询*

右外连接包含右表中所有的匹配行，右表中有的项在左表中没有对应的项将以null值填充。

**right join…on** 或**right outer join…on**关键字进行表之间的关联。

*（3）自连接*

把一个表作为两个表使用。

```sql
#创建一个表
CREATE TABLE book(
id INT(10),
sort INT(10),
books VARCHAR(10) NOT NULL
);
#插入数据
INSERT INTO book VALUES (2,1,'古文书'),
(3,1,'现代书'),
(4,2,'《三字经》'),
(5,2,'《唐诗三百首》'),
(6,3,'《我与地坛》'),
(7,2,'《游大林寺》'),
(8,2,'《王右军年减十岁时》'),
(9,3,'《致橡树》');

#查询结果为：
#书籍类型       书籍名
#古文书         三字经....
#现代书         我与地坛....

SELECT a.books 书籍类型, b.books 书籍名  
FROM book a,book b
WHERE a.id=b.sort;
```

![在这里插入图片描述](https://img-blog.csdn.net/20181010183000513?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

自连接查询结果：

![在这里插入图片描述](https://img-blog.csdn.net/20181010183011161?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## MySQL的事务，视图，索引，备份和恢复

**1.事务**

事务是指将一系列数据操作捆绑成为一个整体进行统一管理。

把所有的命令作为一个整体一起向系统提交或者撤销造组偶请求。

事务属性：原子性，一致性，隔离性，持久性。

myISA存储引擎不支持事务。

关闭事务自动提交：set autocommit=0；

（1）开始事务：begin/start transaction；

（2）提交事务：commit；

（3）回滚/撤销事务：rollback；

恢复自动提交：set autocommit=1；

设置结果集以？？编码格式显示：set names ？？；

**2.视图**

视图是一种查看数据库中一个或多个表中数据的方法。视图是一种虚拟表，作为来自一个或多个表的行或列的子集创建的。视图充当查询中的表筛选器的角色。

（1）创建视图：create view 视图名 as <select语句>

（2）删除视图：drop view 视图名；

（3）查看视图数据：select 。。。。。from 视图名；

**3.索引**

索引类似于书的目录，使用索引可以将数据库程序无须对整个表扫描就可以在其中找到所需数据。

（1）普通数据：允许重复和空值。

（2）唯一索引：不允许出现重复。可以有多个唯一索引。

（3）主键索引：非空，唯一。删除时drop primary key；

（4）复合索引：将多个列组合作为索引。？

（5）全文索引：可重复和空值，在char，varchar，text创建。

***where match（列名）against （‘查找内容’）；***

（6）空间索引：对空间数据类型的列建立的索引。

**创建索引：**

create 【索引类型】index 索引名 on 表名 （创建索引的列）；

或者创建表时之间在列后面加上索引类型。

或者修改表alter table 表名 add index 索引名 （索引列）；

删除索引：drop index 索引名；

查看索引：show index from 表名；

**4.数据库备份和恢复** **1.使用mysqldump命令备份数据库**

mysqldump -u -p 数据库名>备份数据库位置及名字；

**表数据导出到文本文件**

select *from 表名 where 查询条件 into outfile 备份数据库位置及名字；

**2.使用mysql命令恢复数据库**（先创建新的数据库）

mysql -u -p 新创建数据库名<所要恢复数据库位置及文件名；

**source命令恢复数据库**

source 数据库备份文件；

**新建用户**

```autohotkey
#创建本地用户
CREATE USER `user`@`localhost` IDENTIFIED BY '123123';
#用户可登陆任何远程主机，使用通配符%
CREATE USER `user2`@`123%` IDENTIFIED BY '123123';
#对用户进行全部权限授权
GRANT ALL ON mysql.`user` TO `user2`@`123%`;
#对已创建的用户授权
GRANT SELECT,INSERT ON mysql.`user` TO `user2`@`123%`; 
#创建用户时授权
GRANT SELECT,INSERT ON mysql.`user` TO `user_2`@`123%` IDENTIFIED BY '123123';
#删除用户user2(使用删除语句时必须拥有数据库全局权限或select权限)
DROP USER `user2`@`123%`;
DROP USER `user_2`@`123%`;
DROP USER `user`@`localhost`;
#mysqladmin修改超级用户user2账户密码(mysqladmin命令在cmd中使用，只能修改超级用户密码)
mysqladmin -u root -p PASSWORD "123456";
#修改当前登录用户密码
SET PASSWORD =PASSWORD("123456");
#修改其他用户密码
SET PASSWORD FOR `user2`@`123%`=PASSWORD("123456");
```

![在这里插入图片描述](https://img-blog.csdn.net/201810171849376?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQyOTkyNjQz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



