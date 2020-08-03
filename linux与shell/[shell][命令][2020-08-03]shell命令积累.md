## 删除第一列
```
awk '{$1="";print $0}' file
```

## 循环遍历文件的每一行
### 使用while循环
```
while read -r line
do
 echo $line
done < filename
```
### 使用for有问题，待研究
```
// 文件遇到分隔符就会换行，并不是遇到换行符才
for line in `cat filename`
do
 echo $line
done
```
