[toc]

# Redis

##### 概念：

​			redis是一款高性能的NOSQL系列的非关系型数据库，用于作为关系型数据库的良好补充。

#### NOSQL

###### 1.什么是NOSQL

```markdown
	NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。
	随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和-高并发-的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。
```

###### 2.NOSQL与关系型数据库的比较

```markdown
关系型--时间换空间 非关系型--空间换时间
*优点：*
    1）成本：nosql数据库简单易部署，基本都是开源软件，不需要像使用oracle那样花费大量成本购买使用，相比关系型数据库价格便宜。
    2）查询速度：nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库。
    3）存储数据的格式：nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型。
    4）扩展性：关系型数据库有类似join这样的多表查询机制的限制导致扩展很艰难。

*缺点：*
    1）维护的工具和资料有限，因为nosql是属于新的技术，不能和关系型数据库10几年的技术同日而语。
    2）不提供对sql的支持，如果不支持sql这样的工业标准，将产生一定用户的学习和使用成本。
    3）不提供关系型数据库对事务的处理。
    
*非关系型数据库的优势：*
    1）高性能NOSQL是基于键值对的，可以想象成表中的主键和值的对应关系，而且不需要经过SQL层的解析，所以性能非常高。
    2）可扩展性同样也是因为基于键值对，数据之间没有耦合性，所以非常容易水平扩展。

*关系型数据库的优势：*
    1）复杂查询可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询。
    2）事务支持使得对于安全性能很高的数据访问要求得以实现。对于这两类数据库，对方的优势就是自己的弱势，反之亦然。
    
*总结：*
    1. 关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，
    2. 让NoSQL数据库对关系型数据库的不足进行弥补。
    3. 一般会将数据存储在关系型数据库中，在nosql数据库中备份存储关系型数据库的数据
```

###### 3.主流的NOSQL产品

```markdown
**	键值(Key-Value)存储数据库**
        相关产品： Tokyo Cabinet/Tyrant、Redis、Voldemort、Berkeley DB
        典型应用： 内容缓存，主要用于处理大量数据的高访问负载。 
        数据模型： 一系列键值对
        优势： 快速查询
        劣势： 存储的数据缺少结构化
**	列存储数据库**
        相关产品：Cassandra, HBase, Riak
        典型应用：分布式的文件系统
        数据模型：以列簇式存储，将同一列数据存在一起
        优势：查找速度快，可扩展性强，更容易进行分布式扩展
        劣势：功能相对局限
**	文档型数据库**
        相关产品：CouchDB、MongoDB
        典型应用：Web应用（与Key-Value类似，Value是结构化的）
        数据模型： 一系列键值对
        优势：数据结构要求不严格
        劣势： 查询性能不高，而且缺乏统一的查询语法
**	图形(Graph)数据库**
        相关数据库：Neo4J、InfoGrid、Infinite Graph
        典型应用：社交网络
        数据模型：图结构
        优势：利用图结构相关算法。
        劣势：需要对整个图做计算才能得出结果，不容易做分布式的集群方案。
```

###### 4.什么是Redis

```shell
# Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下的存储需求
============================
优点：
	1. Redis 采用单线程模式处理请求。
	2. 支持持久化，所以 Redis 不仅仅可以用作缓存，也可以用作 NoSQL 数据库。
	3. Redis 提供主从同步机制，以及 Cluster 集群部署能力，能够提供高可用服务。
=============================

**redis的应用场景
    •	缓存（数据查询、短连接、新闻内容、商品内容等等）
    •	聊天室的在线好友列表
    •	任务队列。（秒杀、抢购、12306等等）
    •	应用排行榜
    •	网站访问统计
    •	数据过期处理（可以精确到毫秒
    •	分布式集群架构中的session分离
```

#### Redis安装

###### 1.windows

```markdown
1. 官网：https://redis.io
2. 中文网：http://www.redis.net.cn/
3. 解压直接可以使用：
    * redis.windows.conf：配置文件
    * redis-cli.exe：redis的客户端		
    		服务器先启动才能连接到
    * redis-server.exe：redis服务器端	
    		双击启动，6379端口号
    		DOS命令行启动，redis-server.exe （redis.windows.conf可选指定配置文件）
    		
    		
默认创建0-15号库，默认操作0号
```

###### 2.linux源码安装

```shell
# 下载
 wget http://download.redis.io/releases/redis-6.0.8.tar.gz
# 解压
 tar xzf redis-6.0.8.tar.gz
# 进入目录
cd redis-6.0.8
# make 安装完成
 make
 
# 启动，可以指定配置文件启动，也可以不指定启动，不指定将使用默认配置文件
 cd src
 ./redis-server ../redis.conf
```

#### redis的数据结构

```markdown
* redis存储的是：key,value格式的数据，其中key都是字符串，value有5种不同的数据结构
	* value的数据结构：
        1) 字符串类型 string
        2) 哈希类型 hash ： map格式  
        3) 列表类型 list ： linkedlist格式。支持重复元素
        4) 集合类型 set  ： 不允许重复元素
        5) 有序集合类型 sortedset：不允许重复元素，且元素有顺序
```

##### 1.字符串类型 string

```markdown
1. 存储： set key value	如果该key存在则进行覆盖操作。总是返回”OK”
        127.0.0.1:6379> set username zhangsan
        OK
2. 获取： get key	如果与该key关联的value不是String类型，redis将返回错误信息，因为get命令只能用于获取 String value；如果该key不存在，返回(nil)。
        127.0.0.1:6379> get username
        "zhangsan"
3. 删除： del key
		127.0.0.1:6379> del age
4. 主键自增： incr key	将 key 中储存的数字值增一
        127.0.0.1:6379> incr id 
        (integer) 1
```

##### 2.哈希类型 hash

```markdown
储存的值是键值对，一个键对应一批键值对
1. 存储： hset key field value
			127.0.0.1:6379> hset myhash username lisi
			(integer) 1
			127.0.0.1:6379> hset myhash password 123
			(integer) 1
2. 获取： hget key field: 获取指定的field对应的值
				127.0.0.1:6379> hget myhash username
				"lisi"
		 hgetall key：获取所有的field和value
				127.0.0.1:6379> hgetall myhash
				1) "username"
				2) "lisi"
				3) "password"
				4) "123"
				
3. 删除： hdel key field
			127.0.0.1:6379> hdel myhash username
			(integer) 1
```

##### 3.列表类型 list

```markdown
列表类型 list:可以添加一个元素到列表的头部（左边）或者尾部（右边）
1. 添加：
		lpush key value: 将元素加入列表左表,返回元素个数
		rpush key value：将元素加入列表右边
				
        127.0.0.1:6379> lpush myList a
        (integer) 1
        127.0.0.1:6379> lpush myList b
        (integer) 2
        127.0.0.1:6379> rpush myList c
        (integer) 3
2. 获取：
        lrange key start end ：范围获取
        127.0.0.1:6379> lrange myList 0 -1
        1) "b"
        2) "a"
        3) "c"
3. 删除：
        lpop key： 删除列表最左边的元素，并将元素返回,如果元素不存在，返回nil
        rpop key： 删除列表最右边的元素，并将元素返回
```

##### 4.集合类型 set ： 不允许重复元素

```markdown
1. 存储：sadd key value	
			127.0.0.1:6379> sadd myset a
			(integer) 1
			127.0.0.1:6379> sadd myset a
			(integer) 0
2. 获取：smembers key:获取set集合中所有元素
			127.0.0.1:6379> smembers myset
			1) "a"
3. 删除：srem key value:删除set集合中的某个元素	
			127.0.0.1:6379> srem myset a
			(integer) 1
```

##### 5.有序集合类型 sortedset：

```markdown
有序集合类型 sortedset：不允许重复元素，且元素有顺序.每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

1. 存储：zadd key score value
		向有序集合中加入一个元素和该元素的分数，如果该元素已经存在则会用新的分数替换原有的分数。返回值是新加入到集合中的元素个数，不包含之前已经存在的元素。
        127.0.0.1:6379> zadd mysort 60 zhangsan
        (integer) 1
        127.0.0.1:6379> zadd mysort 50 lisi
        (integer) 1
        127.0.0.1:6379> zadd mysort 80 wangwu
        (integer) 1
2. 获取：zrange key start end [withscores]	
		按照元素分数从小到大的顺序返回索引从start到stop之间的所有元素（包含两端的元素）
        127.0.0.1:6379> zrange mysort 0 -1
        1) "lisi"
        2) "zhangsan"
        3) "wangwu"

		zrevrange key start stop [WITHSCORES] 
		按照元素分数从大到小的顺序返回索引从start到stop之间的所有元素和分数（包含两端的元素）
		127.0.0.1:6379> zrevrange mysort 0 -1 withscores
        1) "zhangsan"
        2) "60"
        3) "wangwu"
        4) "80"
        5) "lisi"
        6) "500"
3. 获取元素分数： zscore key member
		127.0.0.1:6379> zscore zset lisi
		"45"
4. 删除：zrem key value
        127.0.0.1:6379> zrem mysort lisi
        (integer) 1
```

##### 	6.通用命令

```markdown
1. keys * : 查询所有的键
			*代表匹配所有
			keys ll
2. type key : 获取键对应的value的类型
			key表示键名
			type ll
3. del key: 删除指定的key value
			key表示键名
			del ll
4. exists key: 判断key是否存在，存在返回1，不存在返回零。
			key表示键名
5. 缓存
	expire key seconds   设置key的生存时间（单位：秒）key在多少秒后会自动删除 
			127.0.0.1:6379> expire aa 10
			(integer) 1
	ttl key   查看key生于的生存时间 
			127.0.0.1:6379> ttl ee
			(integer) 993
	persist key     清除生存时间
    		127.0.0.1:6379> persist ee
			(integer) 1
	pexpire key milliseconds    生存时间设置单位为：毫秒
			127.0.0.1:6379> pexpire ee 3333333333
			(integer) 1
6. 多数据库操作
	select index： 切换数据库
			127.0.0.1:6379> select 1
			OK
	dbseze： 返回当前库有多少key
			127.0.0.1:6379> dbsize
			(integer) 7
	flushdb： 清空当前数据库
			127.0.0.1:6379[2]> flushdb
			OK
	flushall： 清空当前实例下所有的数据库数据
			127.0.0.1:6379[2]> flushall
			OK
```



### Jedis

```markdown
Java客户端 Jedis
	* Jedis: 一款java操作redis数据库的工具.
	* 使用步骤：
		1. 下载jedis的jar包
		2. 使用
			//1. 获取连接
    		Jedis jedis = new Jedis("localhost",6379);
   			//2. 操作
   			jedis.set("username","zhangsan");
    		//3. 关闭连接
    		jedis.close();
```

#### Jedis操作各种redis中的数据结构

##### 字符串类型 string

```markdown
1. 字符串类型 string
        set
        get

        //1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        //存储
        jedis.set("username","zhangsan");
        //获取
        String username = jedis.get("username");
        System.out.println(username);

        //可以使用setex()方法存储可以指定过期时间的 key value
        jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对

        //3. 关闭连接
        jedis.close();

```

##### 哈希类型 hash ： map格式 

```markdown
2. 哈希类型 hash ： map格式  
        hset
        hget
        hgetAll
        //1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        // 存储hash
        jedis.hset("user","name","lisi");
        jedis.hset("user","age","23");
        jedis.hset("user","gender","female");

        // 获取hash
        String name = jedis.hget("user", "name");
        System.out.println(name);
        // 获取hash的所有map中的数据
        Map<String, String> user = jedis.hgetAll("user");

        // keyset
        Set<String> keySet = user.keySet();
        for (String key : keySet) {
        //获取value
        String value = user.get(key);
        System.out.println(key + ":" + value);
        }

        //3. 关闭连接
        jedis.close();
```

##### 列表类型 list

```markdown
3. 列表类型 list ： linkedlist格式。支持重复元素
        lpush / rpush
        lpop / rpop
        lrange start end : 范围获取

        //1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        // list 存储
        jedis.lpush("mylist","a","b","c");//从左边存
        jedis.rpush("mylist","a","b","c");//从右边存

        // list 范围获取
        List<String> mylist = jedis.lrange("mylist", 0, -1);
        System.out.println(mylist);

        // list 弹出
        String element1 = jedis.lpop("mylist");//c
        System.out.println(element1);

        String element2 = jedis.rpop("mylist");//c
        System.out.println(element2);

        // list 范围获取
        List<String> mylist2 = jedis.lrange("mylist", 0, -1);
        System.out.println(mylist2);

        //3. 关闭连接
        jedis.close();
```

##### 集合类型 set

```markdown
4. 集合类型 set  ： 不允许重复元素
        sadd
        smembers:获取所有元素

        //1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        // set 存储
        jedis.sadd("myset","java","php","c++");

        // set 获取
        Set<String> myset = jedis.smembers("myset");
        System.out.println(myset);

        //3. 关闭连接
        jedis.close();
    
```

##### 有序集合类型 sortedset

```markdown
5. 有序集合类型 sortedset：不允许重复元素，且元素有顺序
        zadd
        zrange

        //1. 获取连接
        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        //2. 操作
        // sortedset 存储
        jedis.zadd("mysortedset",3,"亚瑟");
        jedis.zadd("mysortedset",30,"后裔");
        jedis.zadd("mysortedset",55,"孙悟空");

        // sortedset 获取
        Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);

        System.out.println(mysortedset);
        //3. 关闭连接
        jedis.close();
```

##### jedis连接池： JedisPool

```java
* 使用：
1. 创建JedisPool连接池对象
2. 调用方法 getResource()方法获取Jedis连接
    	//0.创建一个配置对象
    	JedisPoolConfig config = new JedisPoolConfig();
		config.setMaxTotal(50);
		config.setMaxIdle(10);
		
        //1.创建Jedis连接池对象
        JedisPool jedisPool = new JedisPool(config,"localhost",6379);
		
        //2.获取连接
        Jedis jedis = jedisPool.getResource();
        //3. 使用
        jedis.set("hehe","heihei");
        //4. 关闭 归还到连接池中
        jedis.close();
		
连接池工具类
			public class JedisPoolUtils {

			    private static JedisPool jedisPool;
			
			    static{
			        //读取配置文件
			        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
			        //创建Properties对象
			        Properties pro = new Properties();
			        //关联文件
			        try {
			            pro.load(is);
			        } catch (IOException e) {
			            e.printStackTrace();
			        }
			        //获取数据，设置到JedisPoolConfig中
			        JedisPoolConfig config = new JedisPoolConfig();
			        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
			        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
			
			        //初始化JedisPool
			        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
			        }
			        			    /**
			     * 获取连接方法
			     */
			    public static Jedis getJedis(){
			        return jedisPool.getResource();
			    }
			}
```

