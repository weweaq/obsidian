

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


---
## 使用mybatis出现的问题

今天按照例子敲了一个mybatis的例子，却一直报错，这里记录一下~

背景：

只是写了一个简单的select方法，按照要求写了对应的mapper.xml文件，却一直报错对应的xml找不到~

![[Pasted image 20241001170217.png]]

排查：

一开始觉得是路径配错了，或者是环境不纯粹，其他mapper的影响等，一一查看都没发现问题，随后去看狂神的视频，发现了问题~

实际代码运行的是编译出的target目录下的class文件，我们打开对应的文件夹去看，可以发现对应的dao文件夹下并没有对应的xml配置文件
![[Pasted image 20241001170543.png]]

原因就出在了maven的文件过滤机制

在 Maven 导出资源，默认约定资源文件夹 resources 中的资源会自动导出，但是我们往往在项目中不仅仅会把所有的资源配置文件都放在resources中，同时我们也有可能放在项目中的其他位置，那么默认的maven项目构建编译时就不会把我们其他目录下的资源配置文件导出到target目录中，就会导致我们的资源配置文件读取失败，从而导致我们的项目报错出现异常，比如说尤其我们在使用MyBatis框架时，往往Mapper.xml配置文件都会放在dao包中和dao接口类放在一起的,那么执行程序的时候，其中的xml配置文件就一定会读取失败，不会生成到maven的target目录中。

所以需要在pom中添加以下配置来约定导出哪些地方的文件：
```xml
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>

```


添加完成后，先clean原始的包，再调用方法，打的新包包含配置文件，不报这个错误了，问题解决。
![[Pasted image 20241001171341.png]]

经验:

1. 实际执行的是target，并不是自己写的文件
2. maven存在过滤机制，会把预期外的配置文件给过滤掉，编译后丢失