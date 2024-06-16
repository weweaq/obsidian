## 背景

今天连本地的数据库，一直连不上，各种报错，给我弄的一肚子火，于是我宣布以后所有的操作均需记录，并且我选择卸载干净后重装，在此记录

## 卸载mysql

0. 关闭服务
> net stop mysql（有几个服务关闭几个服务，应该可以通过 mysqld -list 去看）
1. 使用geek卸载所有和mysql相关的东西
2. 清理geek没有清理的注册表中的sql相关的东西
>HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Application/MySQL
>HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Application/MySQL
>HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Application/MySQL

通过win+R,输入regedit，进入注册表

路经:  前三个注册表中的\Services\右键查找MySQL\删除

![[Pasted image 20240609144615.png]]
1. 移除原来的MySQL服务
>mysqld -remove MySQL

[Windows10 彻底卸载 MySQL_win10mysql卸载-CSDN博客](https://blog.csdn.net/weixin_42369926/article/details/81042133)

## 安装mysql

选择serverOnly
![[注意事项（安装前必看）.jpg]]

密码 123456
![[Pasted image 20240609135422.png]]


配置环境变量：(其实不知道是不是要自己配，下次试试不自己配，是否会自己配置好)
![[Pasted image 20240609155249.png]]

![[Pasted image 20240609155145.png]]

至此，安装完成

--- 
==以下是选择自定义安装时才需要配置的，如果用的是server only，不需要执行以下操作==

建议选择安装路径
## 配置环境

1. 配置环境变量

2. 初始化

![[Pasted image 20240609140715.png]]

出错
![[Pasted image 20240609141111.png]]
![[Pasted image 20240609141050.png]]


具体的软件
> C:\Program Files\MySQL

具体的数据
> C:\ProgramData\MySQL\MySQL Server 5.7\Data


其实就是在执行完 mysqld --initialize 后，就会生成一个data文件夹，其中data下的.err文件里会有临时密码，==但是我不理解data文件夹到底是生成在哪里？哪里设置的呢？==

我的是生成在 C:\Program Files\MySQL\MySQL Server 5.7\data
![[Pasted image 20240609143021.png]]




[MySQL 5.7 安装教程（全步骤图解教程）_mysql5.7的安装教程-CSDN博客](https://blog.csdn.net/m0_49284219/article/details/121972531)
[安装Mysql时报错[Warning] TIMESTAMP with implicit DEFAULT_[warning] timestamp with implicit default value is-CSDN博客](https://blog.csdn.net/qq_40421671/article/details/136617678)


---

[https://blog.csdn.net/weixin_60975732/article/details/135059652](https://blog.csdn.net/weixin_60975732/article/details/135059652)