
### 本地VMware安装虚拟机

1. 在镜像地址下载ubuntu镜像
[清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/)
我用的是 ubuntu20

2. vmware安装虚拟机
主机名: zhangw
用户名: weweaq
密码：qwqwqwqw

### ~~~按照教程安装依赖 （这是centoros的，用不了）~~~

[技术分享 | Windows 下 MySQL 源码学习环境搭建步骤【建议收藏】-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2326834)

```bash
sudo apt-get install wget -y

wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-boost-8.0.34.tar.gz

tar zxvf mysql-boost-8.0.34.tar.gz
```

一开始没有安装yum，但是看教程都是使用的yum，所以这里装一下yum~

#### ~~~安装yum  ~~~
在linux系统中直接 `apt-get intstall yum`，会报错
```
```shell
weweaq@ubuntu:~$ sudo apt-get update
Hit:1 http://us.archive.ubuntu.com/ubuntu focal InRelease                    
Hit:2 http://security.ubuntu.com/ubuntu focal-security InRelease             
Hit:3 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease
Hit:4 http://us.archive.ubuntu.com/ubuntu focal-backports InRelease
Reading package lists... Done
weweaq@ubuntu:~$ sudo apt-get install yum
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package yum

```
直接安装没有用，看博客是需要换源 参考
[Ubuntu 安装使用yum-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1300406)
清华大学开源软件镜像站：[https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/?spm=a2c6h.12873639.article-detail.7.599c64a8iays4E)


复制这个
[ubuntu | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/?spm=a2c6h.12873639.article-detail.7.599c64a8iays4E)

```shell
# 先安装一手vim
sudo apt-get install vim 

sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

sudo vim /etc/apt/sources.list
```

（1）将文件里面的内容全部删除，将上面我们查找的镜像软件源粘贴进去！！切记：`尽量的将格式也粘贴的和上面的一样，不会出错！！`

（2）再将下面的命令(软件镜像源),也粘贴到上面文件的**第一行**！

deb http://archive.ubuntu.com/ubuntu/ trusty main universe restricted multiverse

```shell
deb http://archive.ubuntu.com/ubuntu/ trusty main universe restricted multiverse
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse

```
![[Pasted image 20240817213252.png|800]]

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install yum
```

发现报错，说缺少这几个包
![[Pasted image 20240817215226.png]]

于是，安装所需依赖即可
`sudo apt-get install python-sqlitecachec python-urlgrabber python-pycurl python2`

最后再尝试一下

`sudo apt-get install yum`

![[Pasted image 20240817215519.png]]
安装成功，看看版本
![[Pasted image 20240817215537.png]]



### vmware硬盘空间不够，进行扩容
#### 背景

在编译mysql源码之前只给虚拟机分配了20G的空间，但是实际编译过程中，发现很快20G空间就被用完了，于是需要对已分配空间的虚拟机进行扩容至50G。

#### 如何进行扩容？
参考
[VMware 虚拟机 ubuntu 20.04 硬盘扩容方法_虚拟机20.04硬盘内存扩展-CSDN博客](https://blog.csdn.net/tcjy1000/article/details/135349729)

![[Pasted image 20240817234741.png]]

![[Pasted image 20240817234923.png]]

**注意首先需要在extended那一块先resize，把unlocated的磁盘空间包括进来，这样后面的/dev/sda5才能把unlocated的磁盘空间包进来。**

![[Pasted image 20240817235209.png]]

![[Pasted image 20240817235218.png]]

大功告成！



---


### 按照ubuntu下的教程安装依赖

### 背景：

前段时间一直在看mysql相关的博客，所以对源码起了浓厚的兴趣，所以尝试通过vmware和vscode在windosw环境中搭建一套编译调试的环境~

看了一下网上的搭建教程基本杂乱无章，想要从零跟着搭建出一个完善的调试环境也不是易事，所以写下了这章搭建教程，系统性梳理好`MySQL8.0`源码编译调试的详细搭建过程。

<font color="#ff0000">注意 vmware提前预留磁盘空间 50G，编译出来的源码最起码占了 30G

如果需要扩容 请参考我的另一篇博客</font> [ubuntu20 vmware硬盘空间不够，进行扩容，实操成功！-CSDN博客](https://blog.csdn.net/qq_38762282/article/details/141301620?spm=1001.2014.3001.5502)
#### 安装依赖，源码编译

```bash
# 安装依赖
sudo apt-get install build-essential cmake bison libncurses5-dev libssl-dev pkg-config

# 下载源码
wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-boost-8.0.16.tar.gz
tar xzv -f mysql-boost-8.0.16.tar.gz

# 进入源码目录
cd mysql-8.0.16/ ; ls

# 开始编译 非常慢的过程，我在虚拟机里搞了快一个小时~
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DWITH_BOOST=boost -DFORCE_INSOURCE_BUILD=ON

# 需要加sudo，否则可能权限不够会报错
# 好在出错后，重新执行命令还能接着执行
sudo make && sudo make install

```


#### mysql初始化
```bash
# 初始化前准备 权限不够的话 就加上 sudo
groupadd mysql
useradd -g mysql mysql
mkdir -p /usr/local/mysql/data
chown -R mysql:mysql /usr/local/mysql

# 开始初始化 权限不够的话 就加上 sudo
/usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
```
 
 初始化完成，注意找个地方记着生成的临时密码
 `A temporary password is generated for root@localhost: 3=0&wjy%yW%<`

```bash
weweaq@ubuntu:/usr/local/mysql$ sudo /usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
2024-08-17T16:23:24.516295Z 0 [System] [MY-013169] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.16) initializing of server in progress as process 19010
2024-08-17T16:23:25.695438Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 3=0&wjy%yW%<
2024-08-17T16:23:26.354597Z 0 [System] [MY-013170] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.16) initializing of server has completed
```


> 写命令的时候，由于不是root用户，所以老是需要输入sudo，并且需要输入密码，非常麻烦
> 所以考虑如何不输入密码？
> 两个解决方案：
> 1. 切root用户
> 2. 将用户加入root用户组？  具体就是 `sudo visudo`, 复制`root ALL=(ALL:ALL) ALL`，将自己的用户名替换了root，放到之下面
> Ctrl+o 保存    按回车确认保存    Ctrl+x 退出
> 
> 方法1 参考
> [VMware _ Ubuntu _ root 密码是什么，怎么进入 root 账户_vmware安装ubuntu22.04完成 root密码-CSDN博客](https://blog.csdn.net/qq_45476428/article/details/133802656)
> 方法2  参考
> [ubuntu避免每次都输入sudo_ubuntu取消sudo-CSDN博客](https://blog.csdn.net/qq_34062683/article/details/124378471)
> 
> 实际方法2并不好用，所以这里使用，切成root用户执行命令 设置root用户的密码为：1111  

#### 配置mysql

在/etc目录下新建一个全局用的简单的配置文件
```bash

vim /etc/my.cnf

# 然后将以下内容复制进去 #
[client]
socket = /tmp/mysql.sock
 
[mysqld]
socket = /tmp/mysql.sock
basedir = /usr/local/mysql
datadir = /usr/local/mysql/data
```


将mysql服务注册到系统服务
```bash
# 注意看自己mysql的安装目录，如果是按照我的教程话，那就是一样的
# 之前cmake的时候定义了安装目录 /usr/local/mysql
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld
update-rc.d mysqld defaults
service mysqld start
```

将`mysql/support-files/`下的`mysql.server`复制到 `/etc/init.d/`下，并且重新命名为`mysqld`，这是为了后续能够通过 `service mysqld start` 来方便的控制mysql，start可以替换成/stop/status/restart分别控制mysql关闭，重启，查看状态~


 添加path
```bash
echo -e '# MySQL PATH\nexport PATH=/usr/local/mysql/bin:$PATH\n' >> /etc/profile
source /etc/profile
```


连接登录MySQL并修改root账户密码(需要使用之前存的临时密码)。
```mysql
mysql -uroot -p'3=0&wjy%yW%<`'

# 主要在mysql8.0中已经不可以使用 update mysql.user set password = password('xxx') where user = 'xxx';的写法了
ALTER USER 'root'@'localhost' IDENTIFIED BY '1111';
```

因为本次安装只是个人使用(非生产环境)，所以可以额外的将账户密码信息写入配置文件，以后连接直接使用`mysql`命令即可，无需再输入账户密码~


```bash
vim /etc/my.cnf
[client]
# 把这几行添加到client选项组下面 #
user = root
password = 1111
port = 3306
```

尝试登陆 直接输入mysql,  并给root用户配置外部所有host均可连接。

```sql
UPDATE `mysql`.`user` SET `Host` = '%' WHERE `User` = 'root';
FLUSH PRIVILEGES;
```

![[Pasted image 20240818181342.png]]

当出现这个界面时，mysql配置成功！

#### 配置ssh

由于我们是利用vscode通过ssh远程登陆虚拟机来调试代码，所以需要先在虚拟机上安装ssh~


```bash
# 安装并开启
sudo apt install openssh-server
sudo service ssh restart

# 开启默认端口号和prohibit-password配置项
# 虚拟机默认是不允许root用户登录的
sudo vi /etc/ssh/sshd_config
```
![[Pasted image 20240818213225.png]]

最后，重启ssh服务

`sudo service ssh restart`

接下来开始配置vscode进行调试！

### vscode配置

#### vscode配置以及连接vmware虚拟机 

首先下载vscode并安装，一路往下点
[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)

随后，下载必须的插件，c/c++与remote-ssh
![[Pasted image 20240818211317.png]]
![[Pasted image 20240818180258.png]]
![[Pasted image 20240818211240.png]]

随后，利用ssh插件远程连接虚拟机
![[Pasted image 20240818211533.png]]

其中给了具体的填写方式，如 `ssh root@xxx`, 其中`xxx`为具体虚拟机的ip，可以通过`ifconfig`命令来查看，如果没有`ifconfig`，使用apt-get安装一下即可。

![[Pasted image 20240818211803.png]]

可以看到此时我的虚拟机IP为 `192.168.113.129`，所以在vscode中填入 `ssh root@192.168.113.129`

选择第一个配置即可
![[Pasted image 20240818211932.png]]

点击connect! 开始连接！
![[Pasted image 20240818211954.png]]

连接选择`Linux`，输入密码进行连接
![[Pasted image 20240818213421.png]]

此时ubuntu的命令行已经弹出来了~说明我们已经成功连接上了虚拟机~
![[Pasted image 20240818213543.png]]

>如果连接失败，提供两个思路进行检查
>1. 虚拟机上的ssh是否开启？sshd的配置是否正常？
>2. ssh xxx@xxx 用户密码是否对应的上？

此时再选择需要打开的目录后，就可以在vscode上看到虚拟机上的代码啦！
![[Pasted image 20240818213921.png]]

不出意外的话，就是这样
![[Pasted image 20240818214011.png]]

#### vscode调试mysql8.0源码

 首先给远端安装插件

- C/C++（gdb 插件调试时使用）

装完后，左侧会如图显示：分上下两栏。上栏是你本地 Windows 上装的 VSCode 插件；下栏是你远端 ubuntu 上装的 VSCode 插件。
![[Pasted image 20240818214526.png]]

随后进入到源码的位置下，配置vscode插件
```bash
cd /home/weweaq/sqlcode/mysql-8.0.16
mkdir .vscode 
cd .vscode 
vi launch.json
```

在`launch.json`中输入

```bash
{

  "version": "0.2.0",

  "configurations": [

    {

      "name": "(gdb) 启动",

      "type": "cppdbg",

      "request": "launch",

      "program": "/usr/local/mysql/bin/mysqld",

      "args": [],

      "stopAtEntry": false,

      "cwd": "/home/weweaq/sqlcode/mysql-8.0.16",

      "environment": [],

      "externalConsole": false,

      "MIMode": "gdb",

      "setupCommands": [

        {

          "description": "为 gdb 启用整齐打印",

          "text": "-enable-pretty-printing",

          "ignoreFailures": true

        },

        {

          "description": "将反汇编风格设置为 Intel",

          "text": "-gdb-set disassembly-flavor intel",

          "ignoreFailures": true

        }

      ]

    }

  ]

}
```


保存退出后，即可尝试调试啦！冲！
![[Pasted image 20240818214930.png]]

不出意外的话应该是会报错~

```bash
2024-08-17T18:23:43.533899Z 0 [System] [MY-010116] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.16) starting as process 12916 2024-08-17T18:23:43.538507Z 0 [ERROR] [MY-010123] [Server] Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!
```


说什么mysql不可以通过root用户来启动~ 没想到还有这种限制~

仔细想来，之前我们给mysql配置的时候不是配置了mysql用户组和mysql用户吗？为什么没通过mysql用户把服务起起来？

检查 `/etc/my.cnf` 配置文件，发现竟然没有配置启动mysql的用户，于是在`mysqld`栏下配置`user=mysql`
![[Pasted image 20240818215336.png]]

报错退出后，再次尝试，服务跑起来了，但是不一会又出错了

![[Pasted image 20240818215417.png]]

仔细观察错误发现，一直在提示./ibdata1文件使用不了，提示可能有其他线程在使用

于是直接kill掉所有mysql相关的线程，重启一下mysql服务

```bash
ps aux |grep mysql  
kill  mysql进程

cd /var/lib/mysql  
删除 mysql.sock.lock 文件

 # 重启mysql
service mysqld restart
```

再次点击gdb调试，好像没有报错了~

小小试试看能否调试

在vscode中按下快捷键`ctrl+p`搜索文件`sql_parse.cc`
![[Pasted image 20240818215915.png]]

在2946行打上断点

![[Pasted image 20240818220531.png]]

咱们去服务器上执行一条select语句，看看是不是真的调试起来了

![[Pasted image 20240818220724.png]]

![[Pasted image 20240818220810.png]]

可以看到真的停下来啦！！

说明我们的调试环境配置完毕！！！

XDM安心去看源码吧 ：)

*(同时通过前一张图可以看到，在进mysql的时候，断点也停了，其实是说明在进库时，mysql也会执行select语句检查输入的账户与密码是否和库中存的相等，这也说明mysql其实默默帮我们做了很多事情~我是还要学还需要学~)**

**后续我会开一个系列专门记录我的mysql学习过程，xdm可以关注一波，一起学习！！！**

参考
[Ubuntu Linux系统MySQL8.0源码编译安装笔记_ubuntu please install the appropriate openssl deve-CSDN博客](https://blog.csdn.net/sweeper_freedoman/article/details/91345114)
[技术分享 | Windows 下 MySQL 源码学习环境搭建步骤【建议收藏】-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2326834)

---


可以 `su root` 来切换普通用户和root用户，但是没法在登陆界面直接以root用户的身份登陆进去
>报错 “it didnt work~”

问了度娘，参考以下文章，才知道默认情况是没有开启root账户的，root账户是禁用状态，需要修改 `sshd_config` 文件中将`PermitRootLogIn yes`选项注释放开 开启root访问权限
[VMware获取root权限及开启root账户的办法_虚拟机root权限怎么开启-CSDN博客](https://blog.csdn.net/bbj12345678/article/details/127667911#:~:text=1%E3%80%81%E9%A6%96%E5%85%88%E6%89%A7%E8%A1%8C%EF%BC%9A%20sudo%20passwd%20root%20%E5%91%BD%E4%BB%A4%E5%88%9D%E5%A7%8B%E5%8C%96%20root%20%E7%94%A8%E6%88%B7%E5%AF%86%E7%A0%81%202%E3%80%81%E4%BF%AE%E6%94%B9,Authentication%20%E9%83%A8%E5%88%86%E5%8E%BB%E6%8E%89%E6%B3%A8%E9%87%8A%EF%BC%9A%204%E3%80%81%E7%BC%96%E8%BE%91%E5%AE%8C%E6%AF%95%E5%90%8E%EF%BC%8C%E9%87%8D%E5%90%AFssh%E6%9C%8D%E5%8A%A1%EF%BC%8C%E6%89%A7%E8%A1%8C%E5%A6%82%E4%B8%8B%E5%91%BD%E4%BB%A4%EF%BC%9A%20sudo%20systemctl%20restart%20sshd%205%E3%80%81%E6%9C%80%E5%90%8E%E5%B0%9D%E8%AF%95%E4%BB%A5root%E7%94%A8%E6%88%B7%E7%99%BB%E9%99%86%EF%BC%8C%E6%98%BE%E7%A4%BA%E7%99%BB%E9%99%86%E6%88%90%E5%8A%9F%EF%BC%8C%E5%B9%B6%E4%B8%94%E7%94%A8%E6%88%B7%E4%B8%BAroot%E8%B4%A6%E6%88%B7%E3%80%82)

```bash

sudo vim /etc/ssh/sshd_config

sudo systemctl restart sshd
```

[VMWare虚拟机安装Ubuntu 20.04 LTS并使用root用户登陆[亲测成功]_ubuntu如何以root身份运行vmware-CSDN博客](https://blog.csdn.net/weixin_42250835/article/details/119063080)
   13  sudo vim /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
   14  sudo gedit /etc/pam.d/gdm-autologin
   15  sudo vim /etc/pam.d/gdm-autologin
   16  sudo vim /etc/pam.d/gdm-password 
   17  sudo vim /root/.profile



---

修改mysql密码为：1111

---

vscode连接

![[Pasted image 20240818013555.png]]


出错
1. ssh登陆不上

2. 没有权限
mysqld: File './binlog.index' not found (OS errno 13 - Permission denied) 2024-08-17T18:03:09.422421Z 0 [System] [MY-010116] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.16) starting as process 10337 2024-08-17T18:03:09.424493Z 0 [Warning] [MY-010091] [Server] Can't create test file /usr/local/mysql/data/ubuntu.lower-test 2024-08-17T18:03:09.424502Z 0 [Warning] [MY-010159] [Server] Setting lower_case_table_names=2 because file system for /usr/local/mysql/data/ is case insensitive 2024-08-17T18:03:09.424671Z 0 [ERROR] [MY-010119] [Server] Aborting 2024-08-17T18:03:09.425745Z 0 [System] [MY-010910] [Server] /usr/local/mysql/bin/mysqld: Shutdown complete (mysqld 8.0.16) Source distribution. [1] + Done "/usr/bin/gdb" --interpreter=mi --tty=${DbgTerm} 0<"/tmp/Microsoft-MIEngine-In-0ekqc3yh.cli" 1>"/tmp/Microsoft-MIEngine-Out-csbcmqum.owd"

所以考虑使用root来远程试试，主要看教程里也是用的root
[技术分享 | Windows 下 MySQL 源码学习环境搭建步骤【建议收藏】-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2326834)

4. root登陆不上
配置ssh

```bash
sudo apt install openssh-server


```

需要配置完才能用root通过ssh进去，之前配置的只能是虚拟机界面上换用户登陆进去，不知道仅通过安装修改ssh能否通过界面登陆进去~
[Linux虚拟机配置ssh远程连接详细步骤(保姆级教程)_虚拟机安装ssh-CSDN博客](https://blog.csdn.net/m0_64655190/article/details/130569010)


---
4.  不能使用root用户
2024-08-17T18:23:43.533899Z 0 [System] [MY-010116] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.16) starting as process 12916 2024-08-17T18:23:43.538507Z 0 [ERROR] [MY-010123] [Server] Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!

[MySQL用户管理：添加用户、授权、删除用户 - 陈树义 - 博客园 (cnblogs.com)](https://www.cnblogs.com/chanshuyi/p/mysql_user_mng.html)

ALTER USER 'zhangsan'@'%' IDENTIFIED BY '1111';


update mysql.user set password = password('1111') where user = 'zhangsan' and host = '%';
flush privileges;

![[Pasted image 20240818024115.png]]

![[Pasted image 20240818024421.png]]

起起来了，但是又出错

![[Pasted image 20240818024747.png]]

参考这个，主要就是把mysql相关的进程关掉即可~
[InnoDB: Unable to lock ./ibdata1 error: 11_could not open or create the system tablespace. if-CSDN博客](https://blog.csdn.net/A___LEi/article/details/117790859)

开始第N此gdb调试：

![[Pasted image 20240818030612.png]]

oh！！！！

至此，大功告成！！！！

感谢万能的网友！！！

感谢通意灵码！！！

从现在开始，世界在我手中！！！