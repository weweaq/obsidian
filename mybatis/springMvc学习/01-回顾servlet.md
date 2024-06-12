2024-05-14 23:07:56
## 背景

今天尝试按照视频的教程来用一遍servlet，欢迎页面可以进去，但是访问hello页面报错，说是servlet类没有找到。

访问现象
![[Pasted image 20240514231205.png]]

![[Pasted image 20240514231310.png]]

##  排查原因

排查发现输出的文件夹下并没有输出class文件，于是怀疑是配置问题

输出目录
![[Pasted image 20240514231226.png]]

排查发现这个 artifacts的output directory 是控制了这个out的位置，而且发现out文件夹下竟然还没有相关的xml文件以及相关的jsp文件！
![[Pasted image 20240514233541.png]]

继续排查发现artifacts是来自modules这边来设置的，spring下没有设置web！
![[Pasted image 20240514233739.png]]
![[Pasted image 20240514233802.png]]

自动生成后，发现生成到了对应包的位置
![[Pasted image 20240514233843.png]]

跑起来看看，发现生成了正确的东西
![[Pasted image 20240514234007.png]]

此时访问hello的输出为：
![[Pasted image 20240514234131.png]]

这里说明类已经实现正常了，这个错不用管哦~

加上method参数就是ok了~
![[Pasted image 20240514234238.png]]

参考
[IDEA启动tomcat out目录里面的classes文件夹中java代码都没有被编译进去；spring输出的out中没有classes；出现404问题_idea,运行后的out文件中没有编写的java文件-CSDN博客](https://blog.csdn.net/weixin_45986454/article/details/137870432)  