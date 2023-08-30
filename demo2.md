# 连接查询
## 什么是链接查询？
跨表查询，多张表连接起来查询数据
## 连接查询的分类
> 内连接
> > 等值连接
> > 非等值链接
> > 自链接
> 外连接
> > 外左连接（左连接）
> > 外右连接（右连接）
> 全连接
## 连接查询不进行任何条件限制会发生什么现象？
>当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是
>两张表条数的乘积，这种现象被称为：笛卡尔积现象。（笛卡尔发现的，这是
>一个数学现象。）
## 内连接之等值连接
	A join B on 条件
## 内连接之非等值连接
	A join B 条件（不是一个等量关系。例如：e.sal between s.losal and s.hisal）
## 内连接之自连接
	A join A 条件（不要忘记起别名）
## 右外连接（右连接）
	select 
		e.ename,d.dname
	from
		emp e 
	right outer join 
		dept d
	on
		e.deptno = d.deptno;
	outer是可以省略的，带着可读性强。
	right代表什么：表示将join关键字右边的这张表看成主表，主要是为了将
	这张表的数据全部查询出来，捎带着关联查询左边的表。
	在外连接当中，两张表连接，产生了主次关系。
## 左外连接（左连接）
	select 
		e.ename,d.dname
	from
		dept d 
	left outer join 
		emp e
	on
		e.deptno = d.deptno;
## 多表连接
	语法：
		select 
			...
		from
			a
		join
			b
		on
			a和b的连接条件
		join
			c
		on
			a和c的连接条件
		right join
			d
		on
			a和d的连接条件
		
		一条SQL中内连接和外连接可以混合。都可以出现！
# 子查询
## 什么是子查询？
select语句中嵌套select语句，被嵌套的select语句被称为子查询
## 子查询出现在哪里？
	select
		..(select).
	from
		..(select).
	where
		..(select).
# union合并查询结果集
	select ename,job from emp where job = 'MANAGER'
	union
	select ename,job from emp where job = 'SALESMAN';
# limit 将查询结果取出一部分
	完整用法：limit startIndex, length
		startIndex是起始下标，length是长度。
		起始下标从0开始。

	缺省用法：limit 5; 这是取前5.
	**limit在order by之后执行！**
# 关于DQL（数据查询语句）的总结：
	select 
		...
	from
		...
	where
		...
	group by
		...
	having
		...
	order by
		...
	limit
		...
	
	执行顺序？
		1.from
		2.where
		3.group by
		4.having
		5.select
		6.order by
		7.limit..
	

