```### 此资源由 58学课资源站 收集整理 ###
	想要获取完整课件资料 请访问：58xueke.com
	百万资源 畅享学习

```


# 1.DDL数据定义篇



# 2.DML数据操作篇

# DML数据操作语言

开发要重点掌握的SQL，运维熟悉就好，也就是DML数据操作语言，因为网站有各式各样的数据写入、数据读取操作，这是开发对于表的设计，是开发必须重点掌握的技能。

运维无须像开发一样，了解各种业务表的结构、设计，只需要熟悉查询数据操作。

也就是DML语句

- select
- insert
- delete
- update



```
mysql> ? Data Manipulation

```

# 1.insert插入数据

前面的DDL语句，我们只是在定义，修改数据库，数据表的结构，属性，还没数据。

DML篇来插入数据



## 先看看表字段

```
先看看表字段
mysql> desc new_students;

mysql> desc new_students;
```



## 完整字段插入数据

插入学生数据，要指定字段，然后插入数值

```

```

## 插入部分字段

- 没写的字段，会填入默认值
  - datetime没有默认值，必须传入
- id字段一般可以不写，会自动的增长+1
- 确保插入的字段，和值的顺序，是对应上的就行
- char长度不能超出，否则报错，可以修改配置文件解决该问题

## 修改datetime字段的默认值

数据库常见做法是，如用户信息录入，以当前系统时间插入即可。

```


```

当前表结构

```

```

再次插入数据

```

```

## 简写，插入数据

- 不建议跳过id添加，按顺序写
- 简写，必须按顺序写值，写全了

## shell插入数据练习

````
1.以班级学员信息，继续添加，试试SQL语句的insert

2. 有没有快捷的办法，批量插入数据？（提示，基于学生名字插入数据即可）

````

# 2.update更新数据

## 更新文杰的地址为云南

```

```

## 修改文杰的年龄为33

```

```

## 修改杨松麟的年龄、手机号、爱好

````

````



## 严重的问题

于超老师提醒，update也是危险命令，，务必加上where条件！！

否则就会出现如下的问题！！

如果没有备份的数据，就直接凉凉了。

```

```





# 3.删除表数据

## delete



### where指定条件删除数据

```

```



## 伪删除

为了有效防止delete误删除数据，可以人为限定SQL的删除逻辑

```

```

想删除数据用update，代替delete，取消delete的执行权限即可

兄弟，再次啰嗦，用update，务必小心，加上where条件。

```

```



后续的查询语句，修改为如下即可

```

```



## 面试题

```
来，说说如下命令的区别

drop table new_students;

truncate table new_students;

delete from new_students; 

区别？
```





# DQL数据查询语言

## 环境准备

```
1.导入数据 
[root@db-51 ~]#mysql -uroot -pwww.yuchaoit.cn < all_db_backup.sql 
mysql: [Warning] Using a password on the command line interface can be insecure.
```



### 数据库查看

```
select * from world.city;
```



### 简单数据查看

```
District 区域

Population 人口数统计
```

```
1.查看前十个城市的信息

2.查看上海的城市信息

3.查看上海的城市信息名，代号，人口

```



### 查看表创建语句

```
mysql> show create table country;

mysql> show create table city;
```

## select语句

```
1.查看变量的值

```



### show查询数据库服务端配置

show 查阅数据库变量的值。

```
1. 查看所有内置变量的信息

2. 查看关于端口的变量信息

3.查看服务器id的信息


4.查看关于数据库目录的信息


5.查看服务器的socket信息

6.查看关于自增

7. 修改内置变量的值

```



## 查看mysql内置函数

函数就是可以复用的一个功能代码

```
mysql> help Functions;
```

### 常见函数执行

```

```

## Where 子查询

mysql强大之处在于基于数据表，进行复杂的查询，适用于各种复杂业务场景下，对数据的查询提取。

子查询几个关键语句执行顺序如下



mysql强大之处在于基于数据表，进行复杂的查询，适用于各种复杂业务场景下，对数据的查询提取。

子查询几个关键语句执行顺序如下

```
select 列
from 表
where 条件
group by 列
having 条件
order by 列
limit 条件
```

> 提示
>
> DQL系列数据查询语句，作为后端开发重点，运维学习了解即可
>
> 运维以架构部署，提供数据库环境，保障数据库稳定性为主。



### 子查询语句顺序

```

```

### 查询city表中所有数据

```
1. 查阅所有字段

2. 查询某个字段

```

### 

### 查询城市信息

查询所有城市名，以及人口数

```
# 查询所有城市、人口数量


```

查询city表里，所有中国的城市信息

```

```

查询人口数小于500人城市信息

```

```

查询中国,人口数超过500w的所有城市信息

```

```

查询中国或者美国的城市信息

```

```

查询人口在200万到500万之间的城市所有信息

```

```

查询中国或者美国，人口数大于500万的城市

```

```

查询城市名以qing开头的城市

```

```

查询淮安市的人口数量

```

```

## Group by 聚合函数

```
1 count() 统计数量 
2 sum() 求和
3 avg() 平均数
4 max() 最大值
5 min() 最小值
6 group_concat() GROUP_CONCAT函数返回一个字符串结果，该结果由分组中的值连接组合而成。
```



### group by 分组功能原理

```
1. 按分组条件进行排序

2. 进行分组列的去重

3. 聚合函数将其他列的结果聚合
```

### group by聚合函数练习题

统计city表有多少行数据

```
select count(*) from city;
```

统计中国城市的个数

```
select count(*) from city where CountryCode='CHN';
```

统计中国总人口数

```
select sum(Population) from city where CountryCode='CHN'
```

统计每个国家的城市个数

```
SELECT countryCode,COUNT(NAME) as country_num  FROM city GROUP BY countryCode;
```

统计每个国家的总人口数

```
select CountryCode,sum(Population) as people_num from city group by CountryCode

select CountryCode,sum(Population) as people_num from city group by CountryCode

# select sum(Population) from city  where CountryCode='CHN' #175953614
```

统计中国每个省的城市个数，和城市名

```
# 统计中国每个省的城市个数，和城市名 
# district 省
select District as 省,count(name) as 城市数量 ,GROUP_CONCAT(name) as 城市名列表 from city where CountryCode='CHN' group by District
```

## having 聚合判断

当group by 排序分组后，如果还要做条件判断。

### Having 练习题

统计每个国家的总人口数，只显示人口超过1亿的国家信息

```
# 统计每个国家的总人口数，只显示人口超过1亿的国家信息
select CountryCode,sum(Population) as 人口总数 from city group by CountryCode having sum(Population) > 100000000
```

## Order by 聚合排序

查询所有城市信息，按人口数排序输出，正序输出

```
select * from city order by Population

逆序输出
select * from city order by Population desc
```

查询中国所有城市信息，且按照人口数从大到小排序输出

```
select * from city where CountryCode='chn' order by 'Population' desc
```

每个国家的总人口数，总人口超过5000w的信息,并按总人口数从大到小排序输出

```
# 每个国家的总人口数，总人口超过5000w的信息,并按总人口数从大到小排序输出
select CountryCode,sum(Population) as 人口总数 from city GROUP BY CountryCode having sum(Population) > 50000000 order by sum(Population)
```

## limit 分页查询

查询中国所有城市信息，以人口数从大到小排序，只显示前十名。

```
select * from city where CountryCode='CHN' order by Population desc limit 10;
```

查询中国所有的城市信息，并按照人口数从大到小排序输出，只显示6-10名

```
# Limit 起点,条数;
# 

写法 1
select * from city where CountryCode='CHN' order by Population desc limit 5,5

写法2
select * from city where CountryCode='CHN' order by Population desc limit 5 offset 5
```

# mysql元数据查询

## show语句

```
SHOW语句有许多形式，提供关于服务器的数据库、表、列或状态信息的信息。

SHOW语法格式：

SHOW 关键字 LIKE 'pattern'

如果对于一个给定的说明语句的语法包括像'模式'，'模式'是一个字符串，可以包含“%”和“_“通配符。该模式是有用的限制语句输出匹配的值。

本节介绍以下：


SHOW DATABASES – 显示当前所有数据库的名称

SHOW TABLES FROM db_name – 显示数据库中的所有表

SHOW ENGINES – 显示MySQL当前支持哪些存储引擎和默认存储引擎

SHOW CHARACTER SET – 显示MySQL当前支持哪些字符集

SHOW COLLATION – 显示MySQL支持字符集的排序规则

SHOW BINARY | MASTER – 显示二进制文件以及文件大小（需要开启二进制日志记录功能

SHOW BINLOG EVENTS – 显示二进制文件的执行过程

SHOW COLUMNS – 显示表的列信息（等同于DESC，需要先创建表）

SHOW CREATE DATABASES – 显示已经创建的库，创建时的语句

SHOW CREATE TABLE – 显示已经创建的表，创建时的语句

SHOW CREATE FUNTCION  – 显示已经创建的函数，创建时的语句

SHOW CREATE PROCEDURE – 显示已经创建的存储过程，创建时的语句

SHOW CREATE TRIGGER - 显示已经创建的触发器，创建时的语句

SHOW CREATE VIEW – 显示已经创建的视图，创建时的语句

SHOW CREATE EVENTS – 显示已经创建的事件，创建时的语句

SHOW ENGINE – 显示存储引擎的详细信息

SHOW WARNINGS – 显示最后一个执行语句所产生的警告信息

SHOW ERRORS – 显示最后一个执行语句所产生的错误信息

SHOW EVENTS – 显示事件信息

SHOW GRANTS – 显示一个用户所拥有的权限

SHOW PROCESSLIST – 显示系统中正在运行的所有进程，普通用户只能查看自己的进行信息

SHOW PRIVILEGES – 显示MySQL所支持的所有权限，及权限可操作的对象

SHOW PLUGINS – 显示MySQL插件信息

SHOW MASTER STATUS – 显示Master当前正在使用的二进制信息

SHOW TABLE STATUS – 显示表属性信息

SHOW INDEX – 显示表索引信息

SHOW PROCEDURE STATUS – 显示存储过程信息

SHOW FUNCTION STATUS – 显示存储函数信息

SHOW TRIGGERS – 显示触发器信息

SHOW PROFILE and SHOW PROFILES – 显示执行语句的资源使用情况

SHOW SLAVE HOSTS – 显示Master主机上已注册的复制主机列表

SHOW SLAVE STATUS – 显示Slave主机状态信息

SHOW GLOBAL | SESSION STATUS – 显示MySQL状态变量信息

SHOW GLOBAL | SESSION VARIABLES – 显示MySQL系统变量信息
```



