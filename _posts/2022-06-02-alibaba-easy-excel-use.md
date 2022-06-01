---
layout: post
title: "阿里巴巴开源excel处理框架EasyExcel的简单使用"
subtitle: "收藏和推荐优秀的后端技术博客"
date: 2020-10-18
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [阿里巴巴, 开源项目, Java]
---
# 一、EasyExcel简介
## 1.1 Excel导入导出的应用场景
1. 数据导入：减轻录入工作量
2. 数据导出：统计信息归档
3. 数据传输：异构系统之间数据传输

## 1.2 EasyExcel特点
1. Java领域解析、生成Excel比较有名的框架有**Apache poi**、**jxl**等。但他们都存在一个严重的问题就是**非常的耗内存**。如果你的系统并发量不大的话可能还行，但是一旦并发上来后一定会OOM或者JVM频繁的full gc。
2. EasyExcel是阿里巴巴开源的一个excel处理框架，**以使用简单、节省内存著称**。EasyExcel能大大减少占用内存的主要原因是**在解析Excel时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析。**
3. **EasyExcel采用一行一行的解析模式**，并将一行的解析结果以观察者的模式通知处理（AnalysisEventListener）。

# 二、实现EasyExcel对Excel写操作
## 2.1 创建一个普通的maven项目
项目名：excel-easydemo
## 2.2 pom中引入xml相关依赖

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/com.alibaba/easyexcel -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>easyexcel</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```
## 2.3 创建实体类
* 设置表头和添加的数据字段

```java
import com.alibaba.excel.annotation.ExcelProperty;

//设置表头和添加的数据字段
public class DemoData {
    //设置表头名称
    @ExcelProperty("学生编号")
    private int sno;
    
	//设置表头名称
    @ExcelProperty("学生姓名")
    private String sname;

    public int getSno() {
        return sno;
    }

    public void setSno(int sno) {
        this.sno = sno;
    }

    public String getSname() {
        return sname;
    }

    public void setSname(String sname) {
        this.sname = sname;
    }

    @Override
    public String toString() {
        return "DemoData{" +
                "sno=" + sno +
                ", sname='" + sname + '\'' +
                '}';
    }
}
```
## 2.4 实现写操作
## 2.4.1 创建方法循环设置要添加到Excel的数据

```java
//循环设置要添加的数据，最终封装到list集合中
private static List<DemoData> data() {
    List<DemoData> list = new ArrayList<DemoData>();
    for (int i = 0; i < 10; i++) {
        DemoData data = new DemoData();
        data.setSno(i);
        data.setSname("张三"+i);
        list.add(data);
    }
    return list;
}
```

## 2.4.2 实现最终的添加操作（写法一）

```java
public static void main(String[] args) throws Exception {
    // 写法1
    String fileName = "F:\\11.xlsx";
    // 这里 需要指定写用哪个class去写，然后写到第一个sheet，名字为模板 然后文件流会自动关闭
    // 如果这里想使用03 则 传入excelType参数即可
    EasyExcel.write(fileName, DemoData.class).sheet("写入方法一").doWrite(data());
}
```

## 2.4.3 实现最终的添加操作（写法二）

```java
public static void main(String[] args) throws Exception {
    // 写法2，方法二需要手动关闭流
    String fileName = "F:\\112.xlsx";
    // 这里 需要指定写用哪个class去写
    ExcelWriter excelWriter = EasyExcel.write(fileName, DemoData.class).build();
    WriteSheet writeSheet = EasyExcel.writerSheet("写入方法二").build();
    excelWriter.write(data(), writeSheet);
    /// 千万别忘记finish 会帮忙关闭流
    excelWriter.finish();
}
```

# 三、实现EasyExcel对Excel读操作
## 3.1 创建实体类

```java
import com.alibaba.excel.annotation.ExcelProperty;
public class ReadData {
    //设置列对应的属性
    @ExcelProperty(index = 0)
    private int sid;
    
    //设置列对应的属性
    @ExcelProperty(index = 1)
    private String sname;

    public int getSid() {
        return sid;
    }
    public void setSid(int sid) {
        this.sid = sid;
    }
    public String getSname() {
        return sname;
    }
    public void setSname(String sname) {
        this.sname = sname;
    }
    @Override
    public String toString() {
        return "ReadData{" +
                "sid=" + sid +
                ", sname='" + sname + '\'' +
                '}';
    }
}
```

## 3.2 创建读取操作的监听器

```java
import com.alibaba.excel.context.AnalysisContext;
import com.alibaba.excel.event.AnalysisEventListener;
import com.alibaba.excel.exception.ExcelDataConvertException;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

//创建读取excel监听器
public class ExcelListener extends AnalysisEventListener<ReadData> {

    //创建list集合封装最终的数据
    List<ReadData> list = new ArrayList<ReadData>();

    //一行一行去读取excle内容
    @Override
    public void invoke(ReadData user, AnalysisContext analysisContext) {
       System.out.println("***"+user);
        list.add(user);
    }

    //读取excel表头信息
    @Override
    public void invokeHeadMap(Map<Integer, String> headMap, AnalysisContext context) {
        System.out.println("表头信息："+headMap);
    }

    //读取完成后执行
    @Override
    public void doAfterAllAnalysed(AnalysisContext analysisContext) {
    }
}
```

## 3.3 调用实现最终的读取

```java
public static void main(String[] args) throws Exception {

        // 写法1：
        String fileName = "F:\\01.xlsx";
        // 这里 需要指定读用哪个class去读，然后读取第一个sheet 文件流会自动关闭
        EasyExcel.read(fileName, ReadData.class, new ExcelListener()).sheet().doRead();

        // 写法2：
        InputStream in = new BufferedInputStream(new FileInputStream("F:\\01.xlsx"));
        ExcelReader excelReader = EasyExcel.read(in, ReadData.class, new ExcelListener()).build();
        ReadSheet readSheet = EasyExcel.readSheet(0).build();
        excelReader.read(readSheet);
        // 这里千万别忘记关闭，读的时候会创建临时文件，到时磁盘会崩的
        excelReader.finish();
}
```
