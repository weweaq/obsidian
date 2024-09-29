

## 缓存

使用缓存有两个优势：
1. 查询快，高效率
2. 减轻查库的负担

### 参考

[MyBatis中文网](https://mybatis.net.cn/index.html)



---
## 第一章 实操

根据配置文件选对应的库

![[Pasted image 20240928185940.png]]

根据实体类的名字选表

![[Pasted image 20240928190143.png]]

![[Pasted image 20240928190120.png]]

一般需要在实体类上加`@TableName("xxx")`注解，指定对应的表名，此时运行正常
![[Pasted image 20240928190250.png]]
	![[Pasted image 20240928190326.png]] 

第二章 使用sqlInjector!

结合源码与这篇博客

[mybatis-plus批量插入InsertBatchSomeColumn-CSDN博客](https://blog.csdn.net/qq_35549286/article/details/113603176)
[SQL注入器 | MyBatis-Plus (baomidou.com)](https://baomidou.com/guides/sql-injector/)
[mybatis-plus-samples/mybatis-plus-sample-sql-injector at master · baomidou/mybatis-plus-samples (github.com)](https://github.com/baomidou/mybatis-plus-samples/tree/master/mybatis-plus-sample-sql-injector)

最终自己实现的长这样，实操可以，源码中的已经过期了~

![[Pasted image 20240928223236.png]]

![[Pasted image 20240928223240.png]]

---
2024-09-29 20:51:21

探索 insertBatchSomeColumn 与 sqlInject

insertBatchSomeColumn是怎么执行的？

最终语句拼接：
![[Pasted image 20240929205625.png]]
![[Pasted image 20240929205548.png]]
![[Pasted image 20240929210412.png]]


>mybatis中的主键插入策略：
>在 MyBatis 中，主键生成策略可以通过枚举来定义。以下是这些枚举值的具体含义：
AUTO (0):
	含义: 自动选择合适的主键生成策略。这通常依赖于底层数据库的支持。
	适用场景: 当数据库支持自动增长（如 MySQL 的 AUTO_INCREMENT）时，MyBatis 会自动使用相应的策略来生成主键。
NONE (1):
	含义: 不使用任何特殊的主键生成策略。
	适用场景: 当表结构不需要主键或主键由应用程序自行管理时使用。例如，如果主键是业务逻辑的一部分，而不是数据库自动生成的。
INPUT (2):
	含义: 主键值由应用程序显式提供。
	适用场景: 当主键值需要由应用程序显式设置时使用。例如，某些业务逻辑要求主键必须是特定的值。
ASSIGN_ID (3):
	含义: 自动生成一个整数类型的主键值。
	适用场景: 当需要为每个新插入的记录分配一个唯一的整数主键时使用。例如，如果数据库不支持自动增长字段，但仍然需要一个唯一的整数主键。
ASSIGN_UUID (4):
	含义: 自动生成一个 UUID（通用唯一识别码）作为主键值。
	适用场景: 当需要为每个新插入的记录分配一个全局唯一的主键值时使用。UUID 可以确保在分布式系统中主键的唯一性。
这些策略可以帮助你在不同场景下选择合适的主键生成方式，从而更好地管理和控制数据库中的数据。具体选择哪种策略取决于你的业务需求和数据库特性。


