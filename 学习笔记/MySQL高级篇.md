# MySQL高级篇

[MySQL高级知识（一）基础](https://www.cnblogs.com/developer_chan/p/9205401.html)

[MySQL高级知识（二）join查询](https://www.cnblogs.com/developer_chan/p/9207687.html)

[MySQL高级知识（三）索引](https://www.cnblogs.com/developer_chan/p/9208404.html)



[MySQL高级应用](https://developer.aliyun.com/course/1762)

[阿里开发者学习中心]

## Linux版MySQL安装

[相关文档](https://blog.csdn.net/qq_45441466/article/details/109670194)

### 1.Linux命令

cat /etc/passwd|grep mysql

cat /etc/group|grep mysql

开启mysql：systemctl start mysqld.service

关闭mysql：systemctl stop mysqld.service

查看mysql是否启动：ps -ef|grep mysql

设置mysql密码：/usr/bin/mysqladmin -u 用户名 password 密码



阿里云mysql密码是：toot



## 关于系统服务chkconfig,systemd

[相关文档](https://blog.csdn.net/wash168/article/details/78495512)



## mysql四大目录

| 路径                   | 解释                      | 备注                                    |
| ---------------------- | ------------------------- | --------------------------------------- |
| /var/lib/mysql         | mysql数据库文件的存放路径 |                                         |
| /usr/share/mysql       | 配置文件目录              | Mysql.server命令及配置文件              |
| /usr/bin               | 相关命令目录              | Mysqladmin mysqldump等命令              |
| /etc/init.d/mysql stop | 启停相关脚本              | 与systemctl stop mysqld.service作用相同 |



## 存储引擎MyISAM和InnoDB比较

![截屏2021-02-01 上午10.55.50](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-01 上午10.55.50.png)



## mysql执行顺序

![截屏2021-02-01 上午11.17.57](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-01 上午11.17.57.png)

![截屏2021-02-01 上午11.20.47](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-01 上午11.20.47.png)

## 索引

### 1.数据库索引原理

[相关文档](https://www.cnblogs.com/aspwebchh/p/6652855.html)

**1.1.可以理解为“排好序的快速查找数据结构”**

**1.2.索引做两件事：检索和排序**



### 2.索引分类

单值索引：即一个索引只包含单个列，一个表可以有多个单列索引（建议：一张表单列索引最好不要超过5个）

唯一索引：索引列的值必须为空，但允许有空值

![复合]()索引：即一个索引包含多个列

|      | 基本语法                                                     |
| ---- | ------------------------------------------------------------ |
| 创建 | CREATE [UNIQUE] INDEX indexName ON mutable(column name(length)); |
|      | ALTER mutable ADD [UNIQUE] INDEX [indexName] ON (column name(length)); |
| 删除 | DROP INDEX [indexName] ON mytable;                           |
| 查看 | SHOW INDEX FROM table_name                                   |



**有四种方式来添加数据表的索引：**

| 代码                                                       | 说明                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| ALTER TABLE tbl_name ADD PRIMARY KEY (column_list)         | 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL |
| ALTER TABLE tbl_name ADD UNIQUE index_name (column_list)   | 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现很多次） |
| ALTER TABLE tbl_name ADD INDEX index_name (column_list)    | 添加普通索引，索引值可以出现很多次                           |
| ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list) | 该语句指定了索引为FULLTEXT，用于全文索引                     |



### 3.哪些情况需要创建索引

| 如下：                                                       |
| ------------------------------------------------------------ |
| 1.主键自动建立唯一索引                                       |
| 2.频繁作为查询条件的字段应该创建索引                         |
| 3.查询中与其他表关联的字段，外键关系建立索引                 |
| 4.单键/组合索引的选择问题，who？（高并发的情况下倾向创建组合索引） |
| 5.查询中的排序字段，排序字段若通过索引去访问将大大提高排序速度 |
| 6.查询中统计或者分组字段                                     |



### 4.哪些情况不需要创建索引

| 如下：                                                       |
| ------------------------------------------------------------ |
| 1.频繁更新的字段不适合创建索引（因为每次更新不单单是更新了记录还会更新索引） |
| 2.where条件里用不到的字段不需要创建索引                      |
| 3.经常增删改的表                                             |
| 4.表记录太少                                                 |
| 5.数据重复且分布平均的表字段，因此应该只为最经常查询和最经常排序的数据列建立索引。注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果 |



### 5.join语句的优化

两表join：select * form table1 left join table2 on table1.id=table2.id; 此时将table2中的id列设置索引，可以最优化sql（right join 同理）。

三表join：select * form table1 left join table2 on table1.id=table2.id left join table3 on table2.id=table3.id;此时将table2，table3中的id列设置索引，可以最优化sql（right join 同理）。

尽可能减少join语句中的nestedloop的循环总次数；“永远用小结果集驱动大的结果集”；（如：书类和书籍）

优先优化nestedloop的内层循环；

保证join语句中被驱动表上join条件字段已经被索引；

当无法保证被驱动表的join条件字段被索引且内存资源充足的前提下，不要太吝啬joinbuffer的设置。

查询条件包含范围查询的（如：where中有 > < between in 等）尽量不要为该列设置索引



### 6.explain

| 能干嘛：                     |
| ---------------------------- |
| 1.表的读取顺序               |
| 2.数据读取操作的操作类型     |
| 3.哪些索引可以使用           |
| 4.哪些索引被实际使用         |
| 5.表之间的引用               |
| 6.每张表有多少行被优化器查询 |

![截屏2021-02-01 下午5.24.56](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-01 下午5.24.56.png)

[相关文档](https://blog.csdn.net/lvhaizhen/article/details/90763799)



**6.1.优化using filesort**

[相关文档](https://blog.csdn.net/lijingkuan/article/details/70341176?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-3.control)



## 查询优化分析

### 1.小表驱动大表

类似嵌套循环Nested Loop

![截屏2021-02-03 上午11.08.13](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 上午11.08.13.png)



### 2.提高order by 的速度

![IMG_0099](/Users/wangjingyu/mac_study_note/学习笔记/picture/IMG_0099.PNG)

![IMG_6EE88501F3FB-1](/Users/wangjingyu/mac_study_note/学习笔记/picture/IMG_6EE88501F3FB-1.jpeg)



### 3.慢查询日志

**日志分析工具mysqldumpslow工作常用参考**

![IMG_0101](/Users/wangjingyu/mac_study_note/学习笔记/picture/IMG_0101.PNG)



### 4.批量插入数据脚本

**开启存储函数创建（只有设置这个才能创建或修改存储函数）**

![截屏2021-02-03 下午8.00.00](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午8.00.00.png)

![截屏2021-02-03 下午8.00.12](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午8.00.12.png)

![截屏2021-02-03 下午8.00.23](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午8.00.23.png)

```sql
CREATE TABLE `dept` (
`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
`deptno` MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
`dname` VARCHAR(20) NOT NULL DEFAULT "",
`loc` varchar(13) NOT NULL DEFAULT ""
) ENGINE = InnoDB DEFAULT CHARSET = GBK;

CREATE TABLE `emp` (
`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
`empno` MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
`ename` VARCHAR(20) NOT NULL DEFAULT "",
`job` VARCHAR(9) NOT NULL DEFAULT "",
`mgr` MEDIUMINT UNSIGNED NOT NULL DEFAULT 0,
`hiredate` DATE NOT NULL,
`sal` DECIMAL(7,2) NOT NULL,
`comm` DECIMAL(7,2) NOT NULL,
`deptno` MEDIUMINT UNSIGNED NOT NULL DEFAULT 0
) ENGINE = InnoDB DEFAULT CHARSET = GBK;

DELIMITER $$
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255)
BEGIN
	DECLARE chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwsyz';
	DECLARE return_str VARCHAR(255) DEFAULT '';
	DECLARE i INT DEFAULT 0;
	WHILE i<n DO
	SET return_str = CONCAT(return_str,SUBSTRING(chars_str,FLOOR(1+RAND()*52),1));
	SET i = i+1;
	END WHILE;
	RETURN return_str;
END $$

DELIMITER $$
CREATE FUNCTION rand_num() RETURNS INT(5)
BEGIN
	DECLARE i INT DEFAULT 0;
	SET i = FLOOR(100+RAND()*10);
	RETURN i;
END $$

DELIMITER $$
CREATE PROCEDURE insert_emp(IN START INT(10), IN max_num INT(10))
BEGIN
	DECLARE i INT DEFAULT 0;
	SET autocommit = 0;
	REPEAT
	SET i = i+1;
	INSERT INTO emp (empno,ename,job,mgr,hiredate,sal,comm,deptno) VALUES ((START+i),rand_string(6),'SALESMAN',0001,CURDATE(),2000,4000,rand_num());
	UNTIL i = max_num
	END REPEAT;
	COMMIT;
END $$

DELIMITER $$
CREATE PROCEDURE insert_dept(IN START INT(10), IN max_num INT(10))
BEGIN
	DECLARE i INT DEFAULT 0;
	SET autocommit = 0;
	REPEAT
	SET i = i+1;
	INSERT INTO dept (deptno,dname,loc) VALUES ((START+i),rand_string(10),rand_string(8));
	UNTIL i = max_num
	END REPEAT;
	COMMIT;
END $$

drop function dbo.tablefun;
```



### 5.Show profile

**5.1.开启profile**

![截屏2021-02-03 下午7.52.45](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午7.52.45.png)

![截屏2021-02-03 下午7.52.52](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午7.52.52.png)

![截屏2021-02-03 下午7.52.58](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午7.52.58.png)



**5.2.获取SQL语句的开销信息**

![截屏2021-02-03 下午8.11.08](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午8.11.08.png)

**5.3诊断具体SQL**

show profile cpu,block io for query [Query_ID];

![截屏2021-02-03 下午8.24.42](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-03 下午8.24.42.png)



## 锁

### 1.表锁（偏读）

查看表上加过的锁：show open tables;

给表加锁：lock table [mylock] read, [book] write；

取掉锁：unlock tables;

**当给某个表添加读锁，在同一个终端中对自己的修改及查看其他表都会报错；在其他终端可以查询到被锁表中的数据，但是对该表的其他操作处于阻塞状态，直到另一个终端解锁，操作才可以正常执行。**

**当给某个表添加写锁，在同一终端中对可以正常select被锁表，select其他表会报错；在其他终端select，update被锁表中的数据会处于阻塞状态，直到另一个终端解锁，操作才可以正常执行。**

**简而言之，读锁会阻塞写，但是不会阻塞读，而写锁读和写都会堵塞。**

![截屏2021-02-04 上午10.09.50](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.09.50.png)



**1.1表锁分析**

![截屏2021-02-04 上午10.17.30](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.17.30.png)

![截屏2021-02-04 上午10.19.56](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午10.19.56.png)



### 2.行锁（偏写）

![截屏2021-02-04 上午11.32.04](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 上午11.32.04.png)



**2.1行锁变表锁：当索引字段是自符类型，但是修改时where条件使用该索引时没有加 ‘ ’ 导致索引失效行锁变表锁。**



**2.2间隙锁的危害**

![截屏2021-02-04 下午12.32.49](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午12.32.49.png)

![截屏2021-02-04 下午12.32.03](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午12.32.03.png)



**2.3如何锁定一行**

![截屏2021-02-04 下午3.55.56](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午3.55.56.png)



**2.4结论**

![截屏2021-02-04 下午4.06.22](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午4.06.22.png)



**2.5行锁分析**

![截屏2021-02-04 下午4.12.37](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午4.12.37.png)

![截屏2021-02-04 下午4.16.23](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午4.16.23.png)

 

## mysql主从复制

### 1.复制的基本原理

![截屏2021-02-04 下午4.23.37](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-04 下午4.23.37.png)

