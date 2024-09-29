
1. 新建spring工程

![[Pasted image 20240928173057.png]]

2. 新建仓库
![[Pasted image 20240928173225.png]]并复制对应的git地址
![[Pasted image 20240928173304.png]]

git@github.com:weweaq/mybatis.git

3. 打开vcs开关
![[Pasted image 20240928173347.png]]
![[Pasted image 20240928173353.png]]

![[Pasted image 20240928173818.png]]

输入刚保存的git地址，添加远端分支，需要验证则正常验证即可~
![[Pasted image 20240928173821.png]]

添加完成后，注意需要获取远端分支，当前是在本地分支
![[Pasted image 20240928175023.png]]

获取完后正常是有远端分支出现，进行分支切换（checkout），切到远端分支
![[Pasted image 20240928175958.png]]
![[Pasted image 20240928180042.png]]

测试一下空的push和update，先push再update~
![[Pasted image 20240928180322.png]]

没问题后，再push文件试试
![[Pasted image 20240928180436.png]]
![[Pasted image 20240928180444.png]]

push成功！



---
还是没有，注意需要点一下更新按钮，选一下将本地分支与远端分支对应起来

![[Pasted image 20240928175409.png]]
![[Pasted image 20240928175514.png]]
此时可以注意到，本地分支与远端分支对应起来
![[Pasted image 20240928175552.png]]

再checkout到远端分支


