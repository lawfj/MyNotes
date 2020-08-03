删除第一列
awk '{$1="";print $0}' file