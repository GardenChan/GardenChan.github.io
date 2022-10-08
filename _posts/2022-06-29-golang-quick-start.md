---
layout: post
title: "Go语言在各种操作系统下的开发环境"
subtitle: "go语言开发环境及其配置保姆级教程"
date: 2022-06-18
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Golang]
---

# 4个步骤配置一个 Go 开发环境

1. Linux服务器申请和配置
2. 依赖安装和配置
3. Go 编译环境安装和配置
4. Go 开发 IDE 安装和配置

本文以CentOS 8.2为例

# 一、Linux 服务器申请和配置
## 1.1 Linux 服务器申请
一般可以通过以下3种方式，安装一个CentOS 8.2系统。
1. 物理机上安装一个CentOS 8.2系统；
2. 虚拟机软件创建CentOS 8.2虚拟机。例如，Windows的VMWare Workstation，MacBook可以使用VirtualBox。
3. 云厂商提供的虚拟机（云服务器），例如腾讯云的CVM、阿里云的ECS、华为云的ECS等。

本文直接使用腾讯云的LightHouse轻量级云服务器。

## 1.2 Linux 服务器配置
在云厂商购买完云服务器后，可以通过云服务器的控制台通过webshell进行登录，但是目前来看，各大云厂商的web远程登陆云服务器的体验还不是很好。还是建议使用Xshell或者SecureCRT等工具来登录云服务器。

完成远程登陆云服务器之后，我们可以开始下面的步骤进行服务器的相关配置。

### 