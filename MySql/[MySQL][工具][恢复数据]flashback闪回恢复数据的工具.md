目前找的了几款：
binlog2sql：需要修改源码，解决中文以及编码问题
MyFlash：目前没有大问题，只能解析出binlog格式文件
lightning：有bug，使用测试语句可以复现，但不清楚触发原理
	2020-8-6 22:24:28：发现并不是bug，去掉-user -password -port -host就成功了，应该是我的使用问题
binlog_rollback：尚未研究
mysql-flashback：尚未研究