##  /etc/sysctl.conf 的优化

对应 /proc/sys/ 文件下的内容

net.core.somaxconn = 65535                带监听端口的网络监听队列个数
net.core.netdev_max_backlog = 65535       传输输速率比内核处理快的时候 允许发送到队列中的数目
net.ipv4.tcp_max_syn_backlog = 65535      没连接的请求 保存的最大数目

net.ipv4.tcp_fin_timeout = 10             连接请求等待的时间 高负载 提前回收
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1                

net.core.wmem_default = 87380             tpc接收和发送缓存区大小的值
net.core.wmem_max = 16777216
net.core.rmem_default = 87380
net.core.rmem_max = 167777216

net.ip4.tcp_keepalive_time = 120        检查活的tcp连接的
net.ip4.tcp_keepalive_intvl = 30        探测没消息时从发消息的间隔
net.ip4.tcp_keepalive_probes = 3        重发探测次数


kernel.shmmax = 4294967252            内存共享值 一般取值 innodb 的大小 
vm.swappiness = 0                     swap分区使用的肯能性


## /etc/security/limit.conf    插入模块 的认证配置 打开文件的限制
* soft nofile 65535 
* hard nofile 65535  
所有用户 当前系统/最大设定值 选项 值




##  磁盘调度策略  /sys/block/devname/queue/scheduler

cat /sys/block/sda/queue/scheduler
noop  deadline [cfq] 策略

cfq           一般
noop          电梯策略 倾向饿死读 利于写 fifo队列
deadline      截止时间调度策略 好
anticipotory  和deadline 一样 对数据库差



### 文件系统对性能的影响

window  fat/ntfs   linux  ext3/ext4/xfs

ext3/4  /etc/fstab

data=writeback|ordered|journal

noatime,nodiratime


