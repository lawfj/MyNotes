# 第二章：系统操作篇
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
help：内部命令使用help帮助
内部命令：shell（命令解释器）自带的命令成为内部命令，其他的是外部命令。内部命令没有实体，说人话就是在linux中找不到这些命令对应的文件。
```
help cd
```
外部命令获取help帮助
```
ls --help
```
