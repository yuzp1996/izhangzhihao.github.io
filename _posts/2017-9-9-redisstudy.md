
## redis基础学习总结

****
####基础

key-value存储系统(同时还提供list，set，zset，hash等数据结构的存储)

支持网络、可基于内存亦可持久化的日志型(Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。)

提供多种语言的API

Redis支持数据的备份，即master-slave模式的数据备份



****
生成器:带有yield的函数，把该函数赋给其他对象，该对象就是生成器（生成器是一种特殊的迭代器，可以使用next()方法）

    def a():
        yield 1
        yield 2
    
    c = a()

此时c就是一个生成器，a()就是生成器函数。

生成器的另一个方法，(i for i in range(10))

****
列表L可以被for进行循环但是不能被内置函数next()用来查找下一个值，所以L是可迭代对象


L通过iter进行包装后设为I，I可以被next()用来查找下一个值，所以I是迭代器

迭代器两个要素：__iter__函数与next()函数,用for循环的时候先检查是否是可迭代对象(即是否含有__iter__)，然后每次循环时调用next()方法  (可以这么记 但是并不准确)


    class d:
    	def __init__(self, data):
        	self.date = data
        	self.index = len(data)
    	def __iter__(self):
       	 	return self
    	def next(self):
        	if self.index == 0:
            	raise StopIteration
        	self.index = self.index - 1
        	return self.date[self.index]


迭代器生成方法：iter([1,2,3,3])



****
#### 优势

**性能极高** – Redis能读的速度是110000次/s,写的速度是81000次/s 。

**丰富的数据类型** – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。


**原子** – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。

**丰富的特性** – Redis还支持 publish/subscribe, 通知, key 过期等等特性。


***
不同

Redis有着更为复杂的数据结构并且提供对他们的原子性操作

Redis运行在内存中但是可以持久化到磁盘

相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情


****

使用

打开一个 cmd 窗口 使用cd命令切换目录到 C:\redis 运行 redis-server.exe redis.windows.conf 。（
如果想方便的话，可以把 redis 的路径加到系统的环境变量里，这样就省得再输路径了，后面的那个 redis.windows.conf 可以省略，如果省略，会启用默认的）


这时候另启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了。
切换到redis目录下运行 redis-cli.exe -h 127.0.0.1 -p 6379 。（如果想方便的话，可以把 redis 的路径加到系统的环境变量里，这样就省得到相应目录了）

>redis-cli
>PING #检查是否连接


#### 在远程服务上执行命令

	$ redis-cli -h host -p port -a password
以下实例演示了如何连接到主机为 127.0.0.1，端口为 6379 ，密码为 mypass 的 redis 服务上。


    $redis-cli -h 127.0.0.1 -p 6379 -a "mypass"
	redis 127.0.0.1:6379>
	redis 127.0.0.1:6379> PING

	PONG


****
## 配置

####语法
Redis CONFIG 命令格式如下：

redis 127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME

如

    redis 127.0.0.1:6379> CONFIG GET loglevel

	1) "loglevel"
	2) "notice"

    redis 127.0.0.1:6379> CONFIG GET *

  	1) "dbfilename"
  	2) "dump.rdb"
  	3) "requirepass"
  	4) ""
  	5) "masterauth"
  	6) ""
  	7) "unixsocket"
  	8) ""
  	9) "logfile"



编辑配置

你可以通过修改 redis.conf 文件或使用 CONFIG set 命令来修改配置。
语法

CONFIG SET 命令基本语法：

	redis 127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
实例

	redis 127.0.0.1:6379> CONFIG SET loglevel "notice"
	OK
	redis 127.0.0.1:6379> CONFIG GET loglevel

	1) "loglevel"
	2) "notice"
****
#### 常见数据类型
|名称   |作用    |使用方法|
| ----- | ---- | ------ |
|String   |redis的string可以包含任何数据。比如jpg图片或者序列化的对象,一个键最大能存储512MB     |>SET name "runoob" >GET name|
|Hash|一个键名对集合,个string类型的field和value的映射表，hash特别适合用于存储对象,每个 hash 可以存储 2**(32 -1) 键值对（40多亿）|> HMSET user:1 username runoob password runoob points 200   > HGETALL user:1|
|List|你可以添加一个元素到列表的头部（左边）或者尾部（右边）,2**(32 - 1) 元素 (4294967295, 每个列表可存储40多亿)|>lpush runoob firstlistobj>lrange runoob 0 10|
|Set|集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1),集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)，集合成员是唯一的，这就意味着集合中不能出现重复的数据。|语法>>sadd key member >sadd runoob mongodb >smembers runoob|
|Zset|Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员.不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。|语法：zadd key score member >zadd runoob 0 redis > zadd runoob 0 mongodb >ZRANGEBYSCORE runoob 0 1000|
|Redis HyperLogLog|在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。|> PFADD runoobkey "redis" > PFADD runoobkey "mongodb" > PFCOUNT runoobkey|

### redis键

    > SET runoobkey redis
    > DEL runoobkey


****

###Redis 发布订阅
Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
Redis 客户端可以订阅任意数量的频道。
下图展示了频道 channel1 ， 以及订阅这个频道的三个客户端 —— client2 、 client5 和 client1 之间的关系：
![订阅消息](http://www.runoob.com/wp-content/uploads/2014/11/pubsub1.png)

当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：
![广播](http://www.runoob.com/wp-content/uploads/2014/11/pubsub2.png)



发布订阅

>SUBSCRIBE redisChat

发布消息  订阅端便会收到消息， redisChat 其实就是个中间的东西，订阅了redisChat，谁通过redisChat传递消息都会在订阅的哪里出现


****

###redis事务
一个事务从开始到执行会经历以下三个阶段：

* 开始事务。

* 命令入队。

* 执行事务。

######执行过程
以下是一个事务的例子， 它先以 MULTI 开始一个事务， 然后将多个命令入队到事务中， 最后由 EXEC 命令触发事务， 一并执行事务中的所有命令：

```
redis 127.0.0.1:6379> MULTI
OK

redis 127.0.0.1:6379> SET book-name "Mastering C++ in 21 days"
QUEUED

redis 127.0.0.1:6379> GET book-name
QUEUED

redis 127.0.0.1:6379> SADD tag "C++" "Programming" "Mastering Series"
QUEUED

redis 127.0.0.1:6379> SMEMBERS tag
QUEUED

redis 127.0.0.1:6379> EXEC
1) OK
2) "Mastering C++ in 21 days"
3) (integer) 3
4) 1) "Mastering Series"
   2) "C++"
   3) "Programming"
```

****
#### Redis 脚本

执行脚本的常用命令为 EVAL。
语法
Eval 命令的基本语法如下：
>redis 127.0.0.1:6379> EVAL script numkeys key [key ...] arg [arg ...]


关闭当前连接 quit