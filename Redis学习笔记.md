

## Redis学习笔记

#### 关系与非关系数据库基本介绍

关系型数据库遵循ACID规则
事务在英文中是transaction，和现实世界中的交易很类似，它有如下四个特性：

**1、A (Atomicity) 原子性**
原子性很容易理解，也就是说事务里的所有操作要么全部做完，要么都不做，事务成功的条件是事务里的所有操作都成功，只要有一个操作失败，整个事务就失败，需要回滚。比如银行转账，从A账户转100元至B账户，分为两个步骤：1）从A账户取100元；2）存入100元至B账户。这两步要么一起完成，要么一起不完成，如果只完成第一步，第二步失败，钱会莫名其妙少了100元。

**2、C (Consistency) 一致性**
一致性也比较容易理解，也就是说数据库要一直处于一致的状态，事务的运行不会改变数据库原本的一致性约束。

**3、I (Isolation) 独立性**
所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要访问的数据正在被另外一个事务修改，只要另外一个事务未提交，它所访问的数据就不受未提交事务的影响。比如现有有个交易是从A账户转100元至B账户，在这个交易还未完成的情况下，如果此时B查询自己的账户，是看不到新增加的100元的

**4、D (Durability) 持久性**
持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上，即使出现宕机也不会丢失。

------

**C:强一致性 A：高可用性 P：分布式容忍性**

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，

最多只能同时较好的满足两个。
因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：
**CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。**
**CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。**
**AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106222735135.png" alt="image-20210106222735135" style="zoom:80%;" />

#### 1、常用命令：

##### 1、1 开启与关闭redis服务

开启：在resid安装目录的src下./redis-server redis备份配置文件的所在位置 &（表示后台运行）

**redis-server /opt/myredis-conf/redis.conf &**

关闭：在resid安装目录的src下，./redis-cli shutdown（配置环境变量的话可以在任何目录下开启或者关闭如下）

**redis-cli shutdown**

**在/etc/profile配置	export PATH=$PATH:/opt/installDir/redis-5.0.10/src**

##### 1、2 进入redis客户端：

在src目录下运行./redis-cli -h服务器ip -p redis的端口号（需要改配置文件中的注释默认的bind，保护模式为no，默认只能本机登录）

**redis-cli -h 127.0.0.1 -p 6379**

##### 1、3退出redis客户端：

**exit或者quit**

##### 1、4切换数据库：

（redis默认有16个数据库）select 下标；默认在0号库

127.0.0.1:6379> **select 10**
OK
127.0.0.1:6379[10]>

##### 1、5清除数据库：

**flushdb**（当前库),flushall(所有库，别用！！！小心）

##### 1、6查看当前库有多少值

**key：dbsize**

#### 2、key常用命令：

语法：**keys pattern**
作用：查找所有符合模式 pattern 的 key. pattern 可以使用通配符。
通配符：
*****：表示 0-多个字符，例如：keys * 查询所有的 key。  ？：表示单个字符，例如：wo?d , 匹配 word , wood

------

语法：**exists key [key…]作用：判断 key 是否存在**
返回值：整数，存在 key 返回 1，其他返回 0.使用多个 key，返回存在的 key 的数量。不存在返回null.

------

语法：**expire key seconds**
作用：设置 key 的生存时间，超过时间，key 自动删除。单位是秒。
返回值：设置成功返回数字 1，其他情况是 0 。

------

语法：**ttl key**
作用：以秒为单位，返回 key 的剩余生存时间（ttl: time to live）
-1:永不过时；-2：不存在：其它剩余存活时间；

------

语法**：type key**
作用：查看 key 所存储值的数据类型

------

语法：**del key [key…]**
作用：删除存在的 key，不存在的 key 忽略。
返回值：数字，删除的 key 的数量。不存在返回0。

------

#### 3、string类型常用命令（单key单value)

​	set k v：添加一个值
​	setex k ktv:设置这个键的超时时间：expire k t；k、t、v表示键、超时时间、k的值
​	setnx k v：如果不存在则添加一个值，存在不添加。
​	get k：获取这个键所对应的值
​	del k：删除这个键值对
​	mset k1 v1 k2 v2 ... ：添加多个键值对
​	mget k1 k2 ...：获取多个键值对
​	msetnx k1 v1 k2 v2 ...：添加多个键值对（键值对都不存在的情况），如果有一个存在则添加失败返回0。
​	getrange k 0 -1:获取这个key对应的所有值，0 3表示获得这个字符串从0到3的字串，相当于Java的String类型获取字串。
​	setrange k 5 newvalue:设置从下标为5的字符开始，使用后面的value覆盖追加到原来字串。
​	getset k v ：先设置这个键的值为新值，在返回旧的值
​	incr k：使这个键的值加一但是必须是数字！！！
​	incrby k 数字：使这个键的值用后面的数字加
​	decr k ：使这个键的数字减一，同上。
​	decr k 数字：同加的。
​	append k v：向这个键的后面追加值。
​	strlen k：查看这个键的值有多少位。

#### 4、list列表类型常用命令（单key多value,有序可重复，双向链表）

​	lpush key v1 v2 ...：往左边压入value，添加一个k list键值对
​	rpush key v1 v2 v3：往右边压入value，添加。
​	lrange key 范围：如string的range,从左边开始依次从上往下输出list列表中的元素，（0 -1 输出所有值）
​	lpop key：弹出列表左边的第一个元素。
​	rpop key：如上，但是使弹出右边。
​	lindex key index：查看改索引在列表中对应的值。
​	llen key :查看列表的长度
​	lrem key count value : 删除列表中的多个值(lrem k1 2 v1，删除2个列表中值为v1，count为0表示删除所有）
​	ltrim key starti endi : 表示将该列表的开始索引到结束索引的值取出，重新赋给该列表。
​	rpoplpush key1 key2 : 将列表2中的最后一个值赋给列表1中的第一个值。
​	lset key index value : 设置列表中指定的缩影值（注意不要超出列表的长度）。
​	linsert key before|after v1 v2 : 添加一个值，在列表的v1之前或者之后,添加一个v2。
​	它是一个字符串链表，left、right都可以插入添加；
​	如果键不存在，创建新的链表；
​	如果键已存在，新增内容；
​	如果值全移除，对应的键也就消失了。
​	链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。

#### 5、set集合类型常用命令（单key多value，无序不可重复）

​	sadd k v1 v2... : 添加一个集合，值重复添加也只会添加一个。
​	smembers k : 查看集合中的成员。
​	sismembers k v : 查看集合中的是否存在v这个值存在返回1，不存在返回0。
​	spop k : 随机弹出一个成员。
​	srem k v : 删除集合中的一个成员。
​	scard k : 查看集合中有多少个成员。
​	srandmembers k count : 随机查看队列中count个元素，如果count为负数则可能查看的集合可能会重复。
​	smove k1 k2 member : 将k1中指定的成员移动到k2中。
​	sdiff k1 k2 : 取两个集合的差集。
​	sinsert k1 k2 : 取连个集合的交集。
​	sunion k1 k2 : 取连个集合的并集。

#### 6、hash类型的常用命令

**（重要！！！！！！！！k kv类型，相当于map<String,Object> 可以与实体类挂钩，如：hset user username zhangsan;hset user pwd 123;...)**

​	hset k1 f1 v1 : 添加一个集合元素为f1 v1。
​	hget k1 f1 : 查看集合中的元素的值。
​	hmset k1 fi v1 f2 v2 ... : 为集合添加多个值。
​	hmget k1 f1 f2 f3 : 查看集合中的多个元素的值。
​	hsetnx k1 f1 v1 : 为集合添加一个元素，如果该元素不存在的话，反之不添加。
​	hdel k1 f1 : 删除集合中的元素。
​	hgetall k1 : 查看集合中的所有fv。
​	hexists k1 f1 : 查看集合中是否存在这个元素。
​	hkeys k1 : 获取集合中的所有keys
​	hvals k1 : 获取集合中素有的values
​	hincrby k1 f1 i : 使集合中的值增加i
​	hincrbyfloat k1 f1 i : 同上增加i(浮点数)。
​	

#### 7、zset类型的集合与set类似

**在set基础上多了个score ，如：k1 score1 v1 score2 v2。常用方法： 一个值包括s1 v1**。

​	zadd k1 s1 v1 s2 v2 ... : 增加一个集合，值为v1 v2 ...。
​	zrange k1 0 -1 : 查看集合中的所有元素。withscores(查看值与scores一起查看）。
​	zrevrange k1 0 -1 : 结果为上的逆向输出。
​	zrangebyscores k1 0 -1 withscores limit 2 2： 查看所有的scores与value，在从中略过两条查看之后的两条信息。
​	zrevrangebyscores k1 0 -1 : 结果与上相反。
​	zram k1 v1 : 删除集合中的元素。
​	zrank k1 v1 : 获取集合中元素的下标。
​	zrevrank k1 v1 : 与上相反。
​	zcard k1 : 获取集合中的所有元素的个数。
​	zcount k1 minscores maxscores : 查看指定范围中的元素的个数。	

#### 8、redis配置文件说明！

##### 8、1 开篇

```
# Redis configuration file example.
# Redis配置文件示例。

# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
# 请注意，为了读取配置文件，必须以文件路径作为第一个参数＃启动Redis：
# ./redis-server /path/to/redis.conf

# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
# 关于单位的注意事项：当需要内存大小时，可以以通常的1k 5GB 4M形式指定等等：

# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
# 单位不区分大小写，因此1GB 1Gb 1gB都相同。
```

##### 8、2 includes模块

```
includes模块
# 在此处包含一个或多个其他配置文件。
# 如果您具有可用于所有Redis服务器的标准模板，但还需要自定义一些每服务器设置，则此功能很有用。
# 包含文件可以包含个其他文件，因此请明智地使用此文件。
# 注意选项“ include”将不会被admin或Redis Sentinel中的命令“ CONFIG REWRITE”重写。由于Redis始终使用最后处理的
# 行作为配置指令的值，因此最好在此文件的开头放置，以免在运行时覆盖配置更改。
# 如果您反而对使用include覆盖配置选项感兴趣，则最好将include作为最后一行。
```

##### 8、3 MODULES模块

```
MODULES模块
# loadmodule /path/to/my_module.so
# loadmodule /path/to/other_module.so
在启动时加载模块。如果服务器无法加载模块，它将中止。可以使用多个loadmodule指令。
```

##### 8、4 NETWORK模块

```
NETWORK模块

配置redis绑定的服务器ip
bind 127.0.0.1

是否开启保护模式
protected-mode yes

配置redis绑定的端口
port 6379

设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。
在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值，所以需要确认增大somaxconn和tcp_max_syn_backlog两个值
来达到想要的效果
tcp-backlog 511

设置Unix socket.没有默认值，因此未指定时，Redis将不会在Unix套接字上监听。
# unixsocket /tmp/redis.sock
# unixsocketperm 700

设置超时时间,客户端闲置N秒后关闭连接（0禁用）
timeout 0

设置心跳时间
tcp-keepalive 300
```

##### 8、5 GENERAL模块

```
GENERAL模块
设置redis是否问守护进程：默认情况下，Redis不会作为守护程序运行。如果需要，请使用“是”。 ＃注意，Redis守护进程将在/var/run/redis.pid中写入一个pid文件。
daemonize no

可以通过upstart和systemd管理Redis守护进程
选项：
   supervised no - 没有监督互动
   supervised upstart - 通过将Redis置于SIGSTOP模式来启动信号
   supervised systemd - signal systemd将READY = 1写入$ NOTIFY_SOCKET
   supervised auto - 检测upstart或systemd方法基于 UPSTART_JOB或NOTIFY_SOCKET环境变量
supervised no


配置PID文件路径，当redis作为守护进程运行的时候，它会把 pid 默认写到 /var/redis/run/redis_6379.pid 文件里面
pidfile /var/run/redis_6379.pid

设置日志的级别
loglevel notice
定义日志级别。
  可以是下面的这些值：
  debug（记录大量日志信息，适用于开发、测试阶段）
  verbose（较多日志信息）
  notice（适量日志信息，使用于生产环境）
  warning（仅有部分重要、关键信息才会被记录）

设置是否要启用到系统记录器的日志记录
syslog-enabled no
要想把日志记录到系统日志，就把它改成 yes，也可以可选择性的更新其他的syslog 参数以达到你的要求

设置数据库的总数默认是16个0~15
databases 16
设置数据库的数目。默认的数据库是DB 0 ，可以在每个连接上使用select  <dbid> 命令选择一个不同的数据库，dbid是一个介于0到databases - 1 之间的数值。

设置是否输出redis的logo（大蛋糕）
always-show-logo yes
```

##### 8、6 SNAPSHOTTING模块

```
SNAPSHOTTING模块
    设置间隔多少秒有多少次操作就将redis数据存入硬盘中：格式：save <间隔时间（秒）> <写入次数>
    根据给定的时间间隔和写入次数将数据保存到磁盘
    下面的例子的意思是：
    900 秒内如果至少有 1 个 key 的值变化，则保存
    300 秒内如果至少有 10 个 key 的值变化，则保存
    60 秒内如果至少有 10000 个 key 的值变化，则保存
 　　
    注意：你可以注释掉所有的 save 行来停用保存功能。
    也可以直接一个空字符串来实现停用：
    #save ""
  	save 900 1
    save 300 10
    save 60 10000
    
  stop-writes-on-bgsave-error yes
  如果用户开启了RDB快照功能，那么在redis持久化数据到磁盘时如果出现失败，默认情况下，redis会停止接受所有的写请求。
  这样做的好处在于可以让用户很明确的知道内存中的数据和磁盘上的数据已经存在不一致了。
  如果redis不顾这种不一致，一意孤行的继续接收写请求，就可能会引起一些灾难性的后果。
  如果下一次RDB持久化成功，redis会自动恢复接受写请求。
  如果不在乎这种数据不一致或者有其他的手段发现和控制这种不一致的话，可以关闭这个功能，
  以便在快照写入失败时，也能确保redis继续接受新的写请求。
  
  rdbcompression yes
  对于存储到磁盘中的快照，可以设置是否进行压缩存储。
  如果是的话，redis会采用LZF算法进行压缩。如果你不想消耗CPU来进行压缩的话，
  可以设置为关闭此功能，但是存储在磁盘上的快照会比较大。
  
  rdbchecksum yes
  在存储快照后，我们还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，
  如果希望获取到最大的性能提升，可以关闭此功能。
  
  dbfilename dump.rdb
  设置快照的文件名

  dir /var/redis/6379
   设置快照文件的存放路径，这个配置项一定是个目录，而不能是文件名
```

##### 8、7 REPLICATION模块

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106153400324.png" alt="image-20210106153400324" style="zoom:80%;" />

##### 8、8 CLIENTS模块

```
CLIENTS
maxclients 10000
设置redis同时可以与多少个客户端进行连接。默认情况下为10000个客户端。当你
无法设置进程文件句柄限制时，redis会设置为当前的文件句柄限制值减去32，因为redis会为自
身内部处理逻辑留一些句柄出来。如果达到了此限制，redis则会拒绝新的连接请求，并且向这
些连接请求方发出“max number of clients reached”以作回应。
```

##### 8、9 MEMORY MANAGEMENT模块

```
MEMORY MANAGEMENT
maxmemory-policy noeviction
当内存使用达到最大值时，redis使用的清楚策略。有以下几种可以选择：
  1）volatile-lru   利用LRU算法移除设置过过期时间的key (LRU:最近使用 Least Recently Used )
  2）allkeys-lru   利用LRU算法移除任何key
  3）volatile-random 移除设置过过期时间的随机key
  4）allkeys-random  移除随机ke
  5）volatile-ttl   移除即将过期的key(minor TTL)
  6）noeviction  noeviction   不移除任何key，只是返回一个写错误 ，默认选项
  
  maxmemory-samples 5
  LRU 和 minimal TTL 算法都不是精准的算法，但是相对精确的算法(为了节省内存)
  随意你可以选择样本大小进行检，redis默认选择3个样本进行检测，你可以通过maxmemory-samples进行设置样本数
```

##### 8、10 APPEND ONLY MODE模块

```
APPEND ONLY MODE
appendonly no
  默认redis使用的是rdb方式持久化，这种方式在许多应用中已经足够用了。但是redis如果中途宕机，
  会导致可能有几分钟的数据丢失，根据save来策略进行持久化，Append Only File是另一种持久化方式，
  可以提供更好的持久化特性。Redis会把每次写入的数据在接收后都写入appendonly.aof文件，
  每次启动时Redis都会先把这个文件的数据读入内存里，先忽略RDB文件。

appendfilename "appendonly.aof"
  aof文件名
 
appendfsync always
appendfsync everysec
appendfsync no
  aof持久化策略的配置
  no表示不执行fsync，由操作系统保证数据同步到磁盘，速度最快。
  always表示每次写入都执行fsync，以保证数据同步到磁盘。
  everysec表示每秒执行一次fsync，可能会导致丢失这1s数据

 
no-appendfsync-on-rewrite no
   在aof重写或者写入rdb文件的时候，会执行大量IO，此时对于everysec和always的aof模式来说，
   执行fsync会造成阻塞过长时间，no-appendfsync-on-rewrite字段设置为默认设置为no。
   如果对延迟要求很高的应用，这个字段可以设置为yes，否则还是设置为no，这样对持久化特性来说这是更安全的选择。
   设置为yes表示rewrite期间对新写操作不fsync,暂时存在内存中,等rewrite完成后再写入，默认为no，建议yes。
   Linux的默认fsync策略是30秒。可能丢失30秒数据。

 
auto-aof-rewrite-percentage 100
  aof自动重写配置，当目前aof文件大小超过上一次重写的aof文件大小的百分之多少进行重写，
  即当aof文件增长到一定大小的时候，Redis能够调用bgrewriteaof对日志文件进行重写。
  当前AOF文件大小是上次日志重写得到AOF文件大小的二倍（设置为100）时，自动启动新的日志重写过程。

 
auto-aof-rewrite-min-size 64mb
  设置允许重写的最小aof文件大小，避免了达到约定百分比但尺寸仍然很小的情况还要重写

aof-load-truncated yes
  aof文件可能在尾部是不完整的，当redis启动的时候，aof文件的数据被载入内存。
  重启可能发生在redis所在的主机操作系统宕机后，尤其在ext4文件系统没有加上data=ordered选项，出现这种现象
  redis宕机或者异常终止不会造成尾部不完整现象，可以选择让redis退出，或者导入尽可能多的数据。
  如果选择的是yes，当截断的aof文件被导入的时候，会自动发布一个log给客户端然后load。
  如果是no，用户必须手动redis-check-aof修复AOF文件才可以。
```

##### 8、11常用配置

参数说明
redis.conf 配置项说明如下：
1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
    daemonize no
2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
    pidfile /var/run/redis.pid
3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字
    port 6379
4. 绑定的主机地址
    bind 127.0.0.1
5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
    timeout 300
6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
    loglevel verbose
7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
    logfile stdout
8. 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
    databases 16
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
    save <seconds> <changes>
    Redis默认配置文件中提供了三个条件：
    save 900 1
    save 300 10
    save 60 10000
    分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。

10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
    rdbcompression yes
11. 指定本地数据库文件名，默认值为dump.rdb
    dbfilename dump.rdb
12. 指定本地数据库存放目录
    dir ./
13. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
    slaveof <masterip> <masterport>
14. 当master服务设置了密码保护时，slav服务连接master的密码
    masterauth <master-password>
15. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭
    requirepass foobared
16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
    maxclients 128
17. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
    maxmemory <bytes>
18. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
    appendonly no
19. 指定更新日志文件名，默认为appendonly.aof
      appendfilename appendonly.aof
20. 指定更新日志条件，共有3个可选值： 
    no：表示等操作系统进行数据缓存同步到磁盘（快） 
    always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全） 
    everysec：表示每秒同步一次（折衷，默认值）
    appendfsync everysec

21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）
      vm-enabled no
22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
      vm-swap-file /tmp/redis.swap
23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
      vm-max-memory 0
24. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值
      vm-page-size 32
25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。
      vm-pages 134217728
26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
      vm-max-threads 4
27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
    glueoutputbuf yes
28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
    hash-max-zipmap-entries 64
    hash-max-zipmap-value 512
29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
    activerehashing yes
30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
    include /path/to/local.conf

#### 9、redis的持久化

在指定的时间间隔内将内存中的数据集快照写入磁盘，
也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到
一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。
整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能
如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方
式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。

Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）
数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程

##### 9、1 RDB（Redis DataBase）

**Rdb 保存的是dump.rdb文件**

**RDB的配置在配置文件8、4快照中配置**

在修改配置文件的时候先冷备份下以免改错了。

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106163029788.png" alt="image-20210106163029788" style="zoom:80%;" />

**如何触发redis的快照：在配置文件快照模块中的save配置指定多少秒，修改了多少条数据，来判断是否保存快照。**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106163204377.png" alt="image-20210106163204377" style="zoom:89%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106163333495.png" alt="image-20210106163333495" style="zoom:76%;" />

（**flushall与shutdown也会产生dump.rdb文件,导致redis加载的是清空后的dump.rdb）**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106163512226.png" alt="image-20210106163512226" style="zoom: 84%;" />

如何恢复数据：将备份文件 修改为(dump.rdb) 启动服务即可

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106164546689.png" alt="image-20210106164546689" style="zoom:64%;" />

CONFIG GET dir获取工作目录

![image-20210106164637426](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106164637426.png)

有很重要的数据想保存：

**Save：save时只管保存，其它不管，全部阻塞**

**BGSAVE：Redis会在后台异步进行快照操作，**
**快照同时还可以响应客户端请求。可以通过lastsave**
**命令获取最后一次成功执行快照的时间**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106164938656.png" alt="image-20210106164938656" style="zoom:97%;" />

rdb的优势：适合大规模的数据恢复、对数据完整性和一致性要求不高

劣势：在一定间隔时间做一次备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改、Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106160530844.png" alt="image-20210106160530844" style="zoom:80%;" />

##### 9、2  AOF（Append Only File）rdb大部分情况已经够用

**以日志的形式来记录每个写操作**，将Redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

**Aof保存的是appendonly.aof文件**

**aof的开始在配置文件中8、10append only mode 模块**

**启动aof**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106195821773.png" alt="image-20210106195821773" style="zoom:80%;" />

正常恢复：将有数据的aof文件复制一份保存到对应工作目录(config get dir)，重启redis服务

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106200453958.png" alt="image-20210106200453958" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106200716218.png" alt="image-20210106200716218" style="zoom:98%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106200917118.png" alt="image-20210106200917118" style="zoom:80%;" />

异常恢复：乱改aof文件在重启

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106201213397.png" alt="image-20210106201213397" style="zoom:90%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106201500009.png" alt="image-20210106201500009" style="zoom: 57%;" />

![image-20210106201635885](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106201635885.png)

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106201915732.png" alt="image-20210106201915732" style="zoom:60%;" />

##### 9、3 Rewrite

AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制,
当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩，
只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof

AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，
遍历新进程的内存中数据，每条记录有一条的Set语句。重写aof文件的操作，并没有读取旧的aof文件，
而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似

**Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发，在如下配置i文件下修改**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106202507346.png" alt="image-20210106202507346" style="zoom:90%;" />

**aof优势：每修改同步：appendfsync always   同步持久化 每次发生数据变更会被立即记录到磁盘  性能较差但数据完整性比较好**

**每秒同步：appendfsync everysec    异步操作，每秒记录   如果一秒内宕机，有数据丢失**

**不同步：appendfsync no   从不同步**

**劣势：相同数据集的数据而言aof文件要远大于rdb文件，恢复速度慢于rdb**

**Aof运行效率要慢于rdb,每秒同步策略效率较好，不同步效率和rdb相同**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106202722949.png" alt="image-20210106202722949" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106203053887.png" alt="image-20210106203053887" style="zoom:80%;" />

#### 10、redis的事务

##### 10、1 事务是什么

可以一次执行多个命令，本质是一组命令的集合。一个事务中的

所有命令都会序列化，按顺序地串行化执行而不会被其它命令插入，不许加塞

一个队列中，一次性、顺序性、排他性的执行一系列命令

##### 10、2 正常执行事务

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106203757355.png" alt="image-20210106203757355" style="zoom:80%;" />

##### 10、3 放弃事务

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106204003300.png" alt="image-20210106204003300" style="zoom:90%;" />

##### 10、4 全体连坐（在事务执行过程中的语法错误）

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106204324986.png" alt="image-20210106204324986" style="zoom:70%;" />

##### 10、5 冤头债主（语法没有错误，但是实际出现错误）

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106204742537.png" alt="image-20210106204742537" style="zoom:80%;" />

##### 10、6 watch监控（乐观锁）

 **悲观锁(**Pessimistic Lock), 顾名思义，就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁

 乐观锁(Optimistic Lock), 顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，

**乐观锁**策略:提交版本必须大于记录当前版本才能执行更新

**CAS(Check And Set)**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106210101115.png" alt="image-20210106210101115" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106210227949.png" alt="image-20210106210227949" style="zoom:70%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106210325636.png" alt="image-20210106210325636" style="zoom:85%;" />

![image-20210106210406636](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106210406636.png)

#### 11、主从复制

也就是我们所说的主从复制，主机数据更新后根据配置和策略，
自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主

能干嘛：读写分离、容灾恢复

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106211610671.png" alt="image-20210106211610671" style="zoom:80%;" />



##### 11、1 一主二从

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106213715253.png" alt="image-20210106213715253" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106213909986.png" alt="image-20210106213909986" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106214019535.png" alt="image-20210106214019535" style="zoom:80%;" />

**主机之前有的值从机也可以读到。**

**主能写操作，从不能从只能读**

**主down，从不变等待主机回来，之后照旧**

**从down，上线需要重新设置主机，除非写入配置文件**

##### 11、2 薪火相传

上一个Slave可以是下一个slave的Master，Slave同样可以接收其他
slaves的连接和同步请求，那么该slave作为了链条中下一个的master,
可以有效减轻master的写压力

中途变更转向:会清除之前的数据，重新建立拷贝最新的

**从三角关系变为现在的：主->从->从关系**

##### 11、3 反客为主

**主down,挑选一个从变成主，现在81就有了两个选择，等待原主机的回归或者现在认80为主机**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106215701858.png" alt="image-20210106215701858" style="zoom:80%;" />

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106215903174.png" alt="image-20210106215903174" style="zoom:87%;" />

![5](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106220026003.png)

##### 11、4 哨兵模式（自动的反客为主）

**复制的原理**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106220213443.png" alt="image-20210106220213443" style="zoom:80%;" />

**配置哨兵的步骤**

**自定义的/myredis目录下新建sentinel.conf文件，名字绝不能错**

 **配置文件中的内容：sentinel monitor 被监控数据库名字(自己起名字) 127.0.0.1 6379 1**

​	**sentinel monitor host6379 127.0.0.1 6379 1**

**上面最后一个数字1，表示主机挂掉后salve投票看让谁接替成为主机，得票数多少后成为主机**

**启动哨兵：**

**redis-sentinel /opt/newredis/aof/sentinel.conf (自己的配置文件的位置)**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106221224380.png" alt="image-20210106221224380" style="zoom: 67%;" />

**主机down**

<img src="C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106221344344.png" alt="image-20210106221344344" style="zoom: 67%;" />

![image-20210106221422983](C:\Users\takagi\AppData\Roaming\Typora\typora-user-images\image-20210106221422983.png)

**主回哨兵投票，一组sentinel能同时监控多个Master**

**缺点：由于所有的写操作都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。**

#### 12、springboot集成redis

```xml
加入依赖 		
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

自动配置：

- RedisAutoConfiguration 自动配置类。RedisProperties 属性类 --> **spring.redis.xxx是对redis的配置**
- 连接工厂是准备好的。**Lettuce**ConnectionConfiguration、**Jedis**ConnectionConfiguration
- **自动注入了RedisTemplate**<**Object**, **Object**> ： xxxTemplate；
- **自动注入了StringRedisTemplate；k：v都是String**
- **key：value**
- **底层只要我们使用** **StringRedisTemplate、****RedisTemplate就可以操作redis**