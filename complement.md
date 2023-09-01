select *from mysql.user;查询用户名
# 修改表结构
## 添加列
	ALTER TABLE tablename ADD column datatype [primary key/default/not null/...] AFTER 'columnX' 
	//在colunmX列后增加字段
	例：在student表中添加一个年级（grade）字段，类型为varchar，不能为null，在age字段后面
	ALTER TABLE student ADD grade varchar(2) not null AFTER age;
## 删除列
	ALTER TABLE tablename DROP column;
	将student表中的grade列删除
	ALTER TABLE student DROP grade;
## 修改列--重命名列 / 更改列的数据类型
	修改列--重命名列 / 更改列的数据类型：
	ALTER TABLE 表名 CHANGE 原列名 新列名 数据类型 [primary key/default/not null/...];（CHANGE 既能重命名列，也可以更改列的数据类型）
	
	ALTER TABLE 表名 MODIFY 列名 数据类型
	
	更改列名
	例：将gender更名为sex
	ALTER TABLE student CHANGE gender sex varchar(10);
	
	更改列数据类型
	例：将grade列数据类型更改为int型
	ALTER TABLE student CHANGE gender gender int(2);
	例：将grade列数据类型更改为varchar型
	ALTER TABLE student MODIFY grade varchar(2);
## 修改表名
	ALTER TABLE 表名 RENAME TO 新表名；
	例：将student表名更改为students
	ALTER TABLE student RENAME TO students;
## 添加约束
	mysql常用五类约束类型：
	not null：非空约束，指定某列不为空 
	unique： 唯一约束，指定某列和几列组合的数据不能重复 
	primary key：主键约束，指定某列的数据不能重复、唯一 
	foreign key：外键，指定该列记录属于主表中的一条记录，参照另一条数据 
	check：检查，指定一个表达式，用于检验指定数据 
	注意： MySQL不支持check约束，但可以使用check约束，而没有任何效果；
	
	ALTER TABLE 表名 ADD [CONSTRAINT 约束名] 约束类型(column1,[column2,column3,...]);
	例：给student表添加主键name;
	ALTER TABLE student ADD primary key(name);
## 删除约束
	ALTER TABLE 表名 DROP 约束;
	
	例：  删除student表的主键约束
	
	ALTER TABLE student ADD primary key；
	
	修改表后，表结构如下：
	
	1. 删除唯一性约束，语法必须如下：
	
	ALTER TABLE 表名 DROP  INDEX 约束名；
	若不加INDEX，无法删除唯一性约束。
	例：给students表的name列添加唯一性约束，然后删除。
	ALTER TABLE students ADD constraint unique_name UNIQUE(name);
	ALTER TABLE students DROP unique_name;（报错，无法删除唯一性约束）
	ALTER TABLE students DROP INDEX unique_name;（正确）
	2.删除主键约束：
	删除主键约束时，无法使用约束名来删除主键约束。
	
	例：对students表name列增加主键约束，然后删除
	ALTER TABLE students ADD CONSTRAINT primary_key_name PRIMARY KEY(name); 
	删除主键：
	ALTER TABLE students DROP primary_key_name;（错误，无法删除）
	ALTER TABLE students DROP primary key;（删除成功）
	