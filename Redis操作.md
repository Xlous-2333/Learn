# Redis基本操作

## 连接

1. 连接redis：

   redis-cli(客户端) -h host(主机) -p port(端口) -a password(密码) --raw(避免中文乱码)

   如：redis-cli -a xlous2333 --raw

2. 连接数据库：

   **select dataid**：选择数据库(redis数据库无自定义名，以编号命名)
   

## 键基本操作

| 命令                                       | 描述                                            |
| ------------------------------------------ | ----------------------------------------------- |
| del key                                    | 当key存在时删除它                               |
| dump key                                   | 序列化key，并返回被序列化的值                   |
| exists key                                 | 检查key是否存在                                 |
| expire key secinds                         | 给key设置过期时间(以秒为单位)                   |
| expireat key timestamp                     | 给key设置过期时间(时间参数为unix时间戳)         |
| pexpire key milliseconds                   | 给key设置过期时间(以毫秒为单位)                 |
| pexpireat key millseconds-timestamp        | 给key设置过期时间(时间参数为unix时间戳以毫秒记) |
| keys pattern                               | 查找所有符合给定模式(pattern)的key              |
| move key db                                | 将当前数据库的key移动到给定数据库(db)           |
| persist key                                | 移除key的过期时间，使其持久保持                 |
| pttl key                                   | 以毫秒为单位返回key的剩余过期时间               |
| ttl key                                    | 以秒为单位返回key的剩余生存时间                 |
| randomkey                                  | 从当前数据库中随机返回一个key                   |
| rename key newkey                          | 修改key名称为newkey                             |
| renamenx key newkey                        | 仅在newkey不存在将key修改为newkey               |
| scan cursor [ match pattern] [count count] | 迭代数据库中的数据库键                          |
| type key                                   | 返回key所存储的值的类型                         |

## Redis数据类型

| 类型   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| string | 字符串，键-值对\|set添加，get获取 del删除                    |
| hash   | 哈希，键-值对集合\|hmset添加，get获取，del删除               |
| list   | 列表，字符串列表，按插入顺序排序\|lpush/rpush左右添加，lrange/rrange左右查看，del删除 |
| set    | 集合，字符串集合，通过哈希表实现，无重无序\|sadd添加，smembers查看，del删除 |
| zset   | 有序集合，字符串集合，各元素(无重)关联一个double类的分数，按分数排序，分数可重\|zadd添加，zrangebyscore查看，del删除 |

- string:

  ​	set <key> <value> --设置

  ​	get <key> --查看

- hash:
  ​	hmset <key> <field1> <value1> <field2> <value2> --设置

  ​	hget <key> <key1> --查看

  ​	hgetall <key> --查看全部
  
  ​	hlen <key> --获取字段数量
  
  ​	hmget <key> <field> --获取给定字段的值
  
  ​	hkeys <key> --获取全部字段(field)
  
  ​	hvals <key> --获取全部值(value)
  
- list:

  ​	lpush <key> <value1> --从右往左(头插法)插入设置

  ​	rpush <key> <value1> --从左往右(尾插法)插入设置

  ​	lpush <key> <value2>

  ​	lpush <key> <value3>

  ​	lrange <key> <startnum> <endnum> (如lrange test 0 10从头(左)查看key为test的0到10项)

  ​	lpop/rpop key --移除并获取列表的第一个元素/最后一个元素

  ​	blpop/brpop <key> timeout --移除并获取列表的第一个元素/最后一个元素，若没有则阻塞知道超时或发现可弹出元素为止

  ​	lindex key index --通过索引获取列表中的元素(下标从0开始)

  ​	llen key --获取列表长度

- set:

  ​	sadd <key> <value1>

  ​	smembers <key1>

- zset:

  ​	zadd <key1> <value1> <key2> <value2>

  ​	zrange <key> <startnum> <endnum>

  ​	zrangebyscore <key> <startscore> <endscore>

查看所有数据库：

```redis
keys *
keys datebaseName
info
select 0
```

