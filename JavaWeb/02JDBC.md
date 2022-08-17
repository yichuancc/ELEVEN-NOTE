# 

# MySQL

## 一、SQL结构化查询语言

概述：通过SQL语言可以操作所有的关系型数据库。每种数据库之间会存在差异，称为 “方言”。

> DDL（Data Definition Language）：数据定义语言，用来定义数据库对象：库、表、列等；
>DML（Data Manipulation Language）：数据操作语言，用来定义数据库记录(增、删、改); 
> DCL（Data Control Language）：数据控制语言，用来定义访问权限和安全级别；
> DQL（Data Query Language）：数据查询语言，用来查询记录（数据）。

## 二、DDL

```mysql
操作库：
建库： create database 库名;
	例： create database mydb;
	
查看所有库： show databases;
删除： drop database 库名；
	例：drop database mydb;
	
修改数据库编码：
alter database 库名 character set='gbk';
查看数据库： show create database 库名;
切换库： use 库名;

操作表：
查看该库下所有的表： show tables;
创建表： create table 表名(字段1 数据类型,字段2 数据类型...);
例： create table student(name varchar(10),age int);

删除表： drop table 表名;
修改表名: 
alter table 旧表名 rename to 新表名;
对表头的操作： alter
添加一个字段：
alter table 表名 add(列名 数据类型);
删除一个列：
alter table 表名 drop 列名;
修改列名：
alter table 表名 change 旧列名 新列名 新数据类型;
修改列的数据类型：
alter table 表名 modify 列名 新数据类型;
查看建表语句：
show create table 表名;
```

## 三、DML

```mysql
插入数据： insert into
insert into 表名(列名1,列名2...) values(值1,值2...);
给表中所有字段插入值：
insert into 表名 values(值1,值2,...值n);
注意：--字符串类型和日期类型的字段的值要用单引号引起来！
删除数据：
无条件删除表中所有数据：
delete from 表名;	--逐行删，效率低
truncate table 表名;	--先删除整个表，再创建一个空表

条件： where 
= >= <= > < 
and --且 
or  --或
(is not null)	--不为空 
(is null)	--为空 null不能使用=或！=判断
not	--非  
between 值1 and 值2 --在值1和值2之间

条件删除：
delete from 表名 where 条件;
例： delete from student where age>=20;	--删除年龄大于等于20的学生数据

修改表中数据： update
update 表名 set 字段名1=修改的值,字段名2=修改的值... where 条件;
例： update 表名 set age=20 where name='zhangsan'; --把 zhangsan 的年龄改为20
```

## 四、DQL

### 1、单表查询

```mysql
无条件查询：
--查询表中所有数据：
select * from 表名;
* --通配符，通配表中所有字段
--查询个别字段数据：
select 字段名1,字段名2... from 表名;

条件查询：
select 字段名1,字段名2... from 表名 where 条件;
in(值1,值2...)--等于值1或值2或...

字段运算：
例： select sal,sal*12 from emp; 	--月薪，月薪*12=年薪
--注意：null参与运算结果是null
ifnull(num,原始值) --如果字段值为null，则将其替换为 num ,否则不改变其值
distinct --对字段去重

别名：关键字 as
例： select sal as 月薪,sal*12 as 年薪 from emp; 

模糊查询： like
% --通配多个任意字符
_ --通配一个任意字符
例： '_a%' --匹配第二个字母为a的所有单词
order by  --排序，默认升序 asc:升序  desc:降序
例：
select * from emp order by sal asc;--对工资升序排列
select * from emp order by sal desc;--对工资降序排列

聚合函数：
select count(sal) from emp;--查询工资这一列有多少行，null不参与统计
select SUM(sal) from emp;--查询工资的总和
select MAX(sal) from emp;--查询工资最大值
select MIN(sal) from emp;--查询工资最小值
select AVG(sal) from emp;--查询工资平均值

分组： group by
where --在分组之前对数据筛选
having --对分组后的数据再次进行筛选
例：按照部门编号分组，并求出每个部门的平均工资
select deptno as 部门号, AVG(sal) as 部门平均工资 from emp group by deptno;
例：按照部门编号分组，查询各个部门工资高于1500的员工的工资总和大于6000的部门
select deptno as 部门号, SUM(sal) as 高于3000的工资总和 from emp where sal>3000 group by deptno having 高于3000的工资总和>6000;

分页查询： limit
select from 表名 limit 起始索引，本页条数;
起始索引=(页码-1)*本页条数
例：emp 一共10行数据，每页展示4行
SELECT * FROM emp LIMIT 0,4;  -- 第一页
SELECT * FROM emp LIMIT 4,4;  -- 第二页
SELECT * FROM emp LIMIT 8,4;  -- 第三页,本页只有2行
```

### 2、约束

```mysql
约束：对插入表中的数据做出一定的限定，为了保证数据的有效性和完整性
primary key --主键约束：非空且唯一，一张表只能有一个主键
方式一：建表时添加约束，字段名后加上约束即可
字段名 数据类型 primary key --添加主键约束
方式2：通过修改表添加约束：
alter table 表名 add primary key(字段名);
注意：表中如果已经有超出约束范围的数据，约束会添加失败
--联合主键：将多个字段看做一个整体设为主键
create table 表名(字段名1 数据类型,字段名2 数据类型...,primary key(字段名1,字段名2...));
alter table 表名 add primary key(字段名1,字段名2...);
删除主键约束：需要分部删除
alter table 表名 drop primary key;--删除唯一
alter table 表名 modify 字段名 数据类型 null;--删除非空

auto_increment --自增长约束：一般配合主键使用
例： id int primary key auto_increment
删除自增长约束：
alter table 表名 change 字段名 字段名 int;

unique	--唯一约束:数据不能重复，对null无效
not null  --非空约束
一个字段可以添加多个约束：
字段名 数据类型 not null unique  --非空且唯一
unsigned --非负约束
TINYINT --范围 -128~127
TINYINT UNSIGNED --范围 0~255

foreign key --外键约束
特点： 1、主表一方不能删除从表一方还在引用的数据；
	  2、从表一方不能添加主表一方没有描述的数据。
*添加外键约束：
在从表一方添加外键约束，去关联主表一方的主键
alter table 从表 add foreign key(从表字段) references 主表(主表字段);
*级联删除：
alter table 从表 add foreign key(从表字段) references 主表(主表字段) on delete cascade;
*级联更新：
alter table 从表 add foreign key(从表字段) references 主表(主表字段) on update cascade;  

*处理一对多:在从表中添加一个外键,名称一般为主表的名称_id,字段类型一般和主表的主键的类型保持一致,为了保证数据的有效性和完整性,在从表的外键上添加外键约束即可.

*处理多对多:引入一张中间表,存放两张表的主键,一般会将这两个字段设置为联合主键,这样就可以将多对多的关系拆分成两个一对多了，为了保证数据的有效性和完整性，需要在中间表上添加两个外键约束.	

*删除主表中的数据：
方式1: 添加级联删除
方式2: 先把带有外键的从表的数据删除,再删除主表中的数据
```

### 3、多表查询

```mysql
--多张表无条件查询，查询出的数据是笛卡尔积，没有任何意义
--笛卡尔积：多张表字段相加，行数相乘
select 表1.*,表2.* from 表1,表2;
--内连接：不符合条件的数据不做展示
格式1： 显式的内连接 inner可以省略不写
select a.*,b.* from a inner join b on 表a表b的连接条件;
格式2：隐式的内连接
select a.*,b.* from a,b where 表a表b的连接条件;

左外连接:
--先展示join左边的(a)表的所有数据,根据条件关联查询join右边的表(b),符合条件展示出来,不符合以null值展示.
select a.*,b.* from a left outer join b on 连接条件;  --outer 可以省略不写
右外连接:
--先展示join右边的(a)表的所有数据,根据条件关联查询join左边的表(b),符合条件展示出来,不符合以null值展示.
select a.*,b.* from b right outer join a on 连接条件; --outer 可以不写

子查询：一个查询的查询条件依赖于另一个查询结果
自连接查询：给一张表起两个别名,将他视为两张表,来进行查询
```

### 4、复制表

```mysql
-- 创建一张表，把另一张表中的字段和对应的数据全部复制过去
create table 表名 as select * from 另一张表 where true; --where true 可以省略不写
-- 创建一张表，只复制另一张表中的个别字段和其对应的数据
create table 表名 as select 字段1,字段2... from 另一张表;
-- 创建一张表,只把另一张表中的字段复制过去
create table 表名 as select * from 另一张表 where false; 
--另一张表也可以是临时表，或者子查询
```

## 五、DCL

```mysql
root:拥有所有权限
权限账户：只拥有部分权限的账户
password：md5加密函数（单向加密）
select password('root')   --查询用户密码
UPDATE USER SET PASSWORD=PASSWORD('123456') WHERE USER='root';--修改密码
SELECT * FROM USER;  --查询数据库用户
分配权限账户：
权限： select insert delete update drop create/  --或all
  	语法：
  GRANT 权限 ON 数据库名.某张表名 TO '用户名'@'localhost' IDENTIFIED BY '123456';
  --@ 后面可以是localhost，也可以是ip ，也可以给% ，%代表任意一台计算机都可以连接上来
  	注意分配多个权限用逗号隔开
  GRANT DELETE,SELECT,UPDATE ON day16.employee TO 'eric'@'localhost' IDENTIFIED BY '123456';
  	删除账户：
  	Delete FROM user Where User='eric' and Host='localhost';
```

## 六、存储过程 procedure

### 1、概述

存储过程是数据库中的一个对象，存储在服务端，用来封装多条SQL语句且带有逻辑性，可以实现一个功能，由于他在创建时，就已经对SQL进行了编译，所以执行效率高，而且可以重复调用，类似于Java中的方法。

### 2、语法

```mysql
DELIMITER $$
CREATE
    PROCEDUR `performance_schema`.`myTestPro`()
    BEGIN
	--SQL语句
    END$$
DELIMITER;
--注意：创建存储过程需要管理员分配权限
drop procedure 存储过程名;--删除存储过程
show procedure status\G;  -- 查看所有的存储过程状态 \G：格式化
show create procedure 存储过程名\G; -- 查看创建存储过程的语句
```

### 3、参数

```mysql
in:输入参数
out：输出参数
inout：输入输出参数
```

### 4、带有IF逻辑的存储过程 if then elseif else

```mysql
DELIMITER $$
CREATE PROCEDURE pro_testIf(IN num INT,OUT str VARCHAR(2))
BEGIN
	IF num>0 THEN
		SET str='正数';		-- 注意要用分号结束
	ELSEIF num<0 THEN        --注意elseif 连写
		SET str='负数';            
	ELSE
		SET str='零';
	END IF;         		 --注意要结束if，要写分号
	END $$
DELIMITER;
--调用存储过程，查看结果
call pro_testIf(100,@rr);
select @rr;
```

### 5、while循环 while do

```mysql
--求1~num的和
DELIMITER $$
CREATE PROCEDURE pro_testWhile(IN num INT,OUT result INT)
BEGIN
	-- 定义一个局部变量
	DECLARE i INT DEFAULT 1;
	DECLARE vsum INT DEFAULT 0;
	WHILE i<=num DO
	      SET vsum = vsum+i;
	      SET i=i+1;
	END WHILE;  	--结束循环
	SET result=vsum;
	END $$
DELIMITER;
--控制循环的关键字
leave 相当于java中的 break 
iterate 相当于java中的 continue
```

### 6、变量

```mysql
全局变量（内置变量）：可以在多个会话中访问
show variables;  -- 查看所有全局变量： 
select @@变量名  -- 查看某个全局变量： 
set @@变量名=新值   -- 修改全局变量： 
--设置SQL服务器输出数据编码
set @@character_set_results='gbk';
--设置SQL服务器接收数据编码
set @@character_set_client='utf8';
会话变量： 只存在于当前客户端与数据库服务器端的一次连接当中。如果连接断开，那么会话变量全部丢失！
 set @变量=值     -- 定义会话变量: 
 select @变量     -- 查看会话变量：    
局部变量：在存储过程或函数中定义的变量是局部变量。只要存储过程执行完毕，局部变量就丢失！
DECLARE i INT DEFAULT 1;  	--定义局部变量
set i=10;		--给变量设置值 
```

## 七、触发器 Trigger

### 1、概述

触发器：数据库中的一个对象，相当于JS中的监听器，触发器可以监听增、删、改三个动作。

### 2、语法

```mysql
DELIMITER $$

CREATE
    TRIGGER `mytestdb`.`myTriger` BEFORE/AFTER INSERT/UPDATE/DELETE
    ON `mytestdb`.`<Table Name>`
    FOR EACH ROW 
    BEGIN

    END$$
DELIMITER ;
BEFORE --行为发生之前就触发
AFTER --行为发生之后触发
FOR EACH ROW --行级触发，每操作一行就触发
old.字段 --可以获取到被监听的表中的字段的旧值
new.字段 --可以获取到被监听表中更新后的字段的新值
```

```mysql
例：修改表t1中的数据，另一张表t2中的数据跟着修改
DELIMITER $$
CREATE
    TRIGGER `mytestdb`.`MyTri7` AFTER UPDATE
    ON `mytestdb`.`t1`
    FOR EACH ROW 
    BEGIN
	UPDATE t2 SET id=new.id,username=new.username,age=new.age WHERE id=old.id;
    END$$

DELIMITER ;
```

## 八、视图

```mysql
视图：跟表一样，具有行和列的结构，可以简化查询,视图中的数据，来源于表
--创建视图
create view my_view as select * from emp;
--查询视图，和查表的语法一样
select * from my_view;
--单表视图：视图中的数据来源于一张表
--多表视图：视图中的数据来源于多张表的联合查询
注意：视图不能封装子查询的数据
--查看创建视图语句：
show create view my_view;
--删除视图
drop view my_view;
--视图本身不能修改，但可以修改视图的来源
alter view 视图名 as 新的select语句;
```

## 九、函数

```mysql
函数：包括内置函数和自定义函数
自定义函数语法：
DELIMITER $$
  CREATE
      FUNCTION `mytestdb`.`myFun`(num INT)
      RETURNS INT
      BEGIN
  	  DECLARE i INT DEFAULT 100;
  	  SET i=i+num;
      RETURN i;
      END$$
  
  DELIMITER ;
调用函数： select 函数名();

函数和存储过程的区别：
1.存储过程没有返回值，函数必须要有返回值。但是存储过程可以用out实现返回值这个功能
2.存储过程有in out inout这几个参数类型，函数没有
```

## 十、数据库表设计

**三大范式**

> 第一范式： 要求表的每个字段必须是不可分割的独立单元；
>
> 第二范式： 在第一范式的基础上，要求每张表只表达一个意思。表的每个字段都和表的主键有依赖。
>
> 第三范式： 在第二范式基础上，要求每张表的主键之外的其他字段都只能和主键有直接决定依赖关系。

# JDBC

## 一、连接数据库

```java
//JDBC:Java为连接数据库提供的一套接口（规范）
//1、导入数据库厂商提供的驱动
//	导入jar包，依赖jar包
//2、加载驱动
//驱动在5.0以上,此步骤可以省略不写

Class.forName("com.mysql.jdbc.Driver");

//3、建立连接
//URL:统一资源定位符
//格式："主协议:子协议://ip:端口/资源"
//本地连接：localhost:3306可以省略不写
//		即："jdbc:mysql:///mydb" 

String url="jdbc:mysql://localhost:3306/mydb";
String username="root";
String password="123456";
Connection conn=DriverManager.getConnection(url,username,password);
//4、获取操作对象
Statement st=conn.createStatement();
//5、编写SQL语句
String sql="select * from student";	//SQL语句
6、执行SQL语句
st.executeQuery(sql);//执行SQL语句
7、释放资源
conn.close();
st.close();
```

## 二、JDBC相关类及常用方法

```java
DriverManager //驱动管理类
	getConnection(); //试图建立到给定数据库 URL 的连接
Connection//接口：与特定数据库的连接（会话）
    createStatement(String sql);//创建一个 Statement 对象来将 SQL 语句发送到数据库
	prepareStatement(String sql);
	//创建一个 PreparedStatement 对象来将参数化的 SQL 语句发送到数据库
	prepareStatement(String sql, int autoGeneratedKeys)
    //创建一个默认 PreparedStatement 对象，该对象能获取自动生成的键。
Statement//接口:用于执行静态 SQL 语句并返回它所生成结果的对象
    executeUpdate();//执行DML语句，返回值是影响行数
	executeQuery();//执行DQL语句，返回值是查询的结果集
	execute();//执行任何语句
	addBatch(String sql)//将给定的 SQL 命令添加到此 Statement 对象的当前命令列表中
    executeBatch()//将一批命令提交给数据库来执行，如果全部命令执行成功，则返回更新计数组成的数组    
ResultSet//接口
/*表示数据库结果集的数据表，通常通过执行查询数据库的语句生成;
ResultSet 对象具有指向其当前数据行的光标。最初，光标被置于第一行之前。next 方法将光标移动到下一行;
因为该方法在 ResultSet 对象没有下一行时返回 false，所以可以在 while 循环中使用它来迭代结果集。
beforeFirst() //将光标移动到此 ResultSet 对象的开头，正好位于第一行之前。*/
        
//遍历取出结果集中对象
ResultSet re=st.executeQuery(sql);    
while(re.next()){
    int id=re.getInt("id");//参数为表头序号或者表头字段名
    String username=re.getString(2);
    /*处理这些零碎数据的方法：
		把查询出来的数据封装到类里
		再把对象存到集合里*/
}
```

### 演示一：模拟登录

```java
import java.sql.*;
import java.util.Scanner;

public class Example03 {
    public static void main(String[] args) throws Exception{
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password=sc.nextLine();
        Class.forName("com.mysql.jdbc.Driver");
        String url="jdbc:mysql:///mydb";
        Connection conn = DriverManager.getConnection(url, "root", "123456");
        String sql="select * from users where username=? and password=?";
        PreparedStatement ps = conn.prepareStatement(sql);
        ps.setString(1,username);
        ps.setString(2,password);
        ResultSet resultSet = ps.executeQuery();
        //如果查到结果，说明登录成功
        if(resultSet.next()){
            System.out.println("登录成功！");
        }else {
            System.out.println("登录失败！");
        }
        ps.close();
        resultSet.close();
        conn.close();
    }
}
```

### 演示二：批量操作

```java
//部分代码省略
String sql="insert into demo values(?)";
        PreparedStatement ps = connection.prepareStatement(sql);
        for (int i = 1; i < 10000; i++) {
            ps.setInt(1,i);
            ps.addBatch();
        }
        ps.executeBatch();
```

### 演示三：获取自增长键的值

```java
//部分代码省略
//要获取自增长键的值,需要在获取操作对象时声明一个参数 Statement.RETURN_GENERATED_KEYS
PreparedStatement preparedStatement = conn.prepareStatement(sql,Statement.RETURN_GENERATED_KEYS);
//获取自增长键的结果集
ResultSet generatedKeys = preparedStatement.getGeneratedKeys();
while (generatedKeys.next()){
    keyValue = generatedKeys.getInt(1);
}
```

## 三、安全问题

SQL注入：通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。

### 1、SQL注入案例：使用拼串的形式写SQL语句

```java
//部分代码省略
Statement st=conn.createStatement();
String name="1'or'1'='1";
"select * from student name='"+name+"'";
st.executeQuery(sql);//执行SQL语句,此语句可以查出student表中所有信息
```

### 2、防止SQL注入：使用PrepareStatement 预编译操作对象

```java
//部分代码省略
//SQL语句中的参数先用 ? 占位
String name="张三";
String sql="select * from student where name=?"
PrepareStatement ps=conn.prepareStatement(sql);
//给 ? 赋值
ps.setString(1,name);//参一为 ? 的索引，注意索引从1开始计算，参二是 ? 的值
ps.executeQuery();	//执行SQL语句，注意不再传入SQL语句
```

## 四、调用存储过程和函数

### 1、调用存储过程

```
SQL语句格式 {call <procedure-name>[(<arg1>,<arg2>, ...)]}
```

#### Java代码：部分省略

```java
	String sql="{call myPro(?,?)}";
    CallableStatement callableStatement = connection.prepareCall(sql);
    callableStatement.setInt(1,-1);//给第一个问号赋值
	//确定第二个问号的数据类型
    callableStatement.registerOutParameter(1, Types.VARCHAR);
    callableStatement.execute();//执行SQL语句
    String r = callableStatement.getString(2);//获取返回值
    System.out.println(r);
```

#### myPro 存储过程代码

```mysql
DELIMITER $$
CREATE PROCEDURE pro_testIf(IN num INT,OUT str VARCHAR(2))
BEGIN
	IF num>0 THEN
		SET str='正数';		-- 注意要用分号结束
	ELSEIF num<0 THEN        --注意elseif 连写
		SET str='负数';            
	ELSE
		SET str='零';
	END IF;         		 --注意要结束if，要写分号
	END $$
DELIMITER;

```

### 2、调用函数

```
SQL语句格式： {?= call <procedure-name>[(<arg1>,<arg2>, ...)]}
```

#### Java代码：部分省略

```java
	String sql="{?=call myFun(?)}";
    CallableStatement callableStatement = connection.prepareCall(sql);
    callableStatement.setInt(2,1);//给第二个 ? 赋值，删除 id=1 的人
    callableStatement.registerOutParameter(1,Types.INTEGER);//确定第一个 ? 数据的类型
    callableStatement.execute();//执行sql语句
    int r = callableStatement.getInt(1);//获取函数返回值
    System.out.println(r);

```

#### myFun函数代码

```mysql
create function myFun(num int)
  returns int
  BEGIN
    declare i int default 0;-- 定义i;相当于java中 //int i=0;
    delete from mydemo where id=num;-- 删除id=1的人
    select count(*) from mydemo into i;-- 统计剩余人数,赋给i
    return i;-- 返回i
    END;
```

**表格**

![2.1](images/2.1.png)

## 五、事务

事务是指一组最小逻辑操作单元，里面有多个操作组成。组成事务的每一部分必须要同时提交成功，如果有一个操作失败，整个操作就回滚。

### 1、事务四大特性(ACID)

> 原子性（Atomicity）：原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
>
> 一致性（Consistency）：事务必须使数据库从一个一致性状态变换到另外一个一致性状态。
>
> 隔离性（Isolation）：事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所干扰，多个并发事务之间相互隔离。
>
> 持久性（Durability）：持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来即使数据库发生故障也不应该对其有任何影响。

### 2、事务的提交

```java
默认情况下，Connection 对象处于自动提交模式，这意味着它在执行每个语句后都会自动提交更改,即它的所有 SQL 语句将被执行并作为单个事务提交。
如果禁用了自动提交模式，那么它的 SQL 语句将聚集到事务中，要提交更改就必须显式调用 commit或rollback 方法,否则无法保存数据库更改。
void setAutoCommit ( boolean autoCommit) //将此连接的自动提交模式设置为给定状态。
rollback()//取消在当前事务中进行的所有更改，并释放此 Connection 对象当前持有的所有数据库锁。
commit()//使所有上一次提交/回滚后进行的更改成为持久更改，并释放此 Connection 对象当前持有的所有数据库锁。
setSavepoint()//在当前事务中创建一个未命名的保存点 (savepoint)，并返回表示它的新 Savepoint 对象。
```

**示例：简易模拟转账**

```java
import utils.JDBC_Utils;
import java.sql.*;

public class Example00 {
    public static void main(String[] args){
        Connection conn=null;
        PreparedStatement statement1=null;
        PreparedStatement statement2=null;
        PreparedStatement statement3=null;
        PreparedStatement statement4=null;
        Savepoint savepoint=null;
        try {
            conn = JDBC_Utils.getConnection();//获取Connection对象方法，篇幅有限，不作展示
            conn.setAutoCommit(false);//将conn 自动提交设置为手动提交
            //模拟第一次转账
            String sql1 = "update bank set money=money-1000 where username='zhangsan'";
            statement1 = conn.prepareStatement(sql1);
            statement1.executeUpdate();
            //System.out.println(1/0);
            String sql2="update bank set money=money+1000 where username='lisi'";
            statement2 = conn.prepareStatement(sql2);
            statement2.executeUpdate();
            //将以上两个操作看做一个事务，只有这两操作都完成才会提交保存，否则回滚至初始状态
            savepoint = conn.setSavepoint();//创建一个保存点，可以指定回滚至此状态

            //模拟第二次转账,假设在此过程出现异常
            String sql3 = "update bank set money=money-1000 where username='zhangsan'";
            statement3 = conn.prepareStatement(sql3);
            statement3.executeUpdate();
            System.out.println(1/0);//模拟转账过程中的异常
            String sql4="update bank set money=money+1000 where username='lisi'";
            statement4 = conn.prepareStatement(sql4);
            statement4.executeUpdate();
            //将以上两个操作看做第二个事务
        } catch (Exception e) {
            try {
                if (savepoint != null) {
                    //如果savepoint不为null且出现异常，说明第一次转账成功，第二次失败，此时只需回滚至指定保存点即可
                    conn.rollback(savepoint);
                }else {
                    //如果savepoint为null，说明第一次转账异常，则回滚至初始状态，即第一次之前
                    conn.rollback();
                }
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        } finally {
            try {
                conn.commit();//手动提交事务，并释放Conn 对象当前持有的所有数据库锁
                conn.close();
                if (statement1 != null) {
                    statement1.close();
                }
                if (statement2 != null) {
                    statement2.close();
                }
                if (statement3 != null) {
                    statement3.close();
                }
                if (statement4 != null) {
                    statement4.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 3、事务的隔离级别

> 1、Read uncommitted 读未提交
>
> 当隔离级别设置为Read uncommitted时，就可能出现脏读。
>
> 2、Read committed 读提交 ==>(Oracle默认级别)
>
> 当隔离级别设置为Read committed时，避免了脏读，但是可能会造成不可重复读。
>
> 3、Repeatable read 重复读 ==>(MySQL默认级别)
>
> 当隔离级别设置为Repeatable read时，可避免脏读、不可重复读的发生，但可能会出现幻读（错误读取）。为此级别。
>
> 4、Serializable 串行化
>
> 最高级别，可以避免所有问题，但效率很低，一般不使用

> 四种隔离级别的效率：
>
>  read uncommitted>read committed>repeatable read>serializable
>
>
> 四种隔离级别的安全性：
>
>  read uncommitted<read committed<repeatable read<serializable

**设置查看事务的隔离级别**

```java
将数据库的隔离级别设置成 读未提交
	set session transaction isolation level read uncommitted;
查看数据库的隔离级别
	select @@tx_isolation;
java中控制隔离级别: Connection
	void setTransactionIsolation(int level) //level是常量
```

# 数据库连接池

**为什么要有连接池？**

> 由于建立数据库连接是一种非常耗时、耗资源的行为，所以通过连接池预先同数据库建立一些连接，放在内存中，应用程序需要建立数据库连接时直接到连接池中申请一个就行，使用完毕后再归还到连接池中，能明显提高对数据库操作的性能。

## 一、DBCP连接池

DBCP（DataBase Connection Pool）数据库连接池，是Java数据库连接池的一种，由Apache开发，通过数据库连接池，可以让程序自动管理数据库连接的释放和断开。

### 使用步骤

#### 1、导入 jar包(commons-dbcp-1.4.jar和commons-pool-1.5.6.jar)

#### 2、配置信息

**采用硬编码方式**

```java
//创建连接池
BasicDataSource ds = new BasicDataSource();
//配置信息
ds.setDriverClassName("com.mysql.jdbc.Driver");
ds.setUrl("jdbc:mysql:///mydb");
ds.setUsername("root");
ds.setPassword("123456");
//ds.setMaxWait(5000); //设置最大等待连接时间
//其他参数也可自行调用方法设置，不设置即为默认值
```

**采用配置文件方式**

**Java代码**

```java
Properties properties = new Properties();
properties.load(new FileReader("src/dbcp.properties"));
//创建一个工厂，获取连接池
DataSource ds = new BasicDataSourceFactory().createDataSource(properties);
```

**配置文件**

```properties
#连接基本设置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mydb
username=root
password=123456

#<!--扩展配置 了解-->
#初始化连接
initialSize=10

#最大连接数量
maxActive=50

#<!-- 最大空闲连接 -->
maxIdle=20

#<!-- 最小空闲连接 -->
minIdle=5

#<!-- 超时等待时间以毫秒为单位 6000毫秒/1000等于60秒 -->
maxWait=60000

#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：[属性名=property;] 
#注意："user" 与 "password" 两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=gbk

#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true

#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=

#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_UNCOMMITTED
```

#### 3、获取连接对象

```java
Connection conn = ds.getConnection();
```

## 二、C3P0连接池

C3P0是一个开源的JDBC连接池，它实现了数据源和JNDI绑定，支持JDBC3规范和JDBC2的标准扩展。目前使用它的开源项目Hibernate、Spring等。

### 使用步骤

#### 1、导入jar包(c3p0-0.9.1.2.jar)

#### 2、配置信息

##### 采用硬编码方式

```java
ComboPooledDataSource ds = new ComboPooledDataSource();
//设置基本参数
ds.setDriverClass("com.mysql.jdbc.Driver");
ds.setJdbcUrl("jdbc:mysql:///mydb");
ds.setUser("root");
ds.setPassword("123456");
```

##### 采用配置文件方式

> 采用此方式需注意：
>
> 要求1:配置文件的名称：c3p0.properties 或者 c3p0-config.xml
>
> 要求2:配置文件的路径：必须在 src 下

```java
	//new ComboPooledDataSource()	//使用默认的配置

	//使用命名的配置 若配置的名字找不到,使用默认的配置
	//new ComboPooledDataSource(String configName)
	
	//c3p0-config.xml的第二种配置
    ComboPooledDataSource ds = new ComboPooledDataSource("MyConfig");
```

**c3p0.properties**

```properties
c3p0.driverClass=com.mysql.jdbc.Driver
c3p0.jdbcUrl=jdbc:mysql:///mydb
c3p0.user=root
c3p0.password=123456
```

**c3p0-config.xml**

```xml
<c3p0-config>
	<!-- 默认配置，如果没有指定则使用这个配置 -->
	<default-config>
		<!-- 基本配置 -->
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/mydb</property>
		<property name="user">root</property>
		<property name="password">123456</property>
	
		<!--扩展配置-->
		<property name="checkoutTimeout">30000</property>
		<property name="idleConnectionTestPeriod">30</property>
		<property name="initialPoolSize">10</property>
		<property name="maxIdleTime">30</property>
		<property name="maxPoolSize">100</property>
		<property name="minPoolSize">10</property>
		<property name="maxStatements">200</property>
	</default-config> 
	
	
	<!-- 命名的配置 第二配置 -->
	<named-config name="MyConfig">
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="jdbcUrl">jdbc:mysql://127.0.0.1:3306/mydb</property>
		<property name="user">root</property>
		<property name="password">123456</property>

		<!--扩展配置-->
		<property name="acquireIncrement">5</property>
		<property name="initialPoolSize">20</property>
		<property name="minPoolSize">10</property>
		<property name="maxPoolSize">40</property>
		<property name="maxStatements">20</property>
		<property name="maxStatementsPerConnection">5</property>
	</named-config>
</c3p0-config>

```

#### 3、获取连接对象

```java
Connection conn = ds.getConnection();
```

## 三、Druid 阿里德鲁伊连接池

DRUID是阿里巴巴开源平台上一个数据库连接池实现，它结合了C3P0、DBCP、PROXOOL等DB池的优点，同时加入了日志监控，可以很好的监控DB池连接和SQL的执行情况，可以说是针对监控而生的DB连接池。

### 使用步骤

#### 1、导入jar包（druid-1.1.9.jar）

#### 2、配置信息

**采用硬编码方式**

```java
     //创建数据源
     DruidDataSource ds = new DruidDataSource();
     ds.setDriverClassName("com.mysql.jdbc.Driver");
     ds.setUrl("jdbc:mysql:///mydb");
     ds.setUsername("root");
     ds.setPassword("123456");
```

**采用配置文件方式**

```java
	Properties properties = new Properties();
    properties.load(new FileReader("src/druid.properties"));
	//通过一个工厂类，创建一个数据源
	DataSource ds =new DruidDataSourceFactory().createDataSource(properties);
```

**配置文件**

```properties
#基本配置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost/mydb
username=root
password=123456
#可选配置
filters=stat
initialSize=2
maxActive=300
maxWait=60000
timeBetweenEvictionRunsMillis=60000
minEvictableIdleTimeMillis=300000
validationQuery=SELECT 1
testWhileIdle=true
testOnBorrow=false
testOnReturn=false
poolPreparedStatements=false
maxPoolPreparedStatementPerConnectionSize=200
```

#### 3、获取连接对象

```java
Connection conn = ds.getConnection();
```

# DBUtils 工具类库

Commons DbUtils 是Apache组织提供的一个对JDBC进行简单封装的开源工具类库，使用它能够简化JDBC应用程序的开发，同时也不会影响程序的性能。

```java
//导入jar包（commons-dbutils-1.4.jar）
//创建 QueryRunner 对象
    Properties properties = new Properties();
    properties.load(new FileReader("src/druid.properties"));
    DataSource dataSource = new DruidDataSourceFactory().createDataSource(properties);
    //注意传入数据源
    QueryRunner queryRunner = new QueryRunner(dataSource);
	//编写执行SQL语句
	//编写执行DML、DQL语句  返回值：影响的行数 参2、参3为问号的值
    int i = queryRunner.update("insert into users values(?,?)", "张三", "123456");
	//查询语句
	//BeanListHandler 把从数据库查出来的多条数据，封装到对象里面，再把对象放到集合里面
	List<User> list = queryRunner.query("select * from users", new BeanListHandler<User>(User.class));
	//BeanHandler 查询一条结果，把这个结果封装进对象里面
    User user = queryRunner.query("select * from users", new BeanHandler<User>(User.class));
	// MapHandler 把查询的结果封装到Map集合里面
	Map<String, Object> map = queryRunner.query("select * from users", new MapHandler());
```

> 参考引用链接：https://blog.csdn.net/y_engineer/article/list/1