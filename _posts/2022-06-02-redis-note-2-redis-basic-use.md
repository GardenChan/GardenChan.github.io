---
layout: post
title: "Redis笔记3--最简单的Redis基本操作"
subtitle: "Redis小白入门"
date: 2020-08-20
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Redis, NOSQL]
---
[Redis笔记【1】:为什么要使用Nosql](https://blog.csdn.net/weixin_44870909/article/details/108109353)
[Redis笔记【3】：五大数据类型之string类型](https://blog.csdn.net/weixin_44870909/article/details/108156007)
[Redis笔记【4】：五大数据类型之hash类型](https://blog.csdn.net/weixin_44870909/article/details/108228659)
[Redis笔记【5】：五大数据类型之 list 类型](https://blog.csdn.net/weixin_44870909/article/details/108266854)
[Redis笔记【6】：五大数据类型之 set 类型](https://blog.csdn.net/weixin_44870909/article/details/108267431)
[Redis笔记【7】五大数据类型之zset类型](https://blog.csdn.net/weixin_44870909/article/details/108268336)
#### redis的基本操作

##### 信息添加

* 功能：设置key、value数据

* 命令：

  ```shell
  set key value
  ```

* 示例：

  ```shell
  set city xi'an
  ```

##### 信息查询

*  功能：根据 key 查询对应的 value，如果不存在，返回空（nil）

* 命令：

  ```shell
  get key
  ```

* 示例：

  ```shell
  get name
  ```

##### 清除屏幕

* 功能：清楚屏幕中的信息

* 命令：

  ```shell
  clear
  ```

##### 退出客户端

*  功能：退出客户端

* 命令

  ```shell
  quit
  exit
  ```



##### 帮助

* 功能：获取命令帮助文档，获取组中所有命令信息名称

* 命令

  ```shell
  help 命令名称
  help @组名
  ```



