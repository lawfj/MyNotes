看网上大神攻略，想试试用vs code远程调试mysql。
# 参考
[Centos7.4 安装MySQL 5.7.21 (通用二进制包)](https://blog.csdn.net/zml3721/article/details/79090983)
[CentOS7.4 源码安装mysql-5.7.21](https://www.jianshu.com/p/41ac166ef477)
MySQL官方文档--2.9 Installing MySQL from Source
# 源码安装
以下操作均在linux下执行
## mysql国内外镜像
国内镜像：如果网速不给力，可以使用国内mysql镜像，但有些版本会没有，比如我需要的mysql 5.7.21
[搜狗国内mysql镜像](https://mirrors.sohu.com/mysql/)
官网镜像：如果实在是想要特定版本MySQL的话，还是推荐用官方方式
[官方下载地址](https://downloads.mysql.com/archives/community/)

## 下载安装MySQL源码版本
1）下载mysql源码包
请确保下载的是源码版，而不是二进制版！（傻傻的我就是）
由于我需要的5.7.21版本国内镜像库上找不到，所以我用官方地址下载，而且源码包挺小的，才50M，二进制包要600M，果断官方下载。
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/21/1600700888054-1600700888114.png)

```
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.21.tar.gz

--2020-09-21 18:46:22--  https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.21.tar.gz
Resolving downloads.mysql.com (downloads.mysql.com)... 137.254.60.14
Connecting to downloads.mysql.com (downloads.mysql.com)|137.254.60.14|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://cdn.mysql.com/archives/mysql-5.7/mysql-5.7.21.tar.gz [following]
--2020-09-21 18:46:25--  https://cdn.mysql.com/archives/mysql-5.7/mysql-5.7.21.tar.gz
Resolving cdn.mysql.com (cdn.mysql.com)... 104.70.237.54
Connecting to cdn.mysql.com (cdn.mysql.com)|104.70.237.54|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 51822632 (49M) [application/x-tar-gz]
Saving to: ‘mysql-5.7.21.tar.gz’

100%[=================================================================================================================================================>] 51,822,632  54.0KB/s   in 14m 1s 

2020-09-21 19:00:28 (60.2 KB/s) - ‘mysql-5.7.21.tar.gz’ saved [51822632/51822632]
```
2）检查文件MD5值是否匹配：若不匹配需重新下载
```
[root@10-255-0-194 tmp]# md5sum mysql-5.7.21.tar.gz
e26523b174bdc3fd0fde6f36791ce17e  mysql-5.7.21.tar.gz
```
3）解压文件
```
tar -vxzf mysql-5.7.21.tar.gz
drwxr-xr-x  35 7161 31415      4096 Dec 28  2017 mysql-5.7.21/
```
## 安装依赖及准备
1、Mysql5.7版本更新后有很多变化，安装必须要BOOST库（版本需为1.59.0）
mysql5.7.2x版本的必须指定1.5.9的boost，高版本不行
ps：好慢啊啊啊啊，又找不到镜像，只能慢慢等了，下了70多分钟。。。
1）boost库下载地址：https://sourceforge.net/projects/boost/files/boost/1.59.0/
```
wget https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz
```
2）检查文件MD5值是否匹配：若不匹配需重新下载
```
[root@10-255-0-194 tmp]# md5sum boost_1_59_0.tar.gz 
51528a0e3b33d9e10aaa311d9eb451e3  boost_1_59_0.tar.gz

```
3）解压后放在指定位置备用
```
tar -vxzf boost_1_59_0.tar.gz
mv boost_1_59_0 /data/mysql_5.7.21_source/mysql/boost
# 下载很慢，做完后面的再回来解压，该目录后续会创建
```
2、安装依赖包
```
yum install -y cmake gcc gcc-c++ ncurses-devel bison zlib libxml openssl
```
3、查看MySQL是否安装
```
rpm -qa |grep mysql
删除mysql：rpm -e mysql.xxx / yum remove -y mysql
重点来了：
centos 7 因为mysql收费的原因，不在支持mysql,转而内部集成mariadb，那么首先要卸载mariabd
rpm -qa | grep mariadb
如果有依赖，就强制卸载即可
rpm -e --nodeps mariadb-libs-5.x

[root@10-255-0-194 tmp]# rpm -qa | grep mariadb
mariadb-libs-5.5.65-1.el7.x86_64
[root@10-255-0-194 tmp]# rpm -e --nodeps mariadb-libs-5.5.65-1.el7.x86_64
[root@10-255-0-194 tmp]# rpm -qa | grep mariadb
```
4、添加mysql组及创建MySQL用户
```
查询mysql是否存在，id mysql
存在删除 userdel -r msql
groupadd mysql
useradd -d /home/mysql -g mysql -m mysql
修改密码：passwd mysql ...


[root@10-255-0-194 tmp]# id mysql
id: mysql: no such user
[root@10-255-0-194 tmp]# groupadd mysql
[root@10-255-0-194 tmp]# useradd -d /home/mysql -g mysql -m mysql
[root@10-255-0-194 tmp]# passwd mysql
注：设置密码需要比较高的要求
```
5、设置mysql用户的环境变量
切换到mysql用户下：su - mysql

vi .bash_profile
在PATH=HOME/bib:后面添加mysql bin目录，及配置环境变量。
```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/.local/bin:$HOME/bin

export MYSQL_HOME=/data/mysql_5.7.21_source/mysql/app/mysql
export PATH=$MYSQL_HOME/bin:$PATH
export MYSQL_DATADIR=/data/mysql_5.7.21_source/mysql/data
export MYSQL_LOGDIR=/data/mysql_5.7.21_source/mysql/log
export MYSQL_TMPDIR=/data/mysql_5.7.21_source/mysql/tmp
export MYSQL_UNIX_PORT=/data/mysql_5.7.21_source/mysql/data/mysql.sock
export MYSQL_TCP_PORT=3306
```
/data/mysql_5.7.21_source/mysql/app/mysql/bin是我的mysql bin目录
保存退出，执行source .bash_profile,加载bash_profile
6、创建目录并授权
```
使用root用户执行以下命令：
mkdir -p /data/mysql_5.7.21_source/mysql/app/mysql
mkdir -p /data/mysql_5.7.21_source/mysql/data
mkdir -p /data/mysql_5.7.21_source/mysql/tmp
mkdir -p /data/mysql_5.7.21_source/mysql/log/innodb_log
mkdir -p /data/mysql_5.7.21_source/mysql/log/bin_log
mkdir -p /data/mysql_5.7.21_source/mysql/log/relay_log

授权：
chown -R mysql:mysql /data/mysql_5.7.21_source
对/u01目录授予mysql:mysql组用户的权限
chmod -R 775 /data/mysql_5.7.21_source
更改目录权限
```
## 编译安装
### 进行cmake编译mysql源文件
MySQL常见编译参数
参数|说明|
|-|-|
|-DCMAKE_INSTALL_PREFIX=dir_name|基础的文件夹，对应mysqld的--basedir参数|
|-DINSTALL_BINDIR=dir_name|bin目录位置|
|-DINSTALL_DOCDIR=dir_name|文档目录位置|
|-DINSTALL_DOCREADMEDIR=dir_name|Readme文件位置|
|-DINSTALL_INCLUDEDIR=dir_name|Include目录位置|
|-DINSTALL_LAYOUT=name|布局选项，包括Standalone、RPM、SRV4、DEB|
|-DMYSQL_DATADIR=dir_name|数据存放目录|
|-DSYSCONFDIR=dir_name|默认配置my.cnf目录|
|-DWITH_BOOST|boost源码路径|
|-DEFAULT_CHARSET|数据库默认字符编码|
|-DDEFAULT_COLLATION|默认排序规则|
|-DENABLED_LOCAL_INFILE|允许从本文件导入数据|
|-DEXTRA_CHARSETS|安装所有字符集|
|-DWITH_DEBUG|启用DUBUG模式|

注：更多预编译参数参考官方文档说明：
https://dev.mysql.com/doc/refman/5.7/en/source-configuration-options.html#cmake-general-options
```
在mysql源代码目录下执行
注：-DWITH_DEBUG参数一定不能忘！调试用
cmake \
-DCMAKE_INSTALL_PREFIX=/data/mysql_5.7.21_source/mysql/app/mysql \
-DMYSQL_DATADIR=/data/mysql_5.7.21_source/mysql/data \
-DDEFAULT_CHARSET=utf8mb4 \
-DDEFAULT_COLLATION=utf8mb4_general_ci \
-DEXTRA_CHARSETS=all \
-DWITH_EMBEDDED_SERVER=1 \
-DENABLED_LOCAL_INFILE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DMYSQL_UNIX_ADDR=/data/mysql_5.7.21_source/mysql/data/mysql.sock \
-DMYSQL_TCP_PORT=3306 \
-DSYSCONFDIR=/data/mysql_5.7.21_source/mysql/app/mysql \
-DMYSQL_USER=mysql \
-DENABLE_DOWNLOADS=1 \
-DWITH_BOOST=/data/mysql_5.7.21_source/mysql/boost \
-DWITH_READLINE=on \
-DWITH_DEBUG=1

执行后提示：
-- Configuring done
-- Generating done
CMake Warning:
  Manually-specified variables were not used by the project:

    MYSQL_USER
    WITH_MEMORY_STORAGE_ENGINE
    WITH_READLINE


-- Build files have been written to: /tmp/mysql-5.7.21
预编译完成
```
### 进行编译安装
```
在相同目录执行：
make & make install
#make 进行编译，这里时间比较久，期间可以进行vs code的配置
#make install 进行安装

[100%] Building CXX object libmysqld/examples/CMakeFiles/mysqltest_embedded.dir/__/__/client/mysqltest.cc.o
Linking CXX executable mysqltest_embedded
[100%] Built target mysqltest_embedded
Scanning dependencies of target my_safe_process
[100%] Building CXX object mysql-test/lib/My/SafeProcess/CMakeFiles/my_safe_process.dir/safe_process.cc.o
Linking CXX executable my_safe_process
[100%] Built target my_safe_process

[1]+  完成                  make
[root@localhost mysql-5.7.21]# 

```
注：make报错：
```
[ 47%] Building CXX object sql/CMakeFiles/sql.dir/aggregate_check.cc.o
[ 47%] Building CXX object sql/CMakeFiles/sql.dir/geometry_rtree.cc.o
c++: internal compiler error: Killed (program cc1plus)
Please submit a full bug report,
with preprocessed source if appropriate.
See <http://bugzilla.redhat.com/bugzilla> for instructions.
make[2]: *** [sql/CMakeFiles/sql.dir/geometry_rtree.cc.o] Error 4
make[1]: *** [sql/CMakeFiles/sql.dir/all] Error 2
make: *** [all] Error 2
[1]+  Exit 2                  make
```
参考别人的解决方法，主要原因大体上是因为内存不足,可以临时使用交换分区来解决（毕竟用的滴滴云一年80块，1C1G配置）
```
sudo dd if=/dev/zero of=/swapfile bs=64M count=32
sudo mkswap /swapfile
sudo swapon /swapfile

然后重新执行make & make install
```
完成make & make install后，如下提示：
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600764439200-1600764439262.png)
$MYSQL_HOME内也安装上了相关内容
```
/data/mysql_5.7.21_source/mysql/app/mysql
[mysql@localhost mysql]$ ls
bin      COPYING-test  include  man     mysql-test  README-test  support-files
COPYING  docs          lib      README  share

```
## 初始化MySQL
### my.cnf配置
在/data/mysql_5.7.21_source/mysql/app目录下创建my.cnf文件，并写入下面配置内容
```
[client]
port=3306
socket=/data/mysql_5.7.21_source/mysql/data/mysql.sock

[mysql]
socket=/data/mysql_5.7.21_source/mysql/data/mysql.sock

[mysqld]
autocommit=1
general_log=off
explicit_defaults_for_timestamp=true

# system
basedir=/data/mysql_5.7.21_source/mysql/app/mysql
datadir=/data/mysql_5.7.21_source/mysql/data/
open_files_limit=10240
# 控制mysql接收数据包的大小，主从应该保证一致
max_allowed_packet=32m
# 控制允许的最大连接数
max_connections=2000
# 最大用户连接不宜过大
 max_user_connections=200
wait_timeout=100
pid_file=/data/mysql_5.7.21_source/mysql/data/mysqld.pid
port=3306
server_id=101
# 禁用DNS查找
skip_name_resolve=ON
socket=/data/mysql_5.7.21_source/mysql/data/mysql.sock
tmpdir=/data/mysql_5.7.21_source/mysql/tmp
# 默认值5000
open_files_limit=5000
# 设置 LOAD DATA and  SELECT ... INTO OUTFILE 导出限制
secure_file_priv=''

# bin log 
log_bin=/data/mysql_5.7.21_source/mysql/log/innodb_log
binlog_cache_size=32768
binlog_format=row
# 保留bin-log保存的日期
expire_logs_days=3
log_slave_updates=ON
max_binlog_size=512M
log-bin-index=binlog.index
# 控制mysql如何想磁盘刷新binlog，默认为0，建议设置为1,
sync_binlog=1

# logging
log_error=/data/mysql_5.7.21_source/mysql/data/mysql.err
slow_query_log_file=/data/mysql_5.7.21_source/mysql/log/slow_queries.log
log_queries_not_using_indexes=0
slow_query_log=1
log_slave_updates=ON
log_slow_admin_statements=1
long_query_time=1
log_bin=/data/mysql_5.7.21_source/mysql/log/bin_log/mysql-bin
binlog_cache_size=1M
binlog_format=ROW
binlog_gtid_simple_recovery=1
binlog_rows_query_log_events=1
max_binlog_size=512M

# relay
relay_log=/data/mysql_5.7.21_source/mysql/log/relay_log/mysql-relay
relay_log_index=/data/mysql_5.7.21_source/mysql/log/relay_log/relay.index
relay_log_info_file=/data/mysql_5.7.21_source/mysql/log/relay_log/relay-log.info

# slave 
slave_load_tmpdir=/data/mysql_5.7.21_source/mysql/tmp
slave_skip_errors=OFF
# 禁止非super权限的用户写权限,一定要在从库启用
# read_only=1

# innodb
innodb_data_home_dir=/data/mysql_5.7.21_source/mysql/data
innodb_log_group_home_dir=/data/mysql_5.7.21_source/mysql/log/innodb_log
innodb_adaptive_flushing=ON
innodb_adaptive_hash_index=ON
innodb_autoinc_lock_mode=1
innodb_buffer_pool_instances=8

# default 
innodb_change_buffering=inserts
innodb_buffer_pool_size=256M
innodb_data_file_path=ibdata1:32M;ibdata2:16M:autoextend
# 控制双写缓冲，避免页损坏
innodb_doublewrite=1
# 为每一张表建立一个单独的表空间，不使用系统表空间
innodb_file_per_table=1
# 系统提交事务的方式： 2：每次事物提交，执行log数据写入catch，每秒执行一次flush log到磁盘
innodb_flush_log_at_trx_commit=2
# 文件交换的方式，linux服务器设置为O_DIRECT即可
innodb_flush_method=O_DIRECT
innodb_io_capacity=1000
innodb_lock_wait_timeout=10
innodb_log_buffer_size=20M
innodb_log_file_size=100M
innodb_log_files_in_group=4
innodb_max_dirty_pages_pct=60
# 不能大于open_file_limit
innodb_open_files=4000
innodb_purge_threads=1
innodb_read_io_threads=4
innodb_stats_on_metadata=OFF
innodb_support_xa=ON
innodb_use_native_aio=OFF
innodb_write_io_threads=10

[mysqldump]
quick
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

[mysqld_safe]
basedir=/data/mysql_5.7.21_source/mysql/app/mysql
datadir=/data/mysql_5.7.21_source/mysql/data
pid_file=/data/mysql_5.7.21_source/mysql/data/mysqld.pid
tmpdir=/data/mysql_5.7.21_source/mysql/tmp
```
### 初始化MySQL及用户密码
下列命令均在mysql用户下执行
```
#两种方式任选一种
#1）不生成root密码
mysqld --defaults-file=/data/mysql_5.7.21_source/mysql/app/mysql/my.cnf --initialize-insecure --user=mysql --basedir=/data/mysql_5.7.21_source/mysql/app/mysql --datadir=/data/mysql_5.7.21_source/mysql/data

#2）生成root随机密码，在/data/mysql/mysql-error.log文件中 
mysqld --defaults-file=/data/mysql_5.7.21_source/mysql/app/mysql/my.cnf --initialize --user=mysql --basedir=/data/mysql_5.7.21_source/mysql/app/mysql --datadir=/data/mysql_5.7.21_source/mysql/data

注意：此条语句执行时容易报错，成功至少要满足以下几个条件
0. --defaults-file调整到--initialize前面，不然报错“unknown variable”
1. /data/mysql_5.7.21_source/mysql/data目录存在并且一定要为空目录，否则报错；
2. 如果本机已经存在了其余的mysql，请确实/etc/my.cnf文件不存在，否则会按照/etc/my.cnf中的设置进行初始化，datadir会读取另一个mysql实例的路径，从而导致报错。遇到此情况，可以先将已经存在的mysql实例停止，然后将/etc/my.cnf文件剪切到此实例对应的datadir目录中，再启动此实例，然后重新执行初始化命令；
3. 所有的报错信息都将会在my.cnf指定的mysql.err日志中体现，如果没有配置这个日志，将会在控制台直接输出。也不需要在命令中添加--log-error
```
### 启动MySQL
1、安全启动MySQL
```
mysqld_safe &

[mysql@localhost data]$ ps -ef|grep mysqld
mysql    18107 16866  0 17:09 pts/1    00:00:00 /bin/sh /data/mysql_5.7.21_source/mysql/app/mysql/bin/mysqld_safe
mysql    19071 18107  3 17:09 pts/1    00:00:01 /data/mysql_5.7.21_source/mysql/app/mysql/bin/mysqld --basedir=/data/mysql_5.7.21_source/mysql/app/mysql --datadir=/data/mysql_5.7.21_source/mysql/data --plugin-dir=/data/mysql_5.7.21_source/mysql/app/mysql/lib/plugin --log-error=/data/mysql_5.7.21_source/mysql/data/mysql.err --open-files-limit=5000 --pid-file=/data/mysql_5.7.21_source/mysql/data/mysqld.pid --socket=/data/mysql_5.7.21_source/mysql/data/mysql.sock --port=3306
mysql    19141 16349  0 17:10 pts/2    00:00:00 grep --color=auto mysqld
```
2、登录MySQL，修改密码
```
mysql -uroot -p
#修改密码
1)法一
set password for root='密码';
2)法二
mysqladmin -uroot -p旧密码 password 新密码
3)法三
alter user test identified by '123456';
注，事实上修改root用户密码只能用法二，且上面的方法没有指定host，默认host为localhost，所以需要继续修改host
法一：
update mysql.user set host='%' where user='root';
```
### 配置成服务
以下内容使用root执行
1、添加mysql系统启动脚本
```
cp -p /data/mysql_5.7.21_source/mysql/app/mysql/support-files/mysql.server /etc/init.d/
```
2、添加mysql.server到服务中
```
chkconfig --add mysql.server
```
3. 测试使用systemctl 启动mysql
```
systemctl start mysql.server
```
4. 查看是否启动成功
```
ps -ef | grep mysql
```
有mysql服务则启动成功
5. 设置开机自动开启
```
chkconfig mysql.server on
```
6. 添加到linux服务器开机自启动
systemctl enable mysql.server
# vs code配置
## 远程机器
1、按照上述步骤进行MySQL源码编译安装，记得加-DWITH_DEBUG参数
2、安装gdb
```
yum install gdb
```
2、安装 gdbserver
```
yum install gdb-gdbserver
```
3、启动 ssh 服务端, 通常默认已经启动（废话，都可以通过xshell登录当然是开启的啦~）
## 本地机器
1、安装最新版vs code，支持Remote Development插件
2、安装 VSCode 扩展 “Remote Development”, 方法是左下角  管理（⚙） ->  扩展, 直接搜索商店
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600709969738-1600709969740.png)
安装成功后，左侧边栏会多一个电脑的图标
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600710643542-1600710643544.png)
设置remote.SSH
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600710835784-1600710835786.png)

3、安装兼容 OpenSSH 的 SSH 客户端
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600781522753-1600781522755.png)
4、vscode连接远程服务器
在 VSCode 主界面  ctrl+shift+p 选  Remote.SSH: Connect to host, 输入  root@<ip>, 成功后界面左下角会有  SSH: <ip> 的已连接状态
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600785126949-1600785127002.png)
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600785172545-1600785172547.png)
注：输入密码后连接成功，但第一次远程服务器必须要联网下载插件，不然会卡住
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600785283126-1600785283129.png)
5、然后就可以从侧边栏打开项目路径了, 点击左边的资源管理器，点击打开文件夹，在下图蓝框处输入mysql源码路径（注意不是编译后的运行程序路径是源码解压路径）
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600785873384-1600785873387.png)
### 安装插件
C/C++ IntelliSense, debugging, and code browsing
有些扩展是要安装在 SSH 的目标机器上, 安装时会提示你  install on SSH: xxxx, 而主题类扩展和一部分功能类扩展是安装在本地机器上
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600786023047-1600786023049.png)
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600786468705-1600786468714.png)
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600786599227-1600786599233.png)
### 远程服务器开启mysql
通过gdbserver开启mysql
```
gdbserver localhost:2333 /data/mysql_5.7.21_source/mysql/app/mysql/bin/mysqld --basedir=/data/mysql_5.7.21_source/mysql/app/mysql --datadir=/data/mysql_5.7.21_source/mysql/data --plugin-dir=/data/mysql_5.7.21_source/mysql/app/mysql/lib/plugin --user=mysql --log-error=/data/mysql_5.7.21_source/mysql/data/mysql.err --open-files-limit=5000 --pid-file=/data/mysql_5.7.21_source/mysql/data//localhost.localdomain.pid --socket=/data/mysql_5.7.21_source/mysql/data/mysql.sock --port=3306
```
### 创建配置文件
选择c++(GDB/LLDB)，会在左边窗口新建launch.json，将其修改为下面的代码
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600788277604-1600788277606.png)
```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "/data/mysql_5.7.21_source/mysql/app/mysql/bin/mysqld",
            "stopAtEntry": false,
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "miDebuggerPath": "gdb",
            "miDebuggerArgs": "gdb",
            "linux": {
                "MIMode": "gdb",
                "miDebuggerPath": "/usr/bin/gdb",
                "miDebuggerServerAddress": "192.168.56.102:2333",
            },
            "logging": {
                "moduleLoad": false,
                "engineLogging": false,
                "trace": false
            },
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```
几个注意的地方:

应该是  "request": "launch", 不是 “attach”, 此后也并不需要记录进程ID
需要填对  "miDebuggerServerAddress": "192.168.126.128:2333", 有这个设置才会开启 gdb 远程调试
"engineLogging": true 可以看到 gdb 自身的详细消息
必须是  "externalConsole": false 否则报错
/data/mysql_5.7.21_source/mysql/app/mysql/bin/mysqld 在 gdbserver 和 launch.json 里都要填一次,即mysql编译后的启动程序
### 正式开始调试
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/22/1600789054554-1600789054556.png)
添加断点，可以看到参数
![title](https://raw.githubusercontent.com/lawfj/MyNotesPic/master/MyNotes/2020/09/23/1600791818061-1600791818063.png)
## 调试积累
1. 使用gdbserver启动mysql，如果vs code这边没有开始调试，mysql程序会hang在开始阶段，mysql.err的记录是还没开始启动。一旦我们开始了调试，gdbserver会打印“Remote debugging from host 192.168.56.102”，mysql正式开始启动，并可以登录了。
2. 目前看来，是要在调试前就要打好断点，不然不起效，后续再看