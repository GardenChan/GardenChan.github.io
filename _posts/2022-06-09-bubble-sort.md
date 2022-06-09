---
layout: post
title: "【排序算法系列 1】冒泡排序"
subtitle: "data structure bubble sort"
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
冒泡排序（Bubble Sort），是一种计算机科学领域的较简单的排序算法。
## 需求
* 排序前：{4,5,6,3,2,1}
* 排序后：{1,2,3,4,5,6}

## 排序原理
1. 比较相邻的元素。如果前一个元素比后一个元素大，就交换这两个元素的位置。
2. 对每一对相邻元素做同样的工作，从开始第一对元素到结尾的最后一对元素。最终最后位置的元素就是最大值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020221357641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)
## 冒泡排序API设计
| 类名        | Bubble                                                       |
| ----------- | ------------------------------------------------------------ |
| 构造方法    | Bubble()：**创建Bubble对象**                                 |
| 成员方法 1  | 1.public static void sort(Comparable[] a)：**对数组内的元素进行排序** |
| 成员方法  2 | 2.private static boolean greater(Comparable v,Comparable w):**判断v是否大于w** |
| 成员方法 3  | 3.private static void exch(Comparable[] a,int i,int j)：**交换a数组中，索引i和索引j处的值** |


## 冒泡排序的代码实现

```java
public class Bubble {

	//对数组a中的元素进行排序
    public static void sort(Comparable[] a){
        for(int i=a.length-1;i>0;i--){
            for(int j=0;j<i;j++){
                if(greater(a[j],a[j+1])){
                    exchange(a,j,j+1);
                }
            }
        }
    }
    
	//比较v元素是否大于w元素
    public static boolean greater(Comparable v,Comparable w){
        return v.compareTo(w)>0;
    }
	
	//数组元素i和j交换位置
    public static void exchange(Comparable[] a,int i,int j){
        Comparable temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
}
```
## 冒泡排序的时间复杂度: O(N^2)