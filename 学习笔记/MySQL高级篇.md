# MySQL高级篇

[MySQL高级知识（一）基础](https://www.cnblogs.com/developer_chan/p/9205401.html)

[MySQL高级知识（二）join查询](https://www.cnblogs.com/developer_chan/p/9207687.html)

[MySQL高级知识（三）索引](https://www.cnblogs.com/developer_chan/p/9208404.html)



[MySQL高级应用](https://developer.aliyun.com/course/1762)

[阿里开发者学习中心](

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

复合索引：即一个索引包含多个列

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



## explain

| 能干嘛：                     |
| ---------------------------- |
| 1.表的读取顺序               |
| 2.数据读取操作的操作类型     |
| 3.哪些索引可以使用           |
| 4.哪些索引被实际使用         |
| 5.表之间的引用               |
| 6.每张表有多少行被优化器查询 |

![截屏2021-02-01 下午5.24.56](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-02-01 下午5.24.56.png)

