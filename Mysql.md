# Mysql

### explain 分析查询语句的效率

> 开源的  

+ DCL（Database Controll Language）数据控制语言
+ DDL（Data Definition Language）数据定义语言
+ DML（Data Manipulation Language）数据操纵语言，插入、更新、删除
+ DQL（Data QueryLanguage）数据查询语言
+ TCL  事务（一个完整的事件）控制语言
+ 数据库锁
+ 主从配置
+ 数据库优化操作

#### 命令行启动/关闭：net  start/stop  MySQL

## 配置（DCL）：

+ 默认表
  + user表
  + db表
  + table_priv表，columns_priv表
  + procs_priv表

### 访问控制权限：基础操作：

1. #### 创建账户：

+ `create  user  user_account(username@hostname)  identified  by  password`

2. #### 查看用户权限：

+ `show grants for  username@hostname`

3. #### 设置权限：

+ `grant  权限  on   数据库[.表]  to  user [ identified  by  password ] [require tsl_option] [with grant_option] `可以允许改变
+ `grant  select，update，insert  on  数据库[.表]  to  user`;

4. #### 删除权限：revoke

+ `revoke  privilege_type [column_list]   on  数据库[.表]  from  user@hostname`
+ `revoke  all  privileges，grant option   on  数据库[.表]  from  user@hostname`

5. #### 允许远程连接：

+ `grant all  privileges on *. *  to  'root'@'%'  identified  by  'mysql'  with grant option；flush  privileges`

6. #### 修改密码：

+ `set password for 'root'@'lo‘calhost'=password('密码')`

+ `mysqladmin -u用户名 -p旧密码  password  新密码`

+ 忘记root密码或初始密码
  + 关闭正在运行的MySql服务

  + 打开DOS窗口，转到`mysql\bin`目录

  + 输入  `mysqld  --skip-grant-tables`    跳过权限检查

    于`mysqld –skip-grant-tables`实测在mysql8.0中已失效，现使用`mysqld --console --skip-grant-tables --shared-memory`，

  + 再开一个DOS窗口，转到`mysql\bin`目录

  + 输入`mysql`回车，如果成功，将进入mysql环境

  + 连接权限数据库：`use mysql`；

  + 改密码：`update user set password=password("123")  where  user='root'`

  + 刷新权限：`flush  privileges`

  + 退出 `quit`

7. #### 数据库备份：

+ `mysqldump  -u [username] -p[password] 库名  >  [dump_file.sql]`
+ `mysqldump  -u [username] -p[password]  --no-data  库名  >  [dump_file.sql]`  仅备份数据库结构
+ `mysqldump  -u [username] -p[password]  --no-create-info  库名  >  [dump_file.sql]`  仅备份数据
+ 导出多个数据库
  + `mysqldump  -u [username] -p[password] 库名1，库名2  >  [all_dbs_dump_file.sql]`
  + `mysqldump  -u [username] -p[password]  --all-databases  >  [all_dbs_dump_file.sql]`

8. #### 数据库维护：

+ 分析表语句
  + `analyze   table  表名`
+ 优化表
  + `optimize  table  表名`
+ 检查表
  + `check  table  表名`
+ 修复表
  + `repair  table  表名`

**登陆数据库：**

```mysql
mysql -h（服务器IP地址）localhost -u（用户名）root -p（密码）-P（服务器端MySQL端口号） -D（数据库名）
```

**查看当前用户信息：**

```mysql
select user();
select currect_user();
select user,host,db,command from information_scheng.processlist;
```



**查看有哪些库：**

```mysql
show databases;
```

**创建库：**

```mysql
create database 库名;
```

**删除库：**

```mysql
drop database 库名；
```

**进入库：**

```mysql
use 库名;
```

**创建表：**

例如-创建名为stu的表：

```mysql
mysql> create table stu(
    -> id int(10) auto_increment primary key,
    -> name varchar(255),
    -> age varchar(10),
    -> sex varchar(10))default charset=utf8; 
auto_increment  id自增
primary key  主键
default charset=utf8  设置字符集
```

**查看表的结构：**

```mysql
desc 表名;
```

**查看表的列：**

```mysql
show columns from 表名;
show columns from 表名 like '%e%'; 列名中有e的列
show columns from 表名 where Field='name';字段名为name
```

**查看表：**

```	mysql
show tables;
show full tables;
```

**向表中添加列：**

```mysql
alter table 表名 add 列名 数据类型;
```

**修改数据类型：**

```mysql
alter table 表名 modify column 要修改的数据 varchar(4);
```

**删除表中的某列：**

```mysql
alter table 表名 drop 列名;
```

**添加主键：**

```mysql
alter table 表名 add primary key (列名);
```

**删除主键：**

```mysql
alter table 表名 drop primary key;
```

**增加表中某项为唯一值的约束条件：(添加唯一/全文/普通索引)**

```mysql
alter table 表名 add unique/fulltext(5.7版本以上)/index (列名);
```

**删除索引：**

```mysql
alter table 表名 drop unique/fulltext/index 列名;
```

**修改引擎：**

```mysql
alter table 表名 engine=InnoDB;
show engines;
show create table 表名；
```

**修改自增值：**

```mysql
alter table 表名 auto_increment=1;
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
undate 表名 set sex='女' where id=1;
```

**删除信息：**

```mysql
delete from 表名 where id=2;
```

**删除表：**

```mysql
drop table 表名；
```



### 设置：

**修改密码：**

```mysql
set password for 'root'@'localhost'=password('密码');
```



## 数据定义语言（DDL）

**创建库：**

```mysql
create database [if not exists] 库名;
```

**删除库：**

```mysql
drop database [if exists] 库名；
```

### 数据类型：（数值、时间、字符）

#### 数值类型：

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

> zerofill：数值前面的位数是否需要用0填充
>
> 显示宽度：是否要将数值类型对应的位数都用0显示出来
>
> 无符号：是否需要负数

+ tinyint：1字节
  + 有符号 【-128，127】
  + 无符号 【0，255】 **unsigned [zerofill]**
+ smallint：2字节
  + 【-32768，32767】
  + 【0，65535】
+ mediumint：3字节
+ int/integer：4字节
+ bigint：8字节
+ float：4字节
+ double：8字节

#### 字符串类型：

|    类型    |           大小           |              用途               |
| :--------: | :----------------------: | :-----------------------------: |
|    CHAR    |        0-255字节         |           定长字符串            |
|  VARCHAR   |       0-65535 字节       |           变长字符串            |
|  TINYBLOB  |        0-255字节         | 不超过 255 个字符的二进制字符串 |
|  TINYTEXT  |        0-255字节         |          短文本字符串           |
|    BLOB    |       0-65 535字节       |     二进制形式的长文本数据      |
|    TEXT    |       0-65 535字节       |           长文本数据            |
| MEDIUMBLOB |     0-16 777 215字节     |  二进制形式的中等长度文本数据   |
| MEDIUMTEXT |     0-16 777 215字节     |        中等长度文本数据         |
|  LONGBLOB  |   0-4 294 967 295字节    |    二进制形式的极大文本数据     |
|  LONGTEXT  |   0-4 294 967 295字节    |          极大文本数据           |
|    enum    | 数据长度为1，则为0，1，2 |                                 |
|    set     |           集合           |                                 |

#### 日期类型：

| 类型      | 大小 (字节) | 范围                                                         | 格式                | 用途                     |
| --------- | ----------- | ------------------------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3           | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3           | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1           | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8           | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4           | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

#### 空间数据类型

### 引擎类型：

#### 查看引擎：show  engines;

#### MyISAM

> 不支持事务，不支持外键，访问速度快
>
> ##### MyISAM在磁盘上存储为3个文件：  .frm(存储表定义、表的结构)、 .MYD(存储数据)、.MYI(存储索引)

#### InnoDB

> ##### .frm(结构)、.ibdata1(数据)、.ibd(索引)数据默认存放在安装目录下的data文件夹里面

### 索引类型：

#### 主键索引：primary

+ 一个表中只能有一个
+ 主键的值是唯一的【auto_increment】

#### 唯一键：unique

+ 本字段的值不能重复
+ 一个表中可以设置多个

#### 普通的索引：index

+ 一个表中可以设置多个

#### 文本索引：fulltext  5.7版本以后

+ 文本编辑器
+ 大批文本当中有序查找内容

### 外键：

> 两个表进行关联

```mysql
【constraint  约束名】（尽量设置）
foreign key [外键名（无意义）]  （列名） references  主表（主键）
[ on delete 参数
 on update 参数]
 
 
 参数：
 	district（默认）	 cascade（关联） 
 	set null(将关联的数据设为null)	 no action（什么也不做，不能删除更新但不报错）
```

##### 删除约束：alter  table tablename  drop  foreign key 约束名；

##### 删除键：

```mysql
alter table 表名 drop key 键名;
```

##### 添加外键：

```mysql
alter table 表名 add
【constraint  约束名】（尽量设置）
foreign key [外键名（无意义）]  （列名） references  主表（主键）
```



## 数据操纵语言（DML），插入、更新、删除

#### replace：replace  into  表名 （字段）values （值）

### 更新update：

```mysql
update [low_priority] [ignore] 表名 set 
```

#### 关联更新：

#### 清空：truncate [table] 表名；自增回到初始状态；



## 日志管理：

### 日志种类：

+ ##### 错误日志

  + show global variables like 'log_error'
  + show global variables like 'log_warnings'  1为打开，0未关闭。 打开时 检测错误日志！

+ 一般查询日志

+ ##### 慢查询日志

  + 查询超时时间：long_query_time
  + 启动慢查询日志：show_query_log     [=on]
  + 日志记录文件：show_query_log_file

+ 二进制日志（主服务器）

+ 中继日志（从服务器）

+ ##### 事务日志

  + innodb_flush_log_at_try_commit:
  + 0：每秒同步，并执行磁盘flush操作
  + 1：每事务同步，并执行磁盘flush操作
  + 2：

#### MySql全局变量查询与修改：

```mysql
查询变量
show global variables [like '%log%'];

修改变量
set global variables_name=val;
```

## DQL(查询)：

```mysql
select *** from 
table_1
[inner|left|right] join table_2 on 
where
 ***(比较运算符/逻辑运算符/[not] between ** and **/like) 
group by column_1
having group_conditions
order by column_1
limit offset,length;
```

+ 比较运算符：
  + =
  + <>或！=
  + <（通常使用在数字、时间、日期）/>
  + <=/>=
+ 逻辑运算符：
  + or
  + and
  + not
+ [not]  between运算符
+ like运算符 （子句）
  + %
  + _
  + \ 转义   【或者 escape ‘$’   指定$为转义字符】
+ in （子句）
+ find_in_set(要查找的字符串，查找的字段)

#### group by：

```mysql
group by 分组的条件1,分组的条件2 ...
```

##### group by 中having子句：

聚合函数：

+ avg() 计算一组值或表达式的平均值
+ count() 计算表中的行数
+ instr() 返回子字符串在字符串中第一次出现的位置
+ sum()
+ min()
+ max()

#### order by：

> 可以指定多列排序，后一列的排序一定是在前一列排序的基础上
>
> 可以按照表达式排序 +/-/*//
>
> 自定义排序order by field (gname,'绿茶','纯净水','大米')  [desc]

```mysql
group by 排序的条件1【asc(升序，默认)|desc(降序)】,分组的条件2 ...
```

#### limit：

> 约束结果集中的行数，两个参数必须为0或正整数
>
>  
>
> 和order by 一起使用，来查找最大最小值

```mysql
limit offset（偏移量）,count(条数) ...
```

### 关联查询：

> 表与表之间有关系，通过关系查询数据

+ 交叉连接（cross join）笛卡儿积
+ 内连接（inner join ... on 条件）
+ 左连接（left join）
+ 右连接（right join）

### 联合查询：

> 把多个select语句查询的结果合并起来；
>
>  union (默认去掉重复项) 【all（不去重，查出所有）】
>
> 字段名字为第一个 select 语句的 字段；



```mysql
select * from table1
union
select * from table2;
```

### 子查询：(内部查询)

#### 分类：

+ 标量子查询：返回单一值的标量，最简单的形式（子查询结果为一条）

  ```mysql
  select * from table1 where id =/>/</>=/<=... (select id from table2 where 条件)
  ```

  

+ 列子查询：返回的结果集是N行一列【in     > any   > all】（子查询结果为多条）

  ```mysql
  select * from table1 where id in (select id from table2 where 条件)
  
  select * from table1 where id < any (select id from table2 where 条件)
  
  select * from table1 where id < all (select id from table2 where 条件)
  ```

  

+ 行子查询：返回的结果集是一行N列

  ```mysql
  select * from table1 where (id,name) = (select id,name from table2 where 条件)
  ```

  

+ 表子查询：返回的结果集是N行N列

  ```mysql
  select * from table1 where (id,name) in (select id,name from table2 where 条件)
  
  
  select * from table1 where 条件1 and exists (select * from table2 where 条件)
  ```



## 常用的内置函数：

+ #### 聚合函数：

  + count（） 不忽略NULL
  + avg（）平均数。忽略NULL
  + sum（）
  + max（）
  + min（）
  + group_concat（） 分组查询结果聚合起来，连接起来

+ #### 字符串函数：

  + concat（'1','2'）/ concat_ws（'-','1','2'）结果  12 / 1-2  即连接起来
  + left（字段名，长度） 截取
  + replace（str,old,new）替换
  + substring（str,截取位置[,截取长度]）
  + trim（[ { both | leading |trailing }  [要去掉的字符]  from] str）
  + format（格式化的数字，要舍入的小数位数 [ 表示方式 ]）

+ #### 日期和时间函数：

  ##### 返回当前日期的函数

  + curdate（）当前日期
  + now（）当前这一刻包括时间。程序执行那一瞬间的时间
  + sysdate（）当前这一刻包括时间，时时刻刻都在计算
  + sleep（）

  ##### 返回指定日期的函数

  + day（）  获取 日

  + month（） 获取 月

  + year（） 获取 年

  + week（） 获取 在一年当中的第几周

  + weekday（） 获取 星期 【0-6】

  + dayname（） 获取  星期 【英文】 默认 en_US

    ​	set @@lc_time_names='zh_CN'； 输出为 中文

  ##### 日期计算函数

  + datediff（日期1，日期2）差

  + timediff（time/datetime，time/datetime） 差

  + timestampdiff（unit，time/datetime，time/datetime）

    ###### unit：呈现的单位

    + microsecond（毫秒）
    + second
    + minutes
    + hour
    + day
    + week
    + month
    + quarter（季度）
    + year

  + date_add（开始时间，interval  数字 unit单位[可组合]）

  + date_sub（）



## 视图：

#### 创建视图：

```mysql
create view 视图名字 as select ***
```

#### 修改视图：

```mysql
alter view 视图名字 as select ***

create or replace view 视图名字 as select ***
```

##### create or replace view；

#### 删除视图：

```mysql
drop view 视图名字
```

#### 查看视图：

```mysql
show full tables;

show create view 视图名字;
```

## 临时表：

> 生命周期：数据库连接时，存在内存中，关闭数据库删除；

#### 创建临时表：

```mysql
create temporary table 表名 select ***;
```

#### 删除临时表：

```mysql
drop table 表名;
```



## 事务：

##### 支持事务的引擎：InnoDB

#### 事务的性质：

+ 原子性
+ 一致性
+ 隔离性
+ 持久性

#### 事务控制语句：

+ ##### begin / start  transaction / set autocommit=0      三种方式开启一个事务

+ ##### rollback (回到初始状态) / commit （提交）    结束事务



## 锁：

+ 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低
+ 行级锁：开销大，加锁慢；会出现死锁；锁定颗粒度最小，发生锁冲突的概率最低，并发度最高
+ 页面锁：页面锁：开销和加锁时间介于表级锁和行级锁之间；会出现死锁；锁定颗粒度介于表级锁和行级锁之间，并发度一般  

### 表级锁：

> 表级锁的存储引擎： MyISAM 、 MEMORY
>
>  
>
> 作用范围：表的级别

+ 共享锁（读锁）
+ 独占锁 / 排他锁（写锁）

 如果加了读锁，对其他用户和自己都是只读，而自己也不能更新或访问别的表

 如果加了写锁，对其他用户不可见，自己可读写，但不能更新或访问别的表

#### 如何加表锁：

```mysql
Lock table 表名 read [local(模拟并发插入行为)] ,
Lock table 表名 write [local]

Lock tables 表名1,表名2... read [local],Lock tables 表名 write [local]

Unlock tables;
```

#### 查询表级锁争用情况：

```mysql
show status like 'table%'；
show status like '%lock%'；
show processlist; 查看哪些sql在等待锁
show open tables; 当前被锁住的表及锁的次数
```

#### 并发插入：

> MyISAM存储引擎有一个系统变量 concurrent_insert , 专门以控制其并发插入的行为
>
> + 0  不允许并发插入 【never】
> + 1  表中没有空洞（即没有删除的行）,则从队尾插入【默认值 auto】
> + 2  不管表中有没有空洞，都插入。【always】(注意：要设置为2)

#### 读写锁优先级：

+ 设置写锁的最多次数
  + set  global  max_write_lock_count=1
+ 降低写操作的优先级，给读操作更高的优先级
  + set  global  low_priority_updates=1
  + set  global  sql_low_priority_updates=1

#### 设置写内存：

> 可以根据具体的业务设置读写内存

 ```mysql
max_allowed_packet=1M   #限制接收数据包的大小，大的插入和更新会被限制，导致失败
net_buffer_length=2k    #insert 语句缓存值  2k-16M
bulk_insert_buffer_size=8M   #一次性insert语句插入的大小
 ```

#### 如何优化：

+ 可以利用MyISAM存储引擎的并发插入特性，来解决应用中对同一表查询、插入的锁争用，concurrent_insert=2
+ 定期在系统空闲时间整理碎片
+ 根据业务设置读写优先级
+ 设置内存

### 行级锁：

+ 共享锁（S）

+ 排他锁（X）

+ 意向共享锁（IS）

+ 意向排他锁（IX）

  当前锁模式/是否兼容/请求锁模式	X		IX		S		IS
  			X					冲突	冲突	冲突	冲突
  			IX					冲突	兼容	冲突	兼容
  			S					冲突	冲突	兼容	兼容
  			IS					冲突	兼容	兼容	兼容

+ ##### 如果一个事务请求的锁模式与当前的锁兼容，innoDB就将请求的锁授予该事务；反之，如果两者不兼容，该事务就要等待锁释放

#### 行级锁的存储引擎  

- InnoDB

#### 行级锁的特点 

> 给某一条数据写操作时，自动加排他锁，其他用户不能操作这条数据，对这条数据没有任何权限，但不影响其他客户 对其他数据的操作。
>
> ###### 默认的隔离方式 下表的特性：
>
> - 给某一条数据写操作时，自动加排他锁，其他用户不能操作这条数据，但可以查询用户修改之前的数据，但不影响其他客户 对其他数据的操作。

- ##### InnoDB行锁是通过给索引上的索引项加锁来实现的，只有通过索引条件检索数据，InnoDB才使用行级 锁，否则，InnoDB将使用表锁

  + 即使字段加了索引，但使用时改变了**类型**，则索引失效，InnoDB使用表锁

- 意向锁是InnoDB自动加的，不需用户干预。对于UPDATE、DELETE、INSERT语句， InnoDB会自动始 涉及数据集加排他锁（X）；对于管通SELECT语句，InnoDB不会加任何锁

- 在研究行锁的时候，需要将自动提交关闭，默认为开启  

  + set autocommit=0;

- ##### 间隙锁

  + 操作数据有间隙的也会锁起来，如修改id>1的数据，会把所有id>1的数据锁起来，即使已经删除或者不存在（不能插入数据）的数据

#### 加锁：

```mysql
意向 共享锁（s）： select * from 表名  where ... lock in share mode

意向 排他锁（x）： select * from 表名  where ... for update
```

#### 释放锁：

+ rollback
+ commit

### 查看行级锁的争用情况：

```mysql
show status like 'innodb_row_lock%';
```



###### 默认的隔离方式 下表的特性：

+ 给某一条数据写操作时，自动加排他锁，其他用户不能操作这条数据，但可以查询用户修改之前的数据，但不影响其他客户 对其他数据的操作。

### 事务并发的问题：

+ 脏读：某个事务已更新一份数据，另一个事务在此时读取了同一份数据，由于某些原因，前一个RollBack了操作，则后一个事务所读取的数据就会是不正确的。 
+ 不可重复读：在一个事务的两次查询之中数据不一致，这可能是两次查询过程中间插入了一个事务更新的原有的数据。 
+ 幻读：在一个事务的两次查询中数据笔数不一致，例如有一个事务查询了几列(Row)数据，而另一个事务却在此时插入了新的几列数据，先前的事务在接下来的查询中，就会发现有几列数据是它先前所没有的。 

##### 不可重复读侧重于修改，幻读侧重于新增或删除，解决幻读需要锁表

### MySql事务隔离级别：



##### 查看隔离级别：

```mysql
select @@session.tx_isolation;
```

##### 设置隔离级别：

```[]mysql
set session transaction isolation level  read uncommitted[读未提交]/read committed[读已提交]/repeatable read[可重复读]/serializable[串行读]
```

**Read Uncommitted（读取未提交内容）**

在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）。

**Read Committed（读取提交内容/不可重复读）**

这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别 也支持所谓的不可重复读（Nonrepeatable Read），因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果。

**Repeatable Read（可重读）**

这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。

###### InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。

这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读 （Phantom Read）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻影” 行。InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。

**Serializable（可串行化）**

这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。（可串行化）**

这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。

### 优化行级锁：

+ 尽量使用较低的隔离级别，精心设置索引，并尽量使用索引访问数据，使加锁更精确，从而减少锁冲突的机会
+ 选择合理的事务大小，小事务发生锁冲突几率更小；
+ 给记录集显示加锁时，一次性请求足够级别的锁。如修改时，直接申请排他锁
+ 尽量用想等条件访问数据，避免间隙锁对并发插入的影响；
+ 对于一些特定的事务，可以使用表锁来提高处理数据和减少死锁的可能



## 主从复制：

> 灾备，读写分离
>
>  
>
> 问题：

### 主从配置过程：

1. #### 基本要求：

   + 两台服务器

   + 版本一致，若不一致，从节点版本号要高于主节点的版本号
   + 两台服务器关闭防火墙
     + sudo ufw status; 查看防火墙是否关闭
   + 都允许远程连接



2. 修改配置文件， window(my.ini)放在  [mysqld]  下

   ```mysql
   #mysql唯一id
   serve_id=1
   lod_bin='mysql_bin'
   lod_error='mysql_error'
   binlog_do_db=uek_demo
   binlog_do_db=slaveDB1
   binlog_do_db=slaveDB2
   
   binlog_ignore_db=mysql
   ```

3. 授权给从服务器

   ```mysql
   grant replication slave on *.* to 'root'@'从服务器地址' identified by '从服务器密码';
   flush privileges; 刷新权限
   ```

4. 重启主服务器

   ```mysql
   service mysqld restart
   ```

5.  

   ```mysql
   show master status;
   ```

6. 从服务器配置

   ```mysql
   serve_id=2
   lod_bin='mysql_bin'
   
   replicate_do_db=uek_demo
   
   replicate_ignore_db=mysql
   ```

7. 重启从服务器





# ajax：

> js单线程异步执行

##### 全称：

async javascript and xml

##### 要解决的问题：

1.按需获取问题

2.页面无刷新操作数据

3.让B/S架构的软件可以有C/S架构的操作

##### BS架构：

- 优点：

  1.随时访问新功能

  2.免去用户下载

- 缺点：

  有延时，不能流畅操作，体验性不好

##### CS架构：

- 必须有客户端，更新不及时

##### ajax的基本使用方法：

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

##### 细节：

1、可以处理多种类型的数据

2、传递数据



```mysql
create table (Student)表名1（         #表结构

		id int auto_increment,  #自增

		sno varchar（9） primary key， #primary指定主码

		sanme varchar（20） unique， #unique唯一值

		sage smallint，  #smallint 短整形

		ssex varchar（2），

		sdept varchar（20）

	 ）；

create table Course(
	cno varchar(4) prinmary,
	cname varchar(40),
	cpno varchar(4),
	ccredit smallint,
	foreign key(cpno) references Course(Cno)  #与Course表中Cno关联
);

create table SC(
	sno varchar(9),  #与student表中的sno关联，级联操作
	cno varchar(4),  #与course表中的cno关联，级联操作
	grade smallint,
	primary key(sno,cno),
	foreign key(sno) references Student(sno) on delete cascade on update cascade,  #删除和更新时级联操作
	foreign key(cno) references Course(cno) on delete cascade on update cascade		
)

向student表中添加 ‘sentrance’列：alter table student add sentrance date；
将student表中‘sentrance’的数据类型改为varchar： alter table student modify column sentrance varchar（4）；
删除student表中‘sentrance’列：alter table student `drop` sentraince；
增加course表中的‘cname’为唯一值约束条件：alter table course `add` unique(cname);
修改course表中的cname为cname1：alter table course change cname cname1 varchar（20）
删除基本表sc： drop table sc
```

3.信息的变更
	插入中文时需要使用命令set char set ‘gbk’;
	3-1、insert into 表名（字段） values（值）
	  例如：insert into tudent(sno,sname,ssex,sage,sdept) values(‘201215121’,’李勇’,’男’,20,’CS’);
	

```mysql
3-2、如果插入学生的属性值与表student中定义的属性顺序一致，SQL命令可以为：insert into student values(‘201215121’,’李勇’ ,20,’男’,’CS’); 否则必须按3-1执行
3-3、插入多条：
   insert into sc values(‘201215121’,’1’,92),(‘201215121’,’2’,85);	
3-4、注意，插入时，确保已插入的级联操作的数据已经在数据库中【即按序插入】
3-5、数据的更新和删除和查询
  更新：update student set sdept=‘MA’ where sno=‘2015***’（5、 将student表中学号为2015***的学生的所在系改为MA）
  删除：delete from student where sno=‘2015***’（删除学号为2015***的学生记录）
  查询：
   1. 查询某表的信息：selete * from 表名；
   2. between  and查询：
     查询年龄在17-19岁之间的学生的姓名和年龄：
	select sname，sage
	from 表名
	where sage between 17 and 19；
   3. in查询
      查询计算机（cs）学生的姓名和性别
	select sname，ssex
	from student
	where sdept in（‘cs’）；
   4. 占位符查询（%：多个，_:一个）
	 查询所有以w开头的学生的信息
	select *
	from student
	where sname like ‘w%’；
   5.连接查询
	  1. 查询选修了3号课程且成绩在85分以上的学生的学号和姓名
	select student.sno,sname
	from student,sc
	where student.sno=sc.sno and sc.cno='3' and sc.grade>85;
	  2. 查询所有学生的选课情况
	select student.*,sc.*
	from student,sc
	where student.sno=sc.sno;
   6. 子查询
 	  1. 查询与‘李勇’同在一个系的学生的所有信息
	select sno,sname,sdept
	from student
	where sdept in(
	
	select sdept
	from student
	where sname='李勇'
	);
	   2.  查询其他系中比cs系所有年龄都小的学生的姓名和年龄
	select sanme,sage
	from student 
	where sage<any(
		
	select sage
	from student
	where sdept='cs' and sdept<>'cs'
	);
   7. 统计/计数
     统计学生人数
	select count（*）
	from student【表】；
   8. group bg,order by子句的使用方法
      1.查询student表中男生和女生的人数
	select count(*)
	from student
	group bg ssex;
      2.查询选修了2号课程的学生的学号及成绩，查询结果按成绩降序排列。	
	select student.sno,sc.sno,sc.grade
	from student,sc
	where student.sno=sc.sno and con='2'
	group by grade;
```

​	

