# 行编辑器
vim是文本编辑器
sed awk是行编辑器
交互式与非交互式
文件操作模式与行操作模式
vim的操作对象是文本
sed、awk的操作对象是行，一行行处理
# sed
## sed基本用法
sed一般用于对文本内容做替换
```shell
sed '/user1/s/user1/u1' /etc/passwd
```
注：
分隔符之间可以使用正则表达式
可以使用不同的分隔符
## sed的替换命令
### sed的模式空间（处理过程）
sed的基本工作方式是：
将文件以行为单位读取到内存（模式空间）
使用sed的每个脚本对该行进行才坐
处理完成后输出该行
### 替换命令s
```shell
sed 's/old/new/' filename

可以执行多个命令，但每一条命令前都要加-e
sed -e 's/old/new/' -e 's/old/new' filename ...
或命令间用分号连接
sed 's/old/new/;s/old/new' filename ...

原样写回原来的文件
sed -i 's/old/new/' 's/old/new/' filename ...
```
### 使用正则表达式
```
sed 's/正则表达式/new' filename
sed -r 's/扩展正则表达式/new' filename  #+ ? |
```

替换斜杠
sed 's/!/!abc!' filename
# awk
## awk基本用法
awk一般用于对文本内容进行统计、按需要的格式进行输出
```
cut命令
cut -d: -f 1 /etc/passwd

awk命令
awk -F: '/wd$/{print $1}' /etc/passwd
```