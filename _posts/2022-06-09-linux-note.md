---
layout: post
title: "Linux的入门级笔记"
subtitle: "快速入门Linux常用命令"
date: 2020-09-15 02:11:40
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [Linux, OS]
---

[TOC]

# 学习须知

**学习方向**

* linux下开发项目
* linux运维工程师
* linux嵌入式工程师

**运用领域**
* 个人桌面领域 该领域较薄弱,一直被windows压制
* 服务器领域  在该领域是最强的
* 嵌入式领域 内核最小仅几百K

**学习流程**
1. linux环境下的基本操作命令：文件操作、编辑器使用、用户管理
2. linux的各种配置：环境变量配置、网络配置、服务配置
3. linux下搭建对应语言的开发环境
4. 能写shell脚本 对linux服务器进行维护
5. 能进行安全设置 防止攻击 保障服务器正常运行 能对系统调优
6. 深入理解Linux系统（对内核有研究）熟练掌握大型网站应用架构组成 并熟悉各个环节的部署和维护方法

# Linux基础
## Linux介绍
**linux是一个开源、免费的操作系统，其稳定性、安全性、处理多并发已经得到 业界的认可，目前很多企业级的项目都会部署到Linux/unix系统上。**

* 创始人：Linus Torvalds
* 主要发行版：**Ubuntu**、**RedHat**、**CentOS**、Debain、Fedora、SuSE、OpenSUSE
* 不同产商在linux内核的基础上进行不同的定制形成了不同的linux发行版。一般我们说的都是linux的发行版

## Linux目录结构
**在Linux世界里，一切皆文件。**

linux的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录“/”，然后在此 目录下再创建其他的目录。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200913221255515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)


* /bin (/usr/bin,/usr/local/bin):是Binary的缩写，这个目录存放着最经常使用的命令
* /sbin (/usr/sbin, /usr/local/sbin):s就是super user的意思 这里存放着的是系统管理员使用的系统管理程序
* /home 存放普通用户的主目录 在Linux中每个用户都有一个自己的目录，一般该目录是以用户的账号命名的
* /root 该目录为系统管理员 也称作超级权限者的用户主目录
* /lib 系统开机所需要最基本的动态链接共享库 起作用类似于windows里的DLL文件 几乎所有的应用程序都需要用到这些共享库
* /etc 所有的系统管理所需要的配置文件和子目录my.conf
* /usr 这是一个非常重要的目录 用户的很多应用程序和文件都放在这个目录下 类似windows下的program files目录
* /boot 存放的是启动Linux时使用的一些核心文件 包括一些连接文件以及镜像文件
* /proc 这个目录是一个虚拟的目录 是系统内存的映射 访问这个目录来获取系统信息
* /srv service的缩写 该目录存放一些服务启动之后需要提取的数据
* /sys 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统
* /tmp 这个目录用来存放一些临时文件
* /dev 类似于windows的设备管理器 把所有的硬件用文件的形式存储
* /media linux系统会自动识别一些设备 例如U盘、光驱等等 当识别后 linux会把识别的设备挂载到这个目录下
* /mnt  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂载在/mnt/上，然后进入该目录就可以查看里的内容了。 
* /opt 这是给主机额外安装软件**(安装包)**所摆放的目录。如安装ORACLE数据库就可放到该目录下。默认为空。
* /usr/local 这是另一个给主机额外安装软件所安装的目录**（安装目录）**。一般是通过编译源码方式安装的程序。
* /var 这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下。包括各种日志文件。
* /selinux [security-enhanced linux] 360 SELinux是一种安全子系统,它能控制程序只能访问特定文件。

# Linux实操
## vim编辑器
* Vim 具有程序编辑的能力，可以看做是Vi的增强版本，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补完、编译及错误跳转等方便编程的功能特别 丰富，在程序员中被广泛使用。
* **vim常用的三种模式**
1. **正常模式**：以 vim 打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中， 你可以使用 『上下左右』按键来移动光标，可以使用『删除字符』或『删除整行』来处理档案内容， 也可以使用『复制、贴上』来处理你的文件数据。
2. **插入模式**：按下i, I, o, O, a, A, r, R等任何一个字母之后才会进入编辑模式, 一般来说按**i**即可.
3. **命令行模式**：在这个模式当中， 可以提供你相关指令，完成读取、存盘、替换、离开 vim 、显示行号等的 动作则是在此模式中达成的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914212944771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

```shell
# vim 文件名 进入正常模式
vim xxx

# 点击i或a进入编辑模式

# ESC键退出编辑模式
# 输入shift+： 进入命令模式
:wq
:q
:q！
```
* **vim快捷键**
1. 拷贝当前行  yy 
2. 拷贝当前行向下的 5 行 5yy
3. 粘贴 p 
4. 删除当前行 dd 
5. 删除当前行向下的 5 行 5dd
6. 在文件中查找某个单词,命令行模式下输入 "/  关键字" ， 回车查找 , 输入 n 就是查找下一个
7. 设置文件的行号命令行模式下输入 : set nu 
8. 取消文件的行号命令行模式下输入:set nonu
9. 快捷键到底文档的最末行，正常模式下输入 G
10. 快捷键到文档最首行，正常模式下输入gg
11. 撤销，在正常模式下输入 u

## 开机、重启和用户登录注销
### 关机和重启
```shell
# 立即关机
shutdown -h now
# 1分钟后关机
shutdown -h 1
# 立即重启
shutdown -r now
# halt效果等价于关机
halt
# reboot 重启系统
reboot
# 把内存数据同步到磁盘
sync
```

> 注意⚠️：当我们关机或者重启时，都应该先执行以下 sync 指令，把内存的数据写入磁盘，防止数据丢失。

### 用户登录和注销

> 注意⚠️：
> 1. 登录时尽量少用 root 帐号登录，因为它是系统管理员，最大的权限，避免操作失误。可以利 用普通用户登录，登录后再用”su - 用户名’命令来切换成系统管理员身份.
> 2. 在提示符下输入 logout 即可注销用户
> 3. logout 注销指令在图形运行级别无效，在 运行级别 3 下有效.

```shell
# 切换用户 su - 用户名
su - username
# 注销登录
logout
```

## 用户管理
* Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向 系统管理员申请一个账号，然后以这个账号的身份进入系统。
* Linux的用户需要至少要属于一个组。

### 添加用户
```shell
useradd [选项] 用户名

# 指定家目录添加用户 
useradd -d /home/dog xiaoqiang
```
### 给用户指定或修改密码

```powershell
passwd 用户名
```
### 删除用户

> 注意⚠️：在删除用户时，我们一般不会将家目录删除。

```powershell
# 删除用户，保留家目录
userdel 用户名
# 删除用户及其家目录
userdel -r 用户名
```
### 查询用户信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200914224403114.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
```powershell
# 查询用户信息
id 用户名
id root
```

> 注意⚠️：当用户不存在时，返回”无此用户”

### 切换用户

```powershell
su - 切换用户名
```

> 注意⚠️：
> 1)从权限高的用户切换到权限低的用户，不需要输入密码，反之需要。
> 2)当需要返回到原来用户时，使用 exit 指令

### 用户组
* 利用用户组，系统可以对有共性的多个用户进行统一的管理。

```powershell
# 增加用户组
groupadd 组名
# 删除用户组
groupdel 组名

# 增加用户时直接加上组
useradd -g 用户组 用户名

# 修改用户组
usermod -g 用户组 用户名
```
* /etc/passwd 文件
**用户(user)的配置文件，记录用户的各种信息**
每行的含义: 用户名:口令:用户标识号:组标识号:注释性描述:主目录:登录 Shell
* /etc/shadow 文件
**口令的配置文件**
每行的含义: 登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
* /etc/group 文件
**组(group)的配置文件，记录 Linux 包含的组的信息**
每行含义: 组名:口令:组标识号:组内用户列表

## 实用指令
### 指定运行级别
* 运行级别说明
0 :关机
1 :单用户【找回丢失密码】 2:多用户状态没有网络服务
3:多用户状态有网络服务
4:系统未使用保留给用户
5:图形界面
6:系统重启
**常用运行级别是 3 和 5**
**要修改默认的运行级别可改文件 /etc/inittab 的 id:5:initdefault:这一行中的数字**
* 切换到指定运行级别的指令

```powershell
init 0
init 1
init 2
init 3
init 4
init 5
init 6
```
### 帮助指令
* man

```powershell
# 获得帮助信息
man [命令或配置文件]
# 举例
man ls
```
* help

```powershell
# 获得 shell 内置命令的帮助信息
help 命令
# 举例
help cd
```

### 文件目录类
* pwd

```powershell
# 显示当前工作目录的绝对路径
pwd
```
* ls
-a :显示当前目录所有的文件和目录，包括隐藏的。
 -l :以列表的方式显示信息
```powershell
ls [选项] [目录或是文件]
# 以列表的方式显示信息
ls -l
# 显示当前目录所有的文件和目录，包括隐藏的
ls -a
ls -al
```
* cd

```powershell
cd [参数] (功能描述:切换到指定目录)
# 回到自己的家目录
cd ～
# 回到当前目录的上一级目录
cd ..
# 实用绝对路径
cd /root
```

* mkdir
mkdir 指令用于创建目录(make directory)
 -p :创建多级目录

```powershell
mkdir [选项] 要创建的目录
# 创建多级目录
mkdir -p /home/animal/tiger
```
* rmdir、rm
rmdir 指令删除空目录
rmdir 删除的是空目录，如果目录下有内容时无法删除的。
```powershell
# 删除空目录
rmdir [选项] 要删除的空目录
# rm 指令移除【删除】文件或目录
# -r :递归删除整个文件夹
# -f : 强制删除不提示
rm -rf 要删除的目录
```
* touch

```powershell
touch 文件名称
touch hello.txt
```
* cp

-r :递归复制整个文件夹
```powershell
cp [选项] source dest
# 将当前目录的aaa.txt文件拷贝到当前目录的bbb这个目录下
cp aaa.txt bbb/
# 将/home/test 整个目录拷贝到 /home/zwj 目录
cp -r /home/test /home/zwj
```
使用cp指令 当发现目标目录下有相同文件，会提示是否覆盖，使用\cp会强制覆盖，不提示

```powershell
\cp
```
* mv
mv 移动文件与目录或重命名

```powershell
# 重命名
mv oldNameFile newNameFile
# 将 /home/aaa.txt 文件 重新命名为 pig.txt
mv aaa.txt pig.txt
# 移动文件
mv /temp/movefile /targetFolder 
# 将 /home/pig.txt 文件 移动到 /root 目录下
mv pig.txt /root/
```
* cat
cat 查看文件内容，是以只读的方式打开。
cat 只能浏览文件，而不能修改文件
-n :显示行号
```powershell
cat [选项] 要查看的文件
# 分页浏览
cat 文件名 | more
```
* more
more 指令是一个基于 VI 编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容

```powershell
more 要查看的文件
# 举例
more /etc/profile
```
快捷键
| 操作        | 功能说明                           |
| ----------- | ---------------------------------- |
| 空格键space | 代表向下翻一页                     |
| enter       | 代表向下翻一行                     |
| q           | 代表立刻离开more，不再显示文件内容 |
| Ctrl+F      | 向下滚动一屏                       |
| Ctrl+B      | 返回上一屏                         |
| =           | 输出当前行的行号                   |
| :f          | 输出文件名和当前行的行号           |

* less
less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加强大，支持 各种显示终端。less 指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示 需要加载内容，对于显示大型文件具有较高的效率。

```powershell
less 要查看的文件
```
* 输出重定向

```powershell
> 输出重定向 : 会将原来的文件的内容覆盖
>> 追加: 不会覆盖原来文件的内容，而是追加到文件的尾部。
# 列表的内容写入文件 a.txt 中(覆盖写)
ls -l >文件
ls -l > a.txt 
# 列表的内容追加到文件 aa.txt 的末尾
ls -al >>aa.txt
# 将文件 1 的内容覆盖到文件 2
cat 文件 1 > 文件 2 
# 把输出内容追加写到文件
echo "内容" >> 文件
```
* echo
echo 输出内容到控制台。

```powershell
echo [选项] [输出内容]
#  使用 echo 指令输出环境变量,输出当前的环境路径。
echo $PATH
```
* head
head 用于显示文件的开头部分内容，默认情况下 head 指令显示文件的前 10 行内容

```powershell
# 查看文件头10行内容
head 文件
# 查看文件头 5 行内容，5 可以是任意行数
head -n 5 文件
# 查看/etc/profile 的前面 5 行代码
head -n 5 /etc/profile

```
* tail
tail 用于输出文件中尾部的内容，默认情况下 tail 指令显示文件的后 10 行内容。

```powershell
# 查看文件后 10 行内容
tail 文件
# 查看文件后 5 行内容，5 可以是任意行数
tail -n 5 文件
# 实时追踪该文档的所有更新，工作经常使用
tail -f 文件
```
* ln
软链接也叫符号链接，类似于 windows 里的快捷方式，主要存放了链接其他文件的路径

```powershell
# 给原文件创建一个软链接
ln -s [原文件或目录] [软链接名]
# 在/home 目录下创建一个软连接 linkToRoot，连接到 /root 目录
ln -s /root linkToRoot
# 删除软连接 linkToRoot
rm -f linkToRoot
```
* history
查看已经执行过历史命令,也可以执行历史指令

```powershell
# 查看已经执行过历史命令
history
# 显示最近使用过的 10 个指令。
history 10
# 执行历史编号为 178 的指令
!178
```
* date

```powershell
# 显示当前时间
date
# 显示当前年份
date "+%Y"
# 显示当前月份
date "+%m"
# 显示当前是哪一天
date "+%d"
# 显示年月日时分秒
date "+%Y-%m-%d %H:%M:%S"
# 设置日期
date -s 字符串时间
# 设置系统当前时间 ， 比如设置成 2018-10-10 11:22:22
date -s "2018-10-10 11:22:22"
```
* cal
查看日历指令

```powershell
# 不加选项，显示本月日历
cal
# 显示 2020 年日历
cal 2020
```
* find
find 指令将从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端。

```powershell
find [搜索范围] [选项]
```
| 选项  | 功能                             |
| ----- | -------------------------------- |
| -name | 按照指定的文件名查找模式查找文件 |
| -user | 查找属于指定用户名所有文件       |
| -size | 按照指定的文件大小查找文件       |

```powershell
# 按文件名:根据名称查找/home 目录下的 hello.txt 文件
find /home -name hello.txt
# 按拥有者:查找/opt 目录下，用户名称为 nobody 的文件
find /opt -user nobody
# 查找整个 linux 系统下大于 20m 的文件(+n 大于 -n 小于 n 等于)
find / -size +20M
find / -size -20M
find /-size 20M
# 查询 / 目录下，所有 .txt 的文件
find / -name *.txt
```
* locate
 locaate 指令可以快速定位文件路径。locate 指令利用事先建立的系统中所有文件名称及路径的 locate 数据库实现快速定位给定的文件。Locate 指令无需遍历整个文件系统，查询速度较快。
 由于 locate 指令基于数据库进行查询，所以第一次运行前，必须使用 updatedb 指令创建 locate 数 据库。
```powershell
locate 搜索文件
# 请使用 locate 指令快速定位 hello.txt 文件所在目录
locate hello.txt
```
* grep 指令和 管道符号 |
grep 过滤查找 ， 管道符，“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

```powershell
grep [选项] 查找内容 源文件
# 在 hello.txt 文件中，查找"yes" 所在行，并且显示行号
cat hello.txt | grep -n yes
cat hello.txt | grep -ni yes # 不区分大小写
```
| 选项 | 功能             |
| ---- | ---------------- |
| -n   | 显示匹配行及行号 |
| -i   | 忽略字母大小写   |
* 压缩和解压缩类
1. gzip/gunzip 指令
gzip 用于压缩文件， gunzip 用于解压的
当我们使用 gzip 对文件进行压缩后，不会保留原来的文件。
```powershell
# 压缩文件，只能将文件压缩为*.gz 文件
gzip 文件
# 将 /home 下的 hello.txt 文件进行压缩
gzip hello.txt
# 解压缩文件命令
gunzip 文件.gz
# 将 /home 下的 hello.txt.gz 文件进行解压缩
gunzip hello.txt.gz
```
2. zip/unzip 指令
zip 用于压缩文件， unzip 用于解压的，这个在项目打包发布中很有用的
-r:递归压缩，即压缩目录
-d<目录> :指定解压后文件的存放目录
```powershell
# 压缩文件和目录的命令
zip [选项] XXX.zip 将要压缩的内容
# 解压缩文件
unzip [选项] XXX.zip 
# 将 /home 下的 所有文件进行压缩成 mypackage.zip
zip -r mypackage.zip /home/
# 将 mypackge.zip 解压到 /opt/tmp 目录下
unzip -d /opt/tmp/ mypackage.zip
```
3. tar 指令
tar 指令 是打包指令，最后打包后的文件是 .tar.gz 的文件。
```powershell
# 打包目录，压缩后的文件格式.tar.gz
tar [选项] XXX.tar.gz 打包的内容
# 压缩多个文件，将 /home/a1.txt 和 /home/a2.txt 压缩成 a.tar.gz
tar -zcvf a.tar.gz a1.txt a2.txt
# 将/home 的文件夹 压缩成 myhome.tar.gz
tar -zcvf myhome.tar.gz /home/
# 将 a.tar.gz 解压到当前目录
tar -zxvf a.tar.gz
# 将 myhome.tar.gz 解压到 /opt/ 目录下
tar -zxvf myhome.tar.gz -C /opt/
```
| 选项 | 功能               |
| ---- | ------------------ |
| -c   | 产生.tar打包文件   |
| -v   | 显示详细信息       |
| -f   | 指定压缩后的文件名 |
| -z   | 打包同时压缩       |
| -x   | 解包.tar文件       |

## 组管理和权限管理
### Linux 组基本介绍
* 在 linux 中的每个用户必须属于一个组，不能独立于组外。在 linux 中每个文件
* 所有者、所有组、其他组
一般为文件的创建者,谁创建了该文件，就自然的成为该文件的所有者。
### 文件/目录 所有者
```powershell
# 查看文件的所有者
ls -ahl
# 修改文件所有者
chown 用户名 文件名
# 使用 root 创建一个文件 apple.txt 将其所有者修改成 tom
chown tom apple.txt
```
### 用户组的创建

```powershell
# 创建组
groupadd 组名
# 创建一个组monster 创建一个用户 fox ，并放入到 monster 组中
groupadd monster
useradd -g monster fox
```
### 文件/目录 所在组
当某个用户创建了一个文件后，默认这个文件的所在组就是该用户所在的组。

```powershell
# 查看文件/目录所在组
ls –ahl
# 修改文件所在的组
chgrp 组名 文件名
```
### 其它组
除文件的所有者和所在组的用户外，系统的其它用户都是文件的其它组.
### 改变用户所在组
在添加用户时，可以指定将该用户添加到哪个组中，同样的用 root 的管理权限可以改变某个用户
所在的组。

```powershell
# 改变用户所在组
usermod –g 组名 用户名
#  改变该用户登陆的初始目录。
usermod –d 目录名 用户名
```
###  权限的基本介绍

> ls -l 中显示的内容如下:
> -rwxrw-r-- 1 root root 1213 Feb 2 09:39 abc
> 1)第0位确定文件类型(d, - , l , c , b)
> 2)第 1-3 位确定所有者(该文件的所有者)拥有该文件的权限。---User 
> 3)第 4-6 位确定所属组(同用户组的)拥有该文件的权限，---Group 
> 4)第 7-9 位确定其他用户拥有该文件的权限 ---Other

### rwx 权限详解
* rwx 作用到文件
1) [ r ]代表可读(read): 可以读取,查看
2) [ w ]代表可写(write): 可以修改,但是不代表可以删除该文件,删除一个文件的前提条件是对该 文件所在的目录有写权限，才能删除该文件.
3) [ x ]代表可执行(execute):可以被执行
* rwx 作用到目录
1) [ r ]代表可读(read): 可以读取，ls 查看目录内容
2) [ w ]代表可写(write): 可以修改,目录内创建+删除+重命名目录 
3) [ x ]代表可执行(execute):可以进入该目录

> -rwxrw-r-- 1 root root 1213 Feb 2 09:39 abc
> 10 个字符确定不同用户能对文件干什么
> 第一个字符代表文件类型: 文件 (-),目录(d),链接(l)
> 其余字符每 3 个一组(rwx) 读(r) 写(w) 执行(x)
> 第一组 rwx : 文件拥有者的权限是读、写和执行
> 第二组 rw- : 与文件拥有者同一组的用户的权限是读、写但不能执行 
> 第三组 r-- : 不与文件拥有者同组的其他用户的权限是读不能写和执行
> **可用数字表示为: r=4,w=2,x=1 因此 rwx=4+2+1=7**
### 修改权限-chmod
通过 chmod 指令，可以修改文件或者目录的权限
* 第一种方式:+ 、-、= 变更权限
u所有者 g所有组 o其他人 a所有人（u、g、o的总和）

```powershell
chmod u=rwx,g=rx,o=x 文件目录名
chmod o+w 文件目录名
chmod a-x 文件目录名
# 给 abc 文件 的所有者读写执行的权限，给所在组读执行权限，给其它组读执行权限
chmod u=rwx,g=rx,o=rx abc
# 给 abc 文件的所有者除去执行的权限，增加组写的权限
chmod u-x,g+w abc
# 给 abc 文件的所有用户添加读的权限
chmod a+r abc
```
* 第二种方式:通过数字变更权限
规则:r=4 w=2 x=1 
rwx=4+2+1=7 
**chmod u=rwx,g=rx,o=x 文件目录名 相当于 chmod 751 文件目录名**

> 要求:将 /home/abc.txt 文件的权限修改成 rwxr-xr-x, 使用给数字的方式实现:
>  rwx = 4+2+1 = 7
> r-x = 4+1=5
> r-x = 4+1 =5

```powershell
chmod 755 /home/abc.txt
```
### 修改文件所有者-chown

```powershell
# 改变文件的所有者
chown newowner file
# 改变用户的所有者和所有组
chown newowner:newgroup file
# 将 /home/abc .txt 文件的所有者修改成 tom
chown tom abc.txt
# 将/home/kkk目录下所有的文件和目录的所有者都修改成tom
chown -R tom kkk/ 
```
### 修改文件所在组-chgrp

```powershell
# 改变文件的所有组
chgrp newgroup file 
# 将 /home/abc .txt 文件的所在组修改成 bandit
chgrp bandit /home/abc.txt
# 将 /home/kkk 目录下所有的文件和目录的所在组都修改成 bandit
chgrp -R bandit /home/kkk
```

## 实操篇 crond 任务调度
任务调度:是指系统在某个时间执行的特定的命令或程序。
任务调度分类:
1. 系统工作:有些重要的工作必须周而复始地执行。如病毒扫描等
2. 个别用户工作:个别用户可能希望执行某些程序，比如对 mysql 数据库的备份。

```powershell
crontab [选项]
```
| 选项 | 功能                          |
| ---- | ----------------------------- |
| -e   | 编辑crontab定时任务           |
| -i   | 查询crontab任务               |
| -r   | 删除当前用户所有的crontab任务 |
设置任务调度文件:/etc/crontab 设置个人任务调度。执行 crontab –e 命令。
接着输入任务到调度文件
如:*/1 * * * * ls –l /etc/ > /tmp/to.txt
意思说每小时的每分钟执行 ls –l /etc/ > /tmp/to.txt 命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915013519833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915013519630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915013519402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 案例 1:每隔 1 分钟，就将当前的日期信息，追加到 /tmp/mydate 文件中
1) 先编写一个文件 /home/mytask1.sh 
date >> /tmp/mydate
2) 给 mytask1.sh 一个可以执行权限 
chmod 744 /home/mytask1.sh
3) crontab -e
4) */1 * * * * /home/mytask1.sh 
5) 成功
* 案例 2:每隔 1 分钟， 将当前日期和日历都追加到 /home/mycal 文件 中
1) 先编写一个文件 /home/mytask2.sh 
date >> /tmp/mycal
cal >> /tmp/mycal
2) 给 mytask1.sh 一个可以执行权限 
chmod 744 /home/mytask2.sh
3) crontab -e
4) */1 * * * * /home/mytask2.sh 
5) 成功
* 案例 3: 每天凌晨 2:00 将 mysql 数据库 testdb ，备份到文件中 mydb.bak
1) 先编写一个文件 /home/mytask3.sh 
/usr/local/mysql/bin/mysqldump -u root -proot testdb > /tmp/mydb.bak 
2) 给 mytask3.sh 一个可以执行权限
chmod 744 /home/mytask3.sh
3) crontab -e
4) 0 2 * * * /home/mytask3.sh 
5) 成功

```powershell
#终止任务调度。
conrtab –r
# 列出当前有那些任务调度
crontab –l
#重启任务调度
service crond restart
```
## 网络配置

```powershell
# 测试当前服务器是否可以连接目的主机
ping 目的主机
```

```powershell
# 指定固定的 ip
# 直接修改配置文件来指定 IP,并可以连接到外网(程序员推荐)，编辑 
vi /etc/sysconfig/network-scripts/ifcfg-eth0
# 将 ip 地址配置的静态的，ip 地址为 192.168.184.130
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915014048210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

```powershell
# 重启服务
service network restart
# 重启系统
reboot
```
## 进程管理
1)在 LINUX 中，每个执行的程序(代码)都称为一个进程。每一个进程都分配一个 ID 号。 
2)每一个进程，都会对应一个父进程，而这个父进程可以复制多个子进程。例如 www 服务器。 
3)每个进程都可能以两种方式存在的。前台与后台，所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。 
4)一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才才结束。
### 显示系统执行的进程
查看进行使用的指令是 ps ,一般来说使用的参数是 ps -aux

```powershell
ps -aux
ps –aux|grep xxx
```

```powershell
# 以全格式显示当前所有的进程
 •ps -ef
 ps -ef|grep more
```
### 终止进程 kill 和 killall
若是某个进程执行一半需要停止时，或是已消了很大的系统资源时，此时可以考虑停止该进程。 使用 kill 命令来完成此项任务。
-9 :表示强迫进程立即停止
```powershell
# 通过进程号杀死进程
kill [选项] 进程号
# 通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变 得很慢时很有用
killall 进程名称
```
* 案例 1:踢掉某个非法登录用户
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915014542881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 终止远程登录服务 sshd, 在适当时候再次重启 sshd 服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/202009150146285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 案例 3: 终止多个 gedit 编辑器 【killall , 通过进程名称来终止进程】
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915014702342.png#pic_center)
* 案例 4:强制杀掉一个终端
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200915014729888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
### 查看进程树 pstree
pstree [选项] ,可以更加直观的来看进程信息
-p :显示进程的 PID
-u :显示进程的所属用户
```powershell
pstree [选项]
```
### 服务(Service)管理
服务(service) 本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其它程序的请 求，比如(mysql , sshd 防火墙等)，因此我们又称为守护进程，是 Linux 中非常重要的知识点。

```powershell
service 服务名 [start | stop | restart | reload | status]
```
在 CentOS7.0 后 不再使用 service ,而是 systemctl

```powershell
# 查看当前防火墙的状况
service iptables status
# 关闭防火墙
service iptables stop
# 重启防火墙
service iptables start
```
* chkconfig 指令
通过 chkconfig 命令可以给每个服务的各个运行级别设置自启动/关闭

```powershell
# 查看服务
chkconfig --list|grep xxx
chkconfig 服务名 --list
# 显示当前系统所有服务的各个运行级别的运行状态
chkconfig --list
# 查看 sshd 服务的运行状态
service sshd status
# 将 sshd 服务在运行级别 5 下设置为不自动启动
chkconfig --level 5 sshd off
# 当运行级别为 5 时，关闭防火墙
chkconfig --level 5 iptables off
# 在所有运行级别下，关闭防火墙
chkconfig iptables off
# 在所有运行级别下，开启防火墙
chkconfig iptables on
```

> 注意⚠️：chkconfig 重新设置服务后自启动或关闭，需要重启机器 reboot 才能生效.

### 动态监控进程
top 与 ps 命令很相似。它们都用来显示正在执行的进程。Top 与 ps 最大的不同之处，在于 top 在 执行一段时间可以更新正在运行的的进程。

```powershell
top [选项]
```
| 选项    | 功能                                                         |
| ------- | ------------------------------------------------------------ |
| -d 秒数 | 指定top命令每隔几秒更新 默认是3秒在top命令的交互模式当中可以执行的命令 |
| -i      | 使top不显示任何闲置或者僵死进程                              |
| -p      | 通过指定监控进程ID来仅仅监控某个进程的状态                   |

* 案例 1.监视特定用户
1. top:输入此命令，按回车键，查看执行的进程。
2. u:然后输入“u”回车，再输入用户名，即可
* 案例 2:终止指定的进程。
3. top:输入此命令，按回车键，查看执行的进程。 
4. k:然后输入“k”回车，再输入要结束的进程 ID 号
* 案例 3:指定系统状态更新的时间(每隔 10 秒自动更新， 默认是 3 秒):

```powershell
top -d 10
```
### 查看系统网络情况 netstat

```powershell
netstat [选项]
# 查看系统所有的网络服务
netstat -anp
# 请查看服务名为 sshd 的服务的信息。
netstat -anp | grep sshd
```
-an 按一定顺序排列输出 
-p 显示哪个进程在调用

## RPM
### rpm 包的管理
* 一种用于互联网下载包的打包及安装工具，它包含在某些 Linux 分发版中。它生成具有.RPM 扩展名的文件。RPM 是 RedHat Package Manager(RedHat 软件包管理工具)的缩写，类似 windows 的 setup.exe，这一文件格式名称虽然打上了 RedHat 的标志，但理念是通用的。Linux 的分发版本都有采用(suse,redhat, centos 等等)，可以算是公认的行业标准了。
### rpm 包的简单查询指令

```powershell
# 查询已安装的 rpm 列表
rpm –qa|grep xx
# 查询看一下，当前的 Linux 有没有安装 firefox
rpm –qa|grep firefox
```
### rpm 包名基本格式
* 一个 rpm 包名:firefox-45.0.1-1.el6.centos.x86_64.rpm
名称:firefox
版本号:45.0.1-1
适用操作系统: el6.centos.x86_64
表示 centos6.x 的 64 位系统
如果是 i686、i386 表示 32 位系统，noarch 表示通用
### rpm 包的其它查询指令

```powershell
# 查询所安装的所有 rpm 软件包
rpm -qa
rpm -qa | more [分页显示]
# 查询软件包是否安装
rpm -q 软件包名
# 查询软件包信息
rpm -qi 软件包名
# 查询软件包中的文件
rpm -ql 软件包名
# 查询文件所属的软件包
rpm -qf 文件全路径名
```
### 卸载 rpm 包

```powershell
rpm -e RPM包的名称
# 删除 firefox 软件包
rpm -e firefox
```
### 安装 rpm 包

```powershell
rpm -ivh RPM 包全路径名称
```
i=install 安装
v=verbose 提示
h=hash 进度条

## yum
Yum 是一个 Shell 前端软件包管理器。基于 RPM 包管理，能够从指定的服务器自动下载 RPM 包 并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包。使用 yum 的前提是可以联 网。

```powershell
# 查询 yum 服务器是否有需要安装的软件
yum list|grep xx 
# 安装指定的 yum 包
yum install xxx
```