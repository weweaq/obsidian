
pc 122

登陆
```mysql
mysql -u root -p123456
```

退出
```
exit
```

查看所有的库
```
show databases;
```

使用数据库
```sql
use 数据库名;
```
 
 查看当前使用的数据库
```sql
select database();
```

查看当前数据库中所有表
```sql
show tables;
```

查看表结构
```sql
desc 表名;
```

查看表的创建语句
```sql
show create table 表名;
```


```sql
select VERSION();  # 查看数据库版本

查看系统隔离级别：select @@global.tx_isolation;
查看会话隔离级别(5.0以上版本)：select @@tx_isolation;
查看会话隔离级别(8.0以上版本)：select @@transaction_isolation;
```



---
# mysql  安装

## linux服务器

<font color="#2DC26B">弹性公网IP： 120.46.38.52</font>
<font color="#2DC26B">密码：123abcABC</font>

### 安装过程

mysql安装过程：

ubuntu安装的过程中使用 `./mysqld` 命令可能会报错，提示缺少`libaio`依赖库

```bash
root@hcss-ecs-3193:/soft/mysql/mysql8.0/bin# ./mysqld --user=mysql --basedir=/soft/mysql/mysql8.0 --datadir=/soft/mysql/mysql8.0/data/ --initialize
./mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory

```

使用` apt-get install libaio1` 安装依赖库即可~ 
**这里有个1**，重新安装mysql即可

生成临时密码后，如果忘记黏贴了，可以把对应的datadir给清空，重新initialize

2024-08-09T13:54:08.850236Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: p:M_B--9)Xbt

<font color="#2DC26B">mysql 信息在 /etc/my.cnf 下配置 </font>
<font color="#2DC26B">端口 3306</font>
<font color="#2DC26B">更新mysql的密码 123456</font>


在ubuntu-18.04.3版本中，使用chkconfig命令报错：command not found，
问题原因 Ubuntu 中 chkconfig 已经被 sysv-rc-conf 所替代
参考 [Ubuntu中使用chconfig命令报错：“chkconfig: command not found“ 的解决方案-CSDN博客](https://blog.csdn.net/willingtolove/article/details/107496063)

然后直接执行这个命令：
sudo systemctl enable mysql

随后就能使用service命令启动、关闭mysql
```bash
# 启动mysql
service mysql start

# 关闭mysql
service mysql stop

# 展示mysql的状态
service mysql status
```

## 本地连接云上数据库

主要就是要注意在云上的服务器安全组中关于mysql的端口需要放开
![[Pasted image 20240813224254.png]]
参考
[本机Navicat连接华为云服务器中的mysql_navicat 怎么链接华为云云数据-CSDN博客](https://blog.csdn.net/Master_Kin/article/details/103494606)