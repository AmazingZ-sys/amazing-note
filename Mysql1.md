# Mysql

## 基础操作：

**登陆数据库：**

```mysql
mysql -hlocalhost -uroot -p
```

**查看有哪些库：**

```mysql
show databases;
```

**创建库：**

```mysql
create database [IF NOT EXISTS] 库名;
```

**删除库：**

```mysql
drop database [IF EXISTS] 库名；
```

**进入库：**

```mysql
use 库名;
```

**创建表：**

例如-创建名为stu的表：

```mysql
mysql> create table [IF NOT EXISTS] stu(
    -> id int(10) auto_increment primary key,
    -> name varchar(255),
    -> age varchar(10),
    -> sex varchar(10))default charset=utf8;

auto_increment  id自增
primary key  主键
default charset=utf8  设置字符集
not null	是否为空
default		设置默认值
engine		设置引擎
unique		唯一约束条件
enum		枚举
comment		注释 
```

**查看表的结构：**

```mysql
desc 表名;
```

**查看表：**

```	mysql
show tables;
```

**向表中添加列：**

```mysql
alter table 表名 add 列名 条件 [after 存在的列];
默认放在最后
first放在最前面
after放在指定列后
```

**修改数据类型：**

```mysql
alter table 表名 modify column 要修改的数据 varchar(4);
```

**删除表中的某列：**

```mysql
alter table 表名 drop 列名;
```

**增加表中某项为唯一值的约束条件：**

```mysql
alter table 表名 add unique(项);
```

**修改表中表头的名称：**

```mysql
alter table 表名 change 原值 修改值 varchar(20);
```

**插入信息：**

```mysql
insert into 表名 (name,age,sex) values ('xxx','18','男');
```

**查看表中全部信息：**

```mysql
select * from 表名;
```

**筛选：（例如：年龄在17-19岁之间的学生的姓名及年龄）**

```mysql
select Sname，Sage from Student where Sage between 17 and 19;
```

**统计：(例如：统计学生总人数）**

```mysql
select count(*) from Student; 
```

**查询：（例如：所有姓名以“w”开头的学生的信息）**

```mysql
select * from Student where Sname like’w%’;
```

**查询：（例如：选修了3号课程且成绩在85分以上的学生的学号和姓名）**

```mysql
select Student.Sno,Sname from Student,SC where Student.Sno=SC.Sno and SC.Cno=’3’ and SC.Grade>85;
```

**修改信息：**

```mysql
update 表名 set sex='女' where id=1;
```

**删除信息：**

```mysql
delete from 表名 where id=2;
```

**删除表：**

```mysql
drop table 表名；
```

**增加主键：**

```mysql
alter table 表名 add primary key(列名);
```

**删除主键：**

```mysql
alter table 表名 drop primary key;
```

**增加索引：**

- 添加唯一索引

  ```mysql
  alter table 表名 add unique ('column');
  ```

- 添加全文索引

  ```mysql
  alter table 表名 add fulltext ('column');
  ```

- 添加普通索引

  ```mysql
  alter table 表名 add index ('column');
  ```


**删除索引：**

```mysql
alter table 表名 drop index column_name;
```

**修改索引：**

没有提供修改的方式，先删除再添加

**修改引擎：**

```mysql
alter table 表名 engine = InnoDB;
show engine;
show create table 表名;
```

**修改自增值：**

```mysql
alter table 表名 auto_increment = 1;
```





## 设置：

- DCL：数据控制语言，用来设置或修改数据库用户或角色权限
- DDL：数据定义语言
- DML：数据操作语言
- DQL：数据查询语言
- TCL：事务控制语言
- 数据库锁
- 主从配置
- 数据库优化操作



## 访问控制权限：

#### 命令行链接命令：

```mysql
mysql -u 用户名 	-p 密码 		-h 数据库地址 	 -p 端口 		-D 数据库名
```

#### 默认表：

user表：包含用户的信息

db表：用户对数据库的访问权限

tables_priv和columns_priv：表级和列级权限

procs_priv：包含存储的函数

## 创建用户账户：

创建

```mysql
create uesr usernaame@hostname identified by possword;
```

查询权限

```mysql
show grants for dbadmin@localhost;
```

创建已存在的用户

```mysql
报错：1396
```

删除用户

```mysql
drop user username@hostname;
```

设置权限

```mysql
geant privilege ,[privilege],.. on privilege_level to user [identified by possword] [require tsl_option] [with [grant_option | resource_option]];
```

允许远程链接：

```mysql
grant all  privileges on *.* to 'root'@'%' identified by possword with  grant option;
flush privileges；  #刷新
```

撤销权限

```mysql
REVOKE
    priv_type [(column_list)]
      [, priv_type [(column_list)]] ...
    ON [object_type] priv_level
    FROM user_or_role [, user_or_role] ...
```

撤消所有权限

```mysql
REVOKE ALL PRIVILEGES, GRANT OPTION
  FROM user_or_role [, user_or_role] ...
```

修改密码

```mysql
set password for 'root'@'localhost'=password('密码');
```

```mysql
mysqladmin -u用户名 -p旧密码 password 新密码;   #不需要进入数据库
```

忘记root密码或初始密码

```mysql
1.关闭正在运行的数据库服务
2.打开DOS窗口，转到mysql\bin目录
3.输入
	mysqld --skip-grant-tables   #跳过检查
4.再开一个DOS窗口，转到mysql\bin目录
5.输入mysql，如果成功，出现提示符
6.链接权限数据库：use mysql;
7.改密码
	update user set possword=possword('123') where User='root';
8.刷新权限
	flush privileges；
9.退出
10.登陆
```

## 数据库备份：

```mysql
mysqldump  -u [username] -p[password] 库名  >  [dump_file.sql]
```

```mysql
mysqldump  -u [username] -p[password]  --no-data  库名  >  [dump_file.sql]  仅备份数据库结构
```

```mysql
mysqldump  -u [username] -p[password]  --no-create-info  库名  >  [dump_file.sql]  仅备份数据
```

**导出多个数据库**

```mysql
mysqldump  -u [username] -p[password] 库名1，库名2  >  [all_dbs_dump_file.sql]
```

```mysql
mysqldump  -u [username] -p[password]  --all-databases  >  [all_dbs_dump_file.sql]
```

## 数据库维护：

- **分析表语句**

  ```mysql
  analyze   table  表名；
  ```

- **优化表**

  ```mysql
  optimize  table  表名；
  ```

- **检查表**

  ```mysql
  check  table  表名；
  ```

- **修复表**

  ```mysql
  repair  table  表名；
  ```

## 数据类型：

###  数值类型：

| 类型         | 大小                                     | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| ------------ | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| TINYINT      | 1 字节                                   | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 字节                                   | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 字节                                   | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 字节                                   | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 字节                                   | (-9 233 372 036 854 775 808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 字节                                   | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 字节                                   | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

### 字符串类型：

| 类型       | 大小                | 用途                            |
| ---------- | ------------------- | ------------------------------- |
| CHAR       | 0-255字节           | 定长字符串                      |
| VARCHAR    | 0-65535 字节        | 变长字符串                      |
| TINYBLOB   | 0-255字节           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255字节           | 短文本字符串                    |
| BLOB       | 0-65 535字节        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535字节        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215字节    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215字节    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295字节 | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295字节 | 极大文本数据                    |

### 日期类型：

| 类型      | 大小 (字节) | 范围                                                         | 格式                | 用途                     |
| --------- | ----------- | ------------------------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3           | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3           | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1           | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8           | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4           | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

## 引擎类型：

**什么是存储引擎：**

      MySOL中的数据用各种不同的技术存储在文件或者内存中、这些技术中的每一种技术都使用不同的存储机制、索引技巧、锁定水平并且最终提供广泛的不同的功能和能力。通过选择不同的技术。你能够获得额外的速度或者功能，从而改善你的应用的整体功能
    
      关系数据库表是用于存储和组织信息的数据结构，可以将表理解为由行和列组成的表格，类似于Excel的电子表格的形式。有的表简单，有的表复杂，有的表根本不用来存储任何长期的数据，有的表读取时非常快，但是插入数据时去很差;而我们在实际开发过程中，就可能需要各种各样的表，不同的表，就意味着存储不同类型的数据，数据的处理上也会存在着差异，那么。对于MySQL来说，它提供了很多种类型的存储引擎(或者说不通的表类型)，我们可以根据对数据处理的需求，选择不同的存储引擎，从而最大限度的利用MySQL强大的功能
    
      在mysq客户端中，使用以下命令可以查看MySQl支持的引擎
**MyISAM：**

它不支持事务，也不支持外键，尤其是访问速度快，对事务完主的应用基本都可以使用这个引擎来创建表。每个MyISAM在名都相同，但是扩展名分别为:

- .frm(存储表定义)
- MYD(MYData, 存储数据)
- MYI(MYIndex, 存储索引)

**InnoDB：**

- InnoDB是个健状的事务型存储引擎， 这种存储引擎已经被很多互联网公司使用，为用户操作非常大的数据存储提供了个强大的解决方案。 我的电脑上安装的MySQl 5.6.13版，InnoDB就是作为默认的存储引擎。InnoDB还引入了行级锁定和外键约束，在以下场合下，使用InnoDB是 最理想的选择
- 更新密集的表。InnoDB存储引擎特别适合处理多重并发的更新请求。
- 事务，InnoDB存储引擎是支持事务的标准MySQL存储引擎。
- 自动灾难恢复，与其它存储引擎不同，InnoDB表能够自动从灾难中恢复。外键约束。MySQL支持外键的存储引擎只有InnoDB.
- 需要事务支持，并且有较高的并发读取频率，InnoDB是不错的选择

## 索引：（数据结构）

1.主键索引：primary

- 主键在一个表中是唯一的，值不能重复

2.唯一键：unique

- 可以给多个字段设置
- 设置后该字段的值不能重复

3.普通索引：index

- 一个表可以有多个普通索引，他会在查询本字段时形成一定的顺序

4.文本索引：fulltext    （MySQL5.7以后才有用）

- 文本编辑器
- 帮助我们在大批的文本中，快速查询

5.外键：

- 定义一个副表的非主键字段和主表的主键字段进行关联的话，那么这个非主键字段我们叫做是外键
- 外键有什么作用：
  - 如果在副表当中添加一个主键里面不存在的数据，插入操作会报错
  - 如果在主表当中进行修改或者是删除的时候，当副表里面有对应的数据，那么主表会阻止执行(默认)

- 创建外键

  选项：district(默认)    cascade(关联)     set null(将关联数据设为null)   no action(什么都不做)

  ```mysql
  mysql> create table [IF NOT EXISTS] stu(
      -> id int(10) auto_increment primary key,
      -> name varchar(255),
      -> age varchar(10),
      -> sex varchar(10))default charset=utf8 
      [constraint 设置的外键名] foreign key [外键名](字段) references 表名(字段)
      on delete 选项
      on update 选项;
  ```


- 添加外键

  ```mysql
  alter table 表名 add [constraint 设置的外键名] foreign key [外键名](字段) references 表名(字段)
  ```

- 删除外键

  ```mysql
  alter table 表名 drop foreign key 外键名
  ```


## 数据库操作语言(DML)：

### 插入：

**多行插入：**

```mysql
INSERT INTO table_name  (field1, field2,...fieldN)  VALUES  (valueA1,valueA2,...valueAN),(valueB1,valueB2,...valueBN),(valueC1,valueC2,...valueCN)......;
```

**具有select子句的insert语句（导入）：**

```mysql
create table 表2 like 表1; 		#表2复制表1表结构
insert into 表2 select * from 表1;	#表2导入表1表信息
```

**on duplicate key update 条件：**

> 放在语句后面，取消报错

**replace语句：**

> 当插入已存在的数据时，是更新；插入新数据时，是插入

```mysql
replace into 表名 (name,age,sex) values ('xxx','18','男');
```



### 更新：

```mysql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference
    SET assignment_list
    WHERE where_condition
   
LOW_PRIORITY	有查询时先查询后更新
IGNORE		出现错误忽略
```



**带有select语句的更新：**

```mysql
select * from teach order by id desc
# order by 排序
# asc 正序  desc 倒序  rand() 随机  
update 表名 set tname=(select * from teach order by rand() limit 1)
```

**关联更新：**

- 第一种写法

  ```mysql
  update table1,table2 [条件]
  set table.attr=val,table2.attr=val,...
  where condition
  ```


- 第二种写法

  ```mysql
  update table1 join table2 on [条件]
  set table1.attr=val,table2.attr=val,...
  where condition
  ```


### 删除：

**关联删除：**

```mysql
delete table1,table2 from table1 inner join table2 on ...
```

**清空数据：**

```mysql
truncate [table] 表名;    
```

```mysql
delete from 表名;		#自增值不会回到初始状态
```



### 日志管理：

> 它记录看服务器的运行信息。许多操作都会写日到日志文日志文件对于个国务经东的是的家行理来万香者服务器的性能，还能对服务器进行排错与故障处件，通过日志文件可以监视服务器的运行状态理，MySO中有六种不同类型的日志

**日志种类**

- 错误日志:记录启动运行或停止时出现的问题，一般也会记录警告信息.
- 一般查询日志:记录建立的客户端连接和执行的语句。
- 慢查询日志记录所有执行时间超过ong quey time秒的所有查询或不使用索引的意询，可以帮
  我们定位服务器性能问题。
- 二进制日志:任何引起或可能引起数据库变化的操作。主要用于复制和即时点恢复
- 中继日志:从主服务器的二进制日志文件中复制而来的事件，并保存为的日志文件
- 事务日志:记录InnoDB等支持事务的存储引擎执行事务时产生的日志

**mysq|全局变量查询与修改**

> 对于日志的管理会涉及到mysql全局变量的直询与修改，所以首先我们要看一下如何操作和查看变量

- 查询变量

  ```mysql
  show global variables [like '%log%']
  ```

- 修改日志

  ```mysql
  set global variables_name=val
  ```


**错误日志**

- 错误日志文件: log_error指定mysqld保存错误日志文件的位置.
- 启用警告信息: log_warnings (默认启用)，数据库警告信息

**一般查询日志**

- 启动开关general_Jog=(ON|OFF)

- 日志文件变量: general_Jog._file[=/PATH/TO/file]

- 全局日志开关:log=(ON|OFF}该开关打开后，所有日志都会被启用

- 记录类型: log_output=(TABLE|FLE|NONE) log_output定义了日志的输出格式，可以是表，文件，若

  设置为NONE,则不启用日志，因此，要启用通用查询日志，需要至少配置general_log=ON,

    log_output=(TABLE|FLE|NONE).而general_log_file如果没有指定，默认名是host_name.log.由于一般查询使用量比较大，启用写入日志文件，服务器的I/O操作较多，会大大降低服务器的性能，所以默认为关闭的

**慢查询日志**

- 查询超时时间: long_query_time

- 启动慢查日志: log_slow_queries=(YES|NO)
- 启动慢查日志 :slow_query_log

- 日志记录文件: slow_query_log_file[=file_name]MysqL如果启用了slow_query_log=NO选项，就会记录执行时间超过long_query_time的直询(初使表锁定的时间不算作执行时间)。日志记录文件如果没有输出file_name值，默认为主机名， 后缀为-slow.log. 如果给出了文件名，但不是绝对路径名，文件则写入数据目录。

## 数据库查询语言（DQL）:

### 查询语句：

```mysql
select
column_1,column_2,...
from
	table_1
[inner|left|rigth] join table_2 on conditions
where
	conditions
group by column_1
having group_conditions
order by column_1
limit offset,length;
```

### WHERE:

- WHERE子句中的比较运算符

| 操作符 | 描述                                                         | 实例                 |
| ------ | ------------------------------------------------------------ | -------------------- |
| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

- WHERE子句中的逻辑运算符

| 操作符 | 描述 |
| :----: | :--: |
|   or   | 或者 |
|  and   | 并且 |
|  not   |  非  |

- between运算符

  - between运算符允许指定要测试的值范围

    ```mysql
    expr [not]  between begin_expr and end_expr;
    ```

    如果expr的值大于或等于(>=)begin_expr的值且小于等于(<=)end_expr的值，则between运算符返回true,否则返回0

  - 获取指定的时间范围

    ```mysql
    between cast('2-13-01-01' as date)
    	and cast('2-13-01-31' as date);
    ```

- like运算符

  ```mysql
  SELECT field1, field2,...fieldN 
  FROM table_name
  WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
  
  如果没有使用百分号 %, LIKE 子句与等号 = 的效果是一样的。
  ```



  ```mysql
  SELECT * from runoob_tbl WHERE runoob_author LIKE '%COM';
  
  +-----------+---------------+---------------+-----------------+
  | runoob_id | runoob_title | runoob_author | submission_date |
  +-----------+---------------+---------------+-----------------+
  | 3 | 学习 Java | RUNOOB.COM | 2015-05-01 |
  | 4 | 学习 Python | RUNOOB.COM | 2016-03-06 |
  +-----------+---------------+---------------+-----------------+
  2 rows in set (0.01 sec)
  ```

  ```mysql
  SELECT * from runoob_tbl WHERE runoob_author LIKE '%COM' escape"$";
  
  %全部匹配  _匹配一个  \转义  escape"$"把"$"设置为转义字符
  ```


- in运算符

  ```mysql
  select * from 表名1 where 字段 in (select * from 表名2)
  ```


- find_in_set()函数

  ```mysql
  select * from 表名 where find_in_set("要查询的内容","字段")
  ```


- group by

  > 在where后执行，进行分组

  ```mysql
  select 
  	c1,c2,...,cn,aggregate_function(c1)
  from
  	table
  where
  	where_conditions
  group by c1,c2,...,cn;
  ```


- group by语句中的having子句

  > 在分组后执行，再次筛选

  ```mysql
  聚合函数
  avg()-计算一组中值或表达式的平均值
  count()-计算表中的行数
  instr()-返回子字符串在子字符中第一次出现的位置
  sum()-计算一组中值或表达式的总和
  min()-在一组值中找最小值
  max()-在一组值中找最大值
  ```

- order by

  > 排序

  ```mysql
  select column1,column2,...
  from table_name
  order by column1[asc|desc]，column2[asc|desc],...
  ```

  - asc-升序   desc-降序

  - 可以指定多列排序，但是后一列总在前一列的基础上排列

  - 也可以用表达式排列

  - order by自定义排序

    ```mysql
    select column1,...
    from table_name
    order by field (column1，val1,val2,...)
    ```


- limit

  ```mysql
  select 
  	column11,column12,...
  from
  	table_name
  limit offset,count;
  
  offset - 偏移量
  count - 指定要返回的最大行数
  ```

  - limit获取前n行

    ```mysql
    select 
    	column11,column12,...
    from
    	table_name
    limit n;
    ```


### 关联查询：

> 表与表之间有关系，通过关系去查询

MySQL支持以下类型的连接：

- 交叉连接
- 内连接
- 左连接
- 右连接

#### 交叉连接（CROSS JOIN）

> 笛卡尔乘积

```mysql
select t1.id,t2.id,...
	from  t1
CROSS JOIN t2;
```

#### 内连接（INNER JOIN）

```mysql
select t1.id,t2.id,...
	from  t1
INNER JOIN 
	t2 ON t1.pattern=t2.pattern;
```

#### 左连接（LEFT JOIN）

```mysql
select t1.id,t2.id,...
	from  t1
LEFT JOIN 
	t2 ON t1.pattern=t2.pattern;
```



#### 右连接（RIGHT JOIN）

```mysql
mysqlx select t1.id,t2.id,...    from  t1RIGHT JOIN 	    t2 ON t1.pattern=t2.pattern;
```



## 联合查询：

> 把多个select语句查询的结果合并起来

```mysql
select column_name from table1
UNION
select column_name from table2;

#可以自动去重
```



```mysql
select column_name from table1
ALL UNION
select coulom_name from table2;
		
#不会去重
```



## 子查询：

- 子查询的分类：

  标量子查询：返回单一的标量，最简单的形式

  列子查询：返回的结果集是N行一列

  行子查询：返回的结果集是一行N列

  表子查询：返回的结果集是N行N列



#### 标量子查询

```mysql
select * from article where uid = (select uid from user where status=1 order by uid desc limit 1)
```



#### 列子查询

```mysql
select * from article where uid in (select uid from user where status=1)
select s1 from table1 where s1>any(select s2 from table2)
select s1 from table1 where s1>all(select s2 from table2)
```

- any-条件满足其一就为真
- all-条件同时满足为真
- in-在其中就为真

#### 行子查询

```mysql
select * from article where （title,content,uid） = (select title,content,uid from blog where bid=1)
```



#### 表子查询

```mysql
select * from article where （title,content,uid） in (select title,content,uid from blog )
```

```mysql
select * from table1
	where city='hangzhou' and exists
		(select *
        from table2 where table1.customer_id=table2.customer_id);
#获得城市为hangzhou，并且存在的订单

#无论查询的子句是否成功返回，exists都不会报错
```



## MySQL函数：

#### 聚合函数：

COUNT()函数

> 返回表中的行数

```mysql
SELECT COUNT(ProductID) AS NumberOfProducts FROM Products;
```

AVG()函数

> 计算平均值

```mysql
SELECT AVG(Price) AS AveragePrice FROM Products;
```

SUM()函数

> 计算总和

```mysql
SELECT SUM(Quantity) AS TotalItemsOrdered FROM OrderDetails;
```



MAX()函数

> 最大值

```mysql
SELECT MAX(Price) AS LargestPrice FROM Products;
```



MIN()函数

> 最小值

```mysql
SELECT MIN(Price) AS LargestPrice FROM Products;
```



GROUP_CONCAT()函数

> 将查询出的数据都显示出来



#### 字符串函数

CONCAT()和CONCAT_WS()

> 连接多个字符串

```mysql
SELECT CONCAT("SQL ", "Runoob ", "Gooogle ", "Facebook") AS ConcatenatedString;
```

```mysql
SELECT CONCAT_WS("-", "SQL", "Tutorial", "is", "fun!")AS ConcatenatedString;
```



LEFT()函数

> 返回字符串 s 的前 n 个字符

```mysql
SELECT LEFT('runoob',2) -- ru
```



REPLACE()函数

> 将字符串 s2 替代字符串 s 中的字符串 s1

```mysql
SELECT REPLACE('abc','a','x') --xbc
```



SUBSTRING()函数

> 从字符串 s 的 start 位置截取长度为 length 的子字符串

```mysql
SELECT SUBSTRING("RUNOOB", 2, 3) AS ExtractString; -- UNO
```



TRIM()函数

> 去掉字符串 s 开始和结尾处的空格

```mysql
SELECT TRIM([{leading|both|trailing}[removed_str]] from '    RUNOOB    ') AS TrimmedString;
```



FORMAT()函数

> 函数可以将数字 x 进行格式化 "#,###.##", 将 x 保留到小数点后 n 位，最后一位四舍五入

```mysql
SELECT FORMAT(250500.5634, 2);     -- 输出 250,500.56
```



#### 时间和日期函数

##### 返回当前日期的函数

CURDATE()

> 返回当前日期

```
SELECT CURDATE();
-> 2018-09-19
```

SYSDATE()

> 返回当前日期和时间

```
SELECT SYSDATE()
-> 2018-09-19 20:57:43
```



NOW()

>   返回当前日期和时间

```
SELECT NOW()
-> 2018-09-19 20:57:43

```



##### 返回指定日期函数

DAY(d)

> 返回日期值 d 的日期部分

```mysql
SELECT DAY("2017-06-15");  
-> 15
```



MONTH(d)

> 返回日期d中的月份值，1 到 12

```mysql
SELECT MONTH('2011-11-11 11:11:11')
->11
```



YEAR(d)

> 返回年份

```mysql
SELECT YEAR("2017-06-15");
-> 2017
```

WEEK(d)

> 计算日期 d 是本年的第几个星期，范围是 0 到 53

```mysql
SELECT WEEK('2011-11-11 11:11:11')
-> 45
```

WEEKDAY(d)

> 日期 d 是星期几，0 表示星期一，1 表示星期二

```mysql
SELECT WEEKDAY("2017-06-15");
-> 3
```



DAYNAME()

> 返回时间名字

```mysql
SELECT DAYNAME(NOW());

set @@lc_time_names = 'zh_CN' #表示方式改为中文
```



##### 日期计算函数

DATEDIFF(d1,d2)

> 计算日期 d1->d2 之间相隔的天数

```mysql
SELECT DATEDIFF('2001-01-01','2001-02-02')
-> -32
```



TIMEDIFF(time1, time2)

> 计算时间差值

```mysql
SELECT TIMEDIFF("13:10:11", "13:10:10");
-> 00:00:01
```



TIMESTAMPDIFF()

```mysql
TIMESTAMPDIFF(unit,begin,end)
```

- unit参数有以下：
  - microsecond
  - second
  - minute
  - hour
  - day
  - week
  - month
  - quarter
  - year

DATE_ADD(d，INTERVAL expr type)

> 计算起始日期 d 加上一个时间段后的日期

```mysql
SELECT ADDDATE('2011-11-11 11:11:11',1)
-> 2011-11-12 11:11:11    (默认是天)

SELECT ADDDATE('2011-11-11 11:11:11', INTERVAL 5 MINUTE)
-> 2011-11-11 11:16:11 (TYPE的取值与上面那个列出来的函数类似)
```

DATE_SUB(date,INTERVAL expr type)

> 函数从日期减去指定的时间间隔。

```mysql
Orders 表中 OrderDate 字段减去 2 天：

SELECT OrderId,DATE_SUB(OrderDate,INTERVAL 2 DAY) AS OrderPayDate
FROM Orders
```



## 视图：

#### 创建视图

```mysql
create view viewname as select ...
```



#### 修改视图

```mysql
alter  view viewname as select ...
```



#### 使用CREATE OR REPLACE VIEW语句修改视图

```mysql
CREATE OR REPLACE VIEW viewname as select ...
```



#### 删除视图

```mysql
drop view viewname
```



#### 查看视图

```mysql
- 查看视图表
	show full tables;
	
- 查看视图定义
	show create view viewname
```





## 临时表：

#### 创建临时表

```mysql
create temporary table select ...
```



#### 删除临时表

> 会话结束后自动删除或者使用drop



## 事务：

#### 事务的性质：

- 原子性：一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
- 一致性：在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
- 隔离：数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
- 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。



#### 事务控制语句：

- BEGIN或START TRANSACTION；显式地开启一个事务；
- COMMIT；也可以使用COMMIT WORK，不过二者是等价的。COMMIT会提交事务，并使已对数据库进行的所有修改成为永久性的；
- ROLLBACK；有可以使用ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
- **SET AUTOCOMMIT=0** 禁止自动提交；
- **SET AUTOCOMMIT=1** 开启自动提交；



#### 事务支持的表类型：

- 有许多类型的表支持事务，但目前最流行的一种是：InnoDB
- 可以使用其他类型的表GEMINI或BDB，但它取决与安装MySQL时，是否支持这两种类型



## 数据库锁：

- 表级锁：开销小，加锁快；不会出现死锁；锁定颗粒度大，发生锁冲突的概率最高，并发度最低
- 行级锁：开销大，加锁慢；会出现死锁；锁定颗粒度最小，发生锁冲突的概率最低，并发度最高
- 页面锁：开销和加锁时间介于表级锁和行级锁之间；会出现死锁；锁定颗粒度介于表级锁和行级锁之间，并发度一般

### 表级锁：

> 有两种模式：表共享锁和表独占写锁
>
> 表级锁的存储引擎
>
> MyISAM引擎
>
> MEMORY引擎



#### 表级锁的特点

- 作用范围在表的级别
- 如果加了读锁， 对MyISAM表的读操作，不会阻塞其他用户对同一表的读请求，但会阻塞对同一表的写请求
- 如果加了读锁，可以查询锁定表中的记录，但更新或访问其他表都会提示错误
- 如果加了写锁，对MyISAM表的写操作， 则会阳塞其他用户对同一表的读和写操作
- 如果加了写锁，可以读写表中的记录，但更新或访问其他表都会提示错误



#### 如何加锁：

> MyISAM在执行查询语句(select)前，会自动给涉及的所有表加读锁；在执行更新操作(update、insert、delete）前，会自动给涉及的表加写锁，这个过程不需要用户参与，因此，用户一般不需要直接用lock table		命令给MyISAM表显示加锁

- 加锁

  ```mysql
  LOCK TABLES table_name read [local],LOCK TABLES table_name write [local];
  ```

- 多表加锁

  ```mysql
  LOCK TABLES table_name [table_name] read [local],LOCK TABLES table_name [table_name] write [local];
  ```

- 释放锁

  ```mysql
  unlock tables;
  ```


#### 查询表级锁争用的命令：

```mysql
show status like 'table%'
或者
show status like '%lock'
或者
show processlist	#此命令可以查看哪些sql命令在等待
或者
show open tables	#当前被锁住的表以及锁的次数
```



#### 并发插入：

> MyISAM存储引擎有一个系统变量concurrent_insert，专门用以控制其并发插入行为，其值分别是0、1、2

- 0-不允许并发插入
- 1-如果MyISAM表中没有空洞（即表的中间没有被删除的行），允许在一个进程读表的同时，另一个进程从表尾插入记录，这也是MyISAM表的默认设置
- 2-无论MyISAM表中是否有空洞，都允许在表尾插入记录



#### 读写锁优先级：

- 设置写锁的最多次数

  ```mysql
  max_write_lock_count=1
  ```

  **有了这样的设置，当系统处理一个写操作后，就会暂停写操作，给读操作执行的机会**

- 降低写操作的优先级，给读操作更高的优先级

  ```mysql
  low_priority_updates=1
  sql_low_priority_updates=1
  ```

  > 在使用update、insert、delete时，要加上[]、[]关键字

  **视情况而定，如果读的场景比较重要或者场景比较多，可以如此设置**



#### 设置写内存：

```mysql
max_allowed_packrt=1M	#限制接受的数据包大小，大的插入或更新会被限制，导致失败
net_buffer_length=2K	#insert语句缓存值  2K-16M
bulk_insert_buffer_size=8M	#一次性insert语句插入的大小
```

#### 如何优化

- 可以利用MyISAM存储引擎的并发插入特性，来解决应用中对同一表查询和插入的锁争用。例如，将袁省街
  concurrent_ insert系统变量设为2，总是允许并发插入
-  同时，通过定期在系统空时段执行OPTIMIZE TABLE语句来整理空间碎片，回收因删除记录而产生的中间空洞
- 是否设置写的优先级，视场景而定，解决查询相对重要的应用（如用户登陆系统）中
- 是否设置写内存，视场景而定，解决批量插入数据（新闻系统更新）场景中



### 行级锁：

> 共享锁（s）：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。
>
> 排他锁（Ｘ）：允许获取排他锁的事务更新数据，阻止其他事务取得相同的数据集共享读锁和排他写锁。
>
> 意向共享锁（IS）：事务打算给数据行共享锁，事务在给一个数据行加共享锁前必须先取得该表的IS锁。
>
> 意向排他锁（IX）：事务打算给数据行加排他锁，事务在给一个数据行加排他锁前必须先取得该表的IX锁。



**InnoDB行锁模式兼容性列表**

| 当前锁模式/是否兼容/请求锁模式 | X    | IX   | S    | IS   |
| ------------------------------ | ---- | ---- | ---- | ---- |
| X                              | 冲突 | 冲突 | 冲突 | 冲突 |
| IX                             | 冲突 | 兼容 | 冲突 | 兼容 |
| S                              | 冲突 | 冲突 | 兼容 | 兼容 |
| IS                             | 冲突 | 兼容 | 兼容 | 兼容 |

如果一个事务请求的锁模式与当前的锁兼容，InnoDB就请求的锁授予该事务；反之，如果两者两者不兼容，该事务就要等待锁释放。



#### 行级锁的存储引擎

- InnoDB

#### 行级锁的特点

- InnoDB行锁是通过给索引上的索引项加锁来实现的，只有通过索引条件检索数据，InnoDB才使用行级
  锁，否则，InnoDB将使用表锁
- 意向锁是InnoDB自动加的，不需用户干预。对于UPDATE、DELETE、INSERT语句， InnoDB会自动始
  涉及数据集加排他锁（X）；对于管通SELECT语句，InnoDB不会加任何锁
- 在研究行锁的时候，需要将自动提交关闭，默认为开启

#### 如何加行锁

- 加锁

  ```mysql
  共享锁（S）：SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE
  排他锁（X)：SELECT * FROM table_name WHERE ... FOR UPDATE
  ```


- 释放锁

  ```mysql
  commit;
  rollback;
  ```




#### 注意事项

在InnoDB默认的隔离方式下：

- 某一条数据有排他锁时，其他人想要操作这条数据时会被锁住
- 当一条数据有排他锁时，不影响其他人访问除这条数据外的其他数据
- 修改没有索引的字段的数据时，行锁会自动升级为表锁
- 即使字段有索引，但是使用时转换了类型，那么索引失效，行锁还会转化为索引
- 写入时一定要明确指定范围，否则会锁住范围包含的所有数据，即发生“间隙锁”和“不存在锁”



#### 间隙锁（Next-Key锁）

当我们用范围条件而不是相等条件检索数据，并请求共享或排他锁时，InnoDB会给符合条件的已有数据记录的索引项加锁；对于键值在条件范围内但并不存在的记录，叫做“间隙（GAP)”，InnoDB也会对这个“间隙”加锁，这种锁机制就是所谓的间隙锁（Next-Key锁）。

#### 查询行级锁争用情况

```mysql
show status like 'innodb_row_lock%'; 
```



#### 事务并发的问题

1.脏读:事务A读取了事务B更新的数据，然后B回滚操作，那么A读到的数据是脏数据

2.不可重复读:事务A多次读取同一数据，事务B在事务A多次读取的过程中，对数据做了更新并提交，导致事务A多次读取同一数据时，结果不一致。

3.幻读:系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还是有一条记录没有改过来，就好像发生了幻觉一样，这就幻读。 

**不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读测重于新增或删除，解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表**



#### MySQL事务隔离级别

| 事务隔离级别                 | 脏读 | 不可重复读 | 幻读 | 描述                                                         |
| ---------------------------- | ---- | ---------- | ---- | :----------------------------------------------------------- |
| 读未提交（read-uncommitted） | 是   | 是         | 是   | 在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。 |
| 不可重复读（read-committed） | 否   | 是         | 是   | 这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别 也支持所谓的不可重复读（Nonrepeatable Read），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果。 |
| 可重复读（repeatable-read）  | 否   | 否         | 是   | 这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。InnoDB和Falcon存储引擎通过**多版本并发控制**（MVCC，Multiversion Concurrency Control）机制解决了该问题。 |
| 串行化（serializable）       | 否   | 否         | 否   | 这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。 |

#### 各个隔离级别的情况

- 查询隔离机别

```mysql
select @@seccion .tx_isolation;
```

- 设置隔离机别

```mysql
set session transaction isolation level read-uncommitted|read-committed|repeatable-read|serializable
```



#### 如何优化行锁

- 尽最使用较低的隔离级别；精心设计索引，并尽量使用索引访问数据，使加锁更精确，从而减少锁冲突的机会
- 选择合理的事务大小，小事务发生锁冲突的几率也更小
- 给记录集显式加锁时，最好一次性请求足够级别的锁。比如要修改数据的话，最好直接申请排他锁， 而不是先申请共享锁，修改时再请求排他锁，这样容易产生死锁
- 尽量用相等条件访问数据，这样可以避免间隙锁对并发插入的影响
- 对于一些特定的事务，可以使用表锁来提高处理速度或减少死锁的可能



## 主从复制：

### 主从复制原理

- master服务器将数据的改变记录二进制日志，当master上的数据发生改变时，则将其写入二进制日志中
- salve服务器会在一定的时间间隔内对master二进制日志进行探测其是否发生改变，如果发生改变，则开始一个I/OThread请求master二进制事件
- 同时主节点每个I/O线程启动一个dump线程，用于向其发送二进制事件，从节点保存支中继日志
- 从节点将启动SQL线程从中继日志中取出二进制日志，在本地重放，使得其数据和主节点一致
- 最后I/OThread和SQLThread将进入睡眠状态，等待下一次被唤醒

### 主从复制配置过程

1.基本要求

- 两台服务器(windows,linux,mac)
- 双方mysql版本需要一致或主低于从
- 两台服务器防火墙关闭
- 双方数据库用户要具有远程访问权限

2.主服务器配置

- 修改主服务器的MYSQL配置文件,window(my.ini),linux(my.cnf)

  ```
  #mysql唯一id
  server-id=1
  #二进制日志文件，此项为必填项，否则不能同步数据；
  log-bin="mysql-bin"
  #指定二进制错误文件
  log-error="mysql-error"
  #需要同步的数据库，如果需要同步多个数据库：
  binlog-do-db=demo
  #binlog-do-db=slaveDB1
  #binlog-do-db=slaveDB2
  #不需要同步的数据库
  binlog-ignore-db=mysql
  ```

  - MASTER_HOST：设置要连接的主服务器的IP地址
  - MASTER_USER：设置要连接的主服务器的用户名
  - MASTER_POSSWORD：设置要连接的主服务器的密码
  - MASTER_LOG_FILE：设置要连接的主服务器的bin日志的日志名称
  - MASTER_LOG_POS：设置要连接的主服务器的bin日志的记录位置

- 授权给从数据库服务器

  ```mysql
  GRANT REPLICATION SLAVE ON *.* to 'root'@'111.111.111.111' identifided by '123456';
  flush privileges;
  ```


- 重启服务器

  ```mysql
  service mysqld restart
  ```


- 查看主服务器BIN日志的信息(执行完之后记录下这两个值，然后在配置完从服务器之前不要对主服务器进行任何操作，因为每次操作数据库时这两个值都会变化)

  ```mysql
  show master status;
  ```


3.从服务器配置

- 修改从服务器的mysql配置文件，注意ID没有被别的mysql服务占用

  ```mysql
  server-id=2 #默认是1改成2
  log-bin="mysql-bin" #这本身有
  replicatioe-do-db=demo #需要同步的数据库
  replicatioe-ignore-db=mysql #不需要同步的数据库
  #read_only #只读权限
  ```


- 启动mysql服务

- 执行同步sql语句

  ```mysql
  change master to
  master_host='111.111.111.111', #设置要连接的主服务器IP地址
  master_user='root', #设置要连接的主服务器用户名
  master_password='123456', #设置要连接的主服务器密码
  master_log_file='mysql-bin.000002', #设置要连接的主服务器bin日志的日志名称
  master_log_pos=1041; #设置要连接的主服务器bin日志的记录位置
  ```


- 启动slave同步进程

  ```mysql
  start slave
  ```

- 主从同步检查

  - 查看状态

  ```mysql
  show slave status\G
  ```

  **其中Slave_IO_Running与Slave_SQL_Running的值必须都为YES**

  - 如果之前从服务器启动过需要先停止，再运行

  ```mysql
  stop slave
  ```



### 二进制文件开启：

+ log_bin

  ```mysql
  show variables like 'log_bin';
  show variables like '%log_bin%';
  show variables like 'datadir';
  ```

+ 查看所有二进制文件

  ```mysql
  show binary logs;
  
  show master logs;
  ```

+ 查看当前使用

  ```mysql
  show master status;
  ```

+ 删除二进制文件

  ```mysql
  #删除某个文件之前的二进制文件
  purge binary logs to '要删除的后一个';
  
  #清空所有二进制文件
  reset master
  
  #自动清理
  show variables like 'expire_logs_days';
  set expire_logs_days=7
  ```



## MySQL优化：（最佳左前缀）

distinct（关键词 去重！）

> 数据库优化维度：硬件、系统配置、数据库表结构、SQL及索引

### explain  SQL语句；

执行结果：

```mysql
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 | SIMPLE      | aaa   | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    2 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
```

1. ##### id:  SQL语句的查询顺序

   + id不同，执行顺序由大到小
   + id相同时，按照表中数据多少排序，表中数据越少，越早查询。数据相同时，按照书写顺序查询。

2. ##### select_type：查询每个select类型

   + simple

   + primary

   + subquery

   + union

   + union result

   + derived（派生表）：不是具体的表，一堆数据（xxx,xxx,xxx）类似于表

     ```mysql
     select aa.cname from (select cnsme,id from course) as aa;
     ```

3. ##### table：显示这一行数据是关于哪张表的

4. ##### type：在表中找到所需行的方式，又称‘访问类型’

   > all < index < range < ref < eq_ref < const < system < NULL 

   + ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行
   + index: Full Index Scan，index与ALL区别为index类型只遍历索引树，也就是查找有索引
   + range: 只检索给定范围的行，使用一个索引来选择行，用 in 索引失效
   + ref: 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值
   + eq_ref: 类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件
   + const、system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量,system是const类型的特例，当查询的表只有一行的情况下，使用system
   + NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成。

5. ##### possible_keys

> 指出MySQL能使用哪个索引在表中找到记录，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用

+ 该列完全独立于EXPLAIN输出所示的表的次序。这意味着在possible_keys中的某些键实际上不能按生成的表次序使用。
+ 如果该列是NULL，则没有相关的索引。在这种情况下，可以通过检查WHERE子句看是否它引用某些列或适合索引的列来提高你的查询性能。如果是这样，创造一个适当的索引并且再次用EXPLAIN检查查询

 

6. ##### Key

>  key列显示MySQL实际决定使用的键（索引）

+ 如果没有选择索引，键是NULL。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。

 

7. ##### key_len

> 表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）

+ 不损失精确性的情况下，长度越短越好 

 

8. ##### ref

> 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

 

9. ##### rows

> 表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数

10. **Extra** 

> 该列包含MySQL解决查询的详细信息,有以下几种情况：

+ Using where:列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤
+ Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询
+ Using filesort：MySQL中无法利用索引完成的排序操作称为“文件排序”, 效率很低
+ Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。
+ Impossible where：这个值强调了where语句会导致没有符合条件的行。
+ Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行

## mysql优化方法：

1. ##### 优化工具

   + explain命令分析
   + 开启慢查询日志

2. ##### 索引优化

   + 不能将索引用作表达式的一部分，也不能是函数的参数
   + 索引不要进行类型转化，否则索引失效
   + 复合索引应该遵循左前缀策略，不要交叉使用
   + 复合索引不要用 ‘ or '关键字，否则索引失效
   + 复合索引不要用 !=  <> 或 is null (is not null)
   + 尽量不要和 in 一起使用
   + 及时删除冗余和长期不使用的索引
   + 使用  like   ’%xx%‘，尽量不要使用左边%

3. ##### 对单表优化

   + 频繁使用的字段加索引
   + 调整索引顺序
   + 删除多余索引
   + 调整查询语句顺序

4. ##### 对多表优化

   + 多表索引添加原则，小表驱动大表(小表在左边 where  小表.x=大表.y)
   + left join 给左表加索引，right join 给右表加索引

## ajax：

> js单线程异步执行

**全称：**

async javascript and xml

**要解决的问题：**

1.按需获取问题

2.页面无刷新操作数据

3.让B/S架构的软件可以有C/S架构的操作

**BS架构：**

- 优点：

  1.随时访问新功能

  2.免去用户下载

- 缺点：

  有延时，不能流畅操作，体验性不好

**ajax的基本使用方法：**

```javascript
let ajax = new XMLHttpRequest();   //构造ajax类
ajax.onload = function () {
    //ajax事件监听
};
//通过get方式：
ajax.open('get', '/home?值');   //以什么方式去获取   去那个地址获取
ajax.send();   //发送


//通过post方式：
ajax.setRequestHeader("content-type","application/x-www-form-urlencoded")  
ajax.send("值")
```

**细节：**

1、可以处理多种类型的数据

2、传递数据