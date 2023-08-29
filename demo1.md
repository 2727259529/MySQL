# MYSQl 笔记
## 什么是数据库？什么是数据库管理系统？什么是SQL？他们之间的关系是怎样的？
数据库（Database）：简称DB。存储文件的地方。
数据库管理系统（DBMS）：对数据库中的数据进行增删改查。  
常见的数据库管理系统：MYSQl，Oracle,MS SQL Sever,PostgreSQL等。  
SQL：结构化查询语言。

## 创建用户

### 仅本地访问的用户
  create user 'username'@'localhost' identified by 'password';
### 可以远程访问的用户
  create user 'username'@'%' identified by 'password';
### 删除用户
  drop user 'username'@'localhost/%';
### 重置密码
  use mysql;
	1.update user set authentication_string=pasword('new password') where user = 'user name';
	2.使用 SET PASSWORD 修改密码命令格式为 set password for username @localhost = password(newpwd);
	3.使用 mysqladmin 命令修改 MySQL 的 root 用户密码格式为 mysqladmin -u用户名 -p旧密码 password 新密码；

注意：修改密码的命令中 -uroot 和 -proot 是整体，不要写成 -u root -p root，-u 和 root 间可以加空格，但是会有警告出现，所以就不要加空格了；

