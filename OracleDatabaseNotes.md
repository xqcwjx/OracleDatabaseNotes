# OracleDatabaseNotes

## oracle登录

	服务器本地登入:sqlplus scott/123456
	客户端登入:sqlplus scott/123456@//192.168.12.12/orcl
	登入管理员:只能在服务器本地登入：sqlplus sys/sys as sysdba
	other登录：sqlplus /nolog  ---  connect / as sysdba

## 查看表的结构和查询表的列

	查看dept表的结构:desc dept; || deptno 部门编号 | dname 部门名称 | loc 地点
	查询dept表中所有的列:select * from dept;
	

	查看emp表的结构：desc emp; || empno 员工编号 | ename 员工姓名 | job 职务 | mgr 上级(老			板) | hiredate 入职如期 | sal 薪水 | comm 年终奖 | deptno 部门编号
	查看emp表所有的列：select * from emp;
	结果 结果 结果   查看#表的结构：desc #;  ||  查询#表的列:select * from #;

## 查询在终端显示不规范:可以设置显示格式

	set linesize 120;//设置一行的大小 || set pagesize 140;//设置一页的大小
	最好修改配置文档,永久生效:
	E:\app\Administrator\product\11.2.0\client_1\sqlplus\admin\glogin.sql



------------------------------------------------------------------------------------------------------------


## 查询语句

	查询员工表信息：select   * from emp;
	+------------------------------------------------------------------------------------------------------------------------------+
	 |  查询员工号，姓名，月薪，奖金，年薪，年收入       -nvl - null						     |
	 |  nvl(a,b);//如果a不为空 返回值等于a a为空返回值是b							             |
	 |  select  empno as "工号",ename  "姓名",sal 薪水,comm "奖 金",sal*12 年薪, sal*12+nvl(comm,0) "年收入" from emp;												     |
	 |  select empno"工号",ename"姓名",sal"薪水",comm"奖金",sal*12"年薪",sal*12+nvl(comm,0)"年收入" from emp;																	     |
	+-------------------------------------------------------------------------------------------------------------------------------+
	清屏：host cls;
	+------------------------------------------------------------------------+
	|  输出计算表达式 3+20*5，显示当前日期（sysdate 伪列）-dual	|
 	|  dual是一个伪表：select 3+20*5 from dual;			|
	+------------------------------------------------------------------------+
	edit命令 | 编辑sql语句: | 编辑完保存挂关闭 | /执行
	sqlplus 与sql命令的区别，哪个是sqlplus的命令，哪个是sql的命令
	sql命令:需要服务器解析命令 select insert…..
	sqlplus : 本地解析的命令  host  cls  set linesize   …..  edit
		结果 结果 结果   nvl(a,b);//如果a不为空 返回值等于a a为空返回值是b  ||   dual是一个伪表:比如.select 3+20*5 from dual;

## 过滤语句

	自定义查询where (自定义)：select * from emp where (自定义......)
	$nls_parameters//查看参数设置
	更改日期格式: alter session set NLS_DATE_FORMAT="yyyy-mm-dd";
	--查询10号部门或者20部门的员工信息（or或）： select * from emp where  deptno=10 or deptno=20;
	--查询10号部门员工工资为1300的员工信息（and和）: select * from emp where  deptno=10 and sal=1300;
	--查询奖金为空的员工信息-null null不能用等号或者不等号作为查询条件，条件永远为假,not is：select *  from emp where comm  is  null;
	--多个条件时怎么写更优？sql的where字句执行的顺序从右到左先后执行
	        ○ and 的条件的时候，将易假的的条件放在右侧
	        ○ or的条件的时候，将易真的条件放在右侧

## 排序语句（可以自定义）

	排序可以 列名，表达式，别名，序号，语法是 order by col|alias|number 默认是asc模式，升序，desc降序
	sal升序：select * from emp order by sal;
	sal降序：select * from emp order by sal desc;
  	 --员工信息按奖金逆序 nulls last --null向后排： select * from emp order by  comm desc nulls last ;、

## 单行函数(了解)	

	字符函数:对一行进行变换,有返回值  | 
	字符函数 | --lower 转小写，upper 转大写，initcap 首字母大写：select lower('Hello wOrld') 一,upper('Hello wOrld') 二,initcap('Hello wOrld') 
		SQL> select 'hello'||'world'||'aaa'||'bbbb' from dual;//只支持2个参数
		SQL> select lpad('hello',12,'#') ,rpad('world',12,'*') from dual;//
		SQL> select replace('helloworllold','llo','kk') from dual;//去掉llo
	数值函数 正、负表示小数点之后，或小数点以前的位数 | --round 四舍五入 trunc mod：SQL> select mod(1000,600),mod(600,1000) from dual;
	ceil向上取整，floor向下取整：SQL> select ceil(121/60),floor(121/60) from dual;
	数值,日期,字符类型的转换函数(了解)：to_char  |  to_date  |  to_number
	分组函数
	11 分组函数 
	对多行变换,得到一个结果(select 自定义(sal)from emp;)
	avg   求平均值 | sum  求和 | count  计数(只要这一行不为null,就加1) | max  求最大值 | min 求最小值
	写的时候最好将过滤条件放group by前面 ,
	注意: where  不能放group by后面
	如果过滤条件出现了组函数,必须使用having 过滤
	

## day

	+-------------------------------------------------------------------------------------------------------------------------------+
	|--查询员工信息：xxx的老板是 yyy  -- 自连接'WRAD'  ||  '的老板是' ||  'BALNK'			|
	|select e.ename||'的老板是'||nvl(f.ename,'自己') from emp e,emp f  where e.mgr = f.empno(+);																			|
	+--------------------------------------------------------------------------------------------------------------------------------+
	显示设置:break on deptno skip 

	2;//将相同的列deptno不显示,skip 空2行
	关闭显示设置:break on??null;
	闪回:flashback table??t2 to before drop

## 数据语言:

###                          创建\    插入、删除、更新表数据

	DML: 数据操纵语言  select insert delete update
	DDL: 数据库定义语言  truncate
	create table(表)、create view(视图)、create index(索引)、create sequence(序列)、
	create synonym(同义词)、alter table、drop table。
	DCL: 数据控制语言   commit(提交)、rollback(回滚)

创建：create	
create table ma(deptno number(12),dname varchar2(21));

插入数据:insert
	insert into dept(deptno,dname) values(&d1,'&d2');
	or
	insert into emp(empno,sal) values(&d1,&d2);
更新数据:update
	//将emp表中的sal值设置为12，条件是emp12
	update emp set sal=12 where empno=12;
删除:delete
	//删除emp中empno12；所有的行
	delete from emp where empno=12;


//摧毁emp10表 ,在重建表,但是表中的内容已经没有了:truncate table emp10;

语句执行时间记录开关：set timing on/off
回显开关：set feedback on/off

删除表的部分行建议使用delete
删除表中的所有行,建议使用truncate

oracle数据库事物的开启:执行第一个DML语句就开启事物:
事物提交时机:
主动提交: commit;
隐式提交: quit  DDL	
	
创建并

//子查询的结果作为一张表tb初始化
create table tb as 
(select e.empno, e.ename, e.sal, e.sal*12 年薪, d.dname
  from emp e, dept d
 where e.deptno = d.deptno)




	