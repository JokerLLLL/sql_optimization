## 索引(B-tree/hash)
mysql带了两种索引方式 二级索引（会带上主键索引标识）

B-tree 可用作范围查询，分组，排序(只有btree同序的时候才有作用)
   全职匹配                  order_sn='xxxx';
   匹配最左前缀的查询        （如果是联合索引 只能是第一列能使用到索引）
   匹配列前缀查询            order_sn like '1201909%' 
   匹配范围值的查询          order_sn > '10999ddd' 
   精确第一例，范围第二列(联合索引的话)     order_sn = 'xxxx'  and data < '1994-10-10'
   值访问索引查询(覆盖索引)    查询值就在索引中。二次索引包含了主键索引的值。不能使用%% like 的查询，否知失效             
   
   限制：  
   不是按照最左列开始查找，则无法使用索引 （order_sn,data） 只有data则不用索引
   使用索引时候不能跳过索引中的列  （order_sn,phone,name） 如果没有phone条件，能只用order_sn的优化
   not in <> 无法使用索引
   对某个列使用索引，之后的列就都不能使用索引
   
Hash索引 
    精确匹配 存储哈希码 索引快，只能都能等值查询
    限制：
    二次查找
    无法优化排序
    哈希索引冲突
    不支持范围查询
    
    
## 索引好处
    索引减少存储引擎所需的扫描的数量
    B-tree索引 帮助我们进行排序避免使用自排临时表
    随机IO变成顺序IO，发挥IO性能

## 索引的坏处
    增加写入成本。
    增加数据维护性，插入修改或删除都要维护索引。
    过多增加查询优化器对索引的选择时间。

## 索引使用的策略
    索引不能使用表达式或函数。         where to_date(out_data) - to_day(current_date) < = 30;
                             优化成: where out_date <= date_add(current_date,interval 30 day)
    
    索引不能的大于767字节（innodb）  可以建立前缀索引 alter table city_demo add key (city(6));
                                   或使用 btree索引模拟前缀索引。
                                   
    联合索引选择顺序，经常被用到的，宽度小的列
    
    
## 通过索引减少锁定的行数 （innodb没有使用到索引将进行锁表）
    select * frome city where name='shanghai' for update;  //排它锁
    
   
## 更新索引信息
    analyze table table_name;
    optimize table table_name;  //锁表
    
    