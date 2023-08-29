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
| 创建数据库：			 create database 库名;|
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
 