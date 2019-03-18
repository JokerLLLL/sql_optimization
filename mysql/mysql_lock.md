```sql

// mysql 的 线程锁 只有 get_lock 释放掉之后 才会执行 下个 get_lock ,而不是直接跳过
select get_lock('key_lock', 100);

update test_lock set name = 'tt2', address = 'aaaaaaaaaaaaaaaaaaaa' where id = 1; #只更新name列

select release_lock('key_lock');

//以下要等以上执行完，等待执行的
select get_lock('key_lock', 100);

update test_lock set name = 'tt', address = 'bbbbbbbbbbbbbbbbbbbbbbb' where id = 1;  #只更新address列

select release_lock('key_lock');
