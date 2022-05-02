# Redis必知必会

安装、启动（linux源码安装）

~~~
# wget http://download.redis.io/releases/redis-6.0.8.tar.gz
# tar xzf redis-6.0.8.tar.gz
# cd redis-6.0.8
# make
~~~

启动

```
# cd src
# ./redis-server
```

进入命令行客户端

~~~
# cd src
# ./redis-cli
~~~

启动redis(windows环境下)

~~~
D:\Program Files\Redis>redis-server.exe redis.windows.conf

D:\Program Files\redis-6.2.6-win>redis-server.exe redis.conf
~~~

进入redis命令行客户端（重新打开一个命令行窗口）

~~~~
D:\Program Files\Redis>redis-cli
127.0.0.1:6379>
~~~~

~~~~
查看redis的版本有两种方式：
1. redis-server --version 和 redis-server -v 
2. redis-cli --version 和 redis-cli -v
~~~~



测试连接是否正常

~~~
127.0.0.1:6379> PING
PONG
~~~

错误回复

~~~
127.0.0.1:6379> ERRORCOMMAND
(error) ERR unknown command 'ERRORCOMMAND'
~~~

~~~
127.0.0.1:6379> LPUSH key 1
(integer) 1
127.0.0.1:6379> GET key
(error) WRONGTYPE Operation against a key holding the wrong kind of value
~~~

整数回复

~~~
127.0.0.1:6379> INCR foo
(integer) 1
~~~

字符串回复

~~~
127.0.0.1:6379> GET foo
"1"
不存在时
127.0.0.1:6379> get noexitsts
(nil)
~~~

多行字符串回复

~~~
127.0.0.1:6379> keys *
1) "user"
2) "foo"
3) "\xac\xed\x00\x05t\x00\x04user"
4) "key"
5) "\xac\xed\x00\x05t\x00\x05myKey"
~~~

更换数据库

~~~
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]> GET foo
(nil)
~~~

注：redis中的Key和Value时区分大小写的,命令不区分大小写, redis是单线程 不适合存储大容量的数据

控制台中文乱码问题

在控制台输入 chcp 65001,将控制台的编码改为UTF-8

然后进入redis目录，输入

~~~
D:\Program Files\redis-6.2.6-win>redis-cli --raw
127.0.0.1:6379> 中文输入
~~~

### 入门

获取符合规则的键名列表

~~~
KEYS pattern
~~~

~~~
127.0.0.1:6379> KEYS pattern
(empty list or set)
~~~

~~~
127.0.0.1:6379> SET bar 1
OK
127.0.0.1:6379> KEYS *
1) "user"
2) "bar"
3) "foo"
4) "\xac\xed\x00\x05t\x00\x04user"
5) "key"
6) "\xac\xed\x00\x05t\x00\x05myKey"
~~~

判断一个键是否存在（存在返回1，不存在返回0）

~~~
EXISTS key
~~~

~~~
127.0.0.1:6379> EXISTS bar
(integer) 1
127.0.0.1:6379> EXISTS nobar
(integer) 0
~~~

删除键（可以删除一到多个键，返回删除键的个数）

~~~
DEL key [key ...]
~~~

~~~
127.0.0.1:6379> DEL bar
(integer) 1
127.0.0.1:6379> DEL bar
(integer) 0
~~~

获取键值的数据类型

~~~
TYPE key
~~~

~~~~
127.0.0.1:6379> SET foo 1
OK
127.0.0.1:6379> TYPE foo
string
127.0.0.1:6379> LPUSH bar 1
(integer) 1
127.0.0.1:6379> TYPE bar
list
~~~~

注：可能返回的结果：string 、hash、list、set、zset、stream

#### 字符串类型

赋值和取值

~~~
SET key value
GET Key
~~~

~~~
127.0.0.1:6379> SET key hello
OK
127.0.0.1:6379> GET key
"hello"
~~~

递增数字

~~~
INCR key
~~~

~~~
127.0.0.1:6379> INCR num
(integer) 1
127.0.0.1:6379> INCR num
(integer) 2
127.0.0.1:6379> INCR num
(integer) 3
~~~

命令

增加指定的整数

~~~~
INCRBY key increment
~~~~

~~~
127.0.0.1:6379>  INCRBY bar 2
(integer) 2
127.0.0.1:6379> INCRBY bar 3
(integer) 5
~~~

减少指定的整数

~~~
DECR key
DECR key decrement
~~~

~~~
127.0.0.1:6379> DECR bar
(integer) 4
127.0.0.1:6379> DECRBY bar 3
(integer) 1
~~~

增加指定浮点数

~~~
INCRBYFLOAT key increment
~~~

~~~
127.0.0.1:6379> INCRBYFLOAT bar 2.7
"6.7"
127.0.0.1:6379> INCRBYFLOAT bar 5E+4
"50006.699999999997"
~~~

向末尾追加值

~~~
APPEND key value
~~~

~~~
127.0.0.1:6379> SET key hello
OK
127.0.0.1:6379> APPEND key " world!"
(integer) 12
~~~

获取字符串长度

~~~~
STRLEN key
~~~~

~~~
D:\Program Files\Redis>redis-cli
127.0.0.1:6379> STRLEN key
(integer) 12
127.0.0.1:6379> SET key niha
OK
127.0.0.1:6379> STRLEN key
(integer) 4
~~~

同时获取/设置多个键值

~~~~
MGET key [key ...]
MSET key value [key value ...]
~~~~

~~~
127.0.0.1:6379> MSET key1 v1 key2 v2 key3 v3
OK
127.0.0.1:6379> GET key2
"v2"
127.0.0.1:6379> MGET key1 key3
1) "v1"
2) "v3"
~~~

位操作

~~~
GETBIT key offset
SETBIT key offset value
BITCOUNT key [start] [end]
BITOP operation destkey key [key ...]
~~~

~~~
127.0.0.1:6379> SET foo bar
OK
127.0.0.1:6379> GETBIT foo 0
(integer) 0
127.0.0.1:6379> GETBIT foo 6
(integer) 1
127.0.0.1:6379> GETBIT foo 100000
(integer) 0
127.0.0.1:6379> SETBIT foo 6 0
(integer) 1
127.0.0.1:6379> SETBIT foo 7 1
(integer) 0
127.0.0.1:6379> GET foo
"aar"
127.0.0.1:6379> SETBIT nofoo 10 1
(integer) 0
127.0.0.1:6379> GETBIT nofoo 5
(integer) 0
127.0.0.1:6379> BITCOUNT foo
(integer) 10
127.0.0.1:6379> BITCOUNT foo 0 1
(integer) 6
127.0.0.1:6379> SET foo1 bar
OK
127.0.0.1:6379> SET foo2 aar
OK
127.0.0.1:6379> BITOP OR res foo1 foo2
(integer) 3
127.0.0.1:6379> GET res
"car"
~~~

#### 哈希类型



赋值与取值

~~~~
HSET key field value
HSET key field
HMSET key field value [field value ...]
HMSET key field [field ...]
HGETALL key
~~~~

~~~~
127.0.0.1:6379> HSET car price 500
(integer) 1
127.0.0.1:6379> HSET car name BMW
(integer) 1
127.0.0.1:6379> HGET car name
"BMW"
~~~~

~~~
HSER key field1 value1
HSER key field2 value2
或
HMSET key field value1 field2 value2
HMGET key field value1 field2 value2
~~~

~~~
127.0.0.1:6379> HMGET car price name
1) "500"
2) "BMW"
~~~

~~~
127.0.0.1:6379> HGETALL car
1) "price"
2) "500"
3) "name"
4) "BMW"
~~~

判断字段是否存在

~~~
HEXISTS key field
~~~

~~~
127.0.0.1:6379> HEXISTS car model
(integer) 0
127.0.0.1:6379> HSET car model C200
(integer) 1
127.0.0.1:6379> HEXISTS car model
(integer) 1
~~~

当字段不存在时赋值

~~~
HSETNX key field value
~~~

~~~
127.0.0.1:6379> HSETNX car1 name BYD
(integer) 1
127.0.0.1:6379> HGETALL car1
1) "name"
2) "BYD"
~~~

增加数字

~~~
HINCRBY  key field increment
~~~

~~~
127.0.0.1:6379> HINCRBY person score 60
(integer) 60
~~~

删除字段

~~~
HDEL key field [field ...]
~~~

~~~
127.0.0.1:6379> HDEL car price
(integer) 1
127.0.0.1:6379>  HDEL car price
(integer) 0
~~~

只获取字段名或字段值

~~~
HKEYS key
HVALS key
~~~

~~~
127.0.0.1:6379> HKEYS car
1) "name"
2) "model"

127.0.0.1:6379> HVALS car
1) "BMW"
2) "C200"
~~~

获取字段数量

~~~
HLEN key
~~~

~~~~
127.0.0.1:6379> HLEN car
(integer) 2
~~~~



#### 列表类型

向列表两端增加元素

~~~
LPUSH key value [value ...]
RPUSH key value [value ...]
~~~

~~~~
127.0.0.1:6379> LPUSH numbers 1
(integer) 1
127.0.0.1:6379> LPUSH numbers 2 3
(integer) 3
127.0.0.1:6379> RPUSH numbers 0 -1
(integer) 5
~~~~

从列表两端弹出元素

~~~
LPOP key
RPOP key
~~~

~~~~
127.0.0.1:6379> LPOP numbers
"3"
127.0.0.1:6379> RPOP numbers
"-1"
127.0.0.1:6379>
~~~~

获取列表中元素的个数

~~~
LLEN key
~~~

~~~
127.0.0.1:6379> LLEN numbers
(integer) 3
~~~

获取列表片段

~~~
LRANGE key start stop
~~~

~~~
127.0.0.1:6379> LRANGE numbers 0 2
1) "2"
2) "1"
3) "0"
127.0.0.1:6379> LRANGE numbers -2 -1
1) "1"
2) "0"
127.0.0.1:6379> LRANGE numbers 1 999
1) "1"
2) "0"
~~~

删除列表中指定的值

~~~
LREM key count value
~~~

~~~~
127.0.0.1:6379> RPUSH numbers 2
(integer) 4
127.0.0.1:6379> LRANGE numbers 0 -1
1) "2"
2) "1"
3) "0"
4) "2"
127.0.0.1:6379> LREM numbers -1 2
(integer) 1
127.0.0.1:6379> LRANGE numbers 0 -1
1) "2"
2) "1"
3) "0"
~~~~

获取/设置指定索引的元素值

~~~~
LINDEX key index
LSET key index value
~~~~

~~~
127.0.0.1:6379> LINDEX numbers 0
"2"
127.0.0.1:6379> LINDEX numbers -1
"0"
127.0.0.1:6379> LSET numbers 1 7
OK
127.0.0.1:6379> LINDEX numbers 1
"7"
~~~

只保留列表指定片段

~~~
LTRIM key start end
~~~

~~~~
127.0.0.1:6379> LRANGE numbers 0 -1
1) "2"
2) "7"
3) "0"
127.0.0.1:6379> LTRIM numbers 1 2
OK
127.0.0.1:6379> LRANGE numbers 0 1
1) "7"
2) "0"
~~~~

向列表中插入元素

~~~~
LINSERT key BEFORE|AFTER pivot value
~~~~

~~~
127.0.0.1:6379> LPUSH numbers 2
(integer) 3
127.0.0.1:6379> LRANGE numbers 0 -1
1) "2"
2) "7"
3) "0"
127.0.0.1:6379> LINSERT numbers AFTER 7 3
(integer) 4
127.0.0.1:6379> LRANGE numbers 0 -1
1) "2"
2) "7"
3) "3"
4) "0"
127.0.0.1:6379> LINSERT numbers BEFORE 2 1
(integer) 5
127.0.0.1:6379> LRANGE numbers 0 -1
1) "1"
2) "2"
3) "7"
4) "3"
5) "0"
~~~

将元素从一个列表转到另一个列表

~~~
RPOPLPUSH source destination
~~~

~~~
127.0.0.1:6379> RPOPLPUSH numbers target1
"0"
127.0.0.1:6379> LRANGE target1 0 -1
1) "0"
127.0.0.1:6379> LRANGE numbers 0 -1
1) "1"
2) "2"
3) "7"
4) "3"
127.0.0.1:6379> RPOPLPUSH numbers numbers
"3"
127.0.0.1:6379> LRANGE numbers 0 -1
1) "3"
2) "1"
3) "2"
4) "7"
~~~

 

#### 集合类型

增加/删除元素

~~~
SADD key member [member ...]
SREM key member [member ...]
~~~

~~~
127.0.0.1:6379> SADD letters a
(integer) 1
127.0.0.1:6379> SADD letters a b c
(integer) 2
127.0.0.1:6379> SREM letters c d
(integer) 1
~~~

获取集合中的所有元素

~~~
SMEMBERS key
~~~

~~~
127.0.0.1:6379> SMEMBERS letters
1) "b"
2) "a"
~~~

判断元素是否在集合中

~~~
SISMEMBER key member
~~~

~~~
127.0.0.1:6379> SISMEMBER letters a
(integer) 1
127.0.0.1:6379> SISMEMBER letters d
(integer) 0
~~~

集合间运算

SDIFF ：差集运算    SINTER ： 交运算     SUNION : 并运算

~~~
SDIFF key [key ...]
SINTER key [key ...]
SUNION key [key ...]
~~~

~~~
127.0.0.1:6379> SADD setA 1 2 3
(integer) 3
127.0.0.1:6379> SADD set B 2 3 4
(integer) 4
127.0.0.1:6379> SADD setB 2 3 4
(integer) 3
127.0.0.1:6379> SDIFF setA setB
1) "1"
127.0.0.1:6379> SDIFF setB setA
1) "4"

127.0.0.1:6379> SADD setC 2 3
(integer) 2
127.0.0.1:6379> SDIFF setA setB setC
1) "1"
~~~

~~~
127.0.0.1:6379> SINTER setA setB
1) "2"
2) "3"
127.0.0.1:6379> SINTER setA setB setC
1) "2"
2) "3"
~~~

~~~
127.0.0.1:6379> SUNION setA setB
1) "1"
2) "2"
3) "3"
4) "4"
127.0.0.1:6379> SUNION setA setB setC
1) "1"
2) "2"
3) "3"
4) "4"
~~~

获取集合中的元素个数

~~~
SCARD key
~~~

~~~
127.0.0.1:6379> SMEMBERS letters
1) "b"
2) "a"
127.0.0.1:6379> SCARD letters
(integer) 2
~~~

进行集合运算存储结果

~~~
SDIFFSTORE destination key [key ...]
SINTERSTORE destination key [key ...]
SUNIONSTORE destination key [key ...]
~~~

随机获取集合中的元素

~~~
SRANDMEMBER key [count]
~~~

~~~
127.0.0.1:6379> SRANDMEMBER letters
"a"
127.0.0.1:6379>  SRANDMEMBER letters
"a"
127.0.0.1:6379>  SRANDMEMBER letters
"b"
127.0.0.1:6379>  SRANDMEMBER letters
"b"
~~~

~~~~
127.0.0.1:6379> SADD letters c d
(integer) 2
127.0.0.1:6379> SRANDMEMBER letters 2
1) "b"
2) "d"
127.0.0.1:6379>  SRANDMEMBER letters 2
1) "c"
2) "a"
127.0.0.1:6379>  SRANDMEMBER letters 100
1) "c"
2) "a"
3) "b"
4) "d"
127.0.0.1:6379>  SRANDMEMBER letters -2
1) "b"
2) "c"
127.0.0.1:6379>  SRANDMEMBER letters -10
 1) "d"
 2) "d"
 3) "d"
 4) "c"
 5) "a"
 6) "a"
 7) "c"
 8) "c"
 9) "b"
10) "c"
~~~~

注：count>0,count个不重复元素，count<0,|count|个有可能重复的元素，count>集合元素个数，获取全部元素

从集合中弹出一个元素(随机弹出)

~~~
SPOP key
~~~

~~~
127.0.0.1:6379> SPOP letters
"d"
127.0.0.1:6379> SMEMBERS letters
1) "c"
2) "b"
3) "a"
~~~



#### 有序集合类型

增加元素

~~~
ZADD key score member [score member ...]
~~~

~~~
127.0.0.1:6379> ZADD scoreboard 89 Tom 67 Peter 100 David
(integer) 3
127.0.0.1:6379> ZADD scoreboard 76 Peter
(integer) 0
127.0.0.1:6379> ZADD testboard 17E+307 a
(integer) 1
127.0.0.1:6379> ZADD testboard 1.5 b
(integer) 1
127.0.0.1:6379> ZADD testboard +inf c
(integer) 1
127.0.0.1:6379> ZADD testboard -inf d
(integer) 1
~~~

获取元素的分数

~~~
ZSCORE key member
~~~

~~~
127.0.0.1:6379> ZSCORE scoreboard Tom
"89"
~~~

获取排名在某个范围内的元素列表

~~~
ZRANGE key start stop [WITHSCORES]
ZREVRANGE key start stop [WITHSCORES]
~~~

~~~
127.0.0.1:6379> ZRANGE scoreboard 0 2
1) "Peter"
2) "Tom"
3) "David"
127.0.0.1:6379> ZRANGE scoreboard 1 -1
1) "Tom"
2) "David"
~~~

~~~
127.0.0.1:6379> ZRANGE scoreboard 0 -1 WITHSCORES
1) "Peter"
2) "76"
3) "Tom"
4) "89"
5) "David"
6) "100"
~~~

获取指定分数范围内的元素

~~~~
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
~~~~

~~~~
127.0.0.1:6379> ZRANGEBYSCORE scoreboard 80 100
1) "Tom"
2) "David"
127.0.0.1:6379>  ZRANGEBYSCORE scoreboard 80 (100
1) "Tom"
127.0.0.1:6379>  ZRANGEBYSCORE scoreboard (80 +inf
1) "Tom"
2) "David"
127.0.0.1:6379> ZADD scoreboard 56 Jerry 92 Wendy 67 Yvonne
(integer) 3
127.0.0.1:6379> ZRANGE scoreboard 0 -1 WITHSCORES
 1) "Jerry"
 2) "56"
 3) "Peter"
 4) "67"
 5) "Yvonne"
 6) "67"
 7) "Tom"
 8) "89"
 9) "Wendy"
10) "92"
11) "David"
12) "100"
127.0.0.1:6379> ZRANGEBYSCORE scoreboard 60 +inf LIMIT 1 3
1) "Yvonne"
2) "Tom"
3) "Wendy"
127.0.0.1:6379> ZREVRANGEBYSCORE scoreboard 100 0 LIMIT 0 3
1) "David"
2) "Wendy"
3) "Tom"
~~~~

增加某个元素的分数

~~~~
ZINCRBY key increment member
~~~~

~~~
127.0.0.1:6379> ZINCRBY scoreboard 4 Jerry
"60"
127.0.0.1:6379> ZINCRBY scoreboard -4 Jerry
"56"
~~~

注：如指定元素不存在，则会先建立后赋值为0，再操作

获得集合中元素的数量

~~~
ZCARD key
~~~

~~~
127.0.0.1:6379> ZCARD scoreboard
(integer) 6
~~~

获得指定分数范围内的元素个数

~~~
ZCOUNT key min max
~~~

~~~
127.0.0.1:6379> ZCOUNT scoreboard 90 100
(integer) 2
127.0.0.1:6379> ZCOUNT scoreboard (89 +inf
(integer) 2
~~~

删除一个或多个元素

~~~
ZREM key member [member ...]
~~~

~~~
127.0.0.1:6379> ZREM scoreboard Wendy
(integer) 1
127.0.0.1:6379> ZCARD scoreboard
(integer) 5
~~~

按照排名范围删除元素

~~~~
ZREMRANGEBYRANK key start stop
~~~~

~~~
127.0.0.1:6379> ZADD testRem 1 a 2 b 3 c 4 d 5 e 6 f
(integer) 6
127.0.0.1:6379> ZREMRANGEBYRANK testRem 0 2
(integer) 3
127.0.0.1:6379> ZRANGE testRem 0 -1
1) "d"
2) "e"
3) "f"
~~~

按照分数范围删除元素

~~~
ZREMRANGEBYSCORE key min max
~~~

~~~
127.0.0.1:6379> ZREMRANGEBYSCORE testRem (4 5
(integer) 1
127.0.0.1:6379> ZRANGE testRem 0 -1
1) "d"
2) "f"
~~~

获得元素的排名

~~~
ZRANK key member
ZREVRANK key member
~~~

~~~
127.0.0.1:6379> ZRANK scoreboard Peter
(integer) 1
127.0.0.1:6379> ZREVRANK scoreboard Peter
(integer) 3
~~~

注：排名从1开始，与书上的示例（从0开始）不一致

计算有序集合的交集

~~~
ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE
          SUM|MIN|MAX]
~~~

~~~
127.0.0.1:6379> ZADD sortedSets1 1 a 2 b
(integer) 2
127.0.0.1:6379> ZADD sortedSets2 10 a 20 b
(integer) 2
127.0.0.1:6379> ZINTERSTORE sortedSetsResult 2 sortedSets1 sortedSets2
(integer) 2
127.0.0.1:6379> ZRANGE sortedSetsResult 0 -1 WITHSCORES
1) "a"
2) "11"
3) "b"
4) "22"
~~~

~~~~
127.0.0.1:6379>  ZINTERSTORE sortedSetsResult 2 sortedSets1 sortedSets2 AGGREGATE MIN
(integer) 2
127.0.0.1:6379> ZRANGE sortedSetsResult 0 -1 WITHSCORES
1) "a"
2) "1"
3) "b"
4) "2"
~~~~

~~~
127.0.0.1:6379> ZINTERSTORE sortedSetsResult 2 sortedSets1 sortedSets2 AGGREGATE MAX
(integer) 2
127.0.0.1:6379> ZRANGE sortedSetsResult 0 -1 WITHSCORES
1) "a"
2) "10"
3) "b"
4) "20"
~~~

~~~
127.0.0.1:6379>  ZINTERSTORE sortedSetsResult 2 sortedSets1 sortedSets2 WEIGHTS 1 0.1
(integer) 2
127.0.0.1:6379>  ZRANGE sortedSetsResult 0 -1 WITHSCORES
1) "a"
2) "2"
3) "b"
4) "4"
~~~

#### 流类型

（注：大坑--->>>Redis Stream 是 Redis 5.0 版本新增加的数据结构，5.0之前的会报错）

增加条目

~~~
XADD key [MAXLEN [=|~] threshold] *|ID field value [field value ...]
~~~

~~~
127.0.0.1:6379> XADD nginxLogs * IP 193.180.71.3 status 200
"1650168459437-0"
~~~

~~~
127.0.0.1:6379> XADD demo-incr * f v
"1650168848019-0"
127.0.0.1:6379> XADD demo-incr 2-0 f v
(error) ERR The ID specified in XADD is equal or smaller than the target stream top item（超出id范围，报错）
~~~

~~~
只保留最近的100个条目
127.0.0.1:6379> XADD demo-incr MAXLEN 100 * f v
"1650169503431-0"
保留稍多于100个条目
127.0.0.1:6379> XADD demo-incr MAXLEN ~ 100 * f v
"1650169575505-0"
~~~

根据ID来按范围查询条目

~~~
127.0.0.1:6379> XRANGE demo-incr 1650168848019-0 1650169575505-0
1) 1) "1650168848019-0"
   2) 1) "f"
      2) "v"
2) 1) "1650169503431-0"
   2) 1) "f"
      2) "v"
3) 1) "1650169575505-0"
   2) 1) "f"
      2) "v"
~~~

~~~
限制返回结果
127.0.0.1:6379> XRANGE demo-incr 1650168848019-0 1650169575505-0 COUNT 1
1) 1) "1650168848019-0"
   2) 1) "f"
      2) "v"
~~~

删除指定元素

~~~
XDEL key ID [ID ...]
~~~

~~~
127.0.0.1:6379> XADD test-del * a 1
"1650170761963-0"
127.0.0.1:6379> XADD test-del * a 2
"1650170791781-0"
127.0.0.1:6379> XADD test-del * a 3
"1650170813941-0"
127.0.0.1:6379> XDEL test-del 1650170791781-0 1-1
(integer) 1
127.0.0.1:6379> XRANGE test-del - +
1) 1) "1650170761963-0"
   2) 1) "a"
      2) "1"
2) 1) "1650170813941-0"
   2) 1) "a"
      2) "3"
~~~

裁剪流

~~~
XTRIM key MAXLEN [=|~] threshold
~~~

~~~
127.0.0.1:6379> XADD test-trim * a 1
"1650171025637-0"
127.0.0.1:6379>  XADD test-trim * a 2
"1650171034205-0"
127.0.0.1:6379>  XADD test-trim * a 3
"1650171045046-0"
127.0.0.1:6379> XTRIM test-trim MAXLEN 2
(integer) 1
127.0.0.1:6379> XRANGE test-trim - +
1) 1) "1650171034205-0"
   2) 1) "a"
      2) "2"
2) 1) "1650171045046-0"
   2) 1) "a"
      2) "3"
~~~

获取流的长度

~~~
XLEN key
~~~

~~~
127.0.0.1:6379> XADD test-len * a 1
"1650171148545-0"
127.0.0.1:6379>  XADD test-len * a 2
"1650171157106-0"
127.0.0.1:6379>  XADD test-len * a 3
"1650171161651-0"
127.0.0.1:6379> XLEN test-len
(integer) 3
~~~

根据反向ID 按范围查询条目

~~~
XREVRANGE key end start [COUNT count]
~~~

### 进阶

#### 事务

简单的事务操作

~~~
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SADD "user:1:following" 2
QUEUED
127.0.0.1:6379(TX)> SADD "user:2:following" 1
QUEUED
127.0.0.1:6379(TX)> EXEC
1) (integer) 1
2) (integer) 1
~~~

错误处理

（1）语法错误

~~~
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SET key value
QUEUED
127.0.0.1:6379(TX)> SET key
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379(TX)> ERRORCOMMAND key
(error) ERR unknown command 'ERRORCOMMAND'
127.0.0.1:6379(TX)> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
~~~

(2) 运行错误

~~~~
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SET key 1
QUEUED
127.0.0.1:6379(TX)> SADD key 2
QUEUED
127.0.0.1:6379(TX)> SET key 3
QUEUED
127.0.0.1:6379(TX)> EXEC
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
3) OK
127.0.0.1:6379> GET key
"3"
~~~~

WATCH命令（监控命令）

~~~
127.0.0.1:6379> SET key 1
OK
127.0.0.1:6379> WATCH key
OK
127.0.0.1:6379> SET key 2
OK
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379(TX)> SET key 3
QUEUED
127.0.0.1:6379(TX)> EXEC
(nil)
127.0.0.1:6379> GET key
"2"
~~~

设置过期时间

~~~
~~~



