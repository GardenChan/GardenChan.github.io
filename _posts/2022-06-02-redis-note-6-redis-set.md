---
layout: post
title: "Redis笔记6--五大数据类型之set"
subtitle: "Redis set的使用场景"
date:  2020-08-29
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
### Redis笔记
[Redis笔记【1】:为什么要使用Nosql](https://blog.csdn.net/weixin_44870909/article/details/108109353)
[Redis笔记【2】：最简单的redis操作命令](https://blog.csdn.net/weixin_44870909/article/details/108118394)
[Redis笔记【3】：五大数据类型之string类型](https://blog.csdn.net/weixin_44870909/article/details/108156007)
[Redis笔记【4】：五大数据类型之hash类型](https://blog.csdn.net/weixin_44870909/article/details/108228659)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)

#### set类型
* set 基于的新的存储需求：存储大量的数据，在查询方面提供更高的效率
* 需要的存储结构：能够保存大量的数据，**高效**的内部存储机制，**便于查询**
#### set类型存储空间
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827194948144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### set类型数据的基本操作
* **添加数据**

```powershell
sadd key member1 [member2]
```
*  **获取全部数据**
```powershell
smembers key
```
*  **删除数据**
```powershell
srem  key member1 [member2]
```
*  **获取集合数据总量**
```powershell
scard key
```
*  **判断集合中是否包含指定数据**
```powershell
sismember key member
```
#### set类型数据的扩展操作
* 随机获取集合中指定数量的数据
```powershell
srandmember key [count]
```
* 随机获取集合中的某个数据并将该数据移出集合
```powershell
spop key [count]
```
* 求两个集合的交集、并集、差集
```powershell
sinter key1 [key2]
sunion key1 [key2]
sdiff key1 [key2]
```
* 求两个集合的交、并、差集并存储到指定集合中
```powershell
sinterstore destination key1 [key2]
sunionstore destination key1 [key2]
sdiffstore destination key1 [key2]
```
* 将指定数据从原始集合中移动到目标集合中
```powershell
smove source destination member
```
### set类型业务场景
#### 场景1 ： redis 应用于随机推荐类信息检索，例如热点歌单推荐，热点新闻推荐，热卖旅游线路，应用APP推荐，大V推荐等
#### 场景1 描述：每位用户首次使用今日头条时会设置3项爱好的内容，但是后期为了增加用户的活跃度、兴趣点，必须让用户对其他信息类别逐渐产生兴趣，增加客户留存度，如何实现？
##### 业务分析
1. 系统分析出各个分类的最新或最热点信息条目并组织成set集合
2.  随机挑选其中部分信息
3. 配合用户关注信息分类中的热点信息组织成展示的全信息集合
##### 场景1 解决方案
* 随机获取集合中指定数量的数据
```powershell
srandmember key [count]
```
* 随机获取集合中的某个数据并将该数据移出集合
```powershell
spop key [count]
```
#### 场景2 redis 应用于同类信息的关联搜索，二度关联搜索，深度关联搜索
##### 场景举例：
##### 1、脉脉为了促进用户间的交流，保障业务成单率的提升，需要让每位用户拥有大量的好友，事实上职场新人不具有更多的职场好友，如何快速为用户积累更多的好友？
##### 2、新浪微博为了增加用户热度，提高用户留存性，需要微博用户在关注更多的人，以此获得更多的信息或热门话题，如何提高用户关注他人的总量？
##### 3、QQ新用户入网年龄越来越低，这些用户的朋友圈交际圈非常小，往往集中在一所学校甚至一个班级中，如何帮助用户快速积累好友用户带来更多的活跃度？
##### 4、微信公众号是微信信息流通的渠道之一，增加用户关注的公众号成为提高用户活跃度的一种方式，如何帮助用户积累更多关注的公众号？
##### 5、美团外卖为了提升成单量，必须帮助用户挖掘美食需求，如何推荐给用户最适合自己的美食？
##### 场景2 解决方案
* 求两个集合的交集、并集、差集
```powershell
sinter key1 [key2]
sunion key1 [key2]
sdiff key1 [key2]
```
* 求两个集合的交、并、差集并存储到指定集合中
```powershell
sinterstore destination key1 [key2]
sunionstore destination key1 [key2]
sdiffstore destination key1 [key2]
```
* 将指定数据从原始集合中移动到目标集合中
```powershell
smove source destination member
```
##### set类型数据操作的注意事项
*  set 类型不允许数据重复，如果添加的数据在 set 中已经存在，将只保留一份
*  set 虽然与hash的存储结构相同，但是无法启用hash中存储值的空间

#### 场景3 redis 应用于同类型数据的快速去重
##### 场景3 描述：
*  公司对旗下新的网站做推广，统计网站的PV（访问量）,UV（独立访客）,IP（独立IP）。
* PV：网站被访问次数，可通过刷新页面提高访问量
* UV：网站被不同用户访问的次数，可通过cookie统计访问量，相同用户切换IP地址，UV不变
* IP：网站被不同IP地址访问的总次数，可通过IP地址统计访问量，相同IP不同用户访问，IP不变

##### 场景3 解决方案：
*  利用set集合的数据去重特征，记录各种访问数据
*  建立string类型数据，利用incr统计日访问量（PV）
* 建立set模型，记录不同cookie数量（UV）
* 建立set模型，记录不同IP数量（IP）


#### 场景4 redis 应用于基于黑名单与白名单设定的服务控制
##### 场景4 描述
* **黑名单**
> * 资讯类信息类网站追求高访问量，但是由于其信息的价值，往往容易被不法分子利用，通过爬虫技术，快速获取信息，个别特种行业网站信息通过爬虫获取分析后，可以转换成商业机密进行出售。例如第三方火车票、机票、酒店刷票代购软件，电商刷评论、刷好评。
> * 同时爬虫带来的伪流量也会给经营者带来错觉，产生错误的决策，有效避免网站被爬虫反复爬取成为每个网站都要考虑的基本问题。在基于技术层面区分出爬虫用户后，需要将此类用户进行有效的屏蔽，这就是黑名单的典型应用。
> * ps:不是说爬虫一定做摧毁性的工作，有些小型网站需要爬虫为其带来一些流量。

* **白名单**

> * 对于安全性更高的应用访问，仅仅靠黑名单是不能解决安全问题的，此时需要设定可访问的用户群体，
依赖白名单做更为苛刻的访问验证。
##### 场景4 解决方案
*  基于经营战略设定问题用户发现、鉴别规则
*  周期性更新满足规则的用户黑名单，加入set集合
* 用户行为信息达到后与黑名单进行比对，确认行为去向
* 黑名单过滤IP地址：应用于开放游客访问权限的信息源
*  黑名单过滤设备信息：应用于限定访问设备的信息源
*  黑名单过滤用户：应用于基于访问权限的信息源



