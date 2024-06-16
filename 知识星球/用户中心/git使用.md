
### 基础网络连上git

参考这个改host和刷新dns即可

https://blog.csdn.net/weixin_43804496/article/details/131475204

### 下载gitbash软件

https://registry.npmmirror.com/binary.html?path=git-for-windows/

### 配置gitbash

```bash

# 设置密钥git，然后复制到github的ssh上即可，随后即可clone

ssh-keygen -t rsa -C "1773465183@qq.com"

```


```java



git config --global user.name "weweaq-omen"

git config --global user.email "1773465183@qq.com"

git init



```

---
```bash
git clone xxx.git # 远程库拉取到本地

git clone -b xx xxx.git # 远程库的xx分支拉取到本地

git checkout xxx #  记得切换对应的分支

# 其中clone到本地后，不需要再关联远程仓库，（clone就是和对应的仓库关联）
```

---

```bash



```