---
layout: post
title: "小白入门SpringBoot项目之员工管理系统"
subtitle: "小白入门系列项目"
date: 2020-11-17
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [SpringBoot, 小白入门]
---
* **小白入门Spring Boot系列项目**
1. [**小白入门SpringBoot项目【1】：在线文件管理系统**](https://blog.csdn.net/weixin_44870909/article/details/109729357)
2. [**小白入门SpringBoot项目【2】：员工管理系统**](https://blog.csdn.net/weixin_44870909/article/details/109749701)

@[TOC](员工管理系统)


> 说明：
> **本项目非常适合小白入门**，如果读者的水平比较高的话，可能这个项目对你并不会有什么额外的收获。欢迎读者在评论区互相交流讨论。
> 对小白的收获可能包括：入门SpringBoot、Mybatis、Thymeleaf的使用。

* [GitHub项目地址](https://github.com/GardenChan/employee-management-system)
* [Gitee项目地址](https://gitee.com/gardenchan/employee-management-system)

# 一、项目简介
* 基于SpringBoot，利用Mybatis框架，前端使用Thymeleaf整合。
* 实现了管理员的注册、登录，已经对员工的增删改查功能。
* 在管理员的注册流程中引入验证码。在项目中也提供了生成验证码的工具类。
* 本项目用来入门SpringBoot以及练手Mybatis框架，应该会有比较全面的认识。

* **管理员登录界面**
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020111719142077.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

* **管理员注册界面**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117191430284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

* **对员工的增删改查界面**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201117191441296.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
# 二、部分代码解析
## 2.1 实体类
* 根据前面对项目的介绍，可以知道项目比较简单，可以归纳为两个实体类，一个是**User**就是这个系统的使用者（**管理员**），另一个是**Emp**，也就是系统要增删改查的对象，**员工**！

* **User**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String id;
    private String username;
    private String realname;
    private String password;
    private String sex;
}
```

* **Emp**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ToString
public class Emp {
    private String id;
    private String name;
    private Double salary;
    private Integer age;
    private Date bir;
}
```

## 2.2 注册过程中的验证码逻辑
* 调用验证码工具类，生成验证码
* 将验证码的内容信息（字符串）存入session中
* 将验证码图片通过response响应回前端
* 前端页面进入到注册页面后，生成验证码图片并显示

```java
    //生成验证码
    @GetMapping("code")
    public void getImage(HttpSession session, HttpServletResponse response) throws IOException {
        //生成验证码
        String securityCode = ValidateImageCodeUtils.getSecurityCode();
        BufferedImage image = ValidateImageCodeUtils.createImage(securityCode);
        session.setAttribute("code",securityCode);//存入session作用于中
        //响应图片
        ServletOutputStream os = response.getOutputStream();
        ImageIO.write(image,"png",os);
    }
```
* 注册的时候，将前端用户输入的字符串验证码和session中的验证码进行比较
* 如果二者一致，则放行注册。

```java
//注册方法
    @PostMapping("/register")
    public String register(User user,String code,HttpSession session){
        String sessionCode = (String)session.getAttribute("code");
        if(sessionCode.equalsIgnoreCase(code)){
            userService.register(user);
            return "redirect:/index";
        }else{
            return "redirect:/toRegister";
        }

    }
```

## 2.3 验证码工具类

```java
public class ValidateImageCodeUtils {
    /**
     * 验证码难度级别 Simple-数字 Medium-数字和小写字母 Hard-数字和大小写字母
     */
    public enum SecurityCodeLevel {
        Simple, Medium, Hard
    };
    /**
     * 产生默认验证码，4位中等难度
     *
     * @return
     */
    public static String getSecurityCode() {
        return getSecurityCode(4, SecurityCodeLevel.Medium, false);
    }
    /**
     * 产生长度和难度任意的验证码
     *
     * @param length
     * @param level
     * @param isCanRepeat
     * @return
     */
    public static String getSecurityCode(int length, SecurityCodeLevel level, boolean isCanRepeat) {
        // 随机抽取len个字符
        int len = length;
        // 字符集合（--除去易混淆的数字0,1,字母l,o,O）
        char[] codes = {
                '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
                'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z',
                'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z'
        };
        // 根据不同难度截取字符串
        if (level == SecurityCodeLevel.Simple) {
            codes = Arrays.copyOfRange(codes, 0, 10);
        } else if (level == SecurityCodeLevel.Medium) {
            codes = Arrays.copyOfRange(codes, 0, 36);
        }
        // 字符集和长度
        int n = codes.length;
        // 抛出运行时异常
        if (len > n && isCanRepeat == false) {
            throw new RuntimeException(String.format("调用SecurityCode.getSecurityCode(%1$s,%2$s,%3$s)出现异常，" + "当isCanRepeat为%3$s时，传入参数%1$s不能大于%4$s", len, level, isCanRepeat, n));
        }
        // 存放抽取出来的字符
        char[] result = new char[len];
        // 判断能否出现重复字符
        if (isCanRepeat) {
            for (int i = 0; i < result.length; i++) {
                // 索引0 and n-1
                int r = (int) (Math.random() * n);
                // 将result中的第i个元素设置为code[r]存放的数值
                result[i] = codes[r];
            }
        } else {
            for (int i = 0; i < result.length; i++) {
                // 索引0 and n-1
                int r = (int) (Math.random() * n);
                // 将result中的第i个元素设置为code[r]存放的数值
                result[i] = codes[r];
                // 必须确保不会再次抽取到那个字符，这里用数组中最后一个字符改写code[r],并将n-1
                codes[r] = codes[n - 1];
                n--;
            }
        }
        return String.valueOf(result);
    }
	/**
     * 生成验证码图片

     * @param securityCode

     * @return

     */
    public static BufferedImage createImage(String securityCode){

        int codeLength = securityCode.length();//验证码长度

        int fontSize = 18;//字体大小

        int fontWidth = fontSize+1;

        //图片宽高

        int width = codeLength*fontWidth+6;
        int height = fontSize*2+1;
        //图片

        BufferedImage image = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);

        Graphics2D g = image.createGraphics();

        g.setColor(Color.WHITE);//设置背景色

        g.fillRect(0, 0, width, height);//填充背景

        g.setColor(Color.LIGHT_GRAY);//设置边框颜色

        g.setFont(new Font("Arial", Font.BOLD, height-2));//边框字体样式

        g.drawRect(0, 0, width-1, height-1);//绘制边框

        //绘制噪点

        Random rand = new Random();

        g.setColor(Color.LIGHT_GRAY);

        for (int i = 0; i < codeLength*6; i++) {

            int x = rand.nextInt(width);

            int y = rand.nextInt(height);

            g.drawRect(x, y, 1, 1);//绘制1*1大小的矩形

        }

        //绘制验证码

        int codeY = height-10;

        g.setColor(new Color(19,148,246));

        g.setFont(new Font("Georgia", Font.BOLD, fontSize));
        for(int i=0;i<codeLength;i++){
        	double deg=new Random().nextDouble()*20;
        	g.rotate(Math.toRadians(deg), i*16+13,codeY-7.5);
            g.drawString(String.valueOf(securityCode.charAt(i)), i*16+5, codeY);
            g.rotate(Math.toRadians(-deg), i*16+13,codeY-7.5);
        }
       
        g.dispose();//关闭资源
        return image;
    }

    public static void main(String[] args) throws IOException {
        String securityCode = ValidateImageCodeUtils.getSecurityCode();
        System.out.println(securityCode);

        BufferedImage image = ValidateImageCodeUtils.createImage(securityCode);
        ImageIO.write(image,"png",new FileOutputStream("aa.png"));
    } 
}
```
