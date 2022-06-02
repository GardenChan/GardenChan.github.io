---
layout: post
title: "阿里云+LAMP+wordpress搭建博客教程"
subtitle: "博客搭建全过程"
date: 2020-08-07
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [阿里云]
---
## WordPress

------

### WordPress安装

* 阿里云ECS

* 环境使用：[完美网络提供的LAMP环境（CentOS 7.3+http-2.4.25+mysql-5.6.35+php-7.1.4）](https://market.aliyun.com/products/53398003/cmjj018283.html?spm=5176.2020520132.101.1.36fd7218QRTwHm)

  > LAMP需要用到的信息：
  >
  > 1. **产品组成：** **CentOS 7.3**  **http-2.4.25** **mysql-5.6.35**  **php-7.1.4** **phpmyadmin-4.7.0**
  >
  > 2. **mysql**账户密码：**账号：root**  **密码：10@idccom**
  >
  > 3. **ftp**
  >
  >    * **账号：ftpuser**
  >
  >    * **密码：10@idccom**
  >    * **网站存放根目录：/data/www**
  >
  >    * **ftp用户根目录：/data/www**
  >
  >    * **php详细信息：http://ip/index.php**
  >
  >    * **phpmyadmin访问地址：http://ip/phpmyadmin**

  * 安装步骤

    > 1. 使用阿里云ECS，系统盘选择从镜像市场安装，选择合适的LAMP（建议选择CentOS7.x以上，php 7.1以上）
    > 2. 登录**phpmyadmin访问地址：http://ip(你的ip)/phpmyadmin**，账号是root，密码是数据库的密码（这里容易出错，记得不是ECS的密码），登录后新建一个数据库，命名为wordpress（表不用创建，安装wordpress时会自动创建）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807113040309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807112809437.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70)  
>  3. [下载wordpress的安装包](https://cn.wordpress.org/download/)，将安装包解压后，将wordpress文件夹上传到**网站存放根目录：/data/www**（不同的LAMP网站根目录会有差别）
>  4. 浏览器访问**http://ip(你的ip)/wordpress**访问wordpress的主页开始安装，安装主要是给wordpress配置数据库。数据库名就是刚才新建的数据库wordpress，数据库密码是LAMP环境给的数据库密码。**这时候有个问题是可能没法往wordpress目录里写wp-config.php配置文件，可根据网页上提供的文件内容，自行在本地创建wp-config.php后上传到wordpress目录下。**
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807112845335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70)
### 更换主题

WordPress有两种方式更换主题，一种是在管理后台线上安装主题。另一种是，上传自己在网上下载好的主题文件来安装主题。这里介绍的是第二种。

* **主要步骤如下**

> * [主题下载](https://cn.wordpress.org/themes/)
>
> * 编辑**wp-config.php**在底部加上如下php代码，这样就可以在wordpress的控制台上传主题安装包了。如果还不行，修改wp-content目录的权限即可。
>
>   ```php
>   if(is_admin()) {
>   	add_filter('filesystem_method', create_function('$a', 'return 	"direct";' ));
>   	define( 'FS_CHMOD_DIR', 0751 );
>   }
>   ```
>
> * 浏览器**http://ip(你的ip)/wordpress/wp-admin**登录wordpress控制台，进入**外观**添加主题，上传下载好的主题的zip文件，上传成功后点击启用即可切换主题。
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807113310359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807113310345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70)

### 安装插件
WordPress插件安装的方法和主题类似也有两种方法。这里介绍的是自行下载和上传插件的方式来安装。

* **主要步骤如下**

  > * [插件下载](https://cn.wordpress.org/plugins/)
  > * 将插件zip文件解压后，上传到服务器上wordpress/wp-content/plugins插件目录下
  > * 登录wordpress的控制台，进入插件，就可以看到我们新上传的插件，点击启用即可。

### 给脚页添加版权信息和备案号

* 添加版权信息和备案号是给自己使用的主题来添加

* 方法：修改当前使用主题目录（wp-content/glugins）下的footer.php

* 修改前：

  ```php
  <footer id="colophon" class="site-footer" role="contentinfo" itemscope="itemscope" itemtype="http://schema.org/WPFooter">
          <div class="container">
              <div class="row">
              	<div class="site-footer-inner col-sm-12">
              		<div class="site-info">
              			<?php do_action( 'lineday_credits' ); ?>
              			<?php printf( __( 'Proudly powered by %s', 'lineday' ), 'WordPress' ); ?>
              			<span class="sep"> | </span>
              			<?php echo __('Theme: <a href="https://wordpress.org/themes/lineday/" rel="bookmark">LineDay</a>.', 'lineday'); ?>
              		</div><!-- .site-info -->
              	</div><!-- .site-footer-inner -->
              </div><!-- .row -->
          </div><!-- .container -->
      </footer><!-- #colophon -->
  ```

  * 修改后

  ```php
  <footer id="colophon" class="site-footer" role="contentinfo" itemscope="itemscope" itemtype="http://schema.org/WPFooter">
          <div class="container">
              <div class="row">
              	<div style="text-align: center">
                      © 2020-2022 XXXXX·开发笔记 版权所有
                      <br/>
                      <a href="http://www.miibeian.gov.cn/">陕ICP备XXXXXXXX号-1</a>
                  </div>
              </div><!-- .row -->
          </div><!-- .container -->
      </footer><!-- #colophon -->
  ```

  * 显示效果

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200807113342358.png)


