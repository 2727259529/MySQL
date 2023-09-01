# 约束
## 什么是约束？
	约束的英语单词: constraint
	在创建表的时候，我们可以给表中的字段加上一些约束，
	来保证这个表中数据的完整性，有效性！
	约束的作用就是为了保证表中数据的有效性
## 约束包括那些？
	非空约束： not null 
	唯一性约束：nuique
	主键约束：primary key（简称PK）
	外键约束：foreign key （简称FK）
	检查约束：check（mysql不支持，Oracle支持）
### 非空约束：not null
	约束字段不能为null
	drop table if exists t_vip;
	create table t_vip(
		id int,
		name varchar(255) not null  // not null只有列级约束，没有表级约束！
	);
### 唯一性约束：nuique
	唯一性约束unique约束的字段不能重复，但是可以为NULL。
	drop table if exists t_vip;
	create table t_vip(
		id int,
		name varchar(255) unique,
		email varchar(255)
	);
	drop table if exists t_vip;
			create table t_vip(
				id int,
				name varchar(255),
				email varchar(255),
				unique(name,email) // 约束没有添加在列的后面，这种约束被称为表级约束。
### 主键约束（primary key）
	主键约束的相关术语？
		主键约束：就是一种约束。
		主键字段：该字段上添加了主键约束，这样的字段叫做：主键字段
		主键值：主键字段中的每一个值都叫做：主键值。
	
	什么是主键？有啥用？
		主键值是每一行记录的唯一标识。
		主键值是每一行记录的身份证号！！！
	
	记住：任何一张表都应该有主键，没有主键，表无效！！

	主键的特征：not null + unique（主键值不能是NULL，同时也不能重复！）

	怎么给一张表添加主键约束呢？
		drop table if exists t_vip;
		// 1个字段做主键，叫做：单一主键
		create table t_vip(
			id int primary key,  //列级约束
			name varchar(255)
		);
|自然主键|业务主键|
|:----|:----|
|主键值是一个自然数，和业务没关系。|主键值和业务紧密关联，
例如id int primary key auto_increment, | 例如拿银行卡账号做主键值。这就是业务主键！|
| //auto_increment表示自增，从1开始，以1递增！||

### 外键约束（foregin key，简称FK）
	删除表的顺序？
				先删子，再删父。

			创建表的顺序？
				先创建父，再创建子。

			删除数据的顺序？
				先删子，再删父。

			插入数据的顺序？
				先插入父，再插入子。

		思考：子表中的外键引用的父表中的某个字段，被引用的这个字段必须是主键吗？
			不一定是主键，但至少具有unique约束。


