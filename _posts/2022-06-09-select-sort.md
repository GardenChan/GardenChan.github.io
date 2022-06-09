---
layout: post
title: "【排序算法系列 2】选择排序"
subtitle: "data structure and algorithm selelct sort"
date: 2020-10-20
author: "Garden"
header-img: "img/post-bg-2015.jpg"
tags: [数据结构与算法]
---



[【排序算法系列 1】冒泡排序](https://blog.csdn.net/weixin_44870909/article/details/109190837)
[【排序算法系列 2】选择排序](https://blog.csdn.net/weixin_44870909/article/details/109191134)
[【排序算法系列 3】 插入排序](https://blog.csdn.net/weixin_44870909/article/details/109191243)
[【排序算法系列 4】 高级排序——希尔排序（插入排序的改进）](https://blog.csdn.net/weixin_44870909/article/details/109191408)
[【排序算法系列 5】 高级排序——归并排序](https://blog.csdn.net/weixin_44870909/article/details/109191614)
[【排序算法系列 6】 高级排序——归并排序（由冒泡排序改进）](https://blog.csdn.net/weixin_44870909/article/details/109192101)
[【排序算法系列 7】堆排序](https://blog.csdn.net/weixin_44870909/article/details/110728319)

---
## 简介
选择排序是一种更加简单直观的排序方法。
## 需求
* 排序前：{4,6,8,7,9,2,10,1}
* 排序后：{1,2,4,5,7,8,9,10}

## 选择排序原理
1. 每一次遍历的过程中，都假定第一个索引处的元素是最小值，和其他索引处的值依次进行比较，如果当前索引处的值大于其他某个索引处的值，则假定其他某个索引出的值为最小值，最后可以找到最小值所在的索引。
2. 交换第一个索引处和最小值所在的索引处的值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020222840179.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

## 选择排序API设计
| 类名        | Selection                                                    |
| ----------- | ------------------------------------------------------------ |
| 构造方法    | Selection()：**创建Selection对象**                           |
| 成员方法 1  | 1.public static void sort(Comparable[] a)：**对数组内的元素进行排序** |
| 成员方法  2 | 2.private static boolean greater(Comparable v,Comparable w):**判断v是否大于w** |
| 成员方法 3  | 3.private static void exch(Comparable[] a,int i,int j)：**交换a数组中，索引i和索引j处的值** |


## 选择排序的代码实现

```java
public class Selection {

    //对数组a中的元素进行排序
    public static void sort(Comparable[] a){
        for(int i=0;i<=a.length-2;i++){
            //定义一个变量，记录最小元素所在的索引，默认为参与选择排序的第一个元素所在的位置
            int minIndex = i;
            for(int j=i+1;j<a.length;j++){
                //需要比较最小索引minIndex处的值和j索引处的值；
                if (greater(a[minIndex],a[j])){
                    minIndex=j;
                }
            }
            //交换最小元素所在索引minIndex处的值和索引i处的值
            exch(a,i,minIndex);
        }
    }

	//比较v元素是否大于w元素
    private static  boolean greater(Comparable v,Comparable w){
        return v.compareTo(w)>0;
    }

    //数组元素i和j交换位置
    private static void exch(Comparable[] a,int i,int j){
        Comparable temp;
        temp = a[i];
        a[i]=a[j];
        a[j]=temp;
    }
}
```
## 选择排序的时间复杂度: O(N^2)