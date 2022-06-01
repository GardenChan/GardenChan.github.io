---
layout: post
title: "小白入门SpringBoot项目之在线文件管理系统"
subtitle: "收藏和推荐优秀的后端技术博客"
date: 2020-11-06
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [SpringBoot, 小白入门]
---

* **小白入门Spring Boot系列项目**
1. [**小白入门SpringBoot项目【1】：在线文件管理系统**](https://blog.csdn.net/weixin_44870909/article/details/109729357)
2. [**小白入门SpringBoot项目【2】：员工管理系统**](https://blog.csdn.net/weixin_44870909/article/details/109749701)

@[TOC](在线文件管理系统)


> 说明：
> **本项目非常适合小白入门**，如果读者的水平比较高的话，可能这个项目对你并不会有什么额外的收获。欢迎读者在评论区互相交流讨论。
> 对小白的收获可能包括：入门SpringBoot、Mybatis、Thymeleaf的使用。

* [GitHub项目地址](https://github.com/GardenChan/Simple-online-file-management-system)
* [Gitee项目地址](https://gitee.com/gardenchan/simple-online-file-management-system)
# 一、简介
本项目是基于Spring Boot，利用Mybatis框架实现的在线文件管理系统。功能点包括：
1. 用户登录
2. 文件上传（包括文件上传后各类信息的记录）
3. 文件在线预览
4. 文件下载（下载次数统计）
* 用户登录界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116204604132.png#pic_center)
* 文件上传界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116204623773.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 文件上传后界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111620464268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

# 二、技术点
* 单体应用架构
* 比较标准的MVC分层开发（Model-View-Controller 模式）
* Spring Boot、Mybatis、Thymeleaf

## application.properties解释

```java
# 服务器及应用一些基本配置
# 应用名
spring.application.name=files
#应用的端口号
#server.port=8989
# 应用的上下文路径，也可以称为项目路径，访问uri的首部，默认是/
server.servlet.context-path=/files

# thymeleaf及静态资源相关配置
spring.thymeleaf.cache=false
spring.thymeleaf.suffix=.html
spring.thymeleaf.encoding=UTF-8
spring.thymeleaf.prefix=classpath:/templates/
spring.resources.static-locations=classpath:/templates/,classpath:/static/

# 数据库连接池相关配置
# 阿里巴巴的druid数据源
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
# mysql连接驱动
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/files?serverTimezone=GMT
# 数据库用户名
spring.datasource.username=root
# 数据库密码
spring.datasource.password=123456

# Mybatis相关配置
# 告诉mybatis你的Mapper的xml在哪里
mybatis.mapper-locations=classpath:/com/myharbour/mapper/*.xml
# 告诉mybatis你的实体类在哪里
mybatis.type-aliases-package=com.myharbour.entity

```

# 三、核心功能代码解析
## 3.0 首先看一下实体类
* 用户信息对应用户表
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
@Accessors(chain=true)
public class User {
    private Integer id;
    private String username;
    private String password;
}
```
* 文件信息对应文件表（**注意：**这里的文件上传并不是把文件保存到数据库）

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
@Accessors(chain=true)
public class UserFile {
    private Integer id;
    private String oldFileName;
    private String newFileName;
    private String ext;
    private String path;
    private String size;
    private String type;
    private String isImg;
    private Integer downcounts;
    private Date uploadTime;
    private Integer userId;
}
```

## 3.1 文件上传
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201116211059136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

```java
@PostMapping("upload")
    public String upload(MultipartFile aaa,HttpSession session) throws IOException {
        //获取用户的id
        User user = (User) session.getAttribute("user");
        //获取文件原始名称
        String originalFilename = aaa.getOriginalFilename();
        //获取文件后缀
        String extension = "."+FilenameUtils.getExtension(aaa.getOriginalFilename());
        //生成新的文件名称
        String newFileName = new SimpleDateFormat("yyyyMMddHHmmss").format(new Date()) + UUID.randomUUID().toString().replace("-", "") + extension;

        //文件大小
        long size = aaa.getSize();
        //文件类型
        String type = aaa.getContentType();

        //处理根据日期生成目录
        String realPath = ResourceUtils.getURL("classpath:").getPath() + "/static/files";
        String dateFormat = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        String dateDirPath = realPath+"/"+ dateFormat;
        File dateDir = new File(dateDirPath);
        if (!dateDir.exists()) dateDir.mkdirs();


        //处理文件上传
        aaa.transferTo(new File(dateDir,newFileName));

        //将文件信息放入数据库中
        UserFile userFile = new UserFile();
        userFile.setOldFileName(originalFilename).setNewFileName(newFileName).setExt(extension).setSize(String.valueOf(size))
                .setType(type).setPath("/files/"+dateFormat).setUserId(user.getId());
        userFileService.save(userFile);

        return "redirect:/file/showAll";
    }
```


## 3.2 文件下载（预览）
* 这里为文件下载和文件在线预览提供同一个接口download
* 在接口中用传入的参数openStyle来控制是下载文件还是在线预览文件。
* thymeleaf页面中对应这部分是这么写的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111621235952.png#pic_center)
* 如果传入的参数openStyle为空代码的处理逻辑是下载文件，如果openStyle传入的参数不为空（其实就是传入inline），达到的效果则是文件在线预览。
```java
//文件下载
    @GetMapping("download")
    public void download(String openStyle, Integer id, HttpServletResponse response) throws IOException {
        //获取打开方式
        openStyle = openStyle==null ? "attachment" : openStyle;
        //获取文件信息
        UserFile userFile = userFileService.findById(id);
        if("attachment".equals(openStyle)){
            //更新下载次数
            userFile.setDowncounts(userFile.getDowncounts()+1);
            userFileService.update(userFile);
        }
        //根据文件信息中文件名字和文件存储路径获取文件输入流
        String realPath = ResourceUtils.getURL("classpath:").getPath() + "/static" + userFile.getPath();
        //获取文件输入流
        FileInputStream is = new FileInputStream(new File(realPath, userFile.getNewFileName()));
        //附件下载
        response.setHeader("content-disposition",openStyle+";fileName"+ URLEncoder.encode(userFile.getOldFileName(),"UTF-8"));
        //获取响应输出流
        ServletOutputStream os = response.getOutputStream();
        //文件拷贝
        IOUtils.copy(is,os);
        IOUtils.closeQuietly(is);
        IOUtils.closeQuietly(os);

    }
```

## 3.3 文件删除
* 文件删除这个功能在开发的时候要注意的是：文件删除除了把保存在对应路径上的文件删除，还需要把保存在数据库当中的该文件的信息也删除掉。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111621312063.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

```java
/*
    * 删除文件信息
    * */
    @GetMapping("delete")
    public String delete(Integer id) throws FileNotFoundException {
        //根据id查询信息
        UserFile userFile = userFileService.findById(id);
        //删除文件
        String realPath = ResourceUtils.getURL("classpath:").getPath() + "/static" + userFile.getPath();
        File file = new File(realPath, userFile.getNewFileName());
        if(file.exists()) file.delete();//立即删除

        //删除数据库中的记录
        userFileService.delete(id);

        return "redirect:/file/showAll";
    }
```

