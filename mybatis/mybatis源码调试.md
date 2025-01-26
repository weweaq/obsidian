
建表语句：
```mysql
CREATE DATABASE `mybatis`; USE `mybatis`; DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` ( `id` int(20) NOT NULL, `name` varchar(30) DEFAULT NULL, `pwd` varchar(30) DEFAULT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8; 

insert into `user`(`id`,`name`,`pwd`) values (1,'狂神','123456'),(2,'张三','abcdef'),(3,'李四','987654');
```

---

分析过程：

xml解析
![[Pasted image 20241009204124.png]]




---

测试源码：
https://github.com/weweaq/mybatis3.git

测试参考：
[【狂神说】Mybatis学习笔记（完） - hnkjdx_react - 博客园 (cnblogs.com)](https://www.cnblogs.com/hnkjdx-ssf/p/14942618.html#:~:text=MyBatis%20%E5%8F%AF%E4%BB%A5)
