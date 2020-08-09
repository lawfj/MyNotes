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
### ls
ls 查看当前⽬录下的⽂件
• ls [选项，选项… ] 参数 … 
常⽤参数：
• -l ⻓格式显示⽂件
• -a 显示隐藏⽂件
• -r 逆序显示
• -t 按照时间顺序显示
• -R 递归显示
常用组合
```
ls -alF
ls -lFR
ls 
```
## 10 | 详解ls命令
## 11 | 详解cd命令
## 12 | 创建和删除目录
## 13 | 复制和移动目录
## 14 | 如何在Linux下进行文本查看
## 15 | 打包压缩和解压缩
## 16 | Vim的四种模式
## 17 | Vim的正常模式
## 18 | Vim的命令模式
## 19 | Vim的可视模式
## 20 | 用户和用户组管理及密码管理
## 21 | su和sudo命令的区别和使用方法
## 22 | 用户和用户组的配置文件介绍
## 23 | 文件与目录权限的表示方法
## 24 | 文件权限的修改方法和数字表示方法
## 25 | 权限管理以及文件的特殊权限
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