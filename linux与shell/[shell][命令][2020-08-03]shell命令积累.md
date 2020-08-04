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
// 文件中的分隔符被命令认定为换行，并不是遇到换行符才换行，所以有问题
for line in `cat filename`
do
 echo $line
done
```

## 替换变量中的指定字符串
```
${string//substring/replacement}

注：
其中的string为变量名，不用加${}，即：
var_sring="hello"
echo ${var_string//l/a}  //输出heaao
```
## 去掉最后一行
sed -i '$d'
