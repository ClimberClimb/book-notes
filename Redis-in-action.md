# Redis in action
## 第一章
### Redis简介
* Redis是一个速度非常快的非关系数据库，它可以存储键和5种不同类型value之间的映射。

* Redis实现了主从复制特性，执行复制的从服务器会连接主服务器，接受主服务器发送的整个数据库的初始copy，之后的主服务器的写命令，都会被发送给所有的连接的从服务器去执行，从而更新从服务器的数据集。

* 在Redis里面，用户可以直接使用原子的INCR命令及其变种来计算聚合数据，因为Redis数据在内存里面，而且发送给Redis的命令并不需要经过查询分析器或者查询优化器进行处理，所以Redis存储的数据执行随机写的速度总是迅速的。

一些数据库和缓存服务器的特性如下：

|名称| 类型 | 数据存储选项 |查询类型| 附加功能 |
| :--: | :--: | :--: |:--: | :--: |
| Redis | 使用内存存储的非关系数据库 | 字符串、列表、集合、散列表、有序集合|每种数据类型都有自己的专属命令，另外还有批量操作和不完全的事务支持| 发布和订阅，主从复制，持久化，脚本(存储化过程)|
|memcached|使用内存存储的键值缓存|键值之间的映射|创建命令、读取命令、更新命令、删除命令以及其他几个命令|为提升性能而设的多线程服务器|
|MySQL|关系数据库|每个数据库可以包含多个表，没个表可以包含多个行，可以处理多个表的视图，支持空间和第三方扩展| SELECT，INSERT，UPDATE，DELETE，函数，存储过程|支持ACID性质，主从复制和主主复制|
|PostgreSQL|关系数据库|每个数据库可以包含多个表，每个表可以包含多个行，可以处理多个表的视图，支持空间和第三方扩展，支持可定制类型|SELECT，INSERT，UPDATE，DELETE，内置函数，自定义的存储过程| 支持ACID性质，主从复制，由第三方支持的多主复制|
|MongoDB|使用硬盘存储的非关系文档存储|每个数据库可以包含多个表，每个表可以包含多个schema的BSON文档|创建命令，读取命令，更新命令，删除命令，条件查询命令等|支持map-reduce操作，主从复制，分片，空间索引



### Redis 基础命令
#### STRING

<div align="center"> <img src="pics/6019b2db-bc3e-4408-b6d8-96025f4481d6.png" width="400"/> </div><br>

```html
> set hello world
OK
> get hello
"world"
> del hello
(integer) 1
> get hello
(nil)
```

#### LIST

<div align="center"> <img src="pics/fb327611-7e2b-4f2f-9f5b-38592d408f07.png" width="400"/> </div><br>

```html
> rpush list-key item
(integer) 1
> rpush list-key item2
(integer) 2
> rpush list-key item
(integer) 3

> lrange list-key 0 -1
1) "item"
2) "item2"
3) "item"

> lindex list-key 1
"item2"

> lpop list-key
"item"

> lrange list-key 0 -1
1) "item2"
2) "item"
```

#### SET

<div align="center"> <img src="pics/cd5fbcff-3f35-43a6-8ffa-082a93ce0f0e.png" width="400"/> </div><br>

```html
> sadd set-key item
(integer) 1
> sadd set-key item2
(integer) 1
> sadd set-key item3
(integer) 1
> sadd set-key item
(integer) 0

> smembers set-key
1) "item"
2) "item2"
3) "item3"

> sismember set-key item4
(integer) 0
> sismember set-key item
(integer) 1

> srem set-key item2
(integer) 1
> srem set-key item2
(integer) 0

> smembers set-key
1) "item"
2) "item3"
```

#### HASH

<div align="center"> <img src="pics/7bd202a7-93d4-4f3a-a878-af68ae25539a.png" width="400"/> </div><br>

```html
> hset hash-key sub-key1 value1
(integer) 1
> hset hash-key sub-key2 value2
(integer) 1
> hset hash-key sub-key1 value1
(integer) 0

> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"

> hdel hash-key sub-key2
(integer) 1
> hdel hash-key sub-key2
(integer) 0

> hget hash-key sub-key1
"value1"

> hgetall hash-key
1) "sub-key1"
2) "value1"
```

#### ZSET

<div align="center"> <img src="pics/1202b2d6-9469-4251-bd47-ca6034fb6116.png" width="400"/> </div><br>

```html
> zadd zset-key 728 member1
(integer) 1
> zadd zset-key 982 member0
(integer) 1
> zadd zset-key 982 member0
(integer) 0

> zrange zset-key 0 -1 withscores
1) "member1"
2) "728"
3) "member0"
4) "982"

> zrangebyscore zset-key 0 800 withscores
1) "member1"
2) "728"

> zrem zset-key member1
(integer) 1
> zrem zset-key member1
(integer) 0

> zrange zset-key 0 -1 withscores
1) "member0"
2) "982"
```

### 文章点赞

#### 数据模型
两个有序集合：

* time:，有序集合，存储的是根据时间排序的文章
* score:，有序集合存储的是根据评分排序的文章
* vote:acrticle_id，无序集合，存储的是为这个文章投票的用户
* article:article_id,散列表，存储文章的投票数 

#### 操作
##### 投票

```python

ONE_WEEK_IN_SECONDS = 7 * 86400                    
VOTE_SCORE = 432                                   
def article_vote(conn, user, article):
    cutoff = time.time() - ONE_WEEK_IN_SECONDS      
    # 获取文章的post的时间，看是否
    if conn.zscore('time:', article) < cutoff:    
        return

    article_id = article.partition(':')[-1] 
    # 根据sadd是否成功来，判断是否已经用户是否投票过     
    if conn.sadd('voted:' + article_id, user): 
    	 # 如果没有投票，增加文章的评分，每投一票增加VOTE_SCORE,     
        conn.zincrby('score:', article, VOTE_SCORE) 
        # 增加散列表里面文章的投票数
        conn.hincrby(article, 'votes', 1)           
```

##### 增加新文章

```python

def post_article(conn, user, title, link):
	# 自增，获取文章的id呀
    article_id = str(conn.incr('article:'))     #A

    voted = 'voted:' + article_id
    # 把作者添加到voted里面
    conn.sadd(voted, user)                      #B
    # 设置集合的过期日子
    conn.expire(voted, ONE_WEEK_IN_SECONDS)     #B

    now = time.time()
    article = 'article:' + article_id
    # 设置散列表的 key和value
    conn.hmset(article, {                       #C
        'title': title,                         #C
        'link': link,                           #C
        'poster': user,                         #C
        'time': now,                            #C
        'votes': 1,                             #C
    })                                          #C
	# 增加新的article 增加到score和time的有序集合
    conn.zadd('score:', article, now + VOTE_SCORE)  #D
    conn.zadd('time:', article, now)                #D

    return article_id          
```

##### 获取文章
```python
ARTICLES_PER_PAGE = 25

def get_articles(conn, page, order='score:'):
    start = (page-1) * ARTICLES_PER_PAGE            #A
    end = start + ARTICLES_PER_PAGE - 1             #A
	 # 获取多个文章的id
    ids = conn.zrevrange(order, start, end)         #B
    articles = []
    for id in ids:      
    	 # 获取文章的详细信息，所有的key，value都取出                            #C
        article_data = conn.hgetall(id)             #C
        article_data['id'] = id                     #C
        articles.append(article_data)               #C

    return articles

```

##### 文章分组
```python

def add_remove_groups(conn, article_id, to_add=[], to_remove=[]):
    article = 'article:' + article_id           #A
    for group in to_add:
        # 添加到所属的群组
        conn.sadd('group:' + group, article)    #B
    for group in to_remove:
        # 从群组里面移除文章
        conn.srem('group:' + group, article)    #C
```

##### 获取分组文章

```python
def get_group_articles(conn, group, page, order='score:'):
    key = order + group                                     #A
    # 看是否存在key
    if not conn.exists(key):                                #B
        # 进行交集计算，计算新的集合
        conn.zinterstore(key,                               #C
            ['group:' + group, order],                      #C
            aggregate='max',                                #C
        )
        #过期时间设置60s
        conn.expire(key, 60)                                #D
    return get_articles(conn, page, key)                    #E
```