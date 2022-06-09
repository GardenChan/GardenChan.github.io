---
layout: post
title: "【排序算法系列 3】 插入排序"
subtitle: "data structure and algorithm insert sort"
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
* 插入排序（Insertion sort）是一种简单直观且稳定的排序算法。
* 插入排序的工作方式非常像人们排序一手扑克牌一样。开始时，我们的左手为空并且桌子上的牌面朝下。然后，我们每次从桌子上拿走一张牌并将它插入左手中正确的位置。为了找到一张牌的正确位置，我们从右到左将它与已在手中的每张牌进行比较，如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020223647151.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)

## 需求
* 排序前：{4,3,2,10,12,1,5,6}
* 排序后：{1,2,3,4,5,6,10,12}

## 插入排序原理
1. 把所有的元素分为两组，已经排序的和未排序的；
2. 找到未排序的组中的第一个元素，向已经排序的组中进行插入；
3. 倒叙遍历已经排序的元素，依次和待插入的元素进行比较，直到找到一个元素小于等于待插入元素，那么就把待插入元素放到这个位置，其他的元素向后移动一位；
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020223828102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDg3MDkwOQ==,size_16,color_FFFFFF,t_70#pic_center)


## 插入排序API设计
| 类名        | Insertion                                                    |
| ----------- | ------------------------------------------------------------ |
| 构造方法    | Insertion()：**创建Insertion对象**                           |
| 成员方法 1  | 1.public static void sort(Comparable[] a)：**对数组内的元素进行排序** |
| 成员方法  2 | 2.private static boolean greater(Comparable v,Comparable w):**判断v是否大于w** |
| 成员方法 3  | 3.private static void exch(Comparable[] a,int i,int j)：**交换a数组中，索引i和索引j处的值** |


## 插入排序的代码实现

```java
public class Insertion {

    //对数组a中的元素进行排序
    public static void sort(Comparable[] a){
        for(int i=1;i<a.length;i++){
            for(int j=i;j>0;j--){
                if (greater(a[j-1],a[j])){
                    exch(a,j-1,j);
                }else{
                    break;
                }
            }
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
## 插入排序的时间复杂度: O(N^2)