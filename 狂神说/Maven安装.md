一、下载Maven并解压
1. Maven官网下载地址：http://maven.apache.org/download.cgi

2. 一般下载bin.zip为后缀的压缩包，下载后解压，将Maven的压缩包解压到：E:\Java\apache-maven-3.6.3


二、配置环境变量
1.添加系统变量MAVEN_HOME，变量值为E:\Java\apache-maven-3.6.3


2.在系统变量Path环境变量中添加 %MAVEN_HOME%\bin


3.验证环境变量是否配置正确
打开cmd命令行窗口输入mvn -v，如果出现Maven的版本信息，则配置成功。



三、本地仓储配置
从中央仓库下载的jar包，都会统一存放到本地仓库中。我们需要配置本地仓库的位置。

1.新建一个文件夹，名称为“mvn-repository”，路径为：E:\Java\mvn-repository


2.配置setting.xml文件
在Maven解压文件中打开conf目录下的settings.xml文件


添加下面这行语句以配置本地仓储位置
```xml
<localRepository>E:\Java\mvn-repository</localRepository>
```


四、修改Maven默认的JDK版本
1.在命令行窗口中输入java -version查看已安装的JDK版本信息


查看得知JDK版本为1.8。

2.修改Maven默认的JDK版本
打开conf目录下的settings.xml文件，在<profiles>标签下添加一个<profile>标签：

```xml
<!-- 我的jdk版本为1.8，需根据自己的jdk版本做对应修改 --!>

<profile>
    <id>JDK-1.8</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.8</jdk>
    </activation>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>
</profile>
``` 
五、设置阿里云镜像
Maven默认访问国外服务器下载包，速度很慢。配置阿里云镜像下载包会比较快。

打开conf目录下的settings.xml文件，在<mirrors>标签下添加<mirror>标签：

```xml
<mirror>  
    <id>alimaven</id>  
    <name>aliyun maven</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
    <mirrorOf>central</mirrorOf>          
</mirror>
```
