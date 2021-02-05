# MySQL

## ACID

事务具有4个特征，分别是原子性、一致性、隔离性和持久性，简称事务的ACID特性；

![截屏2021-02-04 上午10.29.01](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.29.01.png)

### 1.原子性（atomicity)

一个事务要么全部提交成功，要么全部失败回滚，不能只执行其中的一部分操作，这就是事务的原子性

### 2.一致性（consistency)

事务的执行不能破坏数据库数据的完整性和一致性，一个事务在执行之前和执行之后，数据库都必须处于一致性状态。

如果数据库系统在运行过程中发生故障，有些事务尚未完成就被迫中断，这些未完成的事务对数据库所作的修改有一部分已写入物理数据库，这是数据库就处于一种不正确的状态，也就是不一致的状态

### 3.隔离性（isolation）

事务的隔离性是指在并发环境中，并发的事务时相互隔离的，一个事务的执行不能不被其他事务干扰。不同的事务并发操作相同的数据时，每个事务都有各自完成的数据空间，即一个事务内部的操作及使用的数据对其他并发事务时隔离的，并发执行的各个事务之间不能相互干扰。

在标准SQL规范中，定义了4个事务隔离级别，不同的隔离级别对事务的处理不同，分别是：未授权读取，授权读取，可重复读取和串行化

3.1、读未提交（Read Uncommited），该隔离级别允许脏读取，其隔离级别最低；比如事务A和事务B同时进行，事务A在整个执行阶段，会将某数据的值从1开始一直加到10，然后进行事务提交，此时，事务B能够看到这个数据项在事务A操作过程中的所有中间值（如1变成2，2变成3等），而对这一系列的中间值的读取就是未授权读取

3.2、授权读取也称为已提交读（Read Commited），授权读取只允许获取已经提交的数据。比如事务A和事务B同时进行，事务A进行+1操作，此时，事务B无法看到这个数据项在事务A操作过程中的所有中间值，只能看到最终的10。另外，如果说有一个事务C，和事务A进行非常类似的操作，只是事务C是将数据项从10加到20，此时事务B也同样可以读取到20，即授权读取允许不可重复读取。

3.3、可重复读（Repeatable Read)

就是保证在事务处理过程中，多次读取同一个数据时，其值都和事务开始时刻是一致的，因此该事务级别禁止不可重复读取和脏读取，但是有可能出现幻影数据。所谓幻影数据，就是指同样的事务操作，在前后两个时间段内执行对同一个数据项的读取，可能出现不一致的结果。在上面的例子中，可重复读取隔离级别能够保证事务B在第一次事务操作过程中，始终对数据项读取到1，但是在下一次事务操作中，即使事务B（注意，事务名字虽然相同，但是指的是另一个事务操作）采用同样的查询方式，就可能读取到10或20；

3.4、串行化

是最严格的事务隔离级别，它要求所有事务被串行执行，即事务只能一个接一个的进行处理，不能并发执行。

### 4.持久性（durability）

一旦事务提交，那么它对数据库中的对应数据的状态的变更就会永久保存到数据库中。--即使发生系统崩溃或机器宕机等故障，只要数据库能够重新启动，那么一定能够将其恢复到事务成功结束的状态



## 并发事物处理带来的问题

### 1.更新丢失

![截屏2021-02-04 上午10.31.53](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.31.53.png)



### 2.脏读

![截屏2021-02-04 上午10.33.01](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.33.01.png)



### 3.幻读

![截屏2021-02-04 上午10.34.06](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.34.06.png)



### 4.不可重复读

![截屏2021-02-04 上午10.34.44](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.34.44.png)



## 事物隔离级别

![截屏2021-02-04 上午10.50.51](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.50.51.png)



## MySQL时间相关函数及应用

### 1.Mysql INTERVAL函数

**1.1相关文档**

[相关文档](https://blog.csdn.net/reee112/article/details/83818526)

INTERVAL:代表的是时间间隔

MySQL中的时间间隔类型有如下几种:

![20181107103930958](E:\git_workspace\笔记\picture\20181107103930958.jpg)

**1.2 利用INTERVAL做时间的加减法**

示例：

加法:

```sql
SQL>SELECT DATE '2018-11-01' + INTERVAL '10 11' DAY_HOUR;
```

结果:2018-11-11 11:00:00

减法:

```sql
SQL> select date '2018-11-11 11:00:00' -INTERVAL '10 11' DAY_HOUR ;
```

结果:2018-11-01 00:00:00

### 2.MySQL EXTRACT函数

**2.1相关文档**

[相关文档](https://blog.csdn.net/reee112/article/details/83818526)

EXTRACT():用于返回日期/时间的单独部分，比如年、月、日、小时、分钟等等

```sql
格式:EXTRACT(unit FROM date)
```

unit取值表:

![20181107135725638](E:\git_workspace\笔记\picture\20181107135725638.jpg)

### 3.MySQL DATE_SUB函数

**3.1相关文档**

[相关文档](https://blog.csdn.net/reee112/article/details/83818526)

DATE_SUB():从日期减去指定的时间间隔

DATE_SUB有个相反的函数DATE_ADD(),但因为业务上用DATE_SUB
的情况多,而且DATE_SUB也能实现增加时间间隔的功能,所以我大多用
DATE_SUB.

```
格式: DATE_SUB(日期, 时间间隔,  时间间隔类型type)
```

时间间隔即interval函数

间隔类型type可取:

![20181107141104934](E:\git_workspace\笔记\picture\20181107141104934.jpg)

### 4.MySQL NOW() 函数

NOW()： 返回当前的日期和时间

```sql
SELECT NOW(),CURDATE(),CURTIME()
```

结果：

![微信图片_20201224120201](E:\git_workspace\笔记\picture\微信图片_20201224120201.png)

```sql
select date(NOW())
```

结果：2020-12-24

### 5.Mysql to_days()函数

**5.1相关文档**

[相关文档](https://blog.csdn.net/qq_37512323/article/details/84763256)

to_days()：获取某个时间范围内的数据

利用to_days函数查询今天的数据：

select * from 表名 where to_days(时间字段名) = to_days(now());

### 6.MySQL DATE_FORMAT() 函数

**6.1相关文档**

[相关文档](https://www.w3school.com.cn/sql/func_date_format.asp)

DATE_FORMAT(): 函数用于以不同的格式显示日期/时间数据。

```sql
DATE_FORMAT (CREATE_TIME,'%Y-%m-%d')
```

### 例1：MySQL 根据表中时间字段查询近6天数据新增个数（每天数据新增个数）

```sql
 SELECT  DATE_FORMAT (NOW(),'%Y-%m-%d') DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') = date(NOW())
        UNION
        SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 1 DAY ) DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') =
        DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 1 DAY )
        UNION
        SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 2 DAY ) DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') =
        DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 2 DAY )
        UNION
        SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 3 DAY ) DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') =
        DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 3 DAY )
        UNION
        SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 4 DAY ) DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') =
        DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 4 DAY )
        UNION
        SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 5 DAY ) DATE, COUNT(1) COUNT FROM ele_alarm WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') =
        DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 5 DAY )
```

结果：

![微信图片_20201224174739](E:\git_workspace\笔记\picture\微信图片_20201224174739.png)

### 例2：MySQL 根据表中时间字段查询近6天数据总数（每天数据总数）

```sql
SELECT  DATE_FORMAT (NOW(),'%Y-%m-%d') time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <= date(NOW())
UNION
SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 1 DAY ) time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <=
DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 1 DAY ) 
UNION
SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 2 DAY ) time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <=
DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 2 DAY ) 
UNION
SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 3 DAY ) time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <=
DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 3 DAY ) 
UNION
SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 4 DAY ) time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <=
DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 4 DAY ) 
UNION
SELECT DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 5 DAY ) time, COUNT(1) COUNT FROM ele_target WHERE DATE_FORMAT (CREATE_TIME,'%Y-%m-%d') <=
DATE_SUB( DATE_FORMAT (NOW(),'%Y-%m-%d'), INTERVAL 5 DAY )
```

结果：

![微信图片_20201224180024](E:\git_workspace\笔记\picture\微信图片_20201224180024.png)



## UNION

**1.1相关文档**

[相关文档](https://www.runoob.com/sql/sql-union.html)

SQL UNION 操作符合并两个或多个 SELECT 语句的结果。请注意，UNION 内部的每个 SELECT 语句必须拥有相同

数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。

> 默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。



## EXISTS和NOT EXISTS

**1.1相关文档**

[相关文档](https://www.cnblogs.com/xuanhai/p/5810918.html)



## MySQL 分组查询 GROUP BY

### 1.GROUP_CONCAT()

**1.1相关文档**

[相关文档](https://blog.csdn.net/u012620150/article/details/81945004)

GROUP_CONCAT()：将group by产生的同一个分组中的值连接起来，返回一个字符串结果。



## MySQL 字符串相关函数及应用

### 1.截取字符串SUBSTR()

**1.1相关文档**

[相关文档](https://www.cnblogs.com/clwydjgs/p/9324255.html)

```sql
SUBSTR(str FROM pos FOR len)
str表示被截取的字段
pos表示开始的下标
len表示截取的长度
```

### 2.拼接字符串CONCAT()

**2.1相关文档**

[相关文档](https://www.cnblogs.com/aiyr/p/6830593.html)



## 深入了解MySQL存储引擎-------InnoDB

### 1.简介

InnoDB，是MySQL的数据库引擎之一，现为MySQL的默认存储引擎

### 2.相关文档

[相关文档](https://blog.csdn.net/m0_37962600/article/details/81005191)



## MySQL解决批量处理问题

### 1.批量插入

[sql优化：使用batch insert解决MySQL的insert吞吐量问题](https://blog.csdn.net/willson_l/article/details/73558666)

```xml
<insert id="batchInsertDataPopulation" parameterType="list">
    insert into data_population (NAME,AGE,SEX,WORK,BIRTHDAY,STATE,ADDRESS)
    values
    <foreach collection="list" item="item" separator=",">
        (#{item.name},#{item.age},#{item.sex},#{item.work},#{item.birthday},#{item.state},#	{item.address})
    </foreach>

</insert>
```

### 2.批量修改

[方法1：mysql批量修改数据](https://blog.csdn.net/justry_deng/article/details/83064354)

```sql
UPDATE ele_target_status ets INNER JOIN (
SELECT ID,NAME,ROUND(RAND()*10+90,2) PR from ele_target 
WHERE LEVEL_TYPE=3
UNION
SELECT ID,NAME,ROUND(RAND()*30+50,2) PR from ele_target 
WHERE LEVEL_TYPE=2
UNION
SELECT ID,NAME,ROUND(RAND()*50,2) PR from ele_target 
WHERE LEVEL_TYPE=1) el ON ets.TARGET_ID = el.ID SET ets.ATTACK_PROB=el.PR
```

方法2：将所有行的指定字段修改为同一值

```sql
UPDATE ele_target_status SET WAR_EREMY='美' WHERE TARGET_ID LIKE '%%'
```

https://developer.aliyun.com/learning?spm=5176.19720258.J_8058803260.1225.79e62c4a0DG0vH)



## 其他

### 1.MySQL CASE WHEN 用法

**1.1相关文档**

[相关文档](https://www.cnblogs.com/chenduzizhong/p/9590741.html)