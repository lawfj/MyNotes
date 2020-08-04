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
```
sed -i '$d' filename
```
## 获取文件指定行或指定行区间（某几行）的内容
```
sed -n '123p' filename //获取第123行的数据

sed -n '123,125p' filename //获取123~124行的数据，不包括125行！
grep -A10 -B5 'hello' filename //匹配有字符串'hello'的行，并往上5行，往下10行

```
## 显示最后几行
```
tail filename //打印最后10行
tail -n 15 filename //打印最后15行
```

