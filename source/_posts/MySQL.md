---
title: MySQL
date: 
cover: https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202210161023662.jpeg
tags: sql
categories: 学习笔记
description: 数据库、JDBC、数据库连接池
updated:
---

# 	MySQL

**SQL语句分类**

DDL:数据定义语句[create表，库...]

DML:数据操作语句[增加insert,修改update，删除 delete]

DQL:数据查询语句[select ]

DCL:数据控制语句[管理数据库:比如用户权限grant revoke ]

## **数据库基本操作**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208031548136.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208031650195.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208031701557.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208031734240.png)



## 常用数据类型（列类型）

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208031744446.png)



![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208032131524.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208051932191.png)

```sql
CREATE TABLE emp(
		`id` INT,
		`name` VARCHAR(32),
		`sex` CHAR(1),
		`birthday` DATE,
		`entry_date` DATETIME,
		`job` VARCHAR(32),
		`Salary` DOUBLE,
		`resume` TEXT)CHARACTER SET utf8 COLLATE utf8_bin ENGINE INNODB;
		
INSERT INTO emp VALUES(100,'张三','男','2000-12-11','2022-08-03 22:01:20','Java开发','20000','法外狂徒');
		
ALTER TABLE emp ADD 
		`image` VARCHAR(32) NOT NULL DEFAULT '' AFTER resume;
		
ALTER TABLE emp MODIFY  
		`job` VARCHAR(60) NOT NULL DEFAULT '';
		
DESC emp 

ALTER TABLE emp DROP `sex`;

RENAME table emp TO employee;

ALTER TABLE employee CHARACTER SET utf8;

ALTER TABLE employee CHANGE `name` `user_name` VARCHAR(64) NOT NULL DEFAULT '';

DESC employee;
```



## 数据库CRUD语句

1. Insert语句(添加数据)

   ![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208051958270.png)

   ```sql
   #说明 insert 语句的细节
   -- 1.插入的数据应与字段的数据类型相同。
   -- 比如 把 'abc' 添加到 int 类型会错误
   -- 2. 数据的长度应在列的规定范围内，例如：不能将一个长度为 80 的字符串加入到长度为 40 的列中。
   -- 3. 在 values 中列出的数据位置必须与被加入的列的排列位置相对应。
   -- 4. 字符和日期型数据应包含在单引号中。
   -- 5. 列可以插入空值[前提是该字段允许为空]，insert into table value(null)
   -- 6. insert into tab_name (列名..) values (),(),() 形式添加多条记录
   -- 7. 如果是给表中的所有字段添加数据，可以不写前面的字段名称
   -- 8. 默认值的使用，当不给某个字段值时，如果有默认值就会添加默认值，否则报错
   -- 如果某个列 没有指定 not null ,那么当添加数据时，没有给定值，则会默认给 null 
   -- 如果我们希望指定某个列的默认值，可以在创建表时指定
   ```

2. Update语句(更新数据)

   ![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208051958354.png)

   ```sql
   UPDATE employee SET salary =50000;
   
   UPDATE employee SET salary =30000 WHERE user_name='张三';
   
   UPDATE employee SET salary =salary+10000 WHERE user_name='王五';
   
   SELECT *FROM employee;
   
   -- 1.UPDATE语法可以用新值更新原有表行中的各列。
   -- 2.SET子句指示要修改哪些列和要给予哪些值。
   -- 3. WHERE学句指定应更新哪些行。如没有WHERE子句，则更新所有的行(记录)，一定小心。
   -- 4.如果需要修改多个字段，可以通过set字段1=值1,字段2=值2....
   ```

3. Delete语句(删除数据)

   ![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052013134.png)

   ```sql
   DELETE FROM employee WHERE user_name = '张三';
   
   SELECT *FROM employee;
   
   -- 1.如果不使用where子句，将删除表中所有数据。
   -- 2. Delete语句不能删除某一列的值(可使用update设为null 或者"")
   -- 3.使用delete语句仅删除记录,不删除表本身。如要删除表，使用drop table语句。drop table 表名;
   ```

   

4. Select语句(查找数据)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052018639.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052019919.png)

```sql
CREATE TABLE student (
	id INT NOT NULL DEFAULT 1,
	NAME VARCHAR ( 20 ) NOT NULL DEFAULT '',
	chinese FLOAT NOT NULL DEFAULT 0.0,
	english FLOAT NOT NULL DEFAULT 0.0,
	math FLOAT NOT NULL DEFAULT 0.0 
);
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 1, '刘备', 89, 78, 90 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 2, '张飞', 67, 98, 56 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 3, '宋江', 87, 78, 77 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 4, '关羽', 88, 98, 90 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 5, '赵云', 82, 84, 67 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 6, '欧阳锋', 55, 85, 45 );
INSERT INTO student ( id, NAME, chinese, english, math )VALUES( 7, '黄蓉', 75, 65, 30 );
-- 1.查询表中所有学生的信息。
SELECT * FROM student;
-- 2.查询表中所有学生的姓名和对应的英语成绩。
SELECT `name`,english FROM student;
-- 3.过滤表中重复数据distinct .
SELECT DISTINCT english from student;
-- 4.要查询的记录，每个字段都相同，才会去重
SELECT DISTINCT `name` ,english FROM student;
```

### **使用表达式对查询的列进行运算**![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052031553.png)

### **在 select 语句中可使用 as 语句**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052032883.png)

```sql
-- 统计每个学生的总分
SELECT `name` ,(chinese+english+math) FROM student;
-- 在所有学生总分上加10分
SELECT `name` ,(chinese+english+math+10) FROM student;
-- 使用别名表示学生分数
SELECT `name` ,(chinese+english+math+10) AS total_score FROM student;
```

### **在where子句中经常使用的运算符**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052036369.png)

```sql
1.查询姓名为赵云的学生成绩
SELECT * FROM student WHERE `name` = '赵云' ;
2.查询英语成绩大于90分的同学
SELECT * FROM student WHERE english > 90;
3.查询总分大于200分的所有同学
SELECT * FROM student WHERE (chinese+english+math) >200;
4.查询math大于60并且(and) id大于4的学生成绩
SELECT * FROM student WHERE math > 60 AND id > 4;
5.查询英语成绩大于语文成绩的同学
SELECT * FROM student WHERE english > chinese;
6.查询总分大于200分并且数学成绩大于语文成绩,的姓刘的学生.
SELECT * FROM student WHERE (chinese+english+math) >200 AND math > chinese AND `name`LIKE'刘%';
7.查询英语分数在80 - 90之间的同学。
SELECT * FROM student WHERE english BETWEEN 80 AND 90;
8.查询数学分数为89,90,91的同学。
SELECT * FROM student WHERE math IN(89,90,91);
9.查询数学比语文少30分的同学。
SELECT * FROM student WHERE math+30 <= chinese ;
```

### **使用order by 子句排序查询结果**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052056369.png)

```sql
-- 按数学成绩升序
SELECT * FROM student ORDER BY math;
-- 按总分降序
SELECT `name`,(chinese+english+math)AS total_score FROM student ORDER BY total_score DESC;
-- 对姓刘的学生成绩[总分]排序输出(升序) where + order by
SELECT `name`, (chinese + english + math) AS total_score FROM student
WHERE `name` LIKE '刘%' ORDER BY total_score;
```

### **合计/统计函数**

1.count

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052103331.png)

```sql
-- 统计一个班级共有多少学生?
SELECT COUNT(*) FROM student;
-- 统计数学成绩大于80的学生有多少个？
SELECT count(*) FROM student WHERE math > 80;
-- 统计总分大于250的人数有多少？
SELECT count(*) FROM student WHERE (math+english+chinese) > 250;
-- count(*)和count(列)的区别
-- 解释 :count(*) 返回满足条件的记录的行数
-- count(列): 统计满足条件的某列有多少个，但是会排除 为 null 的情况
CREATE TABLE t15 (
`name` VARCHAR(20));
INSERT INTO t15 VALUES('tom');
INSERT INTO t15 VALUES('jack');
INSERT INTO t15 VALUES('mary');
INSERT INTO t15 VALUES(NULL);
SELECT * FROM t15;
SELECT COUNT(*) FROM t15; -- 4
SELECT COUNT(`name`) FROM t15;-- 3
```

2.sum

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052111942.png)

```sql
-- 统计一个班级数学总成绩？
SELECT SUM(math) FROM student; 
-- 统计一个班级语文、英语、数学各科的总成绩
SELECT SUM(math) AS math_total_score,SUM(english),SUM(chinese) FROM student; 
-- 统计一个班级语文、英语、数学的成绩总和
SELECT SUM(math + english + chinese) FROM student; 
-- 统计一个班级语文成绩平均分
SELECT SUM(chinese)/ COUNT(*) FROM student;
-- sum 仅对数值起作用
SELECT SUM(`name`) FROM student;
```

3.avg

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052115350.png)

```sql
-- 求一个班级数学平均分？
SELECT AVG(math) FROM student; -- 求一个班级总分平均分
SELECT AVG(math + english + chinese）FROM student;
```

4.mxa/min

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052116864.png)

```sql
-- 求班级最高分和最低分（数值范围在统计中特别有用）
SELECT MAX(math + english + chinese), MIN(math + english + chinese)FROM student; 
-- 求出班级数学最高分和最低分
SELECT MAX(math) AS math_high_socre, MIN(math) AS math_low_socre FROM student;
```

5.使用 group by 子句对列进行分组

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052118209.png)

6.使用 having 子句对分组后的结果进行过滤

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208052119160.png)

```sql
-- 如何显示每个部门的平均工资和最高工资
SELECT AVG(sal),MAX(sal),deptno FROM emp GROUP BY deptno;
-- 显示每个部门的每种岗位的平均工资和最低工资
SELECT AVG(sal),MIN(sal),deptno,job FROM emp GROUP BY deptno,job;
-- 显示平均工资低于 2000 的部门号和它的平均工资 // 别名
SELECT AVG(sal)AS avg_sal,deptno FROM emp GROUP BY deptno HAVING avg_sal < 2000;
```

### **字符串相关函数**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208072108112.png)

```sql
-- CHARSET(str) 返回字串字符集
SELECT CHARSET(ename) FROM emp; 

-- CONCAT (string2 [,... ]) 连接字串, 将多个列拼接成一列
SELECT CONCAT(ename, ' 工作是 ', job) FROM emp; 

-- INSTR (string ,substring ) 返回 substring 在 string 中出现的位置,没有返回 0
-- dual 亚元表, 系统表 可以作为测试表使用
SELECT INSTR('hanshunping', 'ping') FROM DUAL; 

-- UCASE (string2 ) 转换成大写
SELECT UCASE(ename) FROM emp; 

-- LCASE (string2 ) 转换成小写
SELECT LCASE(ename) FROM emp; 

-- LEFT (string2 ,length )从 string2 中的左边起取 length 个字符
-- RIGHT (string2 ,length ) 从 string2 中的右边起取 length 个字符
SELECT LEFT(ename, 2) FROM emp; 

-- LENGTH (string )string 长度[按照字节]
SELECT LENGTH(ename) FROM emp; 

-- REPLACE (str ,search_str ,replace_str )
-- 在 str 中用 replace_str 替换 search_str
-- 如果是 manager 就替换成 经理
SELECT ename, REPLACE(job,'MANAGER', '经理') FROM emp;

-- STRCMP (string1 ,string2 ) 逐字符比较两字串大小
SELECT STRCMP('hsp', 'hsp') FROM DUAL; 

-- SUBSTRING (str , position [,length ])
-- 从 str 的 position 开始【从 1 开始计算】,取 length 个字符
-- 从 ename 列的第一个位置开始取出 2 个字符
SELECT SUBSTRING(ename, 1, 2) FROM emp; 

-- LTRIM (string2 ) RTRIM (string2 ) TRIM(string)
-- 去除前端空格或后端空格
SELECT LTRIM(' 学习学习') FROM DUAL;
SELECT RTRIM('学习学习 ') FROM DUAL;
SELECT TRIM(' 学习学习 ') FROM DUAL; 

-- 练习: 以首字母小写的方式显示所有员工 emp 表的姓名
-- 思路先取出 ename 的第一个字符，转成小写的
-- 把他和后面的字符串进行拼接输出即可
SELECT CONCAT(LCASE(SUBSTRING(ename,1,1)),SUBSTRING(ename,2))AS new_name FROM emp;
SELECT CONCAT(LCASE(LEFT(ename,1)), SUBSTRING(ename,2)) AS new_name FROM emp
```

### **数学相关函数**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208072125228.png)

```sql
-- 演示数学相关函数

-- ABS(num) 绝对值
SELECT ABS(-10) FROM DUAL; 

-- BIN (decimal_number )十进制转二进制
SELECT BIN(10) FROM DUAL; 

-- CEILING (number2 ) 向上取整, 得到比 num2 大的最小整数
SELECT CEILING(-1.1) FROM DUAL; 

-- CONV(number2,from_base,to_base) 进制转换
-- 下面的含义是 8 是十进制的 8, 转成 2 进制输出
SELECT CONV(8, 10, 2) FROM DUAL; 
-- 下面的含义是 16 是 16 进制的 16, 转成 10 进制输出
SELECT CONV(16, 16, 10) FROM DUAL; 

-- FLOOR (number2 ) 向下取整,得到比 num2 小的最大整数
SELECT FLOOR(-1.1) FROM DUAL; 

-- FORMAT (number,decimal_places ) 保留小数位数(四舍五入)
SELECT FORMAT(78.125458,2) FROM DUAL; 

-- HEX (DecimalNumber ) 转十六进制

-- LEAST (number , number2 [,..]) 求最小值
SELECT LEAST(0,1, -10, 4) FROM DUAL; 

-- MOD (numerator ,denominator ) 求余
SELECT MOD(10, 3) FROM DUAL; 

-- RAND([seed]) RAND([seed]) 返回随机数 其范围为 0 ≤ v ≤ 1.0
-- 1. 如果使用 rand() 每次返回不同的随机数 ，在 0 ≤ v ≤ 1.0
-- 2. 如果使用 rand(seed) 返回随机数, 范围 0 ≤ v ≤ 1.0, 如果 seed 不变，
-- 该随机数也不变了
SELECT RAND() FROM DUAL;
```

### **时间日期相关函数**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208072132800.png)

```sql
-- 日期时间相关函数

-- CURRENT_DATE ( ) 当前日期
SELECT CURRENT_DATE() FROM DUAL; 
-- CURRENT_TIME ( )当前时间
SELECT CURRENT_TIME() FROM DUAL; 
-- CURRENT_TIMESTAMP ( ) 当前时间戳
SELECT CURRENT_TIMESTAMP() FROM DUAL

-- 创建测试表 信息表
CREATE TABLE mes(
id INT ,
content VARCHAR(30), 
send_time DATETIME
);

-- 添加一条记录
INSERT INTO mes VALUES(1, '北京新闻', CURRENT_TIMESTAMP());
INSERT INTO mes VALUES(2, '上海新闻', NOW());
INSERT INTO mes VALUES(3, '广州新闻', NOW());
SELECT * FROM mes;
SELECT NOW() FROM DUAL; 

-- 上应用实例
-- 显示所有新闻信息，发布日期只显示 日期，不用显示时间. 
SELECT id, content, DATE(send_time)FROM mes; 
-- 请查询在 10 分钟内发布的新闻, 思路一定要梳理一下. 
SELECT * FROM mes WHERE DATE_ADD(send_time, INTERVAL 10 MINUTE) >= NOW();
SELECT * FROM mes WHERE send_time >= DATE_SUB(NOW(), INTERVAL 10 MINUTE);

-- 请在 mysql 的 sql 语句中求出 2011-11-11 和 1990-1-1 相差多少天
SELECT DATEDIFF('2011-11-11', '1990-01-01') FROM DUAL; 

-- 请用 mysql 的 sql 语句求出你活了多少天? 1986-11-11 出生
SELECT DATEDIFF(NOW(), '1986-11-11') FROM DUAL; 

-- 如果你能活 80 岁，求出你还能活多少天.1986-11-11 出生
-- 先求出活 80 岁 时, 是什么日期 X
-- 然后在使用 datediff(x, now()); 1986-11-11->datetime
-- INTERVAL 80 YEAR ： YEAR 可以是 年月日，时分秒
-- '1986-11-11' 可以 date,datetime timestamp
SELECT DATEDIFF(DATE_ADD('1986-11-11', INTERVAL 80 YEAR), NOW())FROM DUAL;

SELECT TIMEDIFF('10:11:11', '06:10:10') FROM DUAL; 
-- YEAR|Month|DAY| DATE (datetime )
SELECT YEAR(NOW()) FROM DUAL;
SELECT MONTH(NOW()) FROM DUAL;
SELECT DAY(NOW()) FROM DUAL;
SELECT MONTH('2013-11-10') FROM DUAL; 

-- unix_timestamp() : 返回的是 1970-1-1 到现在的秒数
SELECT UNIX_TIMESTAMP() FROM DUAL; 

-- FROM_UNIXTIME() : 可以把一个 unix_timestamp 秒数[时间戳]，转成指定格式的日期
-- %Y-%m-%d 格式是规定好的，表示年月日
-- 意义：在开发中，可以存放一个整数，然后表示时间，通过 FROM_UNIXTIME 转换
SELECT FROM_UNIXTIME(1618483484, '%Y-%m-%d') FROM DUAL;
SELECT FROM_UNIXTIME(1618483100, '%Y-%m-%d %H:%i:%s') FROM DUAL;

```

### **加密和系统函数**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208072148044.png)

```sql
-- 演示加密函数和系统函数

-- USER() 查询用户
-- 可以查看登录到 mysql 的有哪些用户，以及登录的 IP
SELECT USER() FROM DUAL; -- 用户@IP 地址

-- DATABASE()查询当前使用数据库名称
SELECT DATABASE(); 

-- MD5(str) 为字符串算出一个 MD5 32 的字符串，常用(用户密码)加密
-- root 密码是 hsp -> 加密 md5 -> 在数据库中存放的是加密后的密码
SELECT MD5('fzy') FROM DUAL;
SELECT LENGTH(MD5('fzy')) FROM DUAL; 

-- 演示用户表，存放密码时，是 md5
CREATE TABLE hsp_user
(id INT , `name` VARCHAR(32) NOT NULL DEFAULT '', pwd CHAR(32) NOT NULL DEFAULT '');
INSERT INTO hsp_user VALUES(100, '韩顺平', MD5('hsp'));
SELECT * FROM hsp_user; 
-- SQL 注入问题
SELECT * FROM hsp_user WHERE `name`='韩顺平' AND pwd = MD5('hsp')

-- PASSWORD(str) -- 加密函数, MySQL 数据库的用户密码就是 PASSWORD 函数加密
SELECT PASSWORD('hsp') FROM DUAL; -- 数据库的 *81220D972A52D4C51BB1C37518A2613706220DAC
-- select * from mysql.user \G 从原文密码 str 计算并返回密码字符串
-- 通常用于对 mysql 数据库的用户密码加密
-- mysql.user 表示 数据库.表
SELECT * FROM mysql.user;
```

### **流程控制函数**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208072159288.png)

```sql
# 演示流程控制语句
# IF(expr1,expr2,expr3) 如果 expr1 为 True ,则返回 expr2 否则返回 expr3
SELECT IF(TRUE, '北京', '上海') FROM DUAL;
# IFNULL(expr1,expr2) 如果 expr1 不为空 NULL,则返回 expr1,否则返回 expr2
SELECT IFNULL( NULL, '韩顺平教育') FROM DUAL;
# SELECT CASE WHEN expr1 THEN expr2 WHEN expr3 THEN expr4 ELSE expr5 END; [类似多重分支.]
# 如果 expr1 为 TRUE,则返回 expr2,如果 expr2 为 t, 返回 expr4, 否则返回 expr5
SELECT CASE
WHEN TRUE THEN 'jack' -- jack
WHEN FALSE THEN 'tom' 
ELSE 'mary' END;

-- 1. 查询 emp 表, 如果 comm 是 null , 则显示 0.0
-- 判断是否为 null 要使用 is null, 判断不为空 使用 is not
SELECT ename, IF(comm IS NULL , 0.0, comm)FROM emp;
SELECT ename, IFNULL(comm, 0.0)FROM emp; 
-- 2. 如果 emp 表的 job 是 CLERK 则显示 职员， 如果是 MANAGER 则显示经理
-- 如果是 SALESMAN 则显示 销售人员，其它正常显示
SELECT ename, (SELECT CASE
WHEN job = 'CLERK' THEN '职员' WHEN job = 'MANAGER' THEN '经理' WHEN job = 'SALESMAN' THEN '销售人员' ELSE job END) AS 'job' FROM emp;
```

### **增强**

```sql
-- 查询加强

-- ■ 使用 where 子句
-- ?如何查找 1992.1.1 后入职的员工
-- 在 mysql 中,日期类型可以直接比较, 需要注意格式
SELECT * FROM emp WHERE hiredate > '1992-01-01';

-- ■ 如何使用 like 操作符(模糊)
-- %: 表示 0 到多个任意字符 _: 表示单个任意字符
-- ?如何显示首字符为 S 的员工姓名和工资
SELECT ename, sal FROM emp WHERE ename LIKE 'S%' 

-- ?如何显示第三个字符为大写 O 的所有员工的姓名和工资
SELECT ename, sal FROM emp WHERE ename LIKE '__O%' ;

-- ■ 如何显示没有上级的雇员的情况
SELECT * FROM emp WHERE mgr IS NULL; 

-- ■ 查询表结构
DESC emp

-- 使用 order by 子句
-- ?如何按照工资的从低到高的顺序[升序]，显示雇员的信息
SELECT * FROM emp ORDER BY sal;
-- ?按照部门号升序而雇员的工资降序排列 , 显示雇员信息
SELECT * FROM emp ORDER BY deptno ASC , sal DESC;
```

### **分页查询**

基本语法:select ... limit start, rows

表示从start+1行开始取,取出rows行, start 从0开始计算

### **分组函数和分组子句 group by**

```sql
-- (1) 显示每种岗位的雇员总数、平均工资。
SELECT COUNT(*), AVG(sal), job FROM emp GROUP BY job; 

-- (2) 显示雇员总数，以及获得补助的雇员数。
-- 思路: 获得补助的雇员数 就是 comm 列为非 null, 就是 count(列)，如果该列的值为 null, 是
-- 不会统计 
SELECT COUNT(*), COUNT(comm) FROM emp;

-- 扩展要求：统计没有获得补助的雇员数
SELECT COUNT(*), COUNT(IF(comm IS NULL, 1, NULL)) FROM emp;
SELECT COUNT(*), COUNT(*) - COUNT(comm) FROM emp;

-- (3) 显示管理者的总人数。小技巧:尝试写->修改->尝试[正确的]
SELECT COUNT(DISTINCT mgr) FROM emp; 

-- (4) 显示雇员工资的最大差额。
-- 思路： max(sal) - min(sal)
SELECT MAX(sal) - MIN(sal) FROM emp;
```

### **顺序**

如果select语句同时包含有group by ,having ,limit，order by

那么他们的顺序是group by , having , orderby,limit

```sql
-- 请统计各个部门的平均工资，并且是大于1000的,并且按照平均工资从高到低排序，取出前两行记录.
SELECT deptno,AVG(sal)as avg_sal FROM emp 
				GROUP BY deptno  
				HAVING avg_sal >1000 
				ORDER BY  avg_sal DESC 
				LIMIT 0,2;
```

## 多表查询

```sql
-- 多表查询
-- ?显示雇员名,雇员工资及所在部门的名字 【笛卡尔集】
/*
1. 雇员名,雇员工资 来自 emp 表
2. 部门的名字 来自 dept 表
3. 需求对 emp 和 dept 查询 ename,sal,dname,deptno
4. 当我们需要指定显示某个表的列是，需要 表.列表
*/
SELECT ename,sal,dname,emp.deptno
FROM emp, dept
WHERE emp.deptno = dept.deptno;

SELECT * FROM emp;
SELECT * FROM dept;
SELECT * FROM salgrade; 
-- 小技巧：多表查询的条件不能少于 表的个数-1, 否则会出现笛卡尔集

-- ?如何显示部门号为 10 的部门名、员工名和工资
SELECT ename,sal,dname,emp.deptno
FROM emp, dept
WHERE emp.deptno = dept.deptno AND emp.deptno = 10

-- ?显示各个员工的姓名，工资，及其工资的级别
-- 思路 姓名，工资 来自 emp 13
-- 工资级别 salgrade 5
-- 写 sql , 先写一个简单，然后加入过滤条件... 
select ename, sal, grade
from emp , salgrade
where sal between losal and hisal;

SELECT ename,sal,dname FROM emp,dept WHERE emp.deptno=dept.deptno ORDER BY emp.deptno DESC;
```

### 自连接

自连接是指在同一张表的连接查询[将同一张表看做两张表]。

```sql
-- 多表查询的 自连接
-- 思考题: 显示公司员工名字和他的上级的名字
-- 分析： 员工名字 在 emp, 上级的名字的名字 emp
-- 员工和上级是通过 emp 表的 mgr 列关联
-- 小结：
-- 自连接的特点 1. 把同一张表当做两张表使用
-- 2. 需要给表取别名 表名 表别名
-- 3. 列名不明确，可以指定列的别名 列名 as 列的别名
SELECT worker.ename AS '职员名' , boss.ename AS '上级名' FROM emp worker, emp boss
WHERE worker.mgr = boss.empno;
```

## 子查询

子查询是指嵌入在其它 sql 语句中的 select 语句,也叫嵌套查询

单行子查询是指只返回一行数据的子查询语句

多行子查询指返回多行数据的子查询 使用关键字 in

```sql
-- 子查询的演示

-- 如何显示与 SMITH 同一部门的所有员工?
/*
1. 先查询到 SMITH 的部门号得到
2. 把上面的 select 语句当做一个子查询来使用
*/
SELECT deptno FROM emp WHERE ename = 'SMITH';

SELECT * FROM emp WHERE deptno = (
					SELECT deptno
					FROM emp
					WHERE ename = 'SMITH' 
					)
					
-- 多行子查询
-- 如何查询和部门 10 的工作相同的雇员的
-- 名字、岗位、工资、部门号, 但是不含 10 号部门自己的雇员.
SELECT ename,job,sal,deptno 
		FROM emp 
		WHERE job IN ( 
				SELECT DISTINCT job
						FROM emp 
						WHERE deptno = 10
						) AND deptno != 10;
```

**在多行子查询中使用 all ，any操作符**

```sql
-- 显示工资比部门 30 的所有员工的工资高的员工的姓名、工资和部门号
SELECT ename, sal, deptno
		FROM emp
		WHERE sal > ALL(
				SELECT sal
				FROM emp
				WHERE deptno = 30
		);

SELECT ename, sal, deptno
		FROM emp
		WHERE sal > (
				SELECT MAX(sal)
				FROM emp
				WHERE deptno = 30
		);
		
-- 如何显示工资比部门 30 的其中一个员工的工资高的员工的姓名、工资和部门号
SELECT ename, sal, deptno
		FROM emp
		WHERE sal > any(
			SELECT sal
			FROM emp
			WHERE deptno = 30
		);
		
SELECT ename, sal, deptno
		FROM emp
		WHERE sal > (
			SELECT MIN(sal)
			FROM emp
			WHERE deptno = 30
		);
```

### 当临时表使用

```sql
-- 查询 ecshop 中各个类别中，价格最高的商品
-- 查询 商品表
-- 先得到 各个类别中，价格最高的商品 max + group by cat_id, 当做临时表
-- 把子查询当做一张临时表可以解决很多很多复杂的查询
select goods_id, ecs_goods.cat_id, goods_name, shop_price
		from (
				SELECT cat_id , MAX(shop_price) as max_price
				FROM ecs_goods
				GROUP BY cat_id
		) temp , ecs_goods
		where temp.cat_id = ecs_goods.cat_id
		and temp.max_price = ecs_goods.shop_price;
```



### 多列子查询

```sql
-- 多列子查询
-- 请思考如何查询与 allen 的部门和岗位完全相同的所有雇员(并且不含 allen 本人)
-- (字段 1， 字段 2 ...) = (select 字段 1，字段 2）

SELECT * FROM emp 
			WHERE (deptno , job ) = (
							SELECT deptno ,job 
							FROM emp 
							WHERE ename = 'ALLEN'
				) AND ename <> 'ALLEN';
				
SELECT * FROM student 
			WHERE (math,english,chinese)=(
							SELECT math,english,chinese 
							FROM student 
							WHERE `name`='宋江'
							);
```

## 表复制

### 自我复制数据(蠕虫复制)

```sql
CREATE TABLE my_tab01
				( id INT, `name` VARCHAR(32), sal DOUBLE, job VARCHAR(32), deptno INT);
DESC my_tab01;
SELECT * FROM my_tab01;

-- 演示如何自我复制
-- 1. 先把 emp 表的记录复制到 my_tab01
INSERT INTO my_tab01
		(id, `name`, sal, job,deptno)
		SELECT empno, ename, sal, job, deptno 
		FROM emp; 

-- 2. 自我复制
INSERT INTO my_tab01 SELECT * FROM my_tab01;
SELECT COUNT(*) FROM my_tab01;

-- 如何删除掉一张表重复记录
-- 1. 先创建一张表 my_tab02, 
-- 2. 让 my_tab02 有重复的记录
CREATE TABLE my_tab02 LIKE emp; 
-- 这个语句 把 emp 表的结构(列)，复制到 my_tab02
DESC my_tab02;
INSERT INTO my_tab02 SELECT * FROM emp;
SELECT * FROM my_tab02; 
-- 3. 考虑去重 my_tab02 的记录
/*
思路
(1) 先创建一张临时表 my_tmp , 该表的结构和 my_tab02 一样
(2) 把 my_tmp 的记录 通过 distinct 关键字 处理后 把记录复制到 my_tmp
(3) 清除掉 my_tab02 记录
(4) 把 my_tmp 表的记录复制到 my_tab02
(5) drop 掉 临时表 my_tmp
*/ 
-- (1) 先创建一张临时表 my_tmp , 该表的结构和 my_tab02 一样
create table my_tmp like my_tab02;
-- (2) 把 my_tmp 的记录 通过 distinct 关键字 处理后 把记录复制到 my_tmp
insert into my_tmp select distinct * from my_tab02;
-- (3) 清除掉 my_tab02 记录
delete from my_tab02; 
-- (4) 把 my_tmp 表的记录复制到 my_tab02
insert into my_tab02 select * from my_tmp; 
-- (5) drop 掉 临时表 my_tmp
drop table my_tmp;

select * from my_tab02;
```

## 合并查询

```sql
-- 合并查询

SELECT ename,sal,job FROM emp WHERE sal>2500; -- 5
SELECT ename,sal,job FROM emp WHERE job='MANAGER'; -- 3

-- union all 就是将两个查询结果合并，不会去重
SELECT ename,sal,job FROM emp WHERE sal>2500 -- 5
UNION ALL
SELECT ename,sal,job FROM emp WHERE job='MANAGER'; -- 3

-- union 就是将两个查询结果合并，会去重
SELECT ename,sal,job FROM emp WHERE sal>2500 -- 5
UNION
SELECT ename,sal,job FROM emp WHERE job='MANAGER';
```

## 外连接

1.左外连接(如果左侧的表完全显示我们就说是左外连接)

2.右外连接(如果右侧的表完全显示我们就说是右外连接)

```sql
CREATE TABLE stu (
id INT, `name` VARCHAR(32));
INSERT INTO stu VALUES(1, 'jack'),(2,'tom'),(3, 'kity'),(4, 'nono');
SELECT * FROM stu;

CREATE TABLE exam(
id INT, grade INT);
INSERT INTO exam VALUES(1, 56),(2,76),(11, 8);
SELECT * FROM exam;

-- 使用左连接
-- （显示所有人的成绩，如果没有成绩，也要显示该人的姓名和 id 号,成绩显示为空）
SELECT `name`, stu.id, grade
FROM stu, exam
WHERE stu.id = exam.id; 

-- 改成左外连接
SELECT `name`, stu.id, grade
		FROM stu 
		LEFT JOIN exam
		ON stu.id = exam.id;

-- 使用右外连接（显示所有成绩，如果没有名字匹配，显示空)
-- 即：右边的表(exam) 和左表没有匹配的记录，也会把右表的记录显示出来
SELECT `name`, stu.id, grade
		FROM stu 
		RIGHT JOIN exam
		ON stu.id = exam.id;
		
-- 列出部门名称和这些部门的员工信息(名字和工作)，
-- 同时列出那些没有员工的部门名。
-- 使用左外连接实现
SELECT dname,ename,job
		FROM dept 
		LEFT JOIN emp
		ON dept.deptno=emp.deptno; 
-- 使用右外连接实现
SELECT dname,ename,job
		FROM emp
		RIGHT JOIN dept
		ON dept.deptno=emp.deptno; 
```

## 约束

约束用于确保数据库的数据满足特定的商业规则。在mysql中，约束包括: not null，unique,primary key,foreign key,和check五种.

### primary key（主键）

**字段名 字段类型 primary key**

用于唯一的标示表行的数据,当定义主键约束后，该列不能重复。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208082204642.png)

```sql
-- 主键使用
-- id name email
CREATE TABLE t17
(id INT PRIMARY KEY, -- 表示 id 列是主键
`name` VARCHAR(32),email VARCHAR(32)); 
-- 主键列的值是不可以重复
INSERT INTO t17
VALUES(1, 'jack', 'jack@sohu.com');
INSERT INTO t17
VALUES(2, 'tom', 'tom@sohu.com');
INSERT INTO t17
VALUES(1, 'hsp', 'hsp@sohu.com');
SELECT * FROM t17; 

-- 主键使用的细节讨论
-- primary key 不能重复而且不能为 null。
INSERT INTO t17
VALUES(NULL, 'hsp', 'hsp@sohu.com'); 

-- 一张表最多只能有一个主键, 但可以是复合主键(比如 id+name)
CREATE TABLE t18
(id INT PRIMARY KEY, -- 表示 id 列是主键
`name` VARCHAR(32), PRIMARY KEY -- 错误的
email VARCHAR(32)); 

-- 演示复合主键 (id 和 name 做成复合主键)
CREATE TABLE t18
(id INT , `name` VARCHAR(32),
email VARCHAR(32), PRIMARY KEY (id, `name`) -- 这里就是复合主键
);
INSERT INTO t18
VALUES(1, 'tom', 'tom@sohu.com');
INSERT INTO t18
VALUES(1, 'jack', 'jack@sohu.com');
INSERT INTO t18
VALUES(1, 'tom', 'xx@sohu.com'); -- 这里就违反了复合主键
SELECT * FROM t18; 

-- 主键的指定方式 有两种
-- 1. 直接在字段名后指定：字段名 primakry key
-- 2. 在表定义最后写 primary key(列名);
CREATE TABLE t19
(id INT , `name` VARCHAR(32) PRIMARY KEY, email VARCHAR(32)
);
CREATE TABLE t20
(id INT , `name` VARCHAR(32) , email VARCHAR(32), PRIMARY KEY(`name`) -- 在表定义最后写 primary key(列名)
); 

-- 使用 desc 表名，可以看到 primary key 的情况
DESC t20 -- 查看 t20 表的结果，显示约束的情况
DESC t18
```

### not null(非空)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208082211314.png)

### unique(唯一）

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208082211995.png)

### foreign key(外键)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208082213171.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208082213801.png)

```sql
-- 外键演示

-- 创建 主表 my_class
CREATE TABLE my_class (
id INT PRIMARY KEY , -- 班级编号
`name` VARCHAR(32) NOT NULL DEFAULT ''); 

-- 创建 从表 my_stu
CREATE TABLE my_stu (
id INT PRIMARY KEY , -- 学生编号
`name` VARCHAR(32) NOT NULL DEFAULT '', class_id INT , -- 学生所在班级的编号
-- 下面指定外键关系
FOREIGN KEY (class_id) REFERENCES my_class(id));

DESC my_stu;

-- 测试数据
INSERT INTO my_class
VALUES(100, 'java'), (200, 'web');
INSERT INTO my_class
VALUES(300, 'php');
SELECT * FROM my_class;

SELECT * FROM my_class;
INSERT INTO my_stu
VALUES(1, 'tom', 100);
INSERT INTO my_stu
VALUES(2, 'jack', 200);
INSERT INTO my_stu
VALUES(3, 'hsp', 300);
INSERT INTO my_stu
VALUES(4, 'mary', 400); -- 这里会失败...因为 400 班级不存在

INSERT INTO my_stu
VALUES(5, 'king', NULL); -- 可以, 外键 没有写 not null
SELECT * FROM my_class; 
SELECT * FROM my_stu; 
-- 一旦建立主外键的关系，数据不能随意删除了
DELETE FROM my_class
WHERE id = 100;
```

### check

用于强制行数据必须满足的条件，假定在sal列上定义了check约束，并要求sal列值在1000-2000之间如果不再1000-2000之间就会提示出错。

```sql
CREATE TABLE t23 (
	id INT PRIMARY KEY, 
    `name` VARCHAR(32) , 
    sex VARCHAR(6) CHECK (sex IN('man','woman')), 
    sal DOUBLE CHECK ( sal > 1000 AND sal < 2000)
);
```



```sql
CREATE TABLE goods(
		goods_id INT PRIMARY KEY,
		goods_name VARCHAR(64) NOT NULL DEFAULT '',
		unitprice DECIMAL(10,2) NOT NULL DEFAULT 0 CHECK (unitprice BETWEEN 1.0 AND 9999.99), 
		provider VARCHAR(64) NOT NULL DEFAULT '');
		
CREATE TABLE customer(
		customer_id  VARCHAR(16) PRIMARY KEY,
		`name` VARCHAR(64) NOT NULL DEFAULT '',
		`address` VARCHAR(64)NOT NULL DEFAULT '',
		`email` VARCHAR(64) UNIQUE NOT NULL,
		`sex` ENUM('男','女') NOT NULL,
		card_Id VARCHAR(18));


CREATE TABLE purchase(
		order_id INT UNSIGNED PRIMARY KEY,
		customer_id VARCHAR(16)NOT NULL DEFAULT '',
		goods_id INT NOT NULL DEFAULT 0,
		nums INT NOT NULL DEFAULT 0, 
		FOREIGN KEY (customer_id) REFERENCES customer(customer_id), 
		FOREIGN KEY (goods_id) REFERENCES goods(goods_id));
		
		
DESC goods;
DESC customer;
DESC purchase;
```

## 自增长

1.一般来说自增长是和primary key配合使用的

2自增长也可以单独使用[但是需要配合一个unique]

3.自增长修饰的字段为整数型的(虽然小数也可以但是非常非常少这样使用)

4.自增长默认从1开始,你也可以通过如下命令修改alter table表名auto_increment=新的开始值;

5.如果你添加数据时，给自增长字段(列)指定的有值，则以指定的值为准,如果指定了自增长，一般来说，就按照自增长的规则来添加数据。

```sql
-- 演示自增长的使用
-
- 创建表
CREATE TABLE t24
(id INT PRIMARY KEY AUTO_INCREMENT, email VARCHAR(32)NOT NULL DEFAULT '', `name` VARCHAR(32)NOT NULL DEFAULT '');
DESC t24;

-- 测试自增长的使用
INSERT INTO t24
VALUES(NULL, 'tom@qq.com', 'tom');
INSERT INTO t24
(email, `name`) VALUES('hsp@sohu.com', 'hsp');
SELECT * FROM t24; 

-- 修改默认的自增长开始值

CREATE TABLE t25
(id INT PRIMARY KEY AUTO_INCREMENT, email VARCHAR(32)NOT NULL DEFAULT '', `name` VARCHAR(32)NOT NULL DEFAULT '');
ALTER TABLE t25 AUTO_INCREMENT = 100;
INSERT INTO t25
VALUES(NULL, 'mary@qq.com', 'mary');
INSERT INTO t25
VALUES(666, 'hsp@qq.com', 'hsp');
INSERT INTO t25
VALUES(NULL, 'fzy@qq.com', 'fzy'); --667
SELECT * FROM t25;

```



## 索引

**类型**

1主键索引,主键自动的为主索引(类型Primary key)

2.唯一索引(UNIQUE)

3.普通索引(INDEX)

4.全文索引 (FULLTEXT)[适用于MylSAM]一般开发,不使用mysql自带的全文索引,而是使用:全文搜索Solr和 ElasticSearch (ES)

```sql
-- 演示 mysql 的索引的使用
-- 创建索引
CREATE TABLE t25 (
id INT , `name` VARCHAR(32)); 
-- 查询表是否有索引
SHOW INDEXES FROM t25;

-- 添加索引
-- 添加唯一索引
CREATE UNIQUE INDEX id_index ON t25 (id); 
-- 添加普通索引方式 1
CREATE INDEX id_index ON t25 (id); 

-- 如何选择
-- 1. 如果某列的值，是不会重复的，则优先考虑使用 unique 索引, 否则使用普通索引

-- 添加普通索引方式 2
ALTER TABLE t25 ADD INDEX id_index (id);

-- 添加主键索引
CREATE TABLE t26 (
id INT , `name` VARCHAR(32));
ALTER TABLE t26 ADD PRIMARY KEY (id);
SHOW INDEX FROM t26;

-- 删除索引
DROP INDEX id_index ON t25;
-- 删除主键索引
ALTER TABLE t26 DROP PRIMARY KEY;

-- 修改索引 ， 先删除，在添加新的索引
-- 查询索引
-- 1. 方式
SHOW INDEX FROM t25;
-- 2. 方式
SHOW INDEXES FROM t25;
-- 3. 方式
SHOW KEYS FROM t25;
-- 4 方式
DESC t25;
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208091340169.png)



## 事务

事务用于保证数据的一致性,它由一组相关的dml语句组成，该组的dml语句要么全部成功，要么全部失败。如：转账就要用事务来处理，用以保证数据的一致性。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092127741.png)

```sql
-- 事务的一个重要的概念和具体操作

-- 1. 创建一张测试表
CREATE TABLE t27
( id INT, `name` VARCHAR(32)); 
-- 2. 开始事务
START TRANSACTION
-- 3. 设置保存点
SAVEPOINT a
-- 执行 dml 操作
INSERT INTO t27 VALUES(100, 'tom');
SELECT * FROM t27;
SAVEPOINT b
-- 执行 dml 操作
INSERT INTO t27 VALUES(200, 'jack');
-- 回退到 b
ROLLBACK TO b
-- 继续回退 a
ROLLBACK TO a
-- 如果这样, 表示直接回退到事务开始的状态. ROLLBACK
COMMIT
```

## 事务隔离级别

1.多个连接开启各自事务操作数据库中数据时，数据库系统要负责隔离操作，以保证各个连接在获取数据时的准确性。

2.如果不考虑隔离性，可能会引发如下问题:脏读、不可重复读、幻读。

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092139464.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092142382.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092156869.png)

## 事务 ACID

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092207552.png)

## 表类型和存储引擎

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092212458.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208092213888.png)

1. MylSAM不支持事务、也不支持外键，但其访问速度快，对事务完整性没有要求
2. InnoDB存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是比起MyISAM存储引擎，InnoDB写的处理效率差一些并且会占用更多的磁盘空间以保留数据和索引。
3. MEMORY存储引擎使用存在内存中的内容来创建表。每个MEMORY表只实际对应上个磁盘文件。MEMORY类型的表访问非常得快，因为它的数据是放在内存中的,并且默认使用HASH索引。但是一旦MySQL服务关闭，表中的数据就会丢失掉,表的结构还在。

```sql
-- 表类型和存储引擎
-- 查看所有的存储引擎
SHOW ENGINES;
-- innodb 存储引擎，
-- 1. 支持事务 2. 支持外键 3. 支持行级锁

-- myisam 存储引擎
CREATE TABLE t28 (
id INT, `name` VARCHAR(32)) ENGINE MYISAM
-- 1. 添加速度快 2. 不支持外键和事务 3. 支持表级锁
START TRANSACTION;
SAVEPOINT t1
INSERT INTO t28 VALUES(1, 'jack');
INSERT INTO t28 VALUES(2, 'bob');
SELECT * FROM t28;
ROLLBACK TO t1;

-- memory 存储引擎
-- 1. 数据存储在内存中[关闭了 Mysql 服务，数据丢失, 但是表结构还在]
-- 2. 执行速度很快(没有 IO 读写) 3. 默认支持索引(hash 表)
CREATE TABLE t29 (
id INT, `name` VARCHAR(32)) ENGINE MEMORY;
DESC t29;
INSERT INTO t29
VALUES(1,'tom'), (2,'jack'), (3, 'hsp');
SELECT * FROM t29;

-- 指令修改存储引擎
ALTER TABLE `t29` ENGINE = INNODB;
```

**如何选择表的存储引擎**

1.如果你的应用不需要事务，处理的只是基本的CRUD操作，那么MylSAM是不二选择,速度快

2.如果需要支持事务,选择lnnoDB。

3.Memory存储引擎就是将数据存储在内存中，由于没有磁盘I./O的等待，速度极快。但由于是内存存储引擎，所做的任何修改在服务器重启后都将消失。(经典用法用户的在线状态().)

## 视图

1.视图是根据基表(可以是多个基表)来创建的视图是虚拟的表

2.视图也有列，数据来自基表

3.通过视图可以修改基表的数据

4.基表的改变，也会影响到视图的数据

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208111918791.png)

```sql
-- 视图的使用

-- 创建一个视图 emp_view01，只能查询 emp 表的(empno、ename, job 和 deptno ) 信息
-- 创建视图
CREATE VIEW emp_view01
AS
SELECT empno, ename, job, deptno FROM emp; 
-- 查看视图
DESC emp_view01;
SELECT * FROM emp_view01;
SELECT empno, job FROM emp_view01; 

-- 查看创建视图的指令
SHOW CREATE VIEW emp_view01;
-- 删除视图
DROP VIEW emp_view01;

-- 视图的细节
-- 1. 创建视图后，到数据库去看，对应视图只有一个视图结构文件(形式: 视图名.frm)
-- 2. 视图的数据变化会影响到基表，基表的数据变化也会影响到视图[insert update delete ]
-- 修改视图 会影响到基表
UPDATE emp_view01
SET job = 'MANAGER' WHERE empno = 7369;
SELECT * FROM emp; -- 查询基表
SELECT * FROM emp_view01;
-- 修改基本表， 会影响到视图
UPDATE emp
SET job = 'SALESMAN' WHERE empno = 7369;
-- 3. 视图中可以再使用视图 , 比如从 emp_view01 视图中，选出 empno,和 ename 做出新视图
DESC emp_view01;
CREATE VIEW emp_view02
AS
SELECT empno, ename FROM emp_view01;
SELECT * FROM emp_view02;
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208111926381.png)

## 管理

**创建用户**

create user ‘用户名’@’允许登录位置’ identified by ‘密码’

说明:创建用户，同时指定密码

**删除用户**

drop user ‘用户名’ @ ’允许登录位置’ ;

**用户修改密码**

修改自己的密码：

set password = password(密码');

修改他人的密码(需要有修改用户密码权限):

set password for '用户名'@'登录位置= password('密码');

SET PASSWORD FOR root@localhost = '123456';

**权限**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208111939454.png)

**给用户授权**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208111949344.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208111952117.png)

```sql
-- 演示 用户权限的管理

-- 创建用户 fzy 密码 123 , 从本地登录
CREATE USER 'fzy'@'localhost' IDENTIFIED BY '123' -- 使用 root 用户创建 testdb ,表 news
CREATE DATABASE testdb
CREATE TABLE news (
id INT , content VARCHAR(32)); 
-- 添加一条测试数据
INSERT INTO news VALUES(100, '北京新闻');
SELECT * FROM news; 
-- 给 shunping 分配查看 news 表和 添加 news 的权限
GRANT SELECT , INSERT
ON testdb.news
TO 'shunping'@'localhost' 
-- 可以增加 update 权限
GRANT UPDATE
ON testdb.news
TO 'fzy'@'localhost';

-- 修改 fzy 的密码为 abc
SET PASSWORD FOR fzy@localhost = 'abc';
-- 回收 fzy 用户在 testdb.news 表的所有权限
REVOKE SELECT , UPDATE, INSERT ON testdb.news FROM 'fzy'@'localhost' ;
REVOKE ALL ON testdb.news FROM 'fzy'@'localhost' ;
-- 删除 shunping
DROP USER 'fzy'@'localhost';
```

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208112020822.png)

# JDBC

1.注册驱动–加载Driver类

2.获取连接–得到Connection

3.执行增删改查-发送SQL给mysql执行

4.释放资源–关闭相关连接

```java
import com.mysql.cj.jdbc.Driver;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class jdbc01 {
    public static void main(String[] args) throws SQLException {
        //前置工作： 在项目下创建一个文件夹比如 libs
        // 将 mysql.jar 拷贝到该目录下，点击 add to project ..加入到项目

        //1.注册驱动
        Driver driver = new Driver();

        //2.得到连接
        //(1) jdbc:mysql:// 规定好表示协议，通过 jdbc 的方式连接 mysql
        //(2) localhost 主机，可以是 ip 地址
        //(3) 3306 表示 mysql 监听的端口
        //(4) hsp_db02 连接到 mysql dbms 的哪个数据库
        //(5) mysql 的连接本质就是 socket 连接
        String url = "jdbc:mysql://localhost:3306/fzy_db02";
        //将 用户名和密码放入到 Properties 对象
        Properties properties = new Properties();
        properties.setProperty("user", "root");
        properties.setProperty("password", "mysql@lmh");

        Connection connect = driver.connect(url, properties);

        //3.执行sql
        //String sql ="insert into actor values (null,'刘德华','男','1970-11-11','110')";
        //String sql = "update actor set name='周星驰' where id = 1";
        String sql = "delete from actor where id = 1";
        //statement 用于执行静态 SQL 语句并返回其生成的结果的对象
        Statement statement = connect.createStatement();
        int rows = statement.executeUpdate(sql); // 如果是 dml 语句，返回的就是影响行数
        System.out.println(rows > 0 ? "成功" : "失败");

        //4.关闭连接
        statement.close();
        connect.close();
    }
}
```

## 获取数据库连接

**方式1**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141640386.png)

**方式2**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141640713.png)

**方式3**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141642801.png)

**方式4**（推荐使用）

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141644806.png)

**方式5**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141648260.png)

```java
package jdbc;

import com.mysql.cj.jdbc.Driver;
import org.junit.jupiter.api.Test;

import java.io.FileInputStream;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Properties;

public class JdbcConn {

    //方式 1
    @Test
    public void connect01() throws SQLException {
        Driver driver = new Driver(); //创建 driver 对象
        String url = "jdbc:mysql://localhost:3306/hsp_db02";
        //将 用户名和密码放入到 Properties 对象
        Properties properties = new Properties();
        //说明 user 和 password 是规定好，后面的值根据实际情况写
        properties.setProperty("user", "root");// 用户
        properties.setProperty("password", "hsp"); //密码
        Connection connect = driver.connect(url, properties);
        System.out.println(connect);
    }

    //方式 2
    @Test
    public void connect02() throws ClassNotFoundException, IllegalAccessException, InstantiationException, SQLException {
        //使用反射加载 Driver 类 , 动态加载，更加的灵活，减少依赖性
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");

        Driver driver = (Driver) aClass.newInstance();
        String url = "jdbc:mysql://localhost:3306/hsp_db02";
        //将 用户名和密码放入到 Properties 对象
        Properties properties = new Properties();
        //说明 user 和 password 是规定好，后面的值根据实际情况写
        properties.setProperty("user", "root");// 用户
        properties.setProperty("password", "hsp"); //密码
        Connection connect = driver.connect(url, properties);
        System.out.println("方式 2=" + connect);
    }

    //方式 3 使用 DriverManager 替代 driver 进行统一管理
    @Test
    public void connect03() throws IllegalAccessException, InstantiationException, ClassNotFoundException, SQLException {
        //使用反射加载 Driver
        Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
        Driver driver = (Driver) aClass.newInstance();
        //创建 url 和 user 和 password
        String url = "jdbc:mysql://localhost:3306/hsp_db02";
        String user = "root";
        String password = "hsp";
        DriverManager.registerDriver(driver);//注册 Driver 驱动
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("第三种方式=" + connection);
    }

    //方式 4: 使用 Class.forName 自动完成注册驱动，简化代码
    //这种方式获取连接是使用的最多，推荐使用
    @Test
    public void connect04() throws ClassNotFoundException, SQLException {
        //使用反射加载了 Driver 类
        //在加载 Driver 类时，完成注册
        /*
        源码: 1. 静态代码块，在类加载时，会执行一次. 2. DriverManager.registerDriver(new Driver());
        3. 因此注册 driver 的工作已经完成
        static {
            try {
                DriverManager.registerDriver(new Driver());
            } catch (SQLException var1) {
                throw new RuntimeException("Can't register driver!");
            }
        }
        */

        Class.forName("com.mysql.jdbc.Driver");
        //创建 url 和 user 和 password
        String url = "jdbc:mysql://localhost:3306/hsp_db02";
        String user = "root";
        String password = "hsp";
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("第 4 种方式~ " + connection);
    }

    //方式 5 , 在方式 4 的基础上改进，增加配置文件，让连接 mysql 更加灵活
    @Test
    public void connect05() throws IOException, ClassNotFoundException, SQLException {
        //通过 Properties 对象获取配置文件的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\mysql.properties"));
        //获取相关的值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        Class.forName(driver);//建议写上
        Connection connection = DriverManager.getConnection(url, user, password);
        System.out.println("方式 5 " + connection);
    }
}
```

## Statement

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141720915.png)

## PreparedStatement

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141731535.png)

**好处**

1.不再使用+拼接sql语句，减少语法错误

2.有效的解决了sql注入问题!

3.大大减少了编译次数,效率较高

```java
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;
import java.util.Scanner;

public class PreparedStatement_ {
    public static void main(String[] args) throws Exception {

        Scanner scanner = new Scanner(System.in);
        //让用户输入管理员名和密码
        System.out.print("请输入管理员的名字: "); //next(): 当接收到 空格或者 '就是表示结束
        String admin_name = scanner.nextLine(); // 老师说明，如果希望看到 SQL 注入，这里需要用 nextLine
        System.out.print("请输入管理员的密码: ");
        String admin_pwd = scanner.nextLine();
        //通过 Properties 对象获取配置文件的信息
        Properties properties = new Properties();

        properties.load(new FileInputStream("jdbc\\mysql.properties"));
        //获取相关的值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        //1. 注册驱动
        Class.forName(driver);//建议写上
        //2. 得到连接
        Connection connection = DriverManager.getConnection(url, user, password);

        //3. 得到 PreparedStatement
        //3.1 组织 SqL , Sql 语句的 ? 就相当于占位符
        String sql = "select name , pwd from admin where name =? and pwd = ?";
        //3.2 preparedStatement 对象实现了 PreparedStatement 接口的实现类的对象
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        //3.3 给 ? 赋值
        preparedStatement.setString(1, admin_name);
        preparedStatement.setString(2, admin_pwd);
        //4. 执行 select 语句使用 executeQuery
        // 如果执行的是 dml(update, insert ,delete) executeUpdate()
        // 这里执行 executeQuery ,不要在写 sql
        ResultSet resultSet = preparedStatement.executeQuery();
        if (resultSet.next()) { //如果查询到一条记录，则说明该管理存在
            System.out.println("恭喜， 登录成功");
        } else {
            System.out.println("对不起，登录失败");
        }
        //关闭连接
        resultSet.close();
        preparedStatement.close();
        connection.close();
    }
}
```

```java
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

public class PreparedStatement_ {
    public static void main(String[] args) throws Exception {

        //通过 Properties 对象获取配置文件的信息
        Properties properties = new Properties();

        properties.load(new FileInputStream("jdbc\\mysql.properties"));
        //获取相关的值
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        //1. 注册驱动
        Class.forName(driver);//建议写上
        //2. 得到连接
        Connection connection = DriverManager.getConnection(url, user, password);

//        //3. 得到 PreparedStatement
//        //3.1 组织 SqL , Sql 语句的 ? 就相当于占位符
//        String sql = "insert into admin values(?,?)";
//        //3.2 preparedStatement 对象实现了 PreparedStatement 接口的实现类的对象
//        PreparedStatement preparedStatement = connection.prepareStatement(sql);
//        //3.3 给 ? 赋值
//        Scanner scanner = new Scanner(System.in);
//        String admin_name="";
//        String admin_pwd="";
//        for(int i = 0; i <5;i++){
//            //让用户输入管理员名和密码
//            System.out.print("请输入管理员的名字: ");
//            admin_name = scanner.nextLine();
//            System.out.print("请输入管理员的密码: ");
//            admin_pwd = scanner.nextLine();
//            preparedStatement.setString(1, admin_name);
//            preparedStatement.setString(2, admin_pwd);
//            preparedStatement.executeUpdate();
//        }

//        String sql = "update admin set name='king' where name='abc'";
//        PreparedStatement preparedStatement = connection.prepareStatement(sql);
//        preparedStatement.executeUpdate();

//        String sql = "delete from admin where name='def'";
//        PreparedStatement preparedStatement = connection.prepareStatement(sql);
//        preparedStatement.executeUpdate();

        String sql = "select * from admin";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        ResultSet resultSet = preparedStatement.executeQuery();
        while (resultSet.next()) {
            System.out.println(resultSet.getString(1) + "  " + resultSet.getString(2));
        }
        //关闭连接
        preparedStatement.close();
        connection.close();
    }
}
```

## API小结

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141815529.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208141815099.png)

## 封装 JDBCUtils

在jdbc操作中，获取连接和释放资源是经常使用到，可以将其封装JDBC连接的工具类JDBCUtils

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;

public class JDBCUtils {
    private static String user;
    private static String password;
    private static String url;
    private static String driver;

    // 连接
    static {
        try {
            Properties properties = new Properties();
            properties.load(new FileInputStream("jdbc\\mysql.properties"));
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    // 关闭相关资源
    public static void close(ResultSet set, Statement statement, Connection connection) {
        try {
            if (set != null) {
                set.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

}
```

```java
//使用
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JDBCUtils_Use {
    public static void main(String[] args) {
    }

    @Test
    public void testSelect() {
        //1.得到连接
        Connection connection = JDBCUtils.getConnection();
        //2.组织一个sql
        String sql = "select * from actor where id = ? ";

        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        //3.创建PreparedStatement  对象
        try {
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1, 1);
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                System.out.println(resultSet.getString(2));
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            JDBCUtils.close(resultSet, preparedStatement, connection);
        }
    }

    @Test
    public void testDML() {//insert , update , delete
        //1.得到连接
        Connection connection = JDBCUtils.getConnection();
        PreparedStatement preparedStatement = null;
        //2.组织一个sql
        String sql = "update actor set name = ? where id = ? ";
        //3.创建PreparedStatement  对象
        try {
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1, "王菲");
            preparedStatement.setInt(2, 1);
            preparedStatement.executeUpdate();

        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            JDBCUtils.close(null, preparedStatement, connection);
        }

    }
}
```

## 事务

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208162001330.png)

```java
import jdbc.utils.JDBCUtills;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Transaction_ {
    public static void main(String[] args) {

    }

    @Test
    public void noTransaction() {
        //1.得到连接
        Connection connection = null;//在默认情况下，connection 是默认自动提交
        PreparedStatement preparedStatement = null;
        //2.组织一个sql
        String sql = "update account set balance = balance -100 where id = 1";
        String sql2 = "update account set balance = balance -100 where id = 1";
        //3.创建PreparedStatement  对象
        try {
            connection = JDBCUtills.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.executeUpdate();

            int i = 1 / 0;
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();

        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            JDBCUtills.close(null, preparedStatement, connection);
        }
    }

    @Test
    public void useTransaction() throws SQLException {
        //1.得到连接
        Connection connection = null;

        PreparedStatement preparedStatement = null;
        //2.组织一个sql
        String sql = "update account set balance = balance -100 where id = 1";
        String sql2 = "update account set balance = balance +100 where id = 2";
        //3.创建PreparedStatement  对象
        try {
            connection = JDBCUtills.getConnection();
            connection.setAutoCommit(false);//将 connection 设置为不自动提交
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.executeUpdate();

            //int i = 1 / 0;
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();

            connection.commit();
            System.out.println("提交");

        } catch (Exception e) {
            System.out.println("发生错误，回滚");
            connection.rollback();
            throw new RuntimeException(e);
        } finally {
            //关闭资源
            JDBCUtills.close(null, preparedStatement, connection);
        }
    }
}
```

## 批处理

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208162020850.png)

```java
import jdbc.utils.JDBCUtils;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class Batch_ {
    public static void main(String[] args) {

    }

    //使用批量方式添加数据
    @Test
    public void batch() throws Exception {
        Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin2 values(null, ?, ?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        System.out.println("开始执行");
        long start = System.currentTimeMillis();//开始时间
        for (int i = 0; i < 5000; i++) {//5000 执行
            preparedStatement.setString(1, "jack" + i);
            preparedStatement.setString(2, "666");
            //将 sql 语句加入到批处理包中 -> 看源码
            /*
            //1. //第一就创建 ArrayList - elementData => Object[]
            //2. elementData => Object[] 就会存放我们预处理的 sql 语句
            //3. 当 elementData 满后,就按照 1.5 扩容
            //4. 当添加到指定的值后，就 executeBatch
            //5. 批量处理会减少我们发送 sql 语句的网络开销，而且减少编译次数，因此效率提高
            public void addBatch() throws SQLException {
                synchronized(this.checkClosed().getConnectionMutex()) {
                    if (this.batchedArgs == null) {
                    this.batchedArgs = new ArrayList();
                    }
                    for(int i = 0; i < this.parameterValues.length; ++i) {
                        this.checkAllParametersSet(this.parameterValues[i], this.parameterStreams[i], i);
                    }
                    this.batchedArgs.add(new PreparedStatement.BatchParams(this.parameterValues, this.parameterStreams, this.isStream, this.streamLengths, this.isNull));
                }
            }
            */
            preparedStatement.addBatch();
            //当有 1000 条记录时，在批量执行
            if ((i + 1) % 1000 == 0) {//满 1000 条 sql
                preparedStatement.executeBatch();
                //清空一把
                preparedStatement.clearBatch();
            }
        }
        long end = System.currentTimeMillis();
        System.out.println("批量方式 耗时=" + (end - start));//批量方式 耗时=843
        //关闭连接
        JDBCUtils.close(null, preparedStatement, connection);
    }
}
```



# 数据库连接池

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208162045574.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208162051368.png)

**数据库连接池种类**

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208162046801.png)

## C3P0

```java
//方式 1： 相关参数，在程序中指定 user, url , password 等
@Test
public void testC3P0_01() throws Exception {
    //1. 创建一个数据源对象
    ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
    //2. 通过配置文件 mysql.properties 获取相关连接的信息
    Properties properties = new Properties();
    properties.load(new FileInputStream("jdbc\\mysql.properties"));
    //读取相关的属性值
    String user = properties.getProperty("user");
    String password = properties.getProperty("password");
    String url = properties.getProperty("url");
    String driver = properties.getProperty("driver");
    //给数据源 comboPooledDataSource 设置相关的参数
    //注意：连接管理是由 comboPooledDataSource 来管理
    comboPooledDataSource.setDriverClass(driver);
    comboPooledDataSource.setJdbcUrl(url);
    comboPooledDataSource.setUser(user);
    comboPooledDataSource.setPassword(password);
    //设置初始化连接数
    comboPooledDataSource.setInitialPoolSize(10);
    //最大连接数
    comboPooledDataSource.setMaxPoolSize(50);
    //测试连接池的效率, 测试对 mysql 5000 次操作
    long start = System.currentTimeMillis();
    for (int i = 0; i < 5000; i++) {
        Connection connection = comboPooledDataSource.getConnection(); //这个方法就是从 DataSource 接口实现的
        //System.out.println("连接 OK");
        connection.close();
    }
    long end = System.currentTimeMillis();
    //c3p0 5000 连接 mysql 耗时=673
    System.out.println("c3p0 5000 连接 mysql 耗时=" + (end - start));
}

//第二种方式 使用配置文件模板来完成
//1. 将 c3p0 提供的 c3p0.config.xml 拷贝到 src 目录下
//2. 该文件指定了连接数据库和连接池的相关参数
@Test
public void testC3P0_02() throws SQLException {
    ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("my-config");
    //测试 5000 次连接 mysql
    long start = System.currentTimeMillis();
    System.out.println("开始执行....");
    for (int i = 0; i < 500000; i++) {
        Connection connection = comboPooledDataSource.getConnection();
        System.out.println("连接 OK~");
        connection.close();
    }
    long end = System.currentTimeMillis();
    //c3p0 的第二种方式 耗时=413
    System.out.println("c3p0 的第二种方式(500000) 耗时=" + (end - start));//1917
}
```

## Druid

```java
@Test
public void testDruid() throws Exception {
    //1. 加入 Druid jar 包
    //2. 加入 配置文件 druid.properties , 将该文件拷贝项目的 src 目录
    //3. 创建 Properties 对象, 读取配置文件
    Properties properties = new Properties();
    properties.load(new FileInputStream("src\\druid.properties"));
    //4. 创建一个指定参数的数据库连接池, Druid 连接池
    DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
    long start = System.currentTimeMillis();
    for (int i = 0; i < 5000; i++) {
        Connection connection = dataSource.getConnection();
        //System.out.println(connection.getClass());
        //System.out.println("连接成功!");
        connection.close();
    }
    long end = System.currentTimeMillis();
    System.out.println("druid 连接池 操作 500000 耗时=" + (end - start));//611
}
```

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JDBCUtilsByDruid {
    private static DataSource ds;

    static {
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src\\druid.properties"));
            ds = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    public static void close(ResultSet rs, Statement st, Connection c) {
        try {
            if (rs != null) {
                rs.close();
            }
            if (st != null) {
                st.close();
            }
            if (c != null) {
                c.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

## Apache—DBUtils

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208171031075.png)

```java
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;
import org.junit.jupiter.api.Test;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

public class DBUtils_USE {
    //使用 apache-DBUtils 工具类 + druid 完成对表的 crud 操作
    @Test
    public void testQueryMany() throws SQLException { //返回结果是多行的情况
        //1. 得到 连接 (druid)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用 DBUtils 类和接口 , 先引入 DBUtils 相关的 jar , 加入到本 Project
        //3. 创建 QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 就可以执行相关的方法，返回 ArrayList 结果集
        //String sql = "select * from actor where id >= ?";
        // 注意: sql 语句也可以查询部分列
        String sql = "select id, name from actor where id >= ?";

        //(1) query 方法就是执行 sql 语句，得到 resultset ---封装到 --> ArrayList 集合中
        //(2) 返回集合
        //(3) connection: 连接
        //(4) sql : 执行的 sql 语句
        //(5) new BeanListHandler<>(Actor.class): 在将 resultset -> Actor 对象 -> 封装到 ArrayList
        // 底层使用反射机制 去获取 Actor 类的属性，然后进行封装
        //(6) 1 就是给 sql 语句中的? 赋值，可以有多个值，因为是可变参数 Object... params
        //(7) 底层得到的 resultset ,会在 query 关闭, 关闭 PreparedStatment

        List<Actor> list =
                queryRunner.query(connection, sql, new BeanListHandler<>(Actor.class), 1);
        System.out.println("输出集合的信息");
        for (Actor actor : list) {
            System.out.println(actor);
        }
        //释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }

    //演示 apache-dbutils + druid 完成 返回的结果是单行记录(单个对象)
    @Test
    public void testQuerySingle() throws SQLException {
        //1. 得到 连接 (druid)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用 DBUtils 类和接口 , 先引入 DBUtils 相关的 jar , 加入到本 Project
        //3. 创建 QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 就可以执行相关的方法，返回单个对象
        String sql = "select * from actor where id = ?";
        // 因为我们返回的单行记录<--->单个对象 , 使用的 Hander 是 BeanHandler
        Actor actor = queryRunner.query(connection, sql, new BeanHandler<>(Actor.class), 2);
        System.out.println(actor);
        // 释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }

    //演示 apache-dbutils + druid 完成查询结果是单行单列-返回的就是 object
    @Test
    public void testScalar() throws SQLException {
        //1. 得到 连接 (druid)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用 DBUtils 类和接口 , 先引入 DBUtils 相关的 jar , 加入到本 Project
        //3. 创建 QueryRunner
        QueryRunner queryRunner = new QueryRunner();

        //4. 就可以执行相关的方法，返回单行单列 , 返回的就是 Object
        String sql = "select name from actor where id = ?";
        //因为返回的是一个对象, 使用的 handler 就是 ScalarHandler
        Object obj = queryRunner.query(connection, sql, new ScalarHandler(), 1);
        System.out.println(obj);
        // 释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }

    //演示 apache-dbutils + druid 完成 dml (update, insert ,delete)
    @Test
    public void testDML() throws SQLException {
        //1. 得到 连接 (druid)
        Connection connection = JDBCUtilsByDruid.getConnection();
        //2. 使用 DBUtils 类和接口 , 先引入 DBUtils 相关的 jar , 加入到本 Project
        //3. 创建 QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        //4. 这里组织 sql 完成 update, insert delete
        //String sql = "update actor set name = ? where id = ?";
        //String sql = "insert into actor values(null, ?, ?, ?, ?)"`
        String sql = "delete from actor where id = ?";

        //(1) 执行 dml 操作是 queryRunner.update()
        //(2) 返回的值是受影响的行数 (affected: 受影响)
        //int affectedRow = queryRunner.update(connection, sql, "林青霞", "女", "1966-10-10", "116");
        int affectedRow = queryRunner.update(connection, sql, 1000);
        System.out.println(affectedRow > 0 ? "执行成功" : "执行没有影响到表");
        // 释放资源
        JDBCUtilsByDruid.close(null, null, connection);
    }
}
```

## BasicDao

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208171122929.png)

![](https://lmh-hexo-blog-img.oss-cn-hangzhou.aliyuncs.com/img/202208171131621.png)
