---
title: Redis
date: 2019-05-28 00:50:09
tags: Web
categories: web
---

## 一、概念

Redis是一款高性能的NOSQL系列（**NoSQL = Not Only SQL** ）的数据库

### 1.1.什么是NOSQL

​		NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
​		随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。

​	1.1.1.	NOSQL和关系型数据库比较
​		优点：
​			1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
​			2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
​			3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
​			4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。

​		缺点：
​			1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
​			2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
​			3）不提供关系型数据库对事务的处理。

​	1.1.2.	非关系型数据库的优势：

​		1）性能NOSQL是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
​		2）可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。

​	1.1.3.	关系型数据库的优势：
​		1）复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
​		2）事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。

​	1.1.4.	总结
​		关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
​		让NoSQL数据库对关系型数据库的不足进行弥补。
​		**一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据**

### 1.2.主流的NOSQL产品

​	•	键值(Key-Value)存储数据库
​			相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
​			典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
​			数据模型： 一系列键值对
​			优势： 快速查询
​			劣势： 存储的数据缺少结构化
​	•	列存储数据库
​			相关产品：Cassandra, HBase, Riak
​			典型应用：分布式的文件系统
​			数据模型：以列簇式存储，将同一列数据存在一起
​			优势：查找速度快，可扩展性强，更容易进行分布式扩展
​			劣势：功能相对局限
​	•	文档型数据库
​			相关产品：CouchDB、MongoDB
​			典型应用：Web应用（与Key-Value类似，Value是结构化的）
​			数据模型： 一系列键值对
​			优势：数据结构要求不严格
​			劣势： 查询性能不高，而且缺乏统一的查询语法
​	•	图形(Graph)数据库
​			相关数据库：Neo4J、InfoGrid、Infinite Graph
​			典型应用：社交网络
​			数据模型：图结构
​			优势：利用图结构相关算法。
​			劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。

### 1.3 什么是Redis 

​	Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求，目前为止Redis支持的键值数据类型如下：
​		1) 字符串类型 string
​		2) 哈希类型 hash
​		3) 列表类型 list
​		4) 集合类型 set
​		5) 有序集合类型 sortedset
​	1.3.1 redis的应用场景
​		•	缓存（数据查询、短连接、新闻内容、商品内容等等）
​		•	聊天室的在线好友列表
​		•	任务队列。（秒杀、抢购、12306等等）
​		•	应用排行榜
​		•	网站访问统计
​		•	数据过期处理（可以精确到毫秒
​		•	分布式集群架构中的session分离



## 二、命令操作

### 1. Redis的数据结构

Redis存储的是键值对，key**都是字符串**，value可以有**5种**不同的类型数据：

1. 字符串 string
2. 哈希类型 hash：map格式，可以存储键值对
3. 列表类型 list：linkedlist格式，元素可以重复
4. 集合类型 set 元素不可以重复
5. 有序集合类型 sortedset 元素可以重复而且有序

### 2. Redis命令操作

1. **字符串string**

   1. 存储：set key value
   2. 获取：get key
   3. 删除：del key

2. **哈希类型 hash**

   1. 存储：hset key field value

   2. 获取：

      1. hget key field：获取指定的field对应的值
      2. hgetall key：获取所有的field和value

   3. 删除：hdel key field

      ```
      hset myhash username Tom
      hset myhash age 18
      
      hget myhash age -- 18
      ```

      {% asset_img redis.png This is an example image %}

3. **列表类型 list**：可以添加一个元素到列表的头部（左）或者尾部（右）

   1. 存储：

      1. lpush key value：将元素加入列表**左边**
      2. rpush key value：将元素加入列表**右边**

   2. 获取：

      1. lrange key start end：范围获取

   3. 删除：

      1. lpop：删除最左边元素，并返回该元素
      2. rpop：删除最右边元素，并返回该元素

      ```Redis
      lpush mylist a
      lpush mylist b
      lpush mylist c -- a->b->c
      
      lrange mylist 0 -1 -- 获取mylist所有的元素
      ```

4. **集合类型 set**

   1. 存储：sadd key value
   2. 获取：smembers key：获取集合中所有的元素
   3. 删除：srem key value：删除集合中某个元素

5. **有序集合类型 sortedset**

   1. 存储：zadd key score value

   2. 获取：zrange key start end（withscores）

   3. 删除：zrem key value

      ```
       127.0.0.1:6379>zadd sorted 10 aaa
      (integer) 1
      127.0.0.1:6379> zadd sorted 30 bbb
      (integer) 1
      127.0.0.1:6379> zadd sorted 20 ccc
      (integer) 1
      127.0.0.1:6379> zrange sorted 0 -1
      // 按照分数（score）排序
      1) "aaa"
      2) "ccc"
      3) "bbb"
      ```

6. **常用的通用命令：**

   1. keys *：查询所有的键
   2. type key：获取键对应的value的类型
   3. del key：删除指定的key以及该key对应的value





## 三、Redis持久化

Redis是一个基于内存的数据库，因此当Redis服务器重启会导致数据丢失，可以将Redis数据库的数据存储到硬盘。

### 1. Redis持久化机制

1. **RDB:** 默认方式，不需要进行配置，默认使用的机制，在一定的时间间隔内，检测key的变化情况，然后持久化数据

   1. 编辑配置文件：redis.windows.conf

      ```
      #   after 900 sec (15 min) if at least 1 key changed
      #   after 300 sec (5 min) if at least 10 keys changed
      #   after 60 sec if at least 10000 keys changed
      
      save 900 1
      save 300 10
      save 60 10000
      ```

   2. 重新启动Redis服务器，并且指定配置文件:$ redis-server.exe redis.windows.conf

   

2. **AOF:** 记录日志的方式，可以记录每一条操作的命令，操作命令后就持久化数据（这样的效果和mysql类似了）

   1. 编辑配置文件：redis.windows.conf

      ```
      # 开启aof模式
      appendonly no --> appendonly yes
      
      # aof模式下的三种持久化方式
      
      # appendfsync always # 总是，每一条操作都持久化
      appendfsync everysec # 每间隔1s进行一次持久化
      # appendfsync no # 不进行持久化
      ```

## 四、Java客户端Jedis

1. Jedis是一款java操作Redis数据库的工具，类似于JDBC

2. 使用步骤：

   1. 下载、导入jar包
   2. 使用

3. 使用Demo

   ```java
   @Test
   public void test2() {
       // 获取连接, 如果使用的是空参数的构造，那么默认就是localhost和6379
       Jedis jedis = new Jedis("localhost", 6379);
       // 操作
       jedis.set("username", "David");
   
       String username = jedis.get("username");
   
       System.out.println(username);// David
   
       // 可以使用方法setex来存储指定的过期时间的key value
       jedis.setex("activeCode", 20, "didi");// 20秒后键activeCode、值didi就被自动删除
       // 关闭连接
       jedis.close();
   }
   ```

4. Jedis连接池

   ```java
   @Test
   public void test5() {
       // 创建一个配置对象
       JedisPoolConfig config = new JedisPoolConfig();
       config.setMaxTotal(50);// 最大的允许连接数量
       config.setMaxIdle(10);// 最大的空闲连接数
   
       // 创建一个JedisPool连接池对象
       JedisPool jedisPool = new JedisPool(config, "localhost", 6379);
   
       // 获取连接
       Jedis jedis = jedisPool.getResource();
   
       // 使用
       jedis.set("username", "Tom");
   
       // 归还连接给连接池
       jedis.close();
   }
   ```