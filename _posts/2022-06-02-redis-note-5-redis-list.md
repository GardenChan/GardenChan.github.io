---
layout: post
title: "Redis笔记5--五大数据类型之list"
subtitle: "Redis list的使用场景"
date: 2020-08-28
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
[Redis笔记【1】:为什么要使用Nosql](https://blog.csdn.net/weixin_44870909/article/details/108109353)
[Redis笔记【2】：最简单的redis操作命令](https://blog.csdn.net/weixin_44870909/article/details/108118394)
[Redis笔记【3】：五大数据类型之string类型](https://blog.csdn.net/weixin_44870909/article/details/108156007)
[Redis笔记【4】：五大数据类型之hash类型](https://blog.csdn.net/weixin_44870909/article/details/108228659)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)
#### list 类型
* **list 基于的数据存储需求**：存储**多个**数据，并**对数据进入存储空间的顺序进行区分**
* **需要的存储结构**：一个存储空间保存多个数据，且通过数据可以**体现进入顺序**
* list 类型：保存多个数据，**底层使用双向链表存储结构实现**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827185940875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### Redis存储空间
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190048864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### list 类型数据基本操作
* **添加/修改数据**

```shell
lpush key value1 [value2] ...
rpush key value1 [value2] ...
```
* **获取数据**

```shell
lrange key start stop  # 指定索引范围获取数据
lindex key index   # 指定某一具体索引值获取数据
llen key # 获取list长度（数据量）
```
* **获取并移除数据**
```shell
lpop key
rpop key
```
#### list 类型数据扩展操作
* **规定时间内获取并移除数据**
```shell
# 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。
blpop key1 [key2] timeout

# 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。
brpop key1 [key2] timeout

# 命令从列表中取出最后一个元素，并插入到另外一个列表的头部； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。
brpoplpush source destination timeout
```
#### list 类型数据操作注意事项
*  list中保存的数据都是string类型的，数据总容量是有限的，最多2^32^ - 1 个元素 (4294967295)
*  list具有索引的概念，但是操作数据时通常以队列的形式进行入队出队操作，或以栈的形式进行入栈出栈操作
* 获取全部数据操作结束索引设置为-1
*  list可以对数据进行分页操作，通常第一页的信息来自于list，第2页及更多的信息通过数据库的形式加载

### list 业务场景
#### 场景1：redis 应用于具有操作先后顺序的数据控制
##### 场景描述：微信朋友圈点赞，要求按照点赞顺序显示点赞好友信息；如果取消点赞，移除对应好友信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827192422294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
##### 场景1 解决方案：移除指定数据
* 移除指定数据
根据参数 count 的值，移除列表中与参数 value 相等的元素

```shell
lrem key count value
```
* **count > 0** : 从表头开始向表尾搜索，移除与 VALUE 相等的元素，数量为 COUNT 。
* **count < 0** : 从表尾开始向表头搜索，移除与 VALUE 相等的元素，数量为 COUNT 的绝对值。
* **count = 0** : 移除表中所有与 VALUE 相等的值。


#### 场景2：redis 应用于最新消息展示
##### 场景2描述：twitter、新浪微博、腾讯微博中个人用户的关注列表需要按照用户的关注顺序进行展示，粉丝列表需要将最近关注的粉丝列在前面
* 新闻、资讯类网站如何将最新的新闻或资讯按照发生的时间顺序展示？
* 企业运营过程中，系统将产生出大量的运营数据，如何保障多台服务器操作日志的统一顺序输出？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827193138136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
##### 场景2 解决方案
1.  依赖list的数据具有顺序的特征对信息进行管理
2. 使用队列模型解决多路信息汇总合并的问题
3. 使用栈模型解决最新消息的问题


