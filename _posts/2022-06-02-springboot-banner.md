---
layout: post
title: "SpringBoot的Banner定制"
subtitle: "好玩！使用个性化的Spring Boot Banner"
date: 2020-08-13
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [SpringBoot]
---
**SpringBoot项目在启动的时候会默认打印一个banner，如下图所示**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200813100804144.png#pic_center)

**这个banner是可以定制的，方法如下：**

* 在resources的目录下，添加banner.txt文件，在文件中写入你想在项目启动时显示的内容即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020081310084499.png#pic_center)
* 可以用这个网站生成自己喜欢的艺术字，保存在banner.txt中

* txt的艺术字生成网址：http://network-science.de/ascii/
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200813100856869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 这边提供几个艺术字，方便读者需要来测试。

```
    __  __     ____         _       __           __    __
   / / / /__  / / /___     | |     / /___  _____/ /___/ /
  / /_/ / _ \/ / / __ \    | | /| / / __ \/ ___/ / __  / 
 / __  /  __/ / / /_/ /    | |/ |/ / /_/ / /  / / /_/ /  
/_/ /_/\___/_/_/\____/     |__/|__/\____/_/  /_/\__,_/   
                                                         
```



```
      _____ _    _____ 
      / /   | |  / /   |
 __  / / /| | | / / /| |
/ /_/ / ___ | |/ / ___ |
\____/_/  |_|___/_/  |_|
                        
```

