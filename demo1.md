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
关键字顺序不能变：  
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








