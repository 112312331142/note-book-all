## 一、初识redis

### 1.1 认识NoSQL

|          | SQL                                                        | NoSQL                                                        |
| -------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| 数据结构 | 结构化的                                                   | 非结构化的                                                   |
| 数据关联 | 关联的                                                     | 无关联的                                                     |
| 查询方式 | SQL查询                                                    | 非SQL                                                        |
| 事务特性 | ACID                                                       | BASE                                                         |
| 存储方式 | 磁盘                                                       | 内存                                                         |
| 扩展性   | 垂直                                                       | 水平                                                         |
| 使用场景 | 1. 数据结构固定<br>2. 相关业务对数据安全性、一致性要求较高 | 1. 数据结构不固定<br>2. 对一致性、安全性要求不高<br>3. 对性能要求 |

### 1.2 认识Redis

Redis诞生于2009年，全称是Remote Dictionary Server，远程词典服务器，是一个基于内存的键值型NoSQL数据库

特征：

- 键值型，value支持多种不同数据结构，功能丰富
- 单线程，每个命令具备原子性
- 低延迟、速度快（基于内存、IO多路复用、良好的编码）
- 支持数据持久化
- 支持主从集群，分片集群
- 支持多语言客户端


### 1.3 安装Redis

#### 1.3.1 Redis启动

默认配置到环境变量，因此可以在任意目录下运行这些命令。其中：

* `redis-cli`：是redis提供的命令行客户端
* `redis-server`：是redis的服务端启动脚本
* `redis-sentinel`：是redis的哨兵启动脚本

修改redis.conf文件中的一些配置：

```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问，修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
dameonize yes
# 密码，设置后访问Redisv必须输入密码
requirepass 123456
```

启动redis：

```properties
1. 基于配置文件启动
# 进入redis安装目录
cd /etc/redis
# 启动
redis-server redis.conf
# 查看配置文件是否启动成功
ps -ef| grep redis
```

操作redis的命令：

```sh
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis

```

#### 1.3.2 Redis命令行客户端

`redis-cli [options] [commonds]`

其中常见的options有：

* `-h 127.0.0.1` 指定要连接的redis节点的IP地址，默认是127.0.0.1
* `-p 6379` 指定要连接的redis节点的端点，默认是6379
* `-a 123456` 指定redis的访问密码

```sh
redis-cli -h 192.168.181.129 -p 6379 -a 123456
```



## 二、 Redis常见命令

### 2.1 Redis数据结构介绍和通用命令

Redis是一个key-value的数据库，key一般是String类型，不过value的类型多种多样

通用命令是部分数据类型的，都可以使用的指令，常见的有：

* `KEYS`：查看符合模板的所有key，不建议在生产环境设备上使用
* `DEL`： 删除一个指定的key
* `EXISTS`：判断一个key是否存在
* `EXPIRE`：给一个key设置有效期，有效期到期时该key会自动删除
* `TTL`：查看一个key 的剩余有效期

### 2.2 String类型

String类型，也就是字符串类型，是Redis中最简单的存储类型

其value是字符串，不过根据字符串的格式不同，又可以分为3类：

* string：普通字符串
* int：整数类型，可以做自增自减的操作
* float：浮点类型，可以做自增自减的操作

不管是那种格式，底层都是字节数组形式存储，只不过是编码格式不同。字符串类型的最大空间不能超过512m

String类型的常见命令：

* `SET`：添加或者修改已经存在的一个String类型的键值对
* `GET`：根据key获取String类型的value
* `MSET`：批量添加多个String类型的键值对
* `MGET`：根据多个key获取多个String类型的value
* `INCR`：让一个整型的key自增1
* `INCRBY`：让一个整型的key自增并指定字长，例如：incrby num 2 让num值自增2
* `INCRBYFLOAT`：让一个浮点类型的数字自增并指定步长
* `SETNX`：添加一个String类型的键值对，前提是这个key不存在，否则不执行
* `SETEX`：添加一个String类型的键值对，并且指定有效期


### 2.3 Key的层级结构

Redis的key允许有多个单词形成层级结构，多个单词之间用':'隔开，格式如下：

`项目名：业务名：类型：id`

这个格式并非固定，也可以根据自己的需求来删除或添加词条

> 例如我们的项目叫chen，有user和product两种不同类型的数据，我们可以这样定义key：
>
> * user的相关key：chen：user：1
> * product的相关key：chen：product：1
>
> 如果Value是一个Java对象，例如一个User对象，则可以将对象序列化为JSON字符串后存储
>
> | KEY            | VALUE                                   |
> | -------------- | --------------------------------------- |
> | chen:user:1    | {"id":1,"name":"Jack","age":21}         |
> | chen:product:1 | {"id":1,"name":"xiaomi11","price":4999} |
>

### 2.4 Hash类型

Hash类型，也叫散列，其value是一个无序字典，类似与Java中的HashMap结构

Hash结构可以将对象中的每个字段独立存储，可以针对单个字段做CRUD：

| KEY         | VALUE：field | VALUE：value |
| ----------- | ------------ | ------------ |
| chen:user:1 | name/age     | Jack/21      |
| chen:user:2 | name/age     | Rose/18      |

Hash的常见命令有：

- `HSET key field value`：添加或者修改hash类型的key的field的值
- `HGET key field`：根据一个hash类型key的field的值
- `HMSET`：批量添加多个hash类型key的field值
- `HMGET`：批量获取多个hash类型key的field的值
- `HGETALL`：获取一个hash类型的key中所有的field和value
- `HKEYS`：获取一个hash类型的key中的所有field
- `HVALS`：获取一个hash类型的key中的所有value
- `HINCRBY`：让一个hash类型key的字段值自增并指定步长
- `HSETNX`：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行

### 2.5 List类型

Redis中的List类型与java中的LinkedList类似，可以看作是一个双向链表结构。即可以支持正向检索和也可以支持反向检索

常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等

LIst的常见命令有：

* `LPUSH key element`：向列表左侧插入一个或多个元素
* `LPOP key`：移除并返回列表左侧的第一个元素，没有则返回nil
* `RPUSH key element`：向列表右侧插入一个或多个元素
* `RPOP key`：移除并返回列表右侧的第一个元素，没有则返回nil
* `LRANGE key start end`：返回一段角标范围内的所有元素
* `BLPOP 和 BRPOP`：与LPOP和RPOP类似，只不过在没有元素时等待指定时间，而不是直接返回nil

> 如何利用List结模拟一个栈？
>
> * 入口和出口在同一边
>
> 如何利用List结构模拟一个队列？
>
> * 出口和入口不在同一遍
>
> 如何利用List结构模拟一个阻塞队列？
>
> * 入口和出口在不同边
> * 出队时采用BLPOP或BRPOP

### 2.6 Set类型

Redis的Set结构与java中的HashSet类似，可以看作是一个value为null的HashMap。因为也是一个hash表，因此具备与HashSet类似的特征 ：

* 无序、元素不可重复、查找快
* 支持交集、并集、差集等功能

Set的常见命令有：

* `SADD key member ...`： 向set中添加一个或多个元素
* `SREM key member ...`：移除set中的指定元素
* `SCARD key`：返回set中元素中的个数
* `SISMEMBER key number`：判断一个元素是否存在于set中
* `SMEmBERS`：获取set中的所有元素
* `SINTER key1 key2`：求key1与key2的交集
* `SDIFF key1 key2`：求key1与key2的差集
* `SUNION key1 key2`：求key1与key2的并集

### 2.7 SortedSet类型

Redis的SortedSet是一个可排序的set集合，与java中的TreeSet有些类似，但底层数据结构却差别很大。SortedSet中的每个元素都带有一个score属性，可以基于score属性对元素排序，底层的实现是一个跳表（SkipList）加hash表。

SortedSet具备下列特性：

* 可排序、元素不重复
* 查询速度快

因为SortedSet的可排序性，进场用来实现排行榜这样的功能

SorttedSet的常见命令：

- `ZADD key score member`： 添加一个或多个元素到sortedSet，如果已经存在则更新其score值
- `SREM key score member`：移除sortedset中的指定元素
- `ZSCORE key number`：获取sortedset中的指定元素的score值
- `ZRANK key number`：获取sortedset中指定元素的排名
- `SCARD key`：返回sortedset中元素中的个数
- `ZCOUNT key min max`：统计score值在给定范围内的所有元素的个数
- `ZINCRBY key increment member`：让scortedset中的指定元素自增，步长指定为increment值
- `ZRANGE key min max`：按照score排序后，获取指定排名范围内的元素
- `ZRANGEBYSCORE key min max`：按照score排序后，获取指定score范围内的元素
- `SINTER、ZINTER、ZUNION`：求key1与key2的交集、差集、并集

注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可

## 三、Redis的Java客户端

### 3.1 Jedis

#### 3.1.1 Jedis 快速入门

1. 引入依赖

   ```xml
   <dependency>
       <groupId>redis.clients</groupId>
       <artifactId>jedis</artifactId>
       <version>5.2.0</version>
   </dependency>
   ```

2. 建立连接

   ```java
   private Jedis jedis;

   @BeforeEach
   void setJedis() {
       // 1.建立连接
       jedis = new Jedis("192.168.181.129");
       // 2.设置密码
       jedis.auth("123456");
       // 3.选择库
       jedis.select(0);
   }
   ```

3. 测试string

   ```java
   @Test
   void testString() {
       // 存入数据
       String result = jedis.set("name", "gaoxin");
       System.out.println("result = " + result);
       // 获取数据
       String name = jedis.get("name");
       System.out.println("name = " + name);
   }
   ```

4. 释放资源

   ```java
   @AfterEach
   void tearDown() {
       if(jedis != null) {
           jedis.close();
       }
   }

   ```

5. 结果

   ```java
   result = OK
   name = gaoxin
   ```

#### 3.1.2 Jedis连接词

Jedis本身是连接不安全的，并且频繁的创建和销毁连接会有性能损失，因此我们推荐大家使用Jedis连接池代替Jedis的直连方式

```java
package com.chen;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.util.List;

public class JedisConnectionFactory {

    private final static JedisPool jedispool;

    static {
        // 配置连接池
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        // 设置最大连接
        jedisPoolConfig.setMaxTotal(8);
        // 设置最大空闲连接
        jedisPoolConfig.setMaxIdle(8);
        // 设置最小空闲连接
        jedisPoolConfig.setMinIdle(0);
        // 设置等待时长
        jedisPoolConfig.setMaxWaitMillis(200);
        // 创建连接池对象
        jedispool = new JedisPool(jedisPoolConfig, "192.168.181.129",
                6379,1000,"123456");

    }

    // 获取Jedis对象
    public static Jedis getJedis() {
        return jedispool.getResource();
    }

    public static void main(String[] args) {
        Jedis jedis = JedisConnectionFactory.getJedis();
        jedis.auth("123456");
        jedis.select(0);

        jedis.lpush("students","gaoxin");
        jedis.lpush("students","control");
        jedis.lpush("students","garbage");

        List<String> students = jedis.lrange("students", 0, 8);
        System.out.println(students);

        jedis.close();
    }
}
```

### 3.2 SpringDataRedis

#### 3.2.1 认识SpringDataRedis

SpringData是Spring中数据操作的模块，包含各种对数据库的集成，其中对Redis的集成模块就叫做SpringDataRedis

SpringDataRedis中提供了RedisTemplate工具类，其中封装了各种对Redis的操作。并且将不同数据类型的操作API封装到了不同的类型中：

| API                         | 返回值类型 | 说明                  |
| --------------------------- | ---------- | --------------------- |
| redisTemplate.opsForValue() | ValueOperations | 操作String类型的数据  |
| redisTemplate.opsForHash()  | HashOperations | 操作Hash类型数据      |
| redisTemplate.opsForList()  | ListOperations | 操作List类型数据      |
| redisTemplate.opsForSet()   | SetOperations        | 操作Set类型数据       |
| redisTemplate.opsForZSet()  |   ZSetOperations     | 操作SortedSet类型数据 |
| redisTemplate               |            | 通用的命令            |

#### 3.2.2 SpringDataRedis快速入门

1. 引入依赖

   ```xml
   <!--redis依赖-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   <!--common-pool-->
   <dependency>
       <groupId>org.apache.commons</groupId>
       <artifactId>commons-pool2</artifactId>
   </dependency>
   ```

2. 配置文件

   ```yaml
   spring:
     data:
       redis:
         host: 192.168.181.129
         port: 6379
         password: 123456
         lettuce:
           pool:
             max-active: 8
             max-idle: 8
             min-idle: 0
             max-wait: 100ms
   ```

3. 注入RedisTemplate

   ```java
   @Autowired
   private RedisTemplate redisTemplate;
   ```

4. 编写测试

   ```java
   @Test
    void testString() {
       // 写入一条String数据
       redisTemplate.opsForValue().set("name","gaoxin");
       // 获取String数据
       Object name = redisTemplate.opsForValue().get("name");
       System.out.println("name = " + name);
   ```


#### 3.2.3 RedisTemplate的RedisSerializer

RedisTemplate可以接受任意Object作为值写入Redis，只不过写入前会把Object序列化为字节形式，默认是采用JDK序列化，得到的结果是这样的：

> "\xac\xed\x00\x05t\x00\x04name"

缺点：可读性差，内存占用较大

我们可以自定义RedisTemplate的序列化方式

```java
  @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        // 创建redisTemplate对象
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        // 设置连接工厂
        template.setConnectionFactory(connectionFactory);
        // 创建JSON序列化工具
        GenericJackson2JsonRedisSerializer jsonRedisSerializer = new
                GenericJackson2JsonRedisSerializer();
        // 设置Key序列化
        template.setKeySerializer(RedisSerializer.string());
        template.setHashKeySerializer(RedisSerializer.string());
        // 设置value的序列化
        template.setValueSerializer(RedisSerializer.json());
        template.setHashValueSerializer(RedisSerializer.json());
        // 返回
        return template;
```

为了节省内存空间，我们不会使用JSON序列化器来处理value，而是统一使用String序列化器，要求只能存储String类型的key和value。当需要存储java对象，手动完成对象的序列化和反序列化

String默认i同了一个StingRedisTemplate类，省去了自定义RedisTemplate的过程：

```java
void testSavaUser() throws JsonProcessingException {
    Student student = new Student("高鑫", 79);
    // 手动序列化
    String json = mapper.writeValueAsString(student);
    // 写入数据
    stringRedisTemplate.opsForValue().set("teacher1", json);
    // 获取数据
    String jsonUser = stringRedisTemplate.opsForValue().get("teacher1");
    // 手动反序列化
    mapper.readValue(jsonUser, Student.class);
    System.out.println("user:" + jsonUser);
}
```