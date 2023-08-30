# MYSQl 笔记
## 什么是数据库？什么是数据库管理系统？什么是SQL？他们之间的关系是怎样的？
数据库（Database）：简称DB。存储文件的地方。
数据库管理系统（DBMS）：对数据库中的数据进行增删改查。  
常见的数据库管理系统：MYSQl，Oracle,MS SQL Sever,PostgreSQL等。  
SQL：结构化查询语言。

 ----

## 创建用户

### 仅本地访问的用户
  ```create user 'username'@'localhost' identified by 'password';```
### 可以远程访问的用户
  ```create user 'username'@'%' identified by 'password';```
### 删除用户
  ```drop user 'username'@'localhost/%';```
### 重置密码
  ```use mysql;```
  
	1. update user set authentication_string=pasword('new password') where user = 'user name';
	2. 使用 SET PASSWORD 修改密码命令格式为 set password for username @localhost = password(newpwd);
	3. 使用 mysqladmin 命令修改 MySQL 的 root 用户密码格式为 mysqladmin -u用户名 -p旧密码 password 新密码；

注意：修改密码的命令中 -uroot 和 -proot 是整体，不要写成 -u root -p root，-u 和 root 间可以加空格，但是会有警告出现，所以就不要加空格了；

### 数据库授权
```grant all privileges on database_name.* to user_name;```
### 撤销授权
```revoke all privileges on database_name.* from user_name;```
-----
## MYSQl的数据类型
- 整形类型：int，bigint
- 浮点类型：float，double
- 字符串类型：char，varchar，
- 日期类型：data，time，datatime，timestamp
- 其他类型
-----
## MYSQL常用命令
|A|B|
|:----|:----|
|退出mysql：			 |exit;|
| 查看mysql中有哪些数据库：|show databases;|
| 选择要使用的数据库：   |  use 库名;|
| 创建数据库：			| create database 库名;|
|查看数据库下有哪些表： |  show tables;|
|查询数据库的版本号：    | select version();|
|查询当前在哪个数据库：|   select database();|
| 终止mysql的命令：		 |\c|
| 查询表中数据： 			| select \* from 表名|
| 查看表结构：		  	 |desc 表名|
	
-----
## SQL语句分类
	DQL：数据查询语言（select）
	DML：数据操作语言（insert，delete，update）针对数据
	DDL：数据定义语言（create，drop，alter）针对表
	TCL：事务控制语言（commit，rollback）
	DCL：数据控制语言（grand，revoke）
-----
 ## 简单查询
 |查询要求 |  sql语句|
 |:----:|:----:|
|查询一个字段？|select 字段名 from 表名;[^1]|
|给查询的列起别名？|select 列名 as 别名 from 表名;[^2]|
|计算年薪？|sal * 12[^3]|

[^1]:查询两个字段，或者多个字段怎么办？使用逗号隔开“,”

[^2]:只是将显示的查询结果列名显示为deptname，原表列名还是叫：dname  
记住：select语句是永远都不会进行修改操作的。（因为只负责查询）  
as关键字可以省略.  
假设起别名的时候，别名里面有空格怎么办？加单引号

[^3]:结论：字段可以使用数学表达式！
-----
## 条件查询
### 什么是条件查询？
语法格式：  
		select  
			字段1,字段2,字段3....   
		from   
			表名  
		where   
			条件;   
### 都有哪些条件？
|符号|作用|
|:----|:----|
|= |等于[^4]|
|\<>或!=| 不等于|
|\< |小于|
|\<= |小于等于|
|> 大于|
|>= |大于等于|
|between … and …. |两个值之间, 等同于 >= and <=[^5]|
|null|空[^6]|
|is not null|不为空|
|and|并且[^7]|
|or|或者|
|in|包含[^8]|
|not in|不在这个范围中|
|like|模糊查询[^具体用法]|
[^4]: =一个字符串的时候要用单引号
[^5]: 使用between and的时候，必须遵循左小右大。between and是闭区间，包括两端的值。
[^6]: 在数据库当中null不能使用等号进行衡量。需要使用is null  
因为数据库中的null代表什么也没有，它不是一个值，所以不能使用  
等号衡量。
[^7]: and和or同时出现，and优先级较高。如果想让or先执行，需要加“小括号”  
以后在开发中，如果不确定优先级，就加小括号就行了。
[^8]:in相当于多个or   
select empno,ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';  
select empno,ename,job from emp where job in('MANAGER', 'SALESMAN');  
注意：in不是一个区间。in后面跟的是具体的值
[^具体用法]: %：匹配任意多个字符。下划线：任意一个字符。  
找出名字中含有O的？ 
select ename from emp where ename like '%O%';  
找出名字以T结尾的？  
select ename from emp where ename like '%T';  	
找出名字以K开始的？  
select ename from emp where ename like 'K%';  
找出第二个字每是A的？  
select ename from emp where ename like '_A%';  
找出第三个字母是R的？  
		select ename from emp where ename like '__R%';
-----
## 排序
order by （默认是升序asc）  
order by xxx desc（降序是desc）  
sal asc, ename asc;（sal在前，起主导，只有sal相等的时候，才会考虑启用ename排序。）   
```关键字顺序不能变：  
		select  
			...  
		from  
			...  
		where  
			...  
		order by  
			...  
		
		以上语句的执行顺序必须掌握：  
			第一步：from  
			第二步：where  
			第三步：select  
			第四步：order by（排序总是在最后执行！）
```
## 数据处理函数
### 单行处理函数
- lower 转换小写   select lower(ename) as ename from emp;   
- upper 转换大写   select upper(name) as name from t_student;
- substr 取子串    substr 取子串（substr( 被截取的字符串, 起始下标,截取的长度)）
```	
	select substr(ename, 1, 1) as ename from emp;
	注意：起始下标从1开始，没有0.
	找出员工名字第一个字母是A的员工信息？
	第一种方式：模糊查询
		select ename from emp where ename like 'A%';
	第二种方式：substr函数
		select 
			ename 
		from 
			emp 
		where 
		substr(ename,1,1) = 'A';
```
- cancat函数进行字符串的拼接   select concat(empno,ename) from emp;
- length 取长度   select length(ename) enamelength from emp;
- trim 去空格    select \* from emp where ename = trim('   KING');
- str_to_date 将字符串转换成日期
- date_format 格式化日期
- format 设置千分位
- **case..when..then..when..then..else..end**
- round 四舍五入  
	```select round(1236.567, 0) as result from emp;   保留整数位。  
	select round(1236.567, 1) as result from emp;      保留1个小数   
	select round(1236.567, 2) as result from emp;      保留2个小数  
	select round(1236.567, -1) as result from emp;     保留到十位。 ``` 
- rand() 生成随机数  select round(rand()\*100,0) from emp;  100以内的随机数
- ifnull 可以将null转换成一个具体值
```ifnull是空处理函数。专门处理空的。
在所有数据库当中，只要有NULL参与的数学运算，最终结果就是NULL。
NULL只要参与运算，最终结果一定是NULL。为了避免这个现象，需要使用ifnull函数。
ifnull函数用法：ifnull(数据, 被当做哪个值)```
### 分组函数（多行处理函数）
多行处理函数的特点：输入多行，最终输出一行。
|函数|作用|
|:----:|:----:|
|count|计数|
|sum|求和|
|avg|平均值|
|max|最大值|
|min|最小值|
**注意：**
	分组函数在使用的时候必须先进行分组，然后才能用。如果你没有对数据进行分组，整张表默认为一组。
	分组函数自动忽略NULL，你不需要提前对NULL进行处理。
	count(具体字段)：表示统计该字段下所有不为NULL的元素的总数。
	count(\*)：统计表当中的总行数。（只要有一行数据count则++）因为每一行记录不可能都为NULL，一行数据中有一列不为NULL，则这行数据就是有效的。
	分组函数不能够直接使用在where子句中。
	所有的分组函数可以组合起来一起用。
## 分组查询
### 什么是分组查询？
在实际的应用中，可能有这样的需求，需要先进行分组，然后对每一组的数据进行操作。
```select
			...
		from
			...
		where
			...
		group by
			...
		order by
			...
		
		以上关键字的顺序不能颠倒，需要记忆。
		执行顺序是什么？
			1. from
			2. where
			3. group by
			4. select
			5. order by
		select sum(sal) from emp; 
		这个没有分组，为啥sum()函数可以用呢？
		因为select在group by之后执行。
		
		
    使用having可以对分完组之后的数据进一步过滤。  
	having不能单独使用，having不能代替where，having必须和group by联合使用。
	
	执行顺序？
		1. from
		2. where
		3. group by
		4. having
		5. select
		6. order by
	
	从某张表中查询数据，
	先经过where条件筛选出有价值的数据。
	对这些有价值的数据进行分组。
	分组之后可以使用having继续筛选。
	select查询出来。
	最后排序输出！