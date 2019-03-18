# wamp msql设置日志：
01.打开 Mysql控制台
02.查看 general_log 状态：SHOW VARIABLES LIKE '%general_log%';确认 log记录档在何处。
03.打开 general_log 监看：SET GLOBAL general_log = 'ON';执行网站 ，看 log记录档，执行了那些SQL语句。
04.关闭 general_log 取消监看:SET GLOBAL general_log = 'OFF';


//设备批量查询日志里的  最新或最旧的消息 

# 列里 嵌套列表(或分组后端前三条)
$sql = "SELECT * from band_step where id in
     (SELECT  max(id) from band_step where MID in (\"{$deviceList2}\") GROUP BY MID)";  //max 时间可能重复 in id解决 不然子查询和外查询都要加 in deviceList条件 还要group by


e.g. <<< 该例子 不不严谨 查询慢。 cat_id 分组 每个分组 价格最高前两条 （坑：若果第二和第三....一样就会造成都查出来）
 SELECT a.* FROM `mygoods` as a 
     WHERE  
     (SELECT COUNT(*) FROM `mygoods` WHERE cat_id = a.cat_id AND price > a.price ) < 2 //判断这个分组的前面数据是<2的 
     ORDER BY a.cat_id asc ,a.price DESC 
说明： 



# 字段裁切

MySQL 字符串截取函数：left(), right(), substring(), substring_index()。

select left(create_time,13) as atime form member; 




# 字符串链接
```sql
UPDATE hy_brand set `logo` = CONCAT('http://obd-admin.com/brand_logo/',`logo_bp`);

```

# mysql  查询重复

```sql

select * from order_notice  where id in (select id from order_notice where uid = :uid group by end_time having count(id) > 1);

// 正确示例
select * from order_notice p1 WHERE (p1.uid,p1.end_time) in (SELECT uid,end_time from order_notice group by uid,end_time HAVING count(*) > 1);


// 不需要最小值的重复数据
SELECT * FROM order_notice WHERE (uid,end_time) IN (SELECT  uid,end_time FROM order_notice GROUP BY uid,end_time HAVING  COUNT(*) > 1) AND id NOT (SELECT MAX(id) FROM order_notice GROUP BY uid,end_time HAVING COUNT(*) >1);

```

```sql
// n2级别的排序  不可取  //而且重复数据 id会重复  错误事例  ！！！
!SQLERROR: select p1.* from order_notice p1, order_notice p2 where p1.id <> p2.id and p1.end_time = p2.end_time and p1.uid = p2.uid;

```


// 重复都查询出来
```sql
EXPLAIN SELECT a1.* FROM `user` as a1 WHERE (SELECT count(*) FROM `user` WHERE phone = a1.phone) > 1;
```



