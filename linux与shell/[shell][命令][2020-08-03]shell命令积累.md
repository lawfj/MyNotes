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
### 