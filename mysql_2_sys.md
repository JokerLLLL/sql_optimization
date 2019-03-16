# mysql结构体

客户端 + 服务层 + 存储引擎层

针对表进行 分存储引擎

# MyISAM

非事务型
3个文件 .frm .MYD .MYI  (.frm 系统文件 二进制 里面表结构)
可以压缩（压缩完了，只能只读） 读取数据时候，会加共享锁。
空间类应用

表级锁
lock table table_name write;
unlock table;

# innodb

事务的特性 ACID 的特性  原子性 隔离性 一致性 持久性
数据存储的方式不同 （innodb_file_per_table 决定 on=>table_name.idb  off=>系统表空间 ibdataX）
optimize table 重建表
在系统位置   innodb 字典信息 （B树的类型） undo回滚段

Redo log  和 Undo log 实现持久性 和 回滚性 
内存缓冲区 show variable like  innodb_log_buffer_size  每一秒刷成文件

支持行级锁，能够支持更大的并发
（
    锁：实现事务的隔离性
    共享锁 ： 读锁
    独占锁 ： 写锁
）

show engine innodb status； 状况检查

# csv
以及文件数据存储的
不能为null
不支持索引(全表扫描)
直接编辑
.csv .csm(表单状态) .frm

# Archive
zlib压缩
.arz .frm
只支持insert select
只支持id索引

# memory
存在内存中
.frm
支持hash索引 BTree索引
定长 varchar = char
表级锁

# federated
提供链接 数据是远程服务器上
show variable federated

# mysql配置
1 配置文件 /etc/my.cnf
2 set global 参数=值

内存配置信息
sort_buffer_size  每个链接排序的内存缓冲区
join_buffer_size  链接缓冲
read_buffer_size  数据
read_rnd_buffer_size 索引

IO配置
innodb_log_file_size 
innodb_log_file_in_group

tmp_table_size max_heap_table_size 控制临死内存表
max_connections 控制对大连接数 2000