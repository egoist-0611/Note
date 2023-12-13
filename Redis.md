## 基础部分

### 介绍与安装

#### Redis基本介绍

redis是一款存储于内存的数据库，通过 key-value 查询数据，与mysql是互相合作的关系

若是将所有的数据都交由mysql处理，mysql会不堪重负。因此，redis就是挡在mysql前面，替mysql分担工作的数据库

要想为mysql分担工作，就需要其性能及功能上能与mysql匹敌：

1. mysql将数据存储在硬盘上的；而redis是将数据存储在内存的（也可持久化到硬盘上）
2. 对于查询，mysql可以全表查询，效率会很低，只能通过条件进行部分查询提高速率；redis是通过键值对来查询的，速度很快
3. mysql是传统的RDBMS关系型数据库，而redis是NOSQL非关系型数据库



在有了redis之后，查询数据时，会先去redis中查看是否有对应的数据；若没有，再去mysql中查询；在mysql查询之后，会将数据缓存在redis中；下次查询时，就可以直接在redis中获取到数据了







#### 下载、安装、配置

##### 下载与安装

redis下载：https://redis.io，版本选择：第二位为偶数，则为稳定版本（如7.2.1）

安装要求：

1. Linux为64位（使用命令 `getconf LONG_BIT` 查看）
2. Linux安装了GCC编译环境（redis是用C语言编写的）
   - `gcc -v`：查看是否安装了gcc
   - `yum -y install gcc-c++`：安装gcc

安装步骤：

1. 将redis文件传至Linux服务器，并存放于 /opt（一般用于存放第三方软件的目录）下
2. 解压radis.tar.gz（`tar -zxf 压缩文件.tar.gz`）
3. 进入到解压后的主目录，运行命令 `make && make install`  进行编译安装





##### 安装目录介绍

默认安装在：/usr/local/bin 目录下（相当于window下的 c:/program file），会有6个文件：

- redis-server：Redis服务器启动命令
- redis-cli：客户端入口
- redis-sentinel：redis集群
- redis-benchmark：测试性能工具
- redis-check-aof、redis-check-dump：修复有问题的aof / rdb 文件





##### 配置文件

redis的启动需要依赖配置文件

配置文件位于解压目录下，名为 redis.conf（复制，不再原文件上进行修改）

修改配置文件默认值：

1. daemonize no 改为 `daemonize yes`（允许redis后台运行）
2. protected-mode yes 改为 `protected-mode no`（关闭保护模式，外部可以访问该redis服务器）
3. bind 127.0.0.1 注释掉（只允许本机访问，不允许远程IP连接，关闭即可）
4. requirepass 取消注释，并在后面添加密码（为redis服务器添加密码）
5. port 6379 默认redis服务器使用的端口号
6. logfile "/路径/日志文件.log" 可以配置生成的日志文件保存的位置

> /xxx ：查询（n，下一个）
>
> :set nu：显示行号





##### 运行

运行redis服务器：

- 在 /usr/local/bin 下的redis，通过指定redis.conf配置文件后，是可以直接运行redis服务器的

  > `ps -ef | grep redis` 可以查看到，redis服务器运行后，默认会占用6379端口号

运行redis客户端：

- `redis-cli` 启动客户端
  - 若是非默认端口号运行的redis服务器，则需要使用 `-p` 指定服务器使用的端口号
  - 若是有密码，可以使用 `-a` 指定密码，也可以在进入客户端后，通过 `auth 密码` 来连接到服务器
  - 若是出现中文乱码，则使用 `--raw` 可以解决

存储键值对：

- `set key名 value值`：存储key所对应的value
- `get key名`：通过key获取对应的value

退出redis客户端：`quit`

关闭redis服务器：

- `shutdown` 直接关闭
- 若是未在客户端内想关闭redis服务器，可以通过 `redis-cli shutdown` 命令关闭（可能需要指定密码或端口号）











### 常用命令

> 命令不区分大小写，但key区分
>
> `help @类型`：获取对应类型的帮助手册

`keys *`：查看当前库中所有的key

`dbsize`：查看当前库中key的数量



`config get 关键字`：获取当前redis服务器使用的配置文件中，指定关键字所对应的设置的值



`exists key名1 key名2`：判断指定的key存在几个

`type key名`：判断key存储的是什么类型



`del key名1 key名2`：删除指定的key（阻塞删除），返回删除的个数

`unlink key名`：将key从keyspace元数据中删除，但真正的删除在后续异步中进行（非阻塞删除）



`ttl key名`：查看key还有多少秒过期（-1表示永不过期，-2表示已过期）

`expire key名 秒数`：设置指定的key多少秒后过期



`move key名 数据库下标`：将指定的key移动到指定的数据库（redis提供了16个数据库，下标从0-15，默认为0）

`select 数据库下标`：切换数据库



`flushdb`：清空当前数据库

`flushall`：清空所有数据库













### 数据类型

#### String

> 字符串，最基本的数据类型，一个key对应一个value，是二进制安全的（可序列化）

`set key名 value值` [NX|XX]  [GET]  [EX | PX | EXAT | PXAT | KEEPTTL]

- NX：当key不存在时才设置value

  XX：当key存在时才设置value

- GET：在设置value前，返回key之前的值

  > set key名 value值 get ==》 getset key名 value值

- EX：设置过期时间（秒）

  PX：设置过期时间（毫秒）

  EXAT：设置过期时间（UNIX中的秒）

  PXAT：设置过期时间（UNIX中的毫秒）

  KEEPTTL：当为某个key设置了过期时间后，若在修改其值时，不再指定其过期时间，那么就会恢复为永不过期。通过该属性，可以使该key在修改值后，过期时间依旧存在



`mset key名1 value值1 key名2 value值2`：连续设置多个key-value

`msetnx key名1 value值1 key名2 value值2`：当所有的key都不存在时才会设置（一个存在，都不设置）

`mget key名1 key名2`：连续获取多个key对应的value



set key名 value值（字符串）

- `getrange key名 起始下标 结束下标`：获取value值（字符串）起始下标到结束下标间的字符串（包括结束下标。起始下标从0开始，结束下标可以-1表示）
- `setrange key名 起始下标 value值`：从起始下标处开始，替换为value值
- `strlen key名`：获取value值（字符串）的长度
- `append key名 xxx`：在value值后追加xxx



set key名 value值（数值）

- `incr key名`：value值+1

  `incrby key名 x`：value值+x

- `decr key名`：value值-1

  `decrby key名 x`：value值-x









#### List

> 字符串列表，可按顺序从头部或尾部插入（底层是双端链表）

`lpush key名 value值1 value值2`：存入value值1后，再向左边存入value值2

`rpush key名 value值1 value值2`：存入value值1后，再向右边存入value值2



`lrange key名 起始下标 结束下标`：从左往右，显示存入的、指定下标（从0开始）所对应的value值

`lindex key名 下标`：显示指定下标处的值

`llen key名`：获取集合存储的个数



`lpop key名`：弹掉集合中（删掉）存储在最左边的值

`rpop key名`：弹掉集合中存储在最右边的值



`lrem key名 x value值`：集合中可以存多个相同的值。删除集合中，x个指定的值

`ltrim key名 起始下标 结束下标`：仅保留集合中，起始下标到结束下标间的值（包含结束下标）



`rpoplpush key名1 key名2`：将集合1中的最右边一个值，移动到集合2的最左边

`lset key名 下标 value值`：替换集合中指定下标处的值

`linsert key名 before value值1 value值2`：在指定的value1前，插入value值2









#### Hash

> 哈希表，存储的是 field（字段）- value（值），字段一般为string类型，存储的值多为对象

`hset key名 field名1 value值1 field名2 value值2`：创建名为key的哈希表，添加一个个field（key）-value

`hsetnx key名 field名 value值`：创建名为key的哈希表，若表中不存在field，则添加对应的field-value



`hget key名 field名`：获取哈希表内field所对应的value值

`hmget key名 field名1 field名2`：一次性获取哈希表内多个field所对应的value值



`hgetall key名`：获取所有的field、value

`hlen key名`：获取field-value的个数

`hexists key名 field名`：判断是否存在指定的field（1表示存在，0表示不存在）



`hkeys key名`：查看全部的field

`hvals key名`：查看全部的value



`hdel key名 field名1 field名2`：删除指定的field-value



`hincrby key名 field名 x`：将field所对应的value值（整型类型）+x

`hincrbyfloat key名 field名 x`：将field所对应的value值（浮点/数值类型）+x









#### Set

> 字符串类型的无序集合，集合成员唯一（底层通过哈希表实现）

`sadd key名 value值1 value值2`：往set集合中，存储多个不重复的value值（若重复会自动去重）

`smembers key名`：获取set集合所有的value



`sismember key名 value值`：判断set集合中是否存在指定的value（存放返回1，不存在返回0）

`scard key名`：获取set集合中value的个数

`srandmember key名 x`：随机获取set集合中的x个value



`srem key名 value值1 value值2`：删除set集合中指定的value

`spop key名 x`：随机删除set集合中的x个value



`smove key名1 key名2 value值1`：将集合1中的value1，移动到集合2中



sadd key名1 1 2 3 a b；sadd key名2 a b c 1 2

- `sdiff key名1 key名2`：取出集合1不存在于集合2中的value
- `sunion key名1 key名2`：列出集合1与集合2合并后，所有的value
- `sinter key名1 key名2`：列出集合1与集合2共有的value
- `sintercard 2+x key名1 key名2` [limit y]：列出集合1、集合2 .... 共有的value的个数
  - 2+x：集合1、集合2 ... 的个数
  - limit y：指定最大的value的个数为y（超过该值时仍以y为主）









#### ZSet

> 字符串类型的有序集合，通过关联一个double类型的分数（score），确保集合中的成员从小到大排序（集合成员唯一，但分数是可以重复的，底层也是通过哈希表实现）

`zadd key名 score值1 value值1 score值2 value值2`：创建一个zset集合，存储一个个score（数值类型）-value（score-value会按score值从小到大进行排序）



`zrange key名 起始下标 结束下标`：获取zset集合中起始下标到结束下标间的value

`zrange key名 起始下标 结束下标 withscores`：获取zset集合中起始下标到结束下标间的score-value

`zrevrange key名 起始下标 结束下标 withscores`：反转获取zset集合中起始下标到结束下标间的score-value



`zrangebyscore key名` [(] `score值1 score值2` [withscores] [limit 起始下标 结束下标]：获取score值1到score值2间，所对应的value（包含score值1和score值2）

- (：使获取的value值中，不包含score值1所对应的
- withscores：是否显示score
- limit：获取到value后，显示指定起始下标到结束下标间的value（不包含结束下标）



`zscore key名 value值`：获取zset集合中，指定value所对应的score

`zcard key名`：获取zset集合中，score-value的个数

`zcount key名 score值1 score值2`：获取zset集合中，score值大小 在score值1到score值2间的 score-value的个数



`zrank key名 value值`：获取value所在的下标（从0开始）

`zrevrank key名 value值`：逆序，获取value所在的下标



`zrem key名 value值`：根据value，移除zset集合中score-value

`zmpop 1 key名 min count x`：移除zset集合中x个score-value，按score值从小到大依次移除



`zincrby key名 x value值`：value所对应的score的值+x







#### BitMap

> 位图，由0和1字符串表示的二进制bit数组

`setbit key名 下标 值`：设置位图指定下标处（从0开始）的值为0或1

> 举例：setbit k1 0 1 ==》1000 0000 ；setbit k2 1 1 ==》0100 0000

`getbit key名 下标`：获取位图指定下标处的值



`strlen key名`：获取位图共占用的字节大小（8bit = 1byte）

`bitcount key名`：获取位图中值为1的个数

`get key名`：获取该位图转换为十进制后的值



`bitop and key名0 key名1 key名2`：将位图1和位图2进行 与运算，最后结果赋值给位图0







#### HyperLogLog

> 用来做基数（不重复的数、去重）统计的 算法 ，特点是：输入的元素数量非常大时，计算基数所需要占用的空间总是固定且很小的（数据类型为字符串类型，但是通过 `get key名` 并不能获取到存储的数值，因为HyperLogLog只是算法）

`pfadd key名 值1 值2`：将值1、值2添加进该HypeLogLog中

`pfcount key名`：获取指定HypeLogLog去重后，值的个数

`pfmerge key名0 key名1 key名2`：将HypeLogLog1和HypeLogLog2混合在一起、去除重复的值后，将最后剩余的值的个数赋值给HypeLogLog0

> 





#### GEO

> 存储地理位置信息

`GEOADD key名 x坐标1 y坐标1 名称1 x坐标2 y坐标2 名称2`：添加名称1、名称2的坐标信息



`geopos key名 名称1 名称2`：获取名称对应的x坐标、y坐标

`geohash key名 名称1 名称2`：将名称对应的坐标转换为hash进行表示



`geodist key名 名称1 名称2 km`：计算坐标1和坐标2间的距离，以km为单位（也可以用m作为单位）

`georadius key名 x坐标 y坐标 a km withdist withcoord withhash count b desc`：显示指定geo中的最多（b）条数据：距离x、y坐标 在 （a）km内 的名称、坐标、hash表示的坐标。按距离由远及近显示



`zrange key名 起始下标 结束下标`：因为存储的数据类型为zset，是可以用该方式来获取所有已存储的名称的

 







#### Stream

> 流，Redis5.0之后消息队列的实现（之前用List、Pub/Sub的方式）

##### 消息队列

`xadd key名 id值 field名1 value值1 field名2 value值2`：创建一个新的消息队列（若存在则加入），设置其id（相当于自增长列），添加相关的field-value

> Stream的创建需要一个不重复的id，该id可以由系统自动生成（`*` 表示），可以自己指定（时间戳+自增id，且后续创建的id不能小于前面创建的id）



`xrange key名 - +` [count x]：按照id从小到大，查看消息队列中所有的field-value

- `count x`：不显示所有，显示指定条数

`xrevrange key名 + -`：反转，按id从大到小显示



`xlen key名`：获取消息队列中的个数（每个id算一个）



`xdel key名 id值`：删除消息队列中，指定id所对应的field-value



`xtrim key名 maxlen x`：根据id值，从大到小依次截取x个（保留x个）

`xtrim key名 minid id值`：截取掉所有比该id值小的id所对应的field-value（不包括该id）



`xread count x` [block ms] `streams key名 $`：以当前已存储了的id值中，最大的那个id作为标准，获取x个大于该id值的field-value的信息（即读取消息队列的末尾处）

- `block ms`：获取过程处于阻塞（监听）状态，等待ms毫秒后才结束获取（0表示永远阻塞，直到获取到值）

`xread count x streams key名 0-0`：以当前已存储了的id值中，最小的那个id作为标准，获取x个大于该id值的field-value的信息（即读取消息队列的开头处）





##### 信息消费

`xgroup create key名 组名 $`：创建消息消费（读取）组，用于读取消息队列中的field-value

- $ 表示从末尾处开始读取消息队列（读取新增的），0-0 表示从头开始读取消息队列



`xreadgroup group 组名 客户名` [count x] `streams key名 >`：让消费组中的指定客户去读取消息队列中的信息

- 一旦消息被客户读取，该消费组内的其他客户将无法读取



`xpending key名 组名`：查看消费组对该消息队列的信息读取情况

`xpending key名 组名 - + x 客户名`：查看消费组内指定客户的x条信息读取情况



`xack key名 组名 id值`：对消费组内已读取的信息，进行确认响应（确认后，不再显示在信息读取情况列表中）









#### BitField

> 相当于一个储存二进制的数组，可以一次性操作多个比特位域（多个比特位），最终返回的是一个响应数组，对应操作的执行结果

`bitfield key名 get ix 起始下标`：获取该比特位域（数组）从指定位置开始，共x位，最终返回其转换为十进制的值（i 表示为有符号，u 表示为无符号）

`bitfield key名` [overflow 方式] `set ux 起始下标 值`：将该比特位域从指定位置开始，之后的x位，用指定的无符号十进制值替换（十进制转换为二进制后替换）



`bitfield key名` [overflow 方式] `incrby ix 起始下标 y`：将该比特位域从指定位置开始，之后的x位有符号数，进行 +y 的操作

- 当操作的位数达到对应的最大值时，会溢出（如：操作1位，值达到2即溢出；操作2位，值达到4则溢出）

- 可以设置溢出时的处理方法：

  wrap：默认，循环处理。当溢出时，有符号则从负数开始，无符号则从0开始，继续赋值

  sat：饱和处理。当溢出时，取最大值

  fail：拒绝处理。当溢出时，返回空值，并不进行赋值











### 持久化

> redis实现内存数据的持久化方式大体分为两种：RDB、AOF

#### RDB

在指定的时间间隔后，执行数据集的时间点快照

- 快照，即：将某一时刻的数据和状态以文件的形式写入到硬盘上（dump.rdb文件）

- 在redis.conf 配置文件中

  - 通过搜索 snapshots 关键字，可以查看到进行快照的时间点（6.0.16版本及之后会有所差异）

    `save x 变化次数`：当超过了指定次数的变化（修改或添加）时，就会保存快照，每x秒内只会保存一次

  - 通过搜索 dir 关键字，可以修改 dump.rdb 文件的保存路径

    `dir 文件保存路径`（文件夹必须存在）

  - 通过搜索 dbfilename 关键字，可以修改保存的 rdb 文件的默认名称

    `dbfilename 文件名.rdb`

  > 修改完配置文件后，要重启redis服务器

- 在进行 `flushdb`、`flushall` 命令时，会自动使用一份空的rdb文件，对原有的rdb文件进行覆盖

  在启动redis服务器、开启redis-cli客户端时，会自动读取 rdb文件，获取其内容
  
- 手动触发备份：

  1. `save`：执行该命令时，主进程会去进行rdb文件的备份，因此对于缓存功能，将会处于阻塞状态

  2. `bgsave`：执行该命令时，会开出一条子进程去备份rdb文件，因此redis的缓存功能依旧可以使用，不会阻塞

     `lastsave`：可以查看到上次子进程备份rdb文件成功时的时间戳（在命令行中通过 `date -d @时间戳` 可以查看具体时间）

- 禁用rdb备份功能：在配置文件中，使用 `save ""` 来表示快照保存时间点



rdb备份的缺点：满足一定条件后才会进行备份。若未对数据进行备份时，服务器宕机，有可能导致数据丢失或文件破损

使用 `redis-check-rdb rdb文件路径` 命令，可以一定程度的检查和修复该破损的rdb文件



优化配置：

1. stop-writes-on-bgsave-error：在后台rdb备份出错时，是否停止继续接收新的备份请求（默认为yes，确保保证备份数据的一致性）
2. rdbcompression：是否进行rdb备份文件的压缩（默认为yes）
3. rdbchecksum：是否检查rdb文件与数据是否一致（默认为yes，确保数据的一致性）
4. rdb-del-sync-files：当关闭rdb备份功能时，是否删除已备份的rdb文件（默认为no）









#### AOF

以日志的形式来记录每一个写的操作（读操作不记录），保存到一个aof文件中。在redis启动时，会读取该文件来重新构建数据

默认情况下，redis没有开启AOF功能，需要在配置文件中开启：`appendonly yes`



设置aof文件默认的保存路径：

- 在redis6及之前，aof文件保存的路径与rdb文件保存的路径相同，都是由 `dir 文件保存路径` 决定的
- 在redis7及之后，aof文件保存的路径，会在 dir 路径的基础上，再追加上一层目录：`appenddirname "文件夹名"`，然后在该文件夹内存放aof文件



设置aof文件默认保存的名字：`appendfilename "基础文件名.aof"`

redis7新特性：会使用MP-AOF的方式，将原先的单个aof文件拆分为多个aof文件，分别为：

1. base：基础aof文件（原始aof文件），一般由子进程重写产生，至多有一个
2. incy：增量aof文件（每次写入的aof文件），一般会在aofrw执行时创建，可以存在多个
3. history：历史aof文件，由base、incy演化而来，每次aofrw执行成功时，base和incy将合并为history。该文件会被redis自动删除
4. manifest：清单文件，追踪、管理aof文件



aof文件修复：若redis服务器宕机等，导致了aof文件出现错误，那么redis服务器将无法启动

- 使用命令进行检查修复：`redis-check-aof --fix incy文件名`



在接收到写入请求后，redis并不会将这些写入命令直接保存到aof文件中，而是将这些写入命令存入到aof缓存区中。根据设置好的写入策略，才将这些命令从缓存区写入到aof文件中

- 写入策略一共三种，在配置文件中，通过 appendfsync 关键字可以设置默认使用的写入策略：
  1. always：同步写入。每执行完写操作时，同步将缓存区中的该命令写入到aof文件中
  2. everysec：每秒写入。每隔1秒钟，将缓存区中的命令写入到aof文件中
  3. no：操作系统控制写入。在执行完写操作时，只会将写操作命令存放于缓存区中，由操作系统控制去写入到aof文件中

随着写入aof文件中的内容增加，会自动根据规则进行命令的合并（aof重写），从而使得aof文件被压缩

- 设置aof文件重写的时机：

  `auto-aof-rewrite-percentage 百分比值`、`auto-aof-rewrite-min-size 占用大小`

  相比上次重写的aof文件，大小增加了百分比，且文件也超过了指定大小，此时才会进行aof文件重写

- aof文件重写，并不是对原文件进行整理，而是直接读取服务器中现有的键值对，用一条命令去代替之前所有对该键值对操作的命令，最终将新生成的命令存放于一个新的aof文件中，代替旧的aof文件（重写后，新生成的base.aof文件中会记录整理后的命令，而新生成的incy.aof文件依旧会重新记录所有写入操作，在达到条件时，再对该文件进行整理）

- 手动触发aof文件重写：`bgrewriteaof`



优化配置：

1. no-appendfsync-on-rewrite：aof文件重写期间，是否停止将写入操作记录到aof文件中（默认为no）









#### 混合模式

rdb持久化方式，能够在指定时间间隔后对数据进行快照存储；aof持久化方式，会根据写入策略，将写操作写入到文件中进行备份

两种持久化方式结合，可以快速加载数据，且能尽可能的恢复数据：

1. 开启混合模式：`aof-use-rdb-preamble yes`

2. 两种持久化方式混合使用时，优先加载的是aof文件

   > 实际上，在重启服务器、加载备份时，先加载rdb文件，确保数据的基本完整，再加载aof文件，追加rdb可能没及时保存的数据







#### 纯净模式

当rdb和aof模式都禁用时，即为纯净模式（纯净模式下依旧可以使用命令进行备份）











### 事务

redis事务的作用，是保证事务内所有命令会被连续执行（单线程），期间不会去执行其他命令

因此：

1. redis事务不能保证原子性：不能确保所有的命令同时执行成功或失败（没有执行到一半失败后回滚的能力）
2. redis事务不存在隔离级别概念：事务提交前，所有的命令并没有被实际执行

redis的事务特性：在一个队列中，一次性、顺序性、排他性的执行完一系列命令集合



`multi`：开启事务

`exec`：结束事务，并执行事务队列中的所有代码

`discard`：结束事务，并取消事务队列中所有代码的执行

- 当编写事务队列中的代码时，出现语法错误（编译就出错），那么会直接报错，整个事务队列中的代码都不会执行
- 当运行事务队列中的代码时，执行过程出现错误，那么执行错误的那条语句不会执行，其他代码正常执行



`watch key名`：对某个key进行监视。当该key的值被修改时，那么下一个的事务执行将会失败（返回空）

`unwatch`：取消对所有key的监视

> watch充当乐观锁的作用：相当于在操作数据前会校验数据的一致性；若已被修改，则不会进行数据的操作
>
> 一旦执行了exec后，之前所有监视的key将会被取消监视；若客户端连接丢失，也会取消所有监视









### 管道

客户端向服务端发送命令，命令会排队执行，执行完后向客户端返回结果，而客户端通常在等待的过程中会进入阻塞状态

当频繁的向服务端发送命令时，服务端性能就会急剧下降。利用管道（pipeline）一次性发送多条命令给服务端，再一次性将结果进行响应，就可以减少客户端与redis间的交互，提高性能

使用方式：

在linux系统中，编写一个文件，内部书写要执行的命令。通过读取该文件，将内容作为结果集传给redis服务器

`cat 文件名 | redis-cli -a 密码 --pipe`

- 管道不具备原子性，若出现语法错误，后续的代码也会执行（不同于事务）
- 管道是一次性将多条命令发送给服务器（不同于事务：一条一条发送，接收到exec时执行命令）
- 管道执行是后台执行，不会阻塞其他命令的执行（事务执行时会阻塞其他命令的执行）
- 管道实现的原理是队列，确保命令执行的顺序性









### 复制与哨兵

#### 复制

- 主机（master）以写为主，从机（slave）以读为主，实现分工合作
- 当主机数据发生变化时，自动将新的数据信息同步到从机上（数据备份）

要实现主从复制的效果，需要准备3台linux服务器作为环境，并对其配置文件进行相关配置：

1. 配置：后台运行、可远程连接、关闭远程保护、指定rdb保存路径、日志信息、指定redis服务器密码

2. 设置从机访问主机redis服务器及其访问密码：`replicaof IP地址 端口号`、`masterauth 密码`

   > 从机想要从主机处同步获取数据，那么就需要主机的访问允许（主机密码）

3. 关闭主机防火墙（`systemctl stop firewalld`）

4. 先启动主机，再启动从机，通过 `info replication` 命令可查看主从机间的关系

   - 从机只能读取内容，不能执行写入命令
   - 只要配置了主从关系，那么从机会时刻同步主机的数据，保证数据的一致性
   - 主机宕机后，从机不会替代主机的位置；当主机恢复后，会重新恢复主从关系

可以使用命令 `slaveof 主机ip 服务器端口号` 来临时为从机添加依赖主机（当从机服务器重启后即失效，且需要确保主机没有密码，否则需要进入配置文件指定 masterauth 主机密码）



当与主机建立连接的从机数量过多时，就可能导致数据拥堵。将主机下的一台从机，作为另一台从机的主机，可以有效减小主机同步数据的压力

`slaveof 从机ip 从机端口号`：使用该命令的从机，将会把目标从机作为主机（该主机实际上是真正主机下的一台从机，本身也是不允许进行写命令的执行的，该效果在使用该命令的从机的服务重启时失效）



`slaveof no one`：可以使从机解除与主机间的关系，自身变更为主机（重启服务时失效）









#### 哨兵

在生产环境中，仅有复制是不行的。当复制中的主机宕机时，从机只会傻傻等待主机恢复，期间不能进行写操作

我们会定义几个吹哨人，监控后台的主机是否出现故障；当主机出现了故障，则会根据投票数自动将主机下的一台从机转换为新的主机，继续对外服务

- 哨兵的作用：自动监控和维护集群，本身不存储数据
- 通常会设置多个哨兵，且一般为奇数个数，防止出现故障时无法起监控作用，而奇数则是为了好投票



哨兵本身不存储数据，因此读取的是 redis 安装包目录下的 sentinel.conf 文件。配置 sentinel.conf 文件

1. 配置：运行后台运行、日志文件路径

2. 设置监控的主机：`sentinel monitor 主机名 主机ip 服务器端口号 确认客观下线的最少哨兵的数量`

   - 确认客观下线的最少哨兵的数量：哨兵认为主机确实下线，同意进行故障迁移的票数

     当网络发生拥堵时，就会导致哨兵可能没有及时收到主机发送的心跳包（确认主机存活的数据包），从而哨兵会误以为主机已经宕机；因此使用该方式，通过多个哨兵来确认主机是否宕机，可以减小该误会的产生
     
   - 监控的主机可以多个（配置多个 sentinel monitor 即可）

   设置连接主机服务器所需要的密码：`sentinel auth-pass 主机名 密码`

3. 配置好主从关系。通常情况下，主机在宕机后恢复时，会作为从机重新加入到集群中，因此需要配置好从机关联主机所需要的masterauth密码

4. 启动哨兵：`redis-sentinel sentinel.conf文件 --sentinel`

   - redis.conf 文件和 sentinel.conf 文件中，都会在文件末尾处等，声明监视的主机及其从机等信息（也可通过sentinel.conf配置的日志查看）
   - 当主机宕机后，所有配置了监控该主机的哨兵会进行投票，确认是否真正宕机。确认主机宕机后，会选出一台从机作为新的主机
   - 在选出新的主机后，客户端在第一次进行操作时，会先重新建立与服务器间的连接
   - 当宕机的主机恢复时，会重新作为新主机的从机



故障迁移流程总结：

1. 多个哨兵监控着主机的运行状态

2. 哨兵主观下线：单个哨兵认为主机已经宕机

3. 哨兵客观下线：多个哨兵通过投票，确认主机已经宕机

4. 哨兵选举出兵王：哨兵间通过Raft算法，选举出一个兵王，由兵王来选出新主机

5. 兵王开始选举新主机：

   ① 先通过redis配置文件中的replica-priority设置的优先级（数字越小，优先级越高，默认为100）

   ② 若优先级相同，则通过复制偏移量（同步主机的数据量，在网络抖动时可能会有所不同）选出

   ③ 若复制偏移量也相同，则会根据 Run ID 的值选举









### 集群

#### 介绍

复制+哨兵的方式，在主机宕机、选举出新的主机时，会花费一定的时间，该时间点上是无法进行写入操作的，因此可能会导致数据的丢失

- 支持多个主机，且每个主机都能挂载多个从机
- 内置哨兵的故障迁移机制
- 客户端与redis服务器（节点）连接，不再需要连接集群中的所有节点，只需连接其中一个即可：所有节点间的数据共享
  - redis使用了哈希槽：共16384个，每一个节点会负责一定数量的哈希槽
    - 槽位为16384的原因：槽位过多，那么发送心跳包时信息头会过大；redis集群的节点数不会超过1000个，16384个槽位够用了；槽位少、节点少的情况下，压缩比高，便于数据传输
  - 不再需要连接集群中的所有节点：在存放key时，会通过算法计算后，决定将key存放于哪个哈希槽中，从而将key存放于该服务器中；而在获取key时，也是通过相同的算法获取哈希槽所对应的服务器中的key
    - 一个槽位是可以映射多个key的
  - 分片：集群中的每一个服务器，都看作是所有数据的一个分片
  - 该方式可以很好的进行节点的添加或删除：添加新节点时，只需将其他节点中的哈希槽分点给新的节点即可；删除节点时，只需将该节点中的哈希槽转移到其他节点中；且这些操作都不会影响到服务
    - 在迁移哈希槽时，若出现槽已被key对应的情况，那么会让原先的key过期，重新对key进行存储



将key分配到哪个节点上，有三种传统方式：

- 哈希取余分区：固定好节点个数，通过哈希算法计算出key的哈希值，再将该值取余于节点的个数，余值决定了该key存放于哪台服务器上

  - 优点：简单、直接
  - 缺点：根据节点数来确定存放的位置。当进行服务器扩容或缩容时，就可能会导致原先存放的位置与现计算后得到的位置不同（原先所有的数据都可能受到影响）

- 一致性哈希算法分区：

  1. 构建一致性哈希环（范围为 0 ~ 2^13-1，想象为一个圆线圈）
  2. 柑橘算法将redis服务器节点映射到哈希环上（将圆线圈根据节点数进行划分）
  3. 根据算法计算key的哈希值，决定key落在哈希环上的位置
  4. 落点位置顺时针往前移动，遇到哪个节点，则该key即属于哪个服务器

  - 优点：
    - 容错性高：若有ABC三台服务器，B宕机了，那么仅有A到B间的数据受到影响，而B到C间的数据不会受到影响，且A到B间的数据会转移到C服务器上进行存储（顺时针）
    - 扩展性强：若有ABC三台服务器，要在AB间新增一台服务器，那么仅有A到X间的数据会受到影响，仅需将A到X间的数据录入到X服务器上即可，其他的服务器数据不会受到影响
  - 缺点：数据倾斜：节点过少时，可能节点在哈希环上会分布不均匀，导致大量数据集中在一台服务器上
  
- 哈希槽分区：通过计算key的哈希值，得出其所在的槽，并找到该槽所对应的服务器







#### 基本使用

1. 配置：可远程连接、关闭远程保护、后台运行、日志、rdb保存目录、访问服务器密码、从机访问主机密码

2. `cluster-enable yes`：开启集群功能

   `cluster-config-file 文件名`：生成的集群配置文件名

   `cluster-node-timeout 毫秒数`：集群间的超时时间

3. 启动redis服务，关闭防火墙；在启动客户端时，建立集群关系：`redis-cli -a 密码 --cluster create --cluster-replicas x 服务器ip1:端口号1 服务器ip2:端口号2`

   - --cluster-replicas x：指定每个master主机的slave从机数量为x个
   - 根据指定的ip及其端口号，关联redis服务器，构建集群

4. 在配置完集群后，存放key时，只能将key存储到对应槽位的服务器中，而不能在任意一台服务器中存储（路由失效）：使用 `redis-cli -a 密码 -c` 来解决这个问题（在设置key时，会自动重定向到该key所存储的槽对应的服务器）

5. 在主机宕机后，从机会自动升职为主机；主机恢复后，默认是以从机的身份运行。在从机上使用命令 `cluster failover` 可以使主从关系互换



`redis-cli -a 密码 --cluster check 集群中的一个ip:端口号`：查看集群的详细信息

`cluster nodes`：查看集群的详细信息（主从关系、槽位分布等）



`cluster keyslot key名`：查看key所对应的槽位

`cluster countkeysinslot 槽位编号`：查看该槽位是否被占用（0表示被没有被占用，1表示已被占用）



集群中，不在同一个槽位下的key，是无法使用 mset、mget 等多键操作的

- 解决：使用 key{组名} 对key进行分组，在同一个组内的key就可以进行多键操作

  `mset key名1{组名} value值1 key名2{组名} value值2`

  `mset key名1{组名} key名2{组名}`



在配置文件中，cluster-require-full-coverage 可以设置：集群是否完整才对外服务，也就是说原本若是有3主3从，现在只有2主2从了（数据丢失），是否还是对外服务，默认为yes（即要求完整）







#### 扩容与缩容

`redis-cli -a 密码 --cluster add-node 要加入的节点ip:端口号 引路的节点ip:端口号`：该命令可以将一个新的节点加入到集群中（相当于有引路人，让该节点通过集群中的某个节点引入进去）

- 新节点加入到集群后，若该节点是空白节点（无任何持久化数据），则会作为master身份加入



`redis-cli -a 密码 --cluster reshard 集群中的一个ip:端口号`：使用该命令来对某个节点的槽位重新分配

- 该方式无法对slave分配槽位
- 在运行该命令时，需要指定些参数：
  1. 转移的槽位数量
  2. 将槽位转移到指定id（节点）上
  3. 选择是从其他所有节点处共同抽取一部分槽位，还是选择在指定id（节点）上抽取所需要的所有槽位
     - 若是要从其他所有节点处共同抽取，则只需要选择 all 即可
     - 若是从指定的一台主机上抽取，则需要填写该主机的id，并选择选项 done
- 无论是扩容分配给新节点槽位，还是缩容转移节点槽位，都是使用该命令
- 当分配完后，原先的master没有槽位了时，会自动降级为slave，且会挂载到槽位最多的master上



`redis-cli -a 密码 --cluster add-node 从机ip:端口号 主机ip:端口号 --cluster-slave --cluster-master-id 主机id`：为集群中的某个主节点挂载一个从节点



`redis-cli -a 密码 --cluster del-node 从机ip:端口号 从机id`：移除集群中主机下的某个从机









### SpringBoot整合Redis

SpringBoot整合redis有三种方式：Jedis、Lettuce、RedisTemplate（推荐）

#### Jedis

jedis对各类API进行了封装

1. 引入依赖

   ```xml
   <dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>4.3.1</version>
   </dependency>
   ```

2. 直接通过创建 Jedis类 建立连接

   ```java
   Jedis jedis = new Jedis("ip地址",端口号);
   jedis.auth("密码");
   // 通过以上两步骤就可以成功建立与redis的连接
   
   String res = jedis.ping();		// 相当于调用ping命令，若成功返回 PONG 则说明连接成功
   ```

3. 相关命令对应的方法：

   keys * ：【Set\<String>】`jedis对象.keys("*")`

   set key名 value值：`jedis对象.set("key名","value值")`

   get key名：【String】`jedis对象.get("key名")`

   expire key名 秒数：`jedis对象.expire("key名",Long)`（Long：设置过期的秒数）

   ttl key名：【Long】`jedis对象.ttl("key名")`







#### Lettuce

SpringBoot2.0之后默认都是使用Lettuce来连接redis服务器的。因为jedis客户端连接redis服务器时，每个线程都要创建一个jedis实例去连接redis客户端，反复的创建和关闭会产生极大的开销。且jedis也是线程不安全的

1. 引入依赖

   ```xml
   <dependency>
       <groupId>io.lettuce</groupId>
       <artifactId>lettuce-core</artifactId>
       <version>6.2.1.RELEASE</version>
   </dependency>
   ```

2. ，建立连接和操作对象

   ```java
   RedisURI uri = RedisURI.builder().redis("ip地址").withPort(端口号).withAuthentication("default","密码").build();
   // 构建连接到redis的URI
   
   RedisClient client = RedisClient.create(uri);		// 开启指定URI对应的客户端
   
   StatefulRedisConnection connect = client.connect();		// 建立客户端与服务器间的连接
   
   RedisCommands commands = connect.sync();		// 创建命令操作对象
   ```

3. 相关命令对应的方法：

   keys * ：【List】`RedisCommands对象.keys("*")`

   set key名 value值：`RedisCommands对象.set("key名","value值")`

   get key名：【String】`RedisCommands对象.get("key名")`









#### RedisTemplate

1. 引入依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   	<version>3.0.5</version>		<!-- 引用与springboot相同版本的依赖即可 -->
   </dependency>
   <dependency>
       <groupId>org.apache.commons</groupId>
       <artifactId>commons-pool2</artifactId>
       <version>2.12.0</version>
   </dependency>
   ```

2. 配置application.yml文件

   ```yaml
   # 单机配置
   spring:
     data:
       redis:
         host: ip地址
         port: 端口号
         password: 密码
         
   # 集群配置
   spring:
     data:
       redis:
         password: 密码
         cluster:
           max-redirects: 次数		# 在获取连接失败时，最大的重定向次数
           nodes: 集群节点ip1:端口号,集群节点ip2:端口号
         lettuce:
           cluster:
             refresh:
               adaptive: true		# 集群拓扑动态感应刷新（默认为false关闭）
               period: 2000		# 定时刷新（毫秒）
   ```

   - 使用集群时，若主机宕机，redis客户端是可以无感启用从机进行操作。但是，SpringBoot客户端没有动态感知到redis主机间的变化，而是继续向已宕机的主机发送请求，超过一段时间未响应则报出超时错误。因此，我们需要设置集群拓扑的动态感应刷新机制，让其能够及时调整请求对象

3. 通过@Autowired注入直接获取 StringRedisTemplate对象

   ```java
   @Autowired
   private StringRedisTemplate redisTemplate;		// 在引入依赖后，直接注入即可
   
   ValueOperations<String,String> operations = redisTemplate.ospForValue();
   // 获取redis操作对象
   
   // 通过redis操作对象调用相关的方法（执行redis命令）
   operations.set("key名","value值");			// set key名 value值
String value = operations.get(Object);;		// get key名
   
   /* StringRedisTemplate 是 RedisTemplate的一个子类
   	相比于RedisTemplate，StringRedisTemplate解决了在存储key时的序列化问题：
   		RedisTemplate底层默认使用的是JDK的序列化方式：JdkSerializationRedisSerializer
   		而StringRedisTemplate，会改用为 StringRedisSerializer 的序列化方式
   */
   ```
   





