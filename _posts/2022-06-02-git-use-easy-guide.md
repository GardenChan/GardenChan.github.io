---
layout: post
title: "Git简洁使用指南"
subtitle: "Git的简单使用指南，快速上手Git的基本使用"
date: 2020-12-09
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Git, 开发工具]
---
> **首先：**
> 本文不适合Git小白作为入门的学习。
> 本文旨在整理常用的Git命令，以便加深印象，和方便日常使用的查阅。
> 俗话说的话，**好记性不如烂笔头。**
> 这篇老早就想写的Git总结，拖着拖着应该有两个月了吧，今天得空，写了。
> 不过需要一提的是，本文属于整理记录类的文章，因此笔者会在日常实践中不断对本文进行补充完善。
> 如果您在此停留过，亦有所收获，不妨点赞收藏关注，回头不迷路！


* ***温馨提示***
本文所有"Firstname Lastname"、"your_email@example.com"读者均需要替换为自己实际的昵称和邮箱。

# 一、Git初始设置
## 1.1 设置姓名和邮箱地址

```bash
git config --global user.name "Firstname Lastname"
git config --global user.mail "your_email@example.com"
```
## 1.2 提高命令输出的可读性

```bash
git config --global color.ui auto
```

# 三、设置SSH Key
* GitHub连接已有仓库时可以通过SSH的公开密钥认证的方式进行。
* 下边介绍如何创建公开密钥认证所需的SSH Key，并将其添加至GitHub。
* 运行下面的命令创建SSH Key

```bash
ssh-keygen -t rsa -C "your_email@example.com"
```
* 输入上述命令后，按如下操作执行，括号内是对应的操作：
（your_user_directory会是你自己对应的目录）
```bash
Generating public/private rsa key pair.
Enter file in which to save the key 
(/Users/your_user_directory/.ssh/id_rsa): (按回车键)
Enter passphrase (empty for no passphrase): (输入密码)
Enter same passphrase again: (再次输入密码)
```
* 上述操作完成后：
* 你的密钥存放在 "***Users/your_user_directory/.ssh/id_rsa***"
* 你的公钥存放在 "***Users/your_user_directory/.ssh/id_rsa.pub***"
* **id_rsa文件是私有密钥**、**id_rsa.pub是公开密钥**

          

* 这里关于私钥公钥就不展开说了，你如果不了解可以后续去查阅资料学习。只需要知道，这两者是配对的，成功配对才能认证通过。
* 所以我们要把公开密钥存放到远程仓库上，也就是GitHub,这样本地的Git连接远程仓库的时候就会拿着本地的私钥和远程GitHub的公钥进行配对，成功配对两者就通过认证连接上了，可以不输入账号密码。
* **下面看如何在GitHub添加公钥：**
1. 登录GitHub，右上角头像，点击有下拉菜单，进入Settins设置。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120900173367.png#pic_center)

2. 右侧菜单栏中选择SSH and GPG keys点击进入。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209001733132.png#pic_center)

3. 进来后点击New SSH key
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120900173398.png#pic_center)

4. 进入添加页面，按图示添加。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201209001733128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

# 四、GitHub创建仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020120900361147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

# 五、Git常用命令
## 5.1 clone仓库
* GitHub的每个仓库会有自己的SSH clone URL格式是这样子的

```bash
	git@github.com:用户名/仓库名.git
```
* 这边以用户名为LiMing为例，仓库名为Hello-World为例，说明clone命令

```bash
git clone git@github.com:LiMing/Hello-World.git
```

## 5.2 初始化仓库
* 从远程仓库clone某仓库到本地，可以在本地初始化一个Git仓库。
* 也可以用，之间在本地初始化创建的方法来初始化仓库，只是这样子初始化（创建）的仓库还没有和远程的某个仓库建立联系。
* 本地初始化仓库（要在工作目录里执行）

```bash
git init
```
* 执行了git init命令的目录下就会生成 .git 目录，这个 .git 目录里存储着管理当前目录内容所需的仓库数据。可以理解为是一个仓库的工作树，你对该仓库所有的操作都会被记录在.git。
## 5.3 查看仓库信息
* 查看仓库状态
```bash
git status
```
* 查看仓库提交日志

```bash
git log
```
* 只显示提交信息的第一行

```bash
git log --pretty=short
```

* 只显示与某目录或文件有关的日志

```bash
git log 目录名或文件名

git log README.md
```
* 查看提交带来的改动

```bash
git log -p 目录名/文件名/不加
```
* 查看更改前后的差别

```bash
git diff
```

## 5.4 向暂存区中添加文件

* 添加某具体文件
```bash
git add hello_world.php
```
* 添加全部文件

```bash
git add .
```

## 5.5 正式提交

```bash
git commit -m "本次提交的说明信息"
```
* git commit命令可以将当前暂存区中的文件实际保存到仓库的历史记录中
* 修改提交信息

```bash

```bash
git commit --amend
```

```

## 5.6 分支操作
* 显示分支一览表

```bash
git branch
```
* 创建并切换到分支

```bash
git checkout -b 分支名
```
和下面两个命令连续执行等价

```bash
git branch 分支名     #创建分支
git checkout 分支名   #切换分支
```
* 切换回上一个分支

```bash
git checkout -
```

* 合并分支

```bash
git merge --no-ff feature-A
```

* 分支合并的冲突

> 当两个分支合并时发生冲突，需要解决冲突（选择保留什么，舍弃什么），才能进行合并。

* 以图表形式查看分支

```bash
git log --graph
```

## 5.7 状态回溯
* 通过日志查询 可以查到每次提交的哈希值，可以通过某个时刻的哈希值作为某个状态，来进行回溯。

```bash
git reset --hard 目标时间点的哈希值
```
* 因为当回溯到之前的某个状态（相当于穿越了），这时候使用git log只能查看该时间点及其之前的日志，无法查到该仓库全部的日志。
* 使用git reflog可以查看当前仓库执行过的所有操作日志，就相当于你穿越回去了，你还可以查看你所在时间之后的所有的日志。

```bash
git reflog
```
还有一个作用就是通过git reflog回到现在。为什么呢？首先要进行状态的回溯是用“git reset --hard 目标时间点的哈希值”，这时候需要有时间点的哈希值。如果你想跳回现在，那么你就需要知道现在的哈希值，所以需要用git reflog查看现在的哈希值。

## 5.8 压缩历史
* 大家想象这么一个场景，有两个时间点，时间1 和 时间2，这两次提交的改动是因为一些认为的误操作导致的，比如拼写错误。那么这两个时间点中间的提交其实没有什么意义，我们觉得记录这种无关紧要的变化是没有意义，因此可以将这两个时间点的两次提交合并为一次，相当于把修正认为错误的最后一次提交舍弃掉。

```bash
git rebase -i HEAD～2
```

## 5.9 推到远程仓库
* 给本地仓库绑定远程仓库
要把本地仓库推到哪个远程仓库，需要提前把两者关联起来。

```bash
git remote add origin git@github:LiMing/HelloWorld.git
```

> 这里的orgin是一个标识符，表示给本地仓库添加了一个代号为origin的远程仓库，这个远程仓库具体是git@github:LiMing/HelloWorld.git

* 推送至master分支

```bash
git push -u origin master
```
将本地仓库的master分支推送到标识符为origin的远程仓库。


## 5.10 从远程仓库获取
* clone
```bash
git clone git@github.com:LiMing/HelloWorld.git
```
执行git clone命令后，本地会默认处于master分支下，同时系统回自动将origin设置为该远程仓库的标识符。

* 获取远程仓库的feature-D分支

```bash
git checkout -b feature-D origin/feature-D
```
* 获取最新的远程仓库分支

```bash
git pull origin feature-D
```
