---
layout: post
title: "Redis笔记3--五大数据类型之string类型"
subtitle: "Redis string的使用场景"
date: 2020-08-23
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
[Redis笔记【1】:为什么要使用Nosql](https://blog.csdn.net/weixin_44870909/article/details/108109353)
[Redis笔记【2】：最简单的redis操作命令](https://blog.csdn.net/weixin_44870909/article/details/108118394)
[Redis笔记【4】：五大数据类型之hash类型](https://blog.csdn.net/weixin_44870909/article/details/108228659)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)
#### Redis业务数据的特殊性
##### 作为缓存使用
1. **原始业务功能设计**
* 秒杀
* 618活动
* 双11活动
* 排队购票
2. **运营平台监控到的突发高频访问数据**
* 突发时政要闻，被强势关注围观
3. **高频、复杂的统计数据**
* 在线人数
* 投票排行榜
##### 附加功能
1. **系统功能优化或升级**
* 单服务器升级集群
* Session管理
* Token管理
----
#### Redis数据存储类型
##### 5种常用数据类型
1. **string**             String
2. **hash**              HashMap
3. **list**                 LinkedList
4. **set**                 HashSet
5. **sorted_set**     TreeSet
##### redis数据存储格式说明（重要）
*  redis 自身是一个 Map，其中所有的数据都是采用 key : value 的形式存储
*  数据类型指的是存储的数据的类型，也就是 value 部分的类型，key 部分永远都是字符串
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821204005347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
----
### String类型
* 存储的数据：单个数据，最简单的数据存储类型，也是最常用的数据存储类型
* 存储数据的格式：一个存储空间保存一个数据
*  存储内容：通常使用字符串，如果字符串以整数的形式展示，可以作为数字操作使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200821213809586.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### string类型数据的基本操作
* 添加/修改数据

```shell
set key value
```
* 获取数据
```shell
get key
```
* 删除数据
```shell
del key
```
* 添加/修改多个数据
```shell
mset key1 value1 key2 value2 ...
```
* 获取多个数据
```shell
mget key1 key2 ...
```
* 获取数据字符个数
```shell
strlen key
```
* 追加信息到原始信息后部（如果原始信息存在就追加，否则新建）
```shell
append key value
```
#### string类型数据的扩展操作 1
* 设置数值数据增加指定范围的值
```shell
incr key
incrby key increment
incrbyfloat key increment
```
* 设置数值数据减少指定范围的值
```shell
decr key
decrby key increment
```
* **注意：string作为数值操作时**
首先，string在redis内部存储默认就是一个字符串，当遇到增减类操作incr、decr时会转成数值型进行计算
其次，redis所有的操作都是原子性的，采用单线程处理所有业务，命令时一个一个执行的，因此无需考虑并发带来的数据影响。
另外，需要注意的是，当string被当作数值来进行操作时，如果原始数据（string）不能转成数值，或超越了redis数值上限范围，将会报错。（redis的数值上限时java中long型数据最大值，Long.MAX_VALUE）

#### string类型数据的扩展操作 2 数据生命周期
* **业务场景举例：**
1. “最强女生”启动海选投票，只能通过微信投票，每个微信号每 4 小时只能投1票。
2. 电商商家开启热门商品推荐，热门商品不能一直处于热门期，每种商品热门期维持3天，3天
3. 新闻网站会出现热点新闻，热点新闻最大的特征是时效性，如何自动控制热点新闻的时效性。
以上例子体现的一个特点是 数据的时效性。
**redis 控制数据的生命周期，通过数据是否失效控制业务行为，适用于所有具有时效性限定控制的操作**

* **解决方案：数据生命周期**
* 设置数据具有指定的生命周期
```shell
setex key seconds value    
psetex key milliseconds value
```

#### string 类型数据操作的注意事项
* 数据操作成功与否
1.  (integer) 0 → false 失败
2.  (integer) 1 → true  成功
* 运行结果值
3.  (integer) 3 → 3 3个 
4.  (integer) 1 → 1 1个
* 数据未获取到
（nil）等同于null
* 数据最大存储量：512MB
* 数值计算最大范围：（java中的long的最大值）9223372036854775807

### string类型应用场景
#### 业务场景：主页高频访问信息显示控制，例如新浪微博大V主页显示粉丝数与微博数量
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082321494169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### 解决方案: redis应用于各种结构型和非结构型高热度数据访问加速
##### 在redis中为大V用户设定用户信息，以用户主键和属性值作为key，后台设定定时刷新策略即可
* eg： user: id :3506728370:fans  → 12210947
* eg： user: id :3506728370:blogs→ 6164
* eg： user: id :3506728370:focus  → 83
##### 在redis中以json格式存储大V用户信息，定时刷新（也可以使用hash类型）
* eg:  user:id :3506728370 →  {"id":3506728370,"name":"春晚","fans":12210862,"blogs":6164, "focus":83}

#### KEY的设置约定
* 数据库中的热点数据key命名惯例：**表名：主键名：主键值：字段名**
* eg1    **order:id :29437595:name**
* eg2 **equip:id :390472345:type**
* eg3 **news:id :202004150:title**




