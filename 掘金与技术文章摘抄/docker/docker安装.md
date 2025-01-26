
经验证确实可行~
但是得在正儿八经的服务器上~

![[Pasted image 20241208104113.png]]

[【Linux】在Linux上安装Docker: 一站式指南-阿里云开发者社区](https://developer.aliyun.com/article/1457025)


---
Error response from daemon: Get "https://registry-1.docker.io/v2/": dial tcp 47.88.58.234:443: connect: connection refused

![[Pasted image 20241208105610.png]]

这个是真行！
[完美解决Docker pull时报错：https://registry-1.docker.io/v2/-CSDN博客](https://blog.csdn.net/qingzhumuqingfeng/article/details/144094325)

---

起了nginx的容器，浏览器却一直连不上？

原来是没放开安全组，放开即可~

![[Pasted image 20241208225234.png]]

![[Pasted image 20241208225122.png]]
