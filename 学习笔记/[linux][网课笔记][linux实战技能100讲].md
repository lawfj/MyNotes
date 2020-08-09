# 第二章：系统操作篇 (18讲)
## 08 | 万能的帮助命令：man、help、info
### man
man：有问题找男人。
man ls
man 7 man
```
The table below shows the section numbers of the manual followed by the types of pages they contain.

1   Executable programs or shell commands
2   System calls (functions provided by the kernel)
3   Library calls (functions within program libraries)
4   Special files (usually found in /dev)
5   File formats and conventions eg /etc/passwd
6   Games
7   Miscellaneous (including macro packages and conventions), e.g. man(7), groff(7)
8   System administration commands (usually only for root)
9   Kernel routines [Non standard]

```
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596949041868-1596949041933.png)
### help命令与--help选项
help：内部命令使用help帮助
内部命令：shell（命令解释器）自带的命令成为内部命令，其他的是外部命令。内部命令没有实体，说人话就是在linux中找不到这些命令对应的文件。
```
help cd
```
外部命令获取help帮助
```
ls --help
```
### tpye
使用type区分命令时内部还是外部命令还是别名还是啥啥啥
```shell
[root@10-255-0-194 ~]# type ls
ls is aliased to `ls --color=auto'
[root@10-255-0-194 ~]# type cd
cd is a shell builtin
[root@10-255-0-194 ~]# type ftp
ftp is /usr/bin/ftp
```

"--help"选项并不是一个“独立”的工具。作为一种命令的选项，它可以用来修改工具或者命令的工作方式。命令的选项通常由一个或两个连字符后跟一个或多个字母来指定。选项出现在所调用的工具名后，用空格隔开。工具的其它参数都跟在选项后，也用空格隔开。"--help"选项就像分页程序"| less"一样，它所提供的是一种快捷、高效的帮助。

Man和info就像两个集合，它们有一个交集部分，但与man相比，info工具可显示更完整的最新的GNU工具信息。若man页包含的某个工具的概要信息在info中也有介绍，那么man页中会有“请参考info页更详细内容”的字样。通常情况下，man工具显示的非GNU工具的信息是唯一的，而info工具显示的非GNU工具的信息是man页内容的副本补充。
## 09 | 初识pwd和ls命令
**在linux的世界里，一切皆文件！**
1. 文件查看
2. 目录文件的创建与删除
3. 通配符
4. 文件操作
5. 文本内容查看
### pwd
显示当前的目录名称
```
[root@10-255-0-194 profile.d]# pwd
/etc/profile.d
```
### cd
更改当前的操作目录
绝对路径、相对路径
## 10 | 详解ls命令
### ls
ls 查看当前⽬录下的⽂件
• ls [选项，选项… ] 参数 … 
常⽤参数：
• -l ⻓格式显示⽂件
• -a 显示隐藏⽂件
• -r 逆序显示
• -t 按照时间顺序显示
• -R 递归显示
• -d 目录 仅显示该目录的属性，而不进入目录显示里面的文件
常用组合
```
ls -alF
ls -lFR
ls -r #按名称倒叙
ls -t #按时间倒叙
ls #显示当前目录下的文件
ls p* #使用通配符，显示当前目录下以P开头的文件
ls a b #显示多个目录下的文件
[root@10-255-0-194 ~]# ls / /root
/:
app  bin  boot  dev  etc  home  lib  lib64  libexec  media  mnt  opt  proc  root  run  sbin  share  srv  sys  tmp  usr  var

/root:
–a  go  –p
```
输出解释
```
[root@10-255-0-194 ~]# ls -alF
total 44
dr-xr-x---.  7 root root  216 Aug  9 14:14 ./
dr-xr-xr-x. 20 root root  263 Aug  9 14:20 ../
-rw-r--r--   1 root root    8 Jul 29 18:14 –a
-rw-------   1 root root 4860 Aug  7 18:19 .bash_history
-rw-r--r--.  1 root root   18 Dec 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 29  2013 .bash_profile
-rw-r--r--.  1 root root  176 Dec 29  2013 .bashrc
drwx------   4 root root   33 Aug  6 17:24 .cache/
-rw-r--r--.  1 root root  100 Dec 29  2013 .cshrc
drwxr-xr-x   4 root root   28 Aug  6 17:36 go/
|权限|文件数|属主|属组|大小(KB)|最近修改时间|文件名

.开头的文件是隐藏文件，要用-a参数才会显示
```
切换用户
su - mysql
## 11 | 详解cd命令
cd - #切换到最近一次所在目录
## 12 | 创建和删除目录
### mkdir
mkdir 文件夹1 [文件夹2 文件夹3 ...]
mkdir -p /a/b/c/d #如果没有对应的路径，创建d文件夹的同时，创建/a、/a/b、/a/b/c。建立多级目录
### rmdir
rmdir可以删除**空的**文件夹
### rm -r
rm -r可以删除文件夹，不管空不空
rm -rf 忽略提示
rm 文件1 [文件2 ...]
## 13 | 复制和移动目录
### cp
cp 文件名 目录 #复制文件名到目录
cp -r 目录1 目录2 #复制目录1到目录2
-v #显示复制的进度
-p #保留最后修改时间。普通的cp，复制后的文件的最后修改时间不是原文件的时间
-a #比-p更厉害，保留权限、属主、属组、最后修改时间
### touch
新建文件
### mv
移动及重命名
mv 文件1 目录1 #将文件1移动到目录1下 
mv 文件1 文件2 #将文件1重命名为文件2
mv 目录1 目录2 #将目录1重命名为目录2
### 通配符
• 定义：shell 内建的符号
• ⽤途：操作多个相似（有简单规律）的⽂件
#### 常⽤通配符
• * 匹配任何字符串
• ? 匹配1个字符串
• [xyz] 匹配xyz任意⼀个字符
• [a-z] 匹配⼀个范围
• [!xyz] 或 [^xyz] 不匹配
## 14 | 如何在Linux下进行文本查看
### cat
将⽂本内容显示到终端
### head
查看⽂件开头
head 文件名 #默认显示前10行
head -5 文件名 #显示前5行
### tail
查看⽂件结尾
tail 文件名 #默认查看最后10行
tail -5 文件名 #查看最后5行
-f #⽂件内容更新后，显示信息同步更新
-20f #先显示最后20行，然后等待文件内容更新
### wc
统计⽂件内容信息
wc -l #显示行数
more、less命令，分页显示
## 15 | 打包压缩和解压缩
最早的 Linux 备份介质是磁带，使⽤的命令是 tar
可以打包后的磁带⽂件进⾏压缩储存，压缩的命令是 gzip 和 bzip2
经常使⽤的扩展名是 .tar.gz .tar.bz2 .tgz
bz2压缩率大，但速度慢。gz压缩没前者厉害，但速度快
### tar 打包命令
• 常⽤参数（注，可以不加-号）
• c 打包
• x 解包
• f 指定操作类型为⽂件
• C 目录 打包时指定目录下的文件；解压时指定解压到的目录

打包
tar -cf file.tar file1
tar -zcf file.tar.gz file1
tar -jcf file.tar.bz2 file1
tar -zcvf file.tar.gz file1 file2 #将file1和file2打包压缩到file.tar.gz
tar -jcvf file.tar.bz2 file1
tar -zxcf file.tar.gz -C /tmp/file* #tar不加参数只能打包当前目录的文件，要用-C指定其他目录的文件
解包
tar -xf file.tar
tar -zxf file.tar.gz
tar -jxf file.tar.bz2
tar -zxvf file.tar.gz
tar -jxvf file.tar.bz2
tar -zxvf file.tar.gz -C /tmp/file/ #将file.tar.gz解压到/tmp/file/文件夹下。不加-C则解压到当前目录
下面两个格式为了方面在网络上进行传播，进行了简写而已
.tgz=.tar.gz
.tbz2=tar.bz2
## 16 | Vim的四种模式
### 四种模式
正常模式 (Normal-mode)
插⼊模式 (Insert-mode) 
命令模式 (Command-mode) 
可视模式 (Visual-mode)
## 17 | Vim的正常模式
## 18 | Vim的命令模式
## 19 | Vim的可视模式
## 20 | 用户和用户组管理及密码管理
超级管理员
管理员
普通用户
普通用户无法进入其他用户的家目录，不能读取编辑一些系统配置文件，如/etc/passwd，除非root授权给他
useradd 新建⽤户，同时创建家目录
userdel 删除⽤户
passwd 修改⽤户密码
usermod 修改⽤户属性
chage 修改⽤户属性
id #查看当前用户的信息
id 用户名 #查看对应用户的信息
```
[root@10-255-0-194 ~]# id
uid=0(root) gid=0(root) groups=0(root)
[root@10-255-0-194 ~]# id ftpuser
uid=1002(ftpuser) gid=1002(ftpuser) groups=1002(ftpuser)
```
### /etc/passwd和/etc/shadow
前者是用户信息，后者是密码信息

useradd没有指定用户组的话，会默认创建同名用户组
只有root用户有创建用户的权限，普通用户不行
root用户可以用passwd修改自己或者其他用户的密码，普通用户只能修改自己的密码

useradd 用户名
useradd -g 用户组名 用户名 #创建用户的同时，指定用户组

userdel
userdel 用户名 #删除用户，不删除家目录
userdel -r 用户名 #连家目录也铲掉
```
[root@10-255-0-194 ~]# useradd user1
[root@10-255-0-194 ~]# id user1
uid=1003(user1) gid=1003(user1) groups=1003(user1)
[root@10-255-0-194 ~]# l /home/
total 0
drwxr-xr-x.  6 root     root      64 Aug  9 16:04 ./
dr-xr-xr-x. 20 root     root     263 Aug  9 16:05 ../
drwx------   3 centos   centos    74 Jul 29 16:38 centos/
drwx------   2 dc2-user dc2-user  62 Dec 11  2019 dc2-user/
drwx------   3 ftpuser  ftpuser   73 Jul 29 18:05 ftpuser/
drwx------   2 user1    user1     62 Aug  9 16:04 user1/
[root@10-255-0-194 ~]# ls /home
centos  dc2-user  ftpuser  user1
[root@10-255-0-194 ~]# userdel user1
[root@10-255-0-194 ~]# l /home/
total 0
drwxr-xr-x.  6 root     root      64 Aug  9 16:04 ./
dr-xr-xr-x. 20 root     root     263 Aug  9 16:05 ../
drwx------   3 centos   centos    74 Jul 29 16:38 centos/
drwx------   2 dc2-user dc2-user  62 Dec 11  2019 dc2-user/
drwx------   3 ftpuser  ftpuser   73 Jul 29 18:05 ftpuser/
drwx------   2     1003     1003  62 Aug  9 16:04 user1/  <---可以看到，这个目录的属主和属组为uid，变成了无主文件，除了root用户谁都无法访问
```
usermod -d /home/w1 w #修改w用户的家目录为/home/w1
usermod -g group1 user1 #修改user1的属主为group1
change #修改用户的密码过期时间

### 切换用户
su uer1 #仅仅切换到user1
su - user1 #切换到user1用户且切换到user1的家目录
root切换其他用户不需要输入密码，而其他用户且其他用户，根据/etc/shadow中的配置，看需不需要密码
exit或ctrl+d退出当前用户登录
## 21 | su和sudo命令的区别和使用方法
有些命令普通用户是执行不了的，比如重启。如果普通用户切换到root用户，需要知道root的密码，这样就存在很大的风险隐患。这种场景可以让root给普通用户指定权限，普通用户在命令前加上sudo就行。
• su 切换⽤户
• su - USERNAME 使⽤ login shell ⽅式切换⽤户
• sudo 以其他⽤户身份执⾏命令
• visudo 设置需要使⽤ sudo 的⽤户（组）

root授权user1重启命令权限
```
root执行visudo，在打开的文件中，到最下方添加：
user1 ALL=/sbin/shutdown -c

切换到user1测试一下语句既可
sudo /sbin/shutdown -c
```
## 22 | 用户和用户组的配置文件介绍
有三个文件：
/etc/passwd
/etc/shadow	保存用户及用户密码相关信息
/etc/group
注：这几个文件都只能用root用户查看、修改
### 文件解析
```
/etc/passwd

ftpuser:x:1002:1002::/home/ftpuser:/bin/bash
用户名称:是否使用密码进行验证:用户id(uid):用户组id(gid):备注:家目录:用户登录的命令解释器
最后一个有/sbin/nologin，代表不给登录终端。
添加用户的另一个方法就是按照格式在/etc/passwd中添加用户
```
```
/etc/shawod

ftpuser:$6$2reS2mo/$C8JYmOTbWlLgmM7HVHMd6Sqz32O/WJIVra/6FmeuE2X7JDo64ghslGvefA7sxzreO1g4YcE6zTDMm0jMF8Xu8/:18472:0:99999:7:::
只需要知道前两个字段：
用户名称:密码密文
注：即使是相同的密码，密码密文也是不一样的，这样别人就不会通过密文发现用户的密码是一样的
```
```
/etc/group

ftpuser:x:1002:
用户名称:组是否需要密码验证:gid:除了同名用户外还有哪些用户归属这个组
```
## 23 | 文件与目录权限的表示方法
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596964066067-1596964066069.png)
• - 普通⽂件
• d ⽬录⽂件
• b 块特殊⽂件		设备，比如u盘
• c 字符特殊⽂件
• l 符号链接
• f 命名管道
• s 套接字⽂件
### 权限
文件权限
![文件权限](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596964453437-1596964453439.png)
默认权限根据umask计算
文件夹权限
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596964489343-1596964489346.png)
注：权限后如果有个点，点表示这个系统目前使用了selinux这个功能，+ 是文件系统的acl
一个用户可以属于多个组，请问查看文件权限权限时，属主属组显示哪个？用户组是可以分主要组和其他组的，显示主要组
## 24 | 文件权限的修改方法和数字表示方法
chmod 修改文件、目录权限
chown 修改属主、属组
chgrp 可单独设置属组，不常用

注：权限设置是针对非root用户的，root用户啥都能操作不受权限限制
```
chmod 777 文件or文件夹
chmod  u+x 文件or文件夹
chmod +x 文件or文件夹

a u g o：all user group other
u+x	u=rwx
g+x	g-r
o+x	o=w
a+r
```
```
chown mysql:mysql 文件or文件夹
chown :mysql 文件or文件夹 #只修改属组，不修改属主
```
## 25 | 权限管理以及文件的特殊权限
特殊权限
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596965593464-1596965593466.png)
SUID
SGID
SBIT
```
/etc/passwd是SUID权限

/tmp目录是SGID权限
[root@10-255-0-194 tmp]# ls -ld /tmp
drwxrwxrwt. 9 root root 228 Aug  9 16:33 /tmp
```
# 第三章：系统管理篇 (29讲)
## 26 | 网络管理
## 27 | 查看网络配置
## 28 | 修改网络配置
## 29 | 网络故障排除命令
## 30 | 网络管理和配置文件
## 31 | 软件包管理器的使用
## 32 | 使用rpm命令安装软件包
## 33 | 使用yum包管理器安装软件包
## 34 | 通过源代码编译安装软件包
## 35 | 如何进行内核升级
## 36 | grub配置文件介绍
## 37 | 使用ps和top命令查看进程
## 38 | 进程的控制与进程之间的关系
## 39 | 进程的通信方式与信号：kill命令
## 40 | 守护进程
## 41 | screen命令和系统日志
## 42 | 服务管理工具systemctl
## 43 | SELinux简介
## 44 | 内存与磁盘管理
## 45 | 内存查看命令
## 46 | 磁盘分区和文件大小查看
## 47 | 文件系统管理
## 48 | i节点和数据块操作
## 49 | 分区和挂载
## 50 | 分区和挂载磁盘配额
## 51 | 交换分区swap的查看与创建
## 52 | 软件RAID的使用
## 53 | 逻辑卷LVM的用途与创建
## 54 | 系统综合状态查看命令sar以及第三方命令
# 第四章：Shell 篇 (31讲)
## 55 | 什么是Shell
## 56 | Linux的启动过程
## 57 | Shell脚本的格式
## 58 | 脚本不同执行方式的影响
## 59 | 管道
## 60 | 重定向
## 61 | 变量赋值
## 62 | 变量引用及作用范围
## 63 | 环境变量、预定义变量与位置变量
## 64 | 环境变量配置文件
## 65 | 数组
## 66 | 转义和引用
## 67 | 运算符
## 68 | 特殊字符大全
## 69 | test比较
## 70 | if判断的使用
## 71 | if-else判断的使用
## 72 | 嵌套if的使用
## 73 | case分支
## 74 | for的基本使用
## 75 | C语言风格的for
## 76 | while循环和until循环
## 77 | 循环的嵌套和break、continue语句
## 78 | 使用循环处理位置参数
## 79 | 自定义函数
## 80 | 系统函数库介绍
## 81 | 脚本资源控制
## 82 | 信号
## 83 | 一次性计划任务
## 84 | 周期性计划任务
## 85 | 为脚本加锁
#第五章：文本操作篇 (15讲)
## 86 | 元字符介绍
## 87 | find 演示
## 88 | sed和awk介绍
## 89 | sed替换命令讲解
## 90 | sed的替换指令加强版
## 91 | sed的其他常用命令
## 92 | sed多行模式空间
## 93 | 什么是sed的保持空间
## 94 | 认识awk
## 95 | awk的字段
## 96 | awk表达式
## 97 | awk判断和循环
## 98 | awk数组
## 99 | awk数组功能的使用
## 100 | awk函数
#第六章：服务管理篇 (18讲)
## 101 | 防火墙概述
## 102 | iptables规则的基本使用演示
## 103 | iptables过滤规则的使用
## 104 | iptables nat表的使用
## 105 | firewalld
## 106 | SSH介绍之Telnet明文漏洞
## 107 | SSH服务演示
## 108 | FTP服务器vsftpd介绍与软件包安装
## 109 | vsftpd配置文件介绍
## 110 | vsftp虚拟用户
## 111 | samba服务演示
## 112 | NFS服务
## 113 | Nginx基本配置文件
## 114 | 使用Nginx配置域名虚拟主机
## 115 | LNMP环境搭建
## 116 | DNS服务的原理
## 117 | NAS演示
## 118 | 结课测试&结束语