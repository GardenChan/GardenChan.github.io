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

### 步骤1：root用户登录Linux系统，创建普通用户

一般来说，一个项目会由多个开发人员协作完成，为了节省企业成本，公司不会给每个开发人员都配备一台服务器，而是让所有开发人员共用一个开发机，通过普通用户登录开发机进行开发。因此，为了模拟真实的企业开发环境，我们也通过一个普通用户
的身份来进行项目的开发，创建方法如下：

``` shell
# 创建 going 用户
useradd going
# 给going用户设置密码
passwd going

Changing password for user going. 
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
```

### 步骤2：添加sudoers
将普通用户加入到sudoers中，这样普通用户就可以通过sudo命令来暂时获取Root的权限。执行如下命令进行添加：
```shell
sed -i '/^root.*ALL=(ALL).*ALL/a\going\tALL=(ALL) \tALL' /etc/sudoers
```
### 步骤3：用新的用户名（going）和密码登录Linux服务器
本步骤用于验证普通用户是否创建成功

### 步骤4：配置$HOME/.bashrc文件
我们登录新服务器后的第一步就是配置 $HOME/.bashrc 文件，以使 Linux 登录 shell 更加易用，例如配置 LANG 解决中文乱码，配置 PS1 可以避免整行都是文件路径，并将$HOME/bin 加入到 PATH 路径中。配置后的内容如下：
```shell
# .bashrc 
# User specific aliases and functions 
alias rm='rm -i' 
alias cp='cp -i' 
alias mv='mv -i' 

# Source global definitions 
if [ -f /etc/bashrc ]; then 
        . /etc/bashrc 
fi

# User specific environment 
# Basic envs 
export LANG="en_US.UTF-8" # 设置系统语言为 en_US.UTF-8，避免终端出现中文乱码

export PS1='[\u@dev \W]\$ ' # 默认的 PS1 设置会展示全部的路径，为了防止过长，这里只展示

export WORKSPACE="$HOME/workspace" # 设置工作目录

export PATH=$HOME/bin:$PATH # 将 $HOME/bin 目录加入到 PATH 变量中
# Default entry folder 
cd $WORKSPACE # 登录系统，默认进入 workspace 目录
```
有一点需要我们注意，在 export PATH 时，最好把 $PATH 放到最后，因为我们添加到目录中的命令是期望被优先搜索并使用的。配置完 $HOME/.bashrc 后，我们还需要创建工作目录 workspace。将工作文件统一放在 $HOME/workspace 目录中，有几点好处。

* 可以使我们的$HOME目录保持整洁，便于以后的文件查找和分类。
* 如果哪一天 /分区空间不足，可以将整个 workspace 目录 mv 到另一个分区中，并在/分区中保留软连接,例如：/home/going/workspace -> /data/workspace/。
* 如果哪天想备份所有的工作文件，可以直接备份 workspace。

具体的操作指令是$ mkdir -p $HOME/workspace。配置好 $HOME/.bashrc 文件后，我们就可以执行 bash 命令将配置加载到当前 shell 中了。

至此，我们就完成了 Linux 开发机环境的申请及初步配置。