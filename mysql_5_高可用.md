## mysql复制

1.实现不同服务器的数据分布
2.实现负载均衡
3.增加数据安全性

## 二进制日志 binlog  (row/statement)
基于sql 基于行

## 复制工作原理

master->binary_log  (slave->relay_log)

## 配置

账号
create user repl@'127.0.0.1' identified by 'password';
grant replication slave on *.* to repl@'127.0.0.1'

master：
bin_log = mysql-bin
server_id = 100

slave:
bin_log = mysql-bin
server_id = 101
relay_log = mysql-relay-bin 中继日志
log_slave_update = on [可选]
read_only = on [可选]

# 基于日志点的复制
mysqldump --master-data=2 -single-transaction  //会加锁
xtrabackup --slave-info        // innodb 不加锁


# 启动复制链路
   change master to master_host = 'master_ip', 
                    master_user='repl',
                    master_password='password',
                    master_log_file='mysql_log_file_name',  
                    master_log_pos='4';
   
   start slave;

# 基于GTID的复制

# 读写分离，负载均衡
有软件实现
有中间件实现maxscale


# sql优化
    select id,user,host,db,command,time,state,info from information_schema.`PROCESSLIST`;
    
    优化 in 和 <> 使用 left join 去优化
    e.g   select name from user where name not in (select name from payment );
    =>>  select user.name from user left join payment where payment.name is null;
     
    汇总表优化查询
    e.g    select  count(*) from comment where pro_id = 199;
    =>> 增加汇总表   create table comment_cnt (pro_id int ,cnt int); //凌晨进行维护
    select sun(cnt) from (
        select cnt from comment where pro_id =199 
        union all 
        select count(*) from comment where pro_id = 199 and time > date(now())
    ) a;
   
    
# 分库分表