看网上大神攻略，想试试用vs code远程调试mysql。结果在滴滴云上用wget下载mysql官网镜像，太慢了受不了，又找不到5.7.21的源码国内镜像，只好用5.7.23代替，先看下吧。

# 源码安装
以下操作均在linux下执行
## mysql国内镜像
[搜狗国内mysql镜像](https://mirrors.sohu.com/mysql/)

## 下载安装MySQL二进制版本
```
wget https://mirrors.sohu.com/mysql/MySQL-5.7/mysql-5.7.23-linux-glibc2.12-x86_64.tar.gz
```
