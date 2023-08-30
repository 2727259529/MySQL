# 表的创建
## 建表的语法格式：（建表属于DDL语句，DDL包括：create，drop，alter）
	create table 表名(字段名1 数据类型, 字段名2 数据类型, 字段名3 数据类型);
	create table 表名(
		字段名1 数据类型, 
		字段名2 数据类型, 
		字段名3 数据类型
	);
## 常用的数据类型
	varchar（最长255）
		可变长度的自符串
		比较智能，节省空间。
		会根据实际的数据长度动态分配空间。
	
		优点：节省空间。
		缺点：需要动态分布空间，速度慢。
	
	char（最长255）
		定长字符串
		不管实际的数据长度是多少
		分配固定长度的空间去存储数据
		使用不恰当的时候会导致空间的浪费
		
		优点：不需要动态分配空间，速度快
		缺点:使用不恰当的时候会导致空间的浪费
	
	int（最长11）
		数字中的整数型。等同于java的int
		
	bigint
		数字中的长整型。等同于java的long
	float
		单精度浮点型数据
	double
		双精度浮点型数据
	clob
		字符大对象
		最多可以存储4G的字符串。
		比如：存储一篇文章，存储一个说明。
		超过255个字符的都要采用CLOB字符大对象来存储。
		Character Large OBject:CLOB
	blob
		二进制大对象
		Binary Large OBject
		专门用来存储图片、声音、视频等流媒体数据。
		往BLOB类型的字段上插入数据的时候，例如插入一个图片、视频等，
		你需要使用IO流才行。
## 删除表（drop）
	drop table 表名；
	drop table t_student;
	当这张表不存在的时候会报错！
	如果这张表存在的话，删除
	drop table if exists t_student;
## 插入数据insert（DML）
	语法格式：
		insert into 表名(字段名1,字段名2,字段名3...) values(值1,值2,值3);

		注意：字段名和值要一一对应。什么是一一对应？
			数量要对应。数据类型要对应。
	insert into t_student(no,name,sex,age,email) values(1,'zhangsan','m',20,'zhangsan@123.com');
	insert into t_student(email,name,sex,age,no) values('lisi@123.com','lisi','f',20,2);
	
	
## insert插入日期
	数字格式化：format(数字, '格式')
	str_to_date：将字符串varchar类型转换成date类型
	date_format：将date类型转换成具有一定格式的varchar字符串类型。
	insert into t_user(id,name,birth) values(1, 'zhangsan', '01-10-1990'); // 1990年10月1日
		出问题了：原因是类型不匹配。数据库birth是date类型，这里给了一个字符串varchar。

		怎么办？可以使用str_to_date函数进行类型转换。
		str_to_date函数可以将字符串转换成日期类型date？
		语法格式：
			str_to_date('字符串日期', '日期格式')

		mysql的日期格式：
			%Y	年
			%m 月
			%d 日
			%h	时
			%i	分
			%s	秒
		
		insert into t_user(id,name,birth) values(1, 'zhangsan', str_to_date('01-10-1990','%d-%m-%Y'));

		str_to_date函数可以把字符串varchar转换成日期date类型数据，
		通常使用在插入insert方面，因为插入的时候需要一个日期类型的数据，
		需要通过该函数将字符串转换成date。
		
		
		date_format
		这个函数可以将日期类型转换成特定格式的字符串。
		select id,name,date_format(birth, '%m/%d/%Y') as birth from t_user;
		
		date_format函数怎么用？
			date_format(日期类型数据, '日期格式')
			这个函数通常使用在查询日期方面。设置展示的日期格式。
		
		 select id,name,birth from t_user;
		 
		 以上的SQL语句实际上是进行了默认的日期格式化，
		自动将数据库中的date类型转换成varchar类型。
		并且采用的格式是mysql默认的日期格式：'%Y-%m-%d'

		select id,name,date_format(birth,'%Y/%m/%d') as birth from t_user;
		
		java中的日期格式？
			yyyy-MM-dd HH:mm:ss SSS
			
## date和datetime两个类型的区别？
	date是短日期：只包括年月日信息。
	datetime是长日期：包括年月日时分秒信息。
	
	id是整数
	name是字符串
	birth是短日期
	create_time是这条记录的创建时间：长日期类型

	mysql短日期默认格式：%Y-%m-%d
	mysql长日期默认格式：%Y-%m-%d %h:%i:%s
	
	insert into t_user(id,name,birth,create_time) values(1,'zhangsan','1990-10-01','2020-03-18 15:49:50');
	
	在mysql当中怎么获取系统当前时间？
	now() 函数，并且获取的时间带有：时分秒信息！！！！是datetime类型的。
	
	insert into t_user(id,name,birth,create_time) values(2,'lisi','1991-10-01',now());
	
## 修改表update（DML）
	语法格式：
	update 表名 set 字段名1=值1,字段名2=值2,字段名3=值3... where 条件;
	注意：没有条件限制会导致所有数据全部更新。
	
## 删除数据 delete（DML）
	语法格式？
		delete from 表名 where 条件;

	注意：没有条件，整张表的数据会全部删除！
## 快删 truncate（DDL）
	删除dept_bak表中的数据
	delete from dept_bak; 
	这种删除数据的方式比较慢。

	 select * from dept_bak;
	

	delete语句删除数据的原理？（delete属于DML语句！！！）
		表中的数据被删除了，但是这个数据在硬盘上的真实存储空间不会被释放！！！
		这种删除缺点是：删除效率比较低。
		这种删除优点是：支持回滚，后悔了可以再恢复数据！！！
	
	truncate语句删除数据的原理？
		这种删除效率比较高，表被一次截断，物理删除。
		这种删除缺点：不支持回滚。
		这种删除优点：快速。

	用法：truncate table dept_bak;
	
	大表非常大，上亿条记录？？？？
		删除的时候，使用delete，也许需要执行1个小时才能删除完！效率较低。
		可以选择使用truncate删除表中的数据。只需要不到1秒钟的时间就删除结束。效率较高。
		但是使用truncate之前，必须仔细询问客户是否真的要删除，并警告删除之后不可恢复！

		truncate是删除表中的数据，表还在！
	
	删除表操作？
		drop table 表名; // 这不是删除表中的数据，这是把表删除。