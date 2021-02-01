# MySQL高级篇

[MySQL高级知识（一）基础](https://www.cnblogs.com/developer_chan/p/9205401.html)

[MySQL高级知识（二）join查询](https://www.cnblogs.com/developer_chan/p/9207687.html)

[MySQL高级知识（三）索引](https://www.cnblogs.com/developer_chan/p/9208404.html)



[MySQL高级应用](https://developer.aliyun.com/course/1762)

[阿里开发者学习中心](

## Linux版MySQL安装

[相关文档](https://blog.csdn.net/qq_45441466/article/details/109670194)

### Linux命令

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

