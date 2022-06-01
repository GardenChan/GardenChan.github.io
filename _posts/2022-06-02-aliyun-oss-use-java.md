---
layout: post
title: "一文读懂什么是阿里云OSS，如何使用Java操作阿里云OSS？"
subtitle: "收藏和推荐优秀的后端技术博客"
date: 2020-10-18
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [云计算, 云原生, Java, 阿里云]
---
# 一、什么是阿里云OSS
* **OSS: Object Storage Service**
* **对象存储服务**是一种海量、安全、低成本、高可靠的云存储服务，适合存放任意类型的文件。容量和处理能力弹性扩展，多种存储类型供选择，全面优化存储成本。
# 二、准备工作
* 阿里云网站: [https://www.aliyun.com](https://www.aliyun.com)
## 1. 注册阿里云账号，建议用支付宝注册。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101813180178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
## 2. 立即开通
* 根据自身实际情况选择**按量付费**或者**包年包月**
* 根据自身需求选择**存储类型**
* 存储类型说明：**存储类型（Storage Class）**，OSS提供**标准**、**低频访问**、**归档**、**冷归档**四种存储类型，全面覆盖从热到冷的各种数据存储场景。

> 1. **标准存储类型**  提供高持久、高可用、高性能的对象存储服务，能够支持频繁的数据访问。
> 2. **低频访问存储类型**  适合长期保存不经常访问的数据（平均每月访问频率1到2次），存储单价低于标准类型。
> 3. **归档存储类型**  适合需要长期保存（建议半年以上）的归档数据。
> 4. **冷归档存储**  适合需要超长时间存放的极冷数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018133004641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
## 3.入门前基本概念
* **存储空间（Bucket）**

> 存储空间是您用于存储对象（Object）的容器，所有的对象都必须隶属于某个存储空间。

* **对象（Object）**

> 对象是OSS存储数据的基本单元，也被称为OSS的文件。对象由元信息（Object Meta）、用户数据（Data）和文件名（Key）组成。

* **地域（Region）**

> 地域表示OSS的数据中心所在物理位置。您可以根据费用、请求来源等选择合适的地域创建Bucket。

* **访问域名（Endpoint）**

> Endpoint表示OSS对外服务的访问域名。OSS以HTTP RESTful API的形式对外提供服务，当访问不同地域的时候，需要不同的域名。
* **访问密钥（AccessKey）**
> AccessKey简称AK，指的是访问身份验证中用到的AccessKey Id和AccessKey Secret。OSS通过使用AccessKey Id和AccessKey Secret对称加密的方法来验证某个请求的发送者身份。AccessKey Id用于标识用户；AccessKey Secret是用户用于加密签名字符串和OSS用来验证签名字符串的密钥，必须保密。
## 4. 管理控制台
### 4.1 首先创建Bucket
* 创建Bucket
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018134906108.png#pic_center)
* 根据介绍创建自己的Bucket，无特殊需要，按照如下配置即可。（具体根据需要，例如数据访问比较低频的可以不选择标准存储，而选择低频访问存储 ，价格便宜一些）
* 重要：读写权限这里选择“**公共读**”，也就是上传（写入）数据需要身份验证，而使用（读取）数据不需要验证。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018135809949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

### 4.2 创建Bucket成功后，打开Bucket列表
* 打开Bucket列表
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020101814033052.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 点击进入自己创建好的Bucket
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018140610103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 进入右侧文件管理，进入后点击“上传文件”，可以手动上传文件。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018140757254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

# 三、获取AccessKey ID和AccesssKey Secret
## 3.1 什么是AccessKey 
* AccessKey简称AK，指的是访问身份验证中用到的AccessKey Id和AccessKey Secret。OSS通过使用AccessKey Id和AccessKey Secret对称加密的方法来验证某个请求的发送者身份。
* AccessKey Id和AccessKey Secret用于我们后续使用java操作OSS的身份认证，因此十分重要。
## 3.2 如何获取AccessKey
* 进入OSS管理控制台，进入Access Kay
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018141750319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 继续使用AccessKey
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018142010943.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 创建AccessKey
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018141956744.png#pic_center)

# 四、使用Java操作阿里云OSS：文件上传
## 4.1 application.properties配置阿里云OSS相关信息

```properties
# 阿里云 OSS
# 外网访问的Endpoint（地域节点）（下方实例是北京，改成你自己的）
# 不同的服务器，地址不同  ！！！注意不要加空格！！！
aliyun.oss.file.endpoint=oss-cn-beijing.aliyuncs.com
aliyun.oss.file.keyid=填你的AccessKey ID
aliyun.oss.file.keysecret=填你的AccessKey Secret
# bucket可以在控制台创建 也可以使用java代码创建
aliyun.oss.file.bucketname=你的Bucket名称
```
## 4.2 写一个工具类绑定properties里的阿里云OSS配置
* 这样做的目的是方便在代码中访问配置信息，如需修改配置信息直接在properties中修改即可，无需修改代码。

```java
@Component
public class ConstantPropertiesUtils implements InitializingBean {
    // 读取配置文件内容
    @Value("${aliyun.oss.file.endpoint}")
    private String endPoint;

    @Value("${aliyun.oss.file.keyid}")
    private String keyId;

    @Value("${aliyun.oss.file.keysecret}")
    private String keySecret;

    @Value("${aliyun.oss.file.bucketname}")
    private String bucketName;

    //定义公开静态常量
    public static String END_POINT;
    public static String ACCESS_KEY_ID;
    public static String ACCESS_KEY_SECRET;
    public static String BUCKET_NAME;

    @Override
    public void afterPropertiesSet() throws Exception {
        END_POINT=endPoint;
        ACCESS_KEY_ID=keyId;
        ACCESS_KEY_SECRET=keySecret;
        BUCKET_NAME=bucketName;
    }
}
```
## 4.3 Controller
```java
@RestController
@RequestMapping("/fileoss")
@CrossOrigin
public class OssController {

    @Autowired
    private OssService ossService;

    @PostMapping
    public String uploadOssFile(MultipartFile file){
        //获取上传文件 MultipartFile
        //返回上传到oss的路径
        String url = ossService.uploadFileAvatar(file);
        return url;
    }
}
```

## 4.4 Service及其实现类

```java
public interface OssService {
    // 上传头像到oss
    String uploadFileAvatar(MultipartFile file);
}
```

```java
@Service
public class OssServiceImpl implements OssService {
    @Override
    public String uploadFileAvatar(MultipartFile file) {
        //工具类获取值
        String endpoint = ConstantPropertiesUtils.END_POINT;
        String accessKeyId = ConstantPropertiesUtils.ACCESS_KEY_ID;
        String accessKeySecret = ConstantPropertiesUtils.ACCESS_KEY_SECRET;
        String bucketName = ConstantPropertiesUtils.BUCKET_NAME;

        try {
            //创建oss实例
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
            //上传文件流
            InputStream inputStream = file.getInputStream();
            //获取文件名称
            String filename = file.getOriginalFilename();
            // 在文件名称里面添加随机 唯一的值
            String uuid = UUID.randomUUID().toString().replaceAll("-", "");
            filename = uuid+filename;

            //文件按日期分类
            // 2019/11/12/01.jpg
            //获取当前日期
            String s = new DateTime().toString("yyyy/MM/dd");
            filename = s+"/"+filename; //拼接
            //调用oss方法实现上传
            //第一个参数 Bucket名称
            //第二个参数 上传到oss 文件路径和文件名称
            //第三个参数 上传文件输入流
            ossClient.putObject(bucketName, filename, inputStream);
            //关闭ossclient
            ossClient.shutdown();
            //返回上传之后文件路径
            //需要把上传到阿里云oss路径手动拼接出来
            String url = "https://" + bucketName + "." + endpoint + "/" + filename;
            return url;
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }
}

```
* 核心代码就是这个service接口的实现类
* 代码主要流程：

> 1. 通过工具类获取配置文件中的OSS配置信息
> 2. 利用OSS配置信息创建OSS实例
> 3. 文件名处理 ，这里采用“UUID+原始文件名”作为上传后存在Bucket中的文件名
> 4. 文件路径拼接，这里采用按日期作为文件路径来归档 ，在文件名前拼接上日期路径
> 5. 调用OSS方法按照文件路径上传文件
> 5. 上传后文件后将访问文件的url拼接出来返回，以供文件的访问

## 4.5 测试效果
* 文件按照日期作为文件夹层级存储。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018143637558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
* 2020年9月25日下的文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201018143736578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
