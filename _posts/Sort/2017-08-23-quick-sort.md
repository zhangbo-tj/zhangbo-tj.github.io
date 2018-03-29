---
layout: post
title: 快速排序
category: 排序算法
tags: 排序 快速排序
Keywords: 快速排序
description:
---
## 1. 原理：
利用分治法原理实现的排序算法，与归并排序不同的是，不是一半一半地分子数组，而是选择一个基准，将比它小的放在该基准的左侧，将比它大的放在该基准的右侧，然后再分别对左右两个子数组也进行同样的操作，直到整个数组有序为止。
## 2. 步骤：
1. 用一个基准数将数组分割成两个子数组
2. 将大于基准数的数移到其右侧，将小于基准数的移到其左侧
3. 递归的进行以上两个步骤，直到整个数组有序。
## 3. 代码：
``` c++
void QuickSort(vector<int>& vec, int start, int end){
    if(start < end - 1){
        int left = start , right = end - 1;
        while(left < right){
            while(vec[left] < vec[start] && left < right){
                left++;
            }
            while(vec[right] > vec[start] && left < right){
                right--;
            }
            swap(vec[left], vec[right]);
        }
        swap(vec[left], vec[start]);
    }
    QuickSort(vec, start, left);
    QuickSort(vec, left + 1,end);
}
```
## 4. 分析：
1. 时间复杂度：
* 最优：O(n)
* 最差：O(n ^ 2)
* 平均： O(nlgn)
2. 空间复杂度： 根据实现方式的不同而不同
3. 稳定性：不稳定
