
## mysql安装

### 安装

![[Pasted image 20240404211605.png]]

### 遇到的错误

[mysql 客户端SSL错误2026 (HY000)](https://www.cnblogs.com/websec80/p/17292282.html)

背景：客户端连接mysql8.x出现“ERROR 2026 (HY000): SSL connection error: unknown error number”

mysql -h 192.168.31.233 -P3306 -uroot -p

  
方案一（已过时）：  
mysql -h10.233.117.225 -P3306 -uroot -p --skip-ssl

方案二（推荐）：  
mysql -h10.233.117.225 -P3306 -uroot -p --ssl-mode=DISABLED