---
layout: post
title: "登录业务的演变、单点登录（SSO）的三种解决方案"
subtitle: "技术的发展不是一蹴而就的，都是伴随在业务需求之后逐步演化的"
date: 2020-11-17
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [后端技术]
---
@[TOC](单点登录)
# 一、登录业务的演变
## 1.1 单一服务器模式的登录业务
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117220809833.gif#pic_center)
早期单体应用的登录使用session对象（技术）实现，登录成功之后，把用户数据放到session里面。后续的请求中判断是否登录，就从session种获取数据，从而获取到登录信息。这样的登录模式无法扩展。

```java
session.setAttribute("user",user);
session.getAttribute("user");
```

## 1.2 分布式集群部署下的登录业务
随着微服务架构的到来，大多应用采用分布式集群部署，不同服务一般部署在不同的服务器上，如何才能达到在其中某一个服务登录后，在其他服务不必再次登录的效果？这就诞生了单点登录（SSO，Single Sign on）这个概念。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117224855719.gif#pic_center)

> *百度百科*
> **单点登录（Single Sign On）**，简称为 SSO，是比较流行的企业业务整合的解决方案之一。SSO的定义是在多个应用系统中，用户只需要登录一次就可以访问所有相互信任的应用系统。

单点登录具有用户身份信息独立管理，更好适用于分布式管理的优点。但是同时其缺点是认证服务器的访问压力较大。


# 二、单点登录（SSO）的3种解决方案
目前单点登录比较常用的方式有**三种**，分别是
**1. 利用session广播（session共享）实现**
**2. 利用cookie+redis实现**
**3. 使用token实现**

下面分别阐述三种方法的原理。

## 2.1 session广播（session共享）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117222717860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 一般情况下，HttpSession是通过Servlet容器创建并进行管理的，创建成功之后都是保存在内存中，而不同的服务一般部署在不同的服务器上，那么只需要解决不同服务能够共享一个session容器就可以了，也就在不同服务之间实现session的共享。
* 更加具体的解决方案是使用redis，就是把原本存储在不同服务器上的Session拿出来放在一个独立的服务器上，之后取用session也从该独立的session服务去取，这样也就实现了session的共享机制。

## 2.2 cookie+redis实现
使用cookie和redis实现，要求在项目任何一个服务（模块）进行登录后，**把用户数据放到两个地方。一个是cookie，一个是redis。**
* **redis** 中存放包含登录用户信息的键值对（Key:Value）,例如键值Key可以采用用户的用户的id或者登录的IP等，Value一般采用用户相关的数据，用户名等等。
* **cookie**中则是放入与上述存入redis一致的键值。
* **逻辑**：

> 当用户在其中一个服务（模块）中登录后，在redis和cookie都有其相关数据，当用户访问另一服务时，发送的请求会携带登录后存在的cookie，服务通过cookie值到redis中进行查询，如果在redis中能查询得到数据，说明该用户已经登录，如果未查到则可以跳转到登录页面。

## 2.3 使用token实现
* 如果不知道token的意义，可以简单的将token理解为按照一定规则生成的字符串，字符串中包含用户信息。
* 使用token登录，当用户在项目某个服务（模块）进行登录后，后端按照规则生成字符串token，把登录用户的部分信息包含到该字符串token中，然后将字符串token返回。
* 返回方式可以通过cookie返回，也可以通过地址栏返回。
* **逻辑**：

> 当用户在某一服务（模块）登录后，再去访问其他的服务（模块）时，只需要按规则获取地址栏的字符串（或者获取cookie值），如果获取到的字符串验证通过则说明是已经登录过的用户，否则需要重新登录。
