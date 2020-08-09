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
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/08/09/1596949041868-1596949041933.png)
help：内部命令使用help帮助
内部命令：shell（命令解释器）自带的命令成为内部命令，其他的是外部命令。内部命令没有实体，说人话就是在linux中找不到这些命令对应的文件。
```
help cd
```
外部命令获取help帮助
```
ls --help
```
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
Made by linusflow
#