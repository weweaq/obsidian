## jsp与servlet的关系

![[Pasted image 20240512143354.png]]

参考：
[浅谈JSP - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/42343690)
[Tomcat外传 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/54121733)
[Servlet（上） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/65588465)

## tomcat

![[Pasted image 20240512152827.png]]
tomcat本身是一个web 服务器，html部署在了这个web服务器上，有些人会称它为Servlet/JSP容器~

在我感觉来看，tomcat就像是一个路由器，一份导游，负责把浏览器的请求进行转发引导，进入到对应的web文件中，再进行处理；其中这份导游不仅仅起到一个带路的作用，作为实现了servlet和jsp的容器，他也会进行对应的业务层处理，最后将结果写回到response中~

## 使用感受

[tomcat启动时启动窗口出现乱码的解决方案（图文教程）_调整tomcat窗口编码-CSDN博客](https://blog.csdn.net/weixin_42460596/article/details/109468258?spm=1001.2101.3001.6650.4&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-4-109468258-blog-121178218.235%5Ev43%5Epc_blog_bottom_relevance_base5&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-4-109468258-blog-121178218.235%5Ev43%5Epc_blog_bottom_relevance_base5&utm_relevant_index=9)


## 遇到的问题

[怎么选择Tomcat对应的JDK版本_tomcat10对应jdk版本-CSDN博客](https://blog.csdn.net/qq_53376718/article/details/129433997)

[IntelliJ IDEA中配置Tomcat（超详细）_idea tomcat 配置-CSDN博客](https://blog.csdn.net/Wxy971122/article/details/123508532)


[idea启动时打印CATALINA_BASE与设置的系统环境变量不一致问题_idea catalina base-CSDN博客](https://blog.csdn.net/qq_36929123/article/details/114604717)



参考：
[Tomcat就是这么简单 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/33564233) （偏向使用教程）
[Tomcat外传 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/54121733)


