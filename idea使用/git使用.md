```java
git config --global user.name "weweaq-omen"

git config --global user.email "1773465183@qq.com"

git init
```


生成密钥：
```java
ssh-keygen -t rsa -C "1773465183@qq.com"
```
![[Pasted image 20240609221320.png]]

在路径 C:\Users\weweaq\AppData\Roaming\SPB_16.6\.ssh 下生成：

![[Pasted image 20240609221435.png]]


这两个就是SSH Key的秘钥对，可以右键用记事本之类的工具打开。`id_rsa`里的内容是私钥，不能泄露出去，`id_rsa.pub`里的内容是公钥，可以放心地告诉任何人。

github的操作

![[Pasted image 20240609222543.png]]
![[Pasted image 20240609222624.png]]
`id_rsa.pub`里的内容（公钥）进行拷贝
![[Pasted image 20240609222728.png]]

### 关联远程仓库

![[Pasted image 20240609223330.png]]

这里需要复制远程仓库地址：

```bash
git remote add origin git@github.com:weweaq/obsidian.git
```

第一次需要使用加上 -u 参数，后面只需直接使用 git push就可以了；其中加上 -u 参数是为了将本地仓库与远程仓库的master分支关联起来~

```bash
git push -u origin master
```

