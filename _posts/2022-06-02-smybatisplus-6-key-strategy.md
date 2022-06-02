---
layout: post
title: "Mybatis-Plus的6种主键生成策略简介"
subtitle: "数据库自增、雪花算法、用户输入、UUID..."
date: 2020-10-17
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [MyBatisPlus, 数据库]
---
# 六种策略

```java
/**
 * 数据库ID自增,数据库需要支持主键自增(如MySQL)，并设置主键自增
 */
AUTO(0),


/**
 * 该类型为未设置主键类型,默认使用雪花算法生成（snowflake）
 */
NONE(1),


/**
 * 用户输入ID,数据类型和数据库保持一致就行
 * <p>该类型可以通过自己注册自动填充插件进行填充</p>
 */
INPUT(2),


/* 以下3种类型、只有当插入对象ID 为空，才自动填充。 */
/**
 * 全局唯一ID (idWorker),数值类型  数据库中也必须是数值类型 否则会报错
 * mp自带策略，生成19位值，数字类型使用这种策略 比如long
 */
ID_WORKER(3),


/**
 * 全局唯一ID (UUID，不含中划线)
 * 每次生成随即唯一的值
 * 缺点：排序不方便
 */
UUID(4),


/**
 * 字符串全局唯一ID (idWorker 的字符串表示)，数据库也要保证一样字符类型
 * mp自带策略，生成19位值 字符串类型使用这种策略
 */
ID_WORKER_STR(5);
```
## 直接在实体类的主键字段加注解配置使用哪一种策略
举例：

```java
@TableId(type = IdType.ID_WORKER_STR)
private String id;
```
