---
layout: post
title: "Redis笔记1--为什么要使用NOSQL"
subtitle: "一切还是要从NOSQL解决的问题场景来谈~"
date: 2020-08-19
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
[Redis笔记【2】：最简单的redis操作命令](https://blog.csdn.net/weixin_44870909/article/details/108118394)
[Redis笔记【3】：五大数据类型之string类型](https://blog.csdn.net/weixin_44870909/article/details/108156007)
[Redis笔记【4】：五大数据类型之hash类型](https://blog.csdn.net/weixin_44870909/article/details/108228659)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)
#### redis面对的问题

1. 海量用户
2. 高并发

#### 问题的罪魁祸首

1. 性能瓶颈： 磁盘IO性能低下
2. 扩展瓶颈：数据关系复杂，扩展性差，不便于大规模集群

#### 解决思路：NoSQL

1. 降低磁盘IO次数 越低越好—————> 使用内存存储
2. 去除数据间的关系，越简单越好——————>不存储关系，仅存储数据

#### Nosql

* Nosql：Not-Only SQL（非关系型数据库），作为关系型数据库的补充
* 作用：应对基于海量用户和海量数据前提下的数据处理问题。
* 特征：
  1. 可扩容，可伸缩
  2. 大数据量下高性能
  3. 灵活的数据模型
  4. 高可用
* 常见的Nosql数据库：
  1. Redis
  2. memcache
  3. HBase
  4. MongoDB

#### Nosql的电商场景解决方案


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819200414802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200819200414992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
#### redis简介

* **什么是redis？**

  Redis (REmote DIctionary Server) 是用 C 语言开发的一个开源的高性能键值对（key-value）数据库。

* **redis的特点：**

  1. 数据间没有必然的关联关系
  2. 内部采用单线程机制进行工作
  3. 高性能
  4. 多数据类型支持
  5. 持久化支持，可以进行数据灾难恢复。

* **redis的应用**

  1. 为热点数据加速查询（主要使用场景），如热点商品、热点新闻、热点资讯、推广类等高访问量信息等
  2. 任务队列，如秒杀、抢购、购票排队等
  3. 即时消息查询，如各位排行榜、各类网站访问统计、公交到站信息、在线人数信息（聊天室、网站）、设
     备信号等
  4. 时效性信息控制，如验证码控制、投票控制等
  5. 分布式数据共享，如分布式集群架构中的 session 分离
  6. 消息队列
  7. 分布式锁

* **windows版本下载与安装**
 1. 下载地址：[windows版redis下载](https://github.com/MSOpenTech/redis/tags)
  2. 下载后解压缩即可
  3. ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200820101619978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
4. 核心文件
     * **redis-server.exe**    服务器启动命令
     * **redis-cli.exe**    命令行客户端
     * **redis.windows.conf**    redis核心配置文件
     * **redis-benchmark.exe**    性能测试工具
     * **redis-check-aof.exe**    AOF文件修复工具
     * **redis-check-dump.exe**    RDB文件检查工具（快照持久化文件）




