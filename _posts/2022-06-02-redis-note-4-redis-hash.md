---
layout: post
title: "Redis笔记4--五大数据类型之hash"
subtitle: "Redis hash的使用场景"
date: 2020-08-27
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
[Redis笔记【1】:为什么要使用Nosql](https://blog.csdn.net/weixin_44870909/article/details/108109353)
[Redis笔记【2】：最简单的redis操作命令](https://blog.csdn.net/weixin_44870909/article/details/108118394)
[Redis笔记【3】：五大数据类型之string类型](https://blog.csdn.net/weixin_44870909/article/details/108156007)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)

#### 结合实际谈谈为什么需要hash
* 其实为什么需要一个新的数据类型，而不是仅仅就用string类型，根本的原因还是string类型满足不了一些需求，用hash来作为解决方案。
* 在存储上有一个问题，就是**对象类型的存储**如果具有**较频繁的更新需求**，用string来频繁操作会显得笨重。
* **新的存储需求**:对一系列存储的数据进行编组，方便管理，典型应用是存储对象信息。
* **需要 的存储结构**：一个存储空间保存多个键值对数据
* **hash类型**：底层使用哈希表结构实现数据存储
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825204539216.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### hash类型数据的基本操作
* **添加/修改数据**

```shell
hset key field value
```
* **添加/修改多个数据**
```shell
hmset key field1 value1 field2 value2 ... 
```
* **获取数据**
```shell
hget key field  # 获取单个field数据
```
```shell
hgetall key # 获取该key的全部field数据
```
* **获取多个数据**
```shell
hmget key field1 field2 ...
```
* **删除数据**
```shell
hdel key field1 [field2]
```
* **获取哈希表中字段的数量**
```shell
hlen key
```
* **获取哈希表中是否存在指定的字段**
```shell
hexists key field
```
#### hash类型数据的扩展操作
* **获取哈希表中所有字段名或字段值**

```shell
hkeys key
hvals key
```
* **设置指定字段的数值数据增加指定范围的值**

```shell
hincrby key field increment
hincrbyfloat key field increment
```
* **hsetnx**
* hsetnx 将哈希表 key 中的域 field 的值设置为 value ，当且仅当域 field 不存在。若域 field 已经存在，该操作无效。如果 key 不存在，一个新哈希表被创建并执行 hsetnx 命令。 
```shell
hsetnx key field value
```
#### hash 类型数据操作的注意事项
1.  hash类型下的value**只能存储字符串**，不允许存储其他数据类型，**不存在嵌套现象**。如果数据未获取到，
对应的值为（nil）。
2. 每个 hash 可以存储  *2^32^-1*个键值对
3.  hash类型十分贴近对象的数据存储形式，并且可以灵活添加删除对象属性。但hash设计初衷不是为了存
储大量对象而设计的，切记不可滥用，更**不可以将hash作为对象列表使用**
4.  hgetall 操作可以获取全部属性，如果**内部field过多，遍历整体数据效率就很会低**，有可能成为数据访问
瓶颈

### hash 类型应用场景
#### 场景1：redis 应用于购物车数据存储设计
#### 电商网站购物车设计与实现
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827182321768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
##### 场景1：业务分析
* 仅分析购物车的redis存储模型：添加、浏览、更改数量、删除、清空
* 购物车于数据库间持久化同步（不讨论）
* 购物车于订单间关系（不讨论）
* 未登录用户购物车信息存储（不讨论）
##### 场景1：解决方案
1. **以客户id作为key**，每位客户创建一个hash存储结构存储对应的购物车信息
2. 将**商品id作为field**，**购买数量作为value**进行存储
3. **添加商品**：追加全新的field与value
4. **浏览**：遍历hash
5. **更改商品数量**：自增、自减，设置value值
6. **删除商品**：删除field
7. **清空购物车**：删除key
##### 场景1：深入思考？
###### 场景1：当前设计是否加速了购物车的呈现
* 当前仅仅是将数据存储到了redis中，并没有起到加速的作用，商品信息还需要二次查询数据库
* 方法：将每条购物车中的商品记录保存成两条field
* field1 专用于保存购买数量
> field: 商品id:nums
> value: 商品购买数量
* field2 专用于保存购物车中显示的信息，包含文字描述，图片地址、所属商家信息等
> field: 商品id:info
> value: json

#### 场景2：redis 应用于抢购，限购类、限量发放优惠卷、激活码等业务的数据存储设计
#### 双11活动日，销售手机充值卡的商家对移动、联通、电信的30元、50元、100元商品推出抢购活动，每种商品抢购上限1000张
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827184839347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
##### 场景2：解决方案
1. 以商家id作为key
2. 将参与抢购的商品id作为field
3. 将参与抢购的商品数量作为对应的value
4. 抢购时使用降值的方式控制产品数量
5. **注意**：实际业务中还有超卖等实际问题，这里不做讨论


