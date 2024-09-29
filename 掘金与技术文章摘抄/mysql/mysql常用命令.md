
```mysql

# 创建schema
create schema zhangmgmt default character set utf8 collate utf8_general_ci;
```

```bash
# 在Linux中使用ll命令按文件修改时间排序，实际上ll不是一个独立的命令，它是ls -l的别名，通常用于以长列表格式列出文件详情。

# 用ll命令按文件修改时间排序，可以使用以下命令 按照文件修改时间降序排序（最新的文件排在前面）
ll -t

# 按照文件修改时间升序排序（最旧的文件排在前面） -r则表示反向排序，即升序
ll -rt
```



### 查看binlog内容

```sql
mysqlbinlog --start-datetime='2024-08-28 00:00:00' --stop-datetime='2024-08-28 23:59:59' binlog.000004
```

![[Pasted image 20240828225035.png]]

```mysql
delete from t where id = 1;  
insert into t values(1,1,1);  
update t set a = 11111 where id = 1;
```

![[Pasted image 20240828224931.png]]


[MySQL面试必备二之binlog日志 - 个人文章 - SegmentFault 思否](https://segmentfault.com/a/1190000044872117)
[MySQL Binlog 介绍 (qq.com)](https://mp.weixin.qq.com/s?__biz=MzI1NDU0MTE1NA==&mid=2247483875&idx=1&sn=2cdc232fa3036da52a826964996506a8&chksm=e9c2edeedeb564f891b34ef1e47418bbe6b8cb6dcb7f48b5fa73b15cf1d63172df1a173c75d0&scene=0&xtrack=1&key=e3977f8a79490c6345befb88d0bbf74cbdc6b508a52e61ea076c830a5b64c552def6c6ad848d4bcc7a1d21e53e30eb5c1ead33acdb97df779d0e6fa8a0fbe4bda32c04077ea0d3511bc9f9490ad0b46c&ascene=1&uin=MjI4MTc0ODEwOQ%3D%3D&devicetype=Windows+7&version=62060719&lang=zh_CN&pass_ticket=h8jyrQ71hQc872LxydZS%2F3aU1JXFbp4raQ1KvY908BcKBeSBtXFgBY9IS9ZaLEDi)
[mysql查看binlog日志 - 沧海一滴 - 博客园 (cnblogs.com)](https://www.cnblogs.com/softidea/p/12624778.html)