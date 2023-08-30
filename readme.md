# MySQL的自述
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
