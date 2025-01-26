![[Pasted image 20241103162841.png]]
## 1. 怎么读取配置的？

在properties或者yml文件中，写Mybatis—plus相关配置时提示

![[Pasted image 20241117162842.png]]

配置的自动启动类，在项目启动时，进行自动配置
![[Pasted image 20241117163005.png]]


com.baomidou.mybatisplus.core.MybatisMapperAnnotationBuilder#parse


**mybatis的整体逻辑可以分为2块**：

1. 配置文件解析：这过程包括解析我们config的配置，以及mapper.xml文件。最终配置都会被解析到一个Configuration对象里面，后面的每个SqlSession也都会包含一个该Configuration对象实例的引用。这个Configuration里面最重要的有2个东西：

![image-20210830223002642.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f72e6daf364f4649894a9cf26eeb2708~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

- mappedStatements：存放mapper对应的sql信息
- mybatisMapperRegistry.knownMappers：存放mapper接口对应的代理类

这2个东西贯穿了mybatis的接口到sql的执行逻辑。

2. 接口的调用：我们接口调用的其实是代理的包装类，这个类也是上图mybatisMapperRegistry.knownMappers里面展示的MybatisMapperProxyFactory（mybatis是MapperProxyFactory）的getObject返回的代理MybatisMapperProxy对象。这个代理类里面的主要逻辑就是拿着该类的全限定类名，指定某个方法的时候，去Configuration的mappedStatements里面去找到对应的sql。

  


---

参考
[MybatisPlus源码详解说到 Mybatis-Plus，想要了解它的源码，就要知道Mybatis-Plus在项目中 - 掘金](https://juejin.cn/post/6844904142658338829)

[MyBatis-plus源码解析 - 郭慕荣 - 博客园](https://www.cnblogs.com/jelly12345/p/15628277.html)

[MyBatis-Plus的BaseMapper实现原理这是我参与8月更文挑战的第21天，活动详情查看：8月更文挑战 My - 掘金](https://juejin.cn/post/7002423698565103653)