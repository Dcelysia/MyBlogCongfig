---
title: MYSQL
toc: true
date: 
cover: /img/明日方舟小人5.jpg
description: 想不出来介绍了
categories: 个人笔记
tags: 后端
post_meta:
  page:
    date_type: both # created or updated or both 主頁文章日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 主頁是否顯示分類
    tags: true # true or false 主頁是否顯示標籤
    label: false # true or false 顯示描述性文字
  post:
    date_type: both # created or updated or both 文章頁日期是創建日或者更新日或都顯示
    date_format: relative # date/relative 顯示日期還是相對日期
    categories: true # true or false 文章頁是否顯示分類
    tags: true # true or false 文章頁是否顯示標籤
    label: false # true or false 顯示描述性文字
copyright_author: Elysian剑彧
copyright_author_href: https://dcelysia.netlify.app/
copyright_url: https://dcelysia.netlify.app/
copyright_info: 此文章版权归Elysian剑彧所有，如有转载，请注明来自原作者
---



# MYSQL

**Tips:** 个人复习类笔记，较浅且不精美，如果错误欢迎指出
## 介绍

mysql是一种数据库管理操作系统，sql才是数据库操作语言

## DDL

### 操作库

在sql语法里，也是以分号作为结尾的；

​	如果没有分号，则会一直等待输入下去

#### 查看所有库

```sql
show databases;
```

#### 创建新的库

```sql
create database db1;(库名)
```

如果库已经被创建了就会报错，所以有先判断在创建的写法

```sql
create database if not exists db1;(库名)
```

#### 删除库

```sql
drop database db1;(库名)
```

同样，如果没有这个库名它就会报错，所以也有先判断在创建的写法

```sql
drop database if exists db1;(库名)
```

#### 使用库

```sql
use db1;(库名)
```

#### 查看自己使用的库

```sql
select database();
```

### 操作表

#### 查看所有表

```sql
show tables;
```

#### 进入特定表查看属性

````sql
desc stu;(表名)
````

#### 新建表

首先要知道sql的数据类型

**tinyint** 大小只有1bytes

**smallint** 大小只有2bytes(跟short类似)

**mediumint** 大小为3bytes

**int** 大小正常4bytes

**bitint** 大小为8bytes(跟long类似)

**double** 在定义的时候要定义长度和小数点后几位，例如我要定义成绩是0~100，为小数点后2位，所以长度就是5

**data** 日期值，大小为3bytes

**char** 定长字符串，即你定义这个字符串长度为10，那么它创建出来的长度一定为10

**varchar** 变长字符串，即你定义这个字符串长度为10，则是它的最大长度就是10，但是长度会随着字符串的大小而改变

```sql
-- 新建表格式
create table student(
    id int,
    name varchar(10),
    sex char(1),
    birthday date,
    score double(5,2),
    email varchar(64),
    telephone varchar(15),
    status tinyint
);
```

先写名称再写数据类型，各个数据之间用,隔开

同理，如果新建的表已经存在，那么就会报错，你可以使用

```sql
create table if no exists stu.....
```

来判断是否存在再创建

#### 删除表

```sql
drop table student;
-- 判断
drop table if no exists student;
```

#### 修改表

##### 修改表名

```sql
alter table student rename to stu;
```

##### 修改表中某一列的数据类型

```sql
alter table student modify address char(50);
```

##### 增加表中列

```sql
alter table student add password int;
```

##### 同时修改列名和列的数据类型

```sql
alter table student change address adr char(50);
```

##### 删除列

```sql
alter table student drop address
```

## DML

### 增加数据

```sql
-- 添加一个的部分数据
INSERT stu(id,name) VALUES(3,'wangwu');
-- 添加全部数据
INSERT stu(id,name,gender,birthday,score,email,telephone,status) 
VALUES(3,'wangwu','男','2000-1-1',99.99,'656546@qq.com','13318739936',2);
-- 添加多个数据 把括号里的多写几遍
INSERT stu(id,name,gender,birthday,score,email,telephone,status) 
VALUES(3,'wangwu','男','2000-1-1',99.99,'656546@qq.com','13318739936',2),
(3,'wangwu','男','2000-1-1',99.99,'656546@qq.com','13318739936',2),
(3,'wangwu','男','2000-1-1',99.99,'656546@qq.com','13318739936',2),
(3,'wangwu','男','2000-1-1',99.99,'656546@qq.com','13318739936',2);
```

### 修改数据

```sql
UPDATE stu set gender='女' where name='张三';
```

### 删除数据

```sql
DELETE from stu where name='张三';
```

注意，如果不加这个where判断语句，这个表里面的所有列都会被修改或者删除

## DQL

### 普通查询

````sql
SELECT id,name,age,sex,address,math,english,hire_date from stu;(表名)
````

如果想查询的时候去重

```sql
SELECT DISTINCT address AS 地址 from stu; 
```

AS是用来改变查询出来的列名，这里是把address改成了地址，更方便查阅

### 条件查询

例

```sql
-- 从stu中查询出年龄小于等于20岁的
SELECT id,name,age,sex,address,math,english,hire_date from stu WHERE age<=20;
-- 查询出英语成绩是null的
SELECT id,name,age,sex,address,math,english,hire_date from stu where english is null;
```

**细节** ： null不能用=或者!=来判断，在sql里判等就只用一个=就行了，并且推荐用and而不是&&，同理，或者用 or，在两者之间可以用betwee and,不等于除了用!=也可以用<>

```sql
-- 找到英语成绩大于等于30并且小于等于90的
SELECT id,name,age,sex,address,math,english,hire_date from stu where english BETWEEN 30 and 90;
```

### 模糊查询

模糊查询也算是条件查询的一种，但相比于条件查询较为精准的查询，模糊查询要查的内容就不需要精准

\_就是一个字符，%就是多个字符，跟正则表达式差不多

```sql
-- 姓马的人
SELECT id,name,age,sex,address,math,english,hire_date from stu where name like '马%';
-- 第二字是马的人
SELECT id,name,age,sex,address,math,english,hire_date from stu where name like '_马%';
-- 名字中带马的人
SELECT id,name,age,sex,address,math,english,hire_date from stu where name like '%马%';
```

### 排序查询

关键字是order by ,desc 为降序，asc为升序

如果同时写两个排序，只有第一个条件相等的情况才会触发之后的判断

```sql
SELECT * from stu order by math desc, english asc;
```

### 聚合函数

**count** 记录这一列一共有多少个元素

```sql
-- 记录这个单一共有多少对象
SELECT COUNT(*) from stu;
```

__max__  求这一列的最大元素

__min__ 求这一列的最小元素

__avg__  求这一列的平均元素

```sql
-- 求数学成绩的最大值
select max(math) from stu;
-- 求数学成绩的最小值
select min(math) from stu;
-- 求数学成绩的平均值
select avg(math) from stu;
```



### 分组函数

可以通过字段来把整个表分组，例如用性别把男女分组并求数量和数学成绩，关键词是 group by,写在from stu后面

```sql
SELECT COUNT(*) as 人数 ,AVG(math) from stu GROUP BY sex;
```

在from stu后面可以再继续加分组条件，比如让数学70分以上的人进来分组

````sql
SELECT COUNT(*) as 人数 ,AVG(math) from stu where math>70 GROUP BY sex; 
````

在分段字段后面也可以加上having去筛选已经分组好的数据

```sql
-- 把分组后数据大于2的组保留
SELECT COUNT(*) as 人数 ,AVG(math) from stu where math>70 GROUP BY sex HAVING count(*)> 2;
```

**注意点**

1. 分组后查询的字段应该只能是分组字段或者是聚合函数，查询别的字段无意义，例如把女生分成一组又要查询单个女生的name
2. where是判断上面数据能进入分组，having是把分组过后的数据进行调整
3. where不能判断聚合函数，但是having可以(如果用where去判断聚合函数就会报错)
4. where> 聚合函数 > having

### 分页查询

关键词是limit**(这是mysql特有的方言)**，格式是limit 起始索引，查询数目；

```sql
-- 查询从0开始的4个元素
SELECT * from stu limit 0, 4;
```

如果要查询第二页，那就是

```sql
SELECT * from stu limit 4, 4;
```

就这样实现了分页查询

查第几页就是 **(页数-1）* 查询数目 **



## 约束

约束可以在创建表的时候给变量后面加上约束，或者是表创建完modify变量然后在变量后面添加约束

```sql
-- 但常用的还是在创建表的时候再后面加上约束，这里你改变了约束一定要把表删了再重装一次才行
drop table staff;
CREATE TABLE staff (
	id int primary KEY auto_increment,
	NAME VARCHAR (20) not NULL unique,
	wage INT NOT NULL,
	bonus INT DEFAULT 0
);
```

**primary ket ** 主键约束 使主键不为null和唯一

**auto_increment** 只有在这个变量被主键约束后才能使用，会自动添加id并递增

```sql
-- 不写和写null都可以
insert into staff(name,wage,bonus) values('张三',5000,8000);
insert into staff(id,name,wage) values(null,'李四',5000);
-- 结果就1是张三 2是李四
```



**unique** 保证数据都不相同

**not null** 保证数据都不为空

**defalut** 给不添加的数据给一个初始值，例如**defalut 0 **就是这个元素不添加的时候初始值不是null而是0，如果你手动给这个数据赋值为null，那还是null

因为相当于你还是给他赋值了



**外键约束** :给两个表添加一个关系，例如一个公司倒闭了，你删除了公司，但公司里的员工都没有删除，这里就造成了数据的混乱

可以在创建表的时候创建constraint+外键约束的名字+外键列名， 

```sql
-- 员工表 
CREATE TABLE emp(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int,
	-- 添加外键 dep_id,关联 dept 表的id主键
	CONSTRAINTw fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)
);
-- 部门表
CREATE TABLE dept(
	id int primary key auto_increment,
	dep_name varchar(20),
	addr varchar(20)
);
```

当然你也可以通过增加数据的方式增加一个外键

```sql
alter table emp add constraint fk_emp_dept foreign key(dep_id) references dept(id);
```

也可以通过删除数据的方式删去一个外键(删除不需要在加constraint了)

```sql
alter table emp drop foreign key fk_emp_dept;
```

## 表之间的关系

有一对多，一对一，多对多

一对多其实就是上面约束的那个例子，一个公司对着很多员工，用外键约束去写就行

一对一其实也是一样的，不过很多员工变成一个人就行了

### 多对多

首先要准备中间表，例如商品和订单，一个订单里可以有多个商品，一个商品里也会有多个订单

```sql
-- 订单表
CREATE TABLE tb_order(
	id int primary key auto_increment,
	payment double(10,2),
	payment_type TINYINT,
	status TINYINT
);
-- 商品表
CREATE TABLE tb_goods(
	id int primary key auto_increment,
	title varchar(100),
	price double(10,2)
);
-- 中间表
CREATE TABLE to_order_goods (
	id INT PRIMARY KEY auto_increment,
	order_id INT,
	goods_id INT
);
```

之后写外键约束，用多的那一方，这里是中间表，去绑定少的那一方
```sql
alter table to_order_goods add constraint foreign key(order_id) references tb_order(id);
alter table to_order_goods add constraint foreign key(goods_id) references tb_goods(id);
```

 ## 多表查询

跟单表查询差不多
```sql
select * from emp , dept;
```

但是普通的这样子出来的就是emp表和dept表所有的排列组合
### 内连接

查询到两个表交集的部分

这样只会查询到两个id相等的数据

隐式内连接

```sql
select * from emp , dept where emp.dep_id = dept.did;
```

显式内连接

```sql
select * from emp join dept on dep_id = dept.did;
```

### 外连接

**左外连接** 查询到两表交集的部分和左表

```sql
select * from emp left join dept on dep_id = dept.did;
```

**右外连接** 查询到两表交集的部分和右表

```sql
select * from emp RIGHT join dept on dep_id = dept.did;
```

一般只用左外连接就行了

### 子查询

#### 普通子查询

例如找孙悟空的公司名字，就先用select查询出孙悟空的公司编号，然后再一个select去查询和孙悟空个公司编号相同的公司

```sql
select * from dept where did=(select dep_id from emp where name='孙悟空');
```

#### 列子查询

**操作符** ：

1. **in** 在指定的集合范围内
2. **not in** 不在指定的集合范围之内
3. **any** 在子查询返回的列表中有一个满足的就可以了
4. **some** 可以用any代替
5. **all** 子查询返回的所有列表都要满足

**in** 和 **not in** 很好理解

```sql
-- (select salary from emp where dep_id = 2)返回的是在公司代号2的人的月薪，这里的意思是找出大于公司2任何一人工资的人的信息
select * from emp where salary > any(select salary from emp where dep_id = 2);
```

```sql
-- (select salary from emp where dep_id = 2)返回的是在公司代号2的人的月薪，这里的意思是找出大于公司2所有人工资的人的信息
select * from emp where salary > all(select salary from emp where dep_id = 2);
```

#### 行子查询

子查询返回的是一行

```sql
-- 行里有多少个数据就可以比较多少个数据
select * from emp where (salary,age) = (select salary,age from emp where name='原神');
```

#### 表子查询

关键词是**in**和**not in**

```sql
-- 找出工资和年龄跟公司代码2中一样的人
select * from emp where (salary,age) in (select salary,age from emp where dep_id = 2);
```
## 事务

事务通俗来讲就是把一组行动变成一套任务，任务要么成功要么失败，其中的流程不能细分了

例如转账，发和收就必须是事务，如果发出去了但是收的时候遇到意外了，那么就出事了

```sql
-- 事件开始
BEGIN;
-- 事件之中的流程
UPDATE account set money=money-500 where name='张三';
UPDATE account set money=money+500 where name='李四';
-- 如果成功了就要commit
commit;
-- 失败了就会回退
ROLLBACK;
```

commit在mysql是会自动commit的，如果不commit别人是看不到更改的数据的，如果commit了更改的数据就是永久性了

**事件的四大特征** :

1. **A** 原子性：事务不可再分，要么同时成功，要么同时失败
2. **C** 一致性：事务完成之后，由于会commit，所有的数据都会保持一致
3. **I**  隔离性：多个事务之间，操作一般是不可见的
4. **D** 持久性：事务完成后的数据是永久的

