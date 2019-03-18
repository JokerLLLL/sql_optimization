# 时间格式换  

DATE_FORMAT(date,format)  时间戳转字符串
UNIX_TIMESTAMP(create_time)  字符串转时间戳

format格式：http://www.w3school.com.cn/sql/func_date_format.asp
select uid,userid,username,email,FROM_UNIXTIME(addtime,'%Y年%m月%d') from members;
SELECT *,FROM_UNIXTIME(UNIX_TIMESTAMP(create_time),'%Y-%m-%d %H') as atime FROM `machine_room_log`;

SELECT *,FROM_UNIXTIME(left(log_time,10),'%Y-%m-%d %H:%i:%s') as create_time FROM `db_log`;


# 时间比较 (本质进行了补充00:00:00操作)
SELECT * FROM `machine_room_log` where create_time<'2018-07-30';
小于不包含2018-07-30
SELECT * FROM `machine_room_log` where create_time>'2018-07-30';
大于包含2018-07-30 


# 查询两个时间查在 10s内的 

SELECT * FROM `test` where ABS(time_to_sec(timediff(`seat_time`, `create_time`))) < 10;

timestampdiff(second, t1, t2),

unix_timestamp(t2) -unix_timestamp(t1)



# 常见时间函数

current_time() ; //时间 
current_date() ; //日期

current_timestamp() ;  //此刻时间 
now();

datediff('2001-12-12','2008-11-23'); //比较天数

DATE_FORMAT('2011-11-11 11:11:11','%Y-%m-%d %r')

