# mysql 的事务锁 
```sql
    //设置事务不自动提交 开始事务 查询数据 并将该数据锁定 （锁定数据只能自读 不能修改删除）
    SET AUTOCOMMIT=0; 
    BEGIN WORK; 
    SELECT quantity FROM products WHERE id=3 FOR UPDATE; 
  
    //判断 如果锁在索引上，将对应的行加锁， 否知会把整张表进行加锁。
    
    //然后操作 进行事务提交
    UPDATE products SET quantity = '1' WHERE id=3 ;
    COMMIT WORK;
    
```
上面介绍过SELECT ... FOR UPDATE 的用法，不过锁定(Lock)的数据是判别就得要注意一下了。
由于InnoDB 预设是Row-Level Lock，所以只有「明确」的指定主键，MySQL 才会执行Row lock (只锁住被选取的数据) ，
否则MySQL 将会执行Table Lock (将整个数据表单给锁住)[严重影响效率]。

//锁表情况 比如 select * where name = `joker` for update; 会导致整张表锁死

FOR UPDATE 仅适用于InnoDB，且必须在事务区块(BEGIN/COMMIT)中才能生效。

## mysql 的乐观锁。
查询字段 version 为 108；
update user set name='jokerl' version = version+1, where uid=199 and version = 108;
影响行数为1 成功， 否知失败；

## 悲观锁 （共享锁/排它锁）

select * from table_name lock in share mode ;
共享锁(S锁, share locks)。其他事务可以读取数据，但不能对该数据进行修改，直到所有的共享锁被释放。
如果事务对某行数据加上共享锁之后，可进行读写操作；其他事务可以对该数据加共享锁，但不能加排他锁，且只能读数据，不能修改数据。

select * from table_name for update;
排他锁(X锁, exclusive locks)。如果事务对数据加上排他锁之后，则其他事务不能对该数据加任何的锁。
获取排他锁的事务既能读取数据，也能修改数据。

测试理解:
当 lock in share mode 时 其他的session可以读取或再加共享锁，加的同时不能进行写操作，会等待在哪里。单个都有写操作
时，会进行一个异常一个成功。

for update 可以锁整张表，或某行记录。加锁的同时，可以查询，但其他session不能加for update的锁，只能等待在那里。
其他的排他锁释放掉才会执行语句。


## mysql的线程锁 更锁数据没关系
```sql

// mysql 的 线程锁 只有 get_lock 释放掉之后 才会执行 下个 get_lock ,而不是直接跳过
select get_lock('key_lock', 100);

update test_lock set name = 'tt2', address = 'aaaaaaaaaaaaaaaaaaaa' where id = 1; #只更新name列

select release_lock('key_lock');

//以下要等以上执行完，等待执行的
select get_lock('key_lock', 100);

update test_lock set name = 'tt', address = 'bbbbbbbbbbbbbbbbbbbbbbb' where id = 1;  #只更新address列

select release_lock('key_lock');
```


