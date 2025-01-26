

##### 1.1 @SpringBootApplication

我们直接追踪@SpringBootApplication的源码，可以看到其实@SpringBootApplication是一个组合注解，他分别是由底下这些注解组成。

代码语言：javascript

复制

```javascript
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
```

这些注解虽然看起来很多，但是除去元注解，真正起作用的注解只有以下三个注解：

代码语言：javascript

复制

```javascript
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```

元注解：
>
元注解（Meta-Annotation）是指用于注解其他注解的注解。在Java中，元注解主要用于定义自定义注解的行为和属性。常见的元注解包括：
@Retention：指定注解的保留策略，即注解在什么级别可用。
SOURCE：注解仅保留在源代码级别，编译时会被丢弃。
CLASS：注解保留在类文件中，但运行时不可访问。
RUNTIME：注解保留在运行时，可以通过反射访问。
@Target：指定注解可以应用的目标元素类型。
TYPE：类、接口（包括注解类型）或枚举声明。
FIELD：字段、枚举常量。
METHOD：方法。
PARAMETER：参数。
CONSTRUCTOR：构造器。
LOCAL_VARIABLE：局部变量。
ANNOTATION_TYPE：注解类型。
PACKAGE：包声明。
@Documented：表示该注解应该被 javadoc 工具记录。
@Inherited：表示该注解可以被子类继承。
@Repeatable：表示该注解可以在同一个地方多次使用。
通过使用这些元注解，可以更灵活地控制自定义注解的行为和作用范围。

@ComponentScan就是默认扫描，所以我们一般都是把springboot启动类放在最外层，以便扫描所有的类。

---
参考
[Spring Boot：最全SpringBoot启动流程原理分析(全网最全最完善)-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2402940)
[华为二面：SpringBoot读取配置文件的原理是什么？加载顺序是什么？SpringBoot以其简化的配置和强大的开箱即 - 掘金](https://juejin.cn/post/7340927432319123482?searchId=202410282310159BCF4E4722CF2426C019)
