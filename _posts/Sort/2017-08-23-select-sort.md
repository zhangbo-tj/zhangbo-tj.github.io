---
layout: post
title: 选择排序
category: 排序算法
tags: 排序 选择排序
Keywords: 选择排序
description:
---
## 1. 原理：
每次都从乱序数组中找出最小的元素，放到有序数组的末尾（乱序数组的头部），最终实现整个数组都有序排列。
## 2. 步骤：
1. 首先从未排序序列中找到最小元素，将其放到排序序列的起始位置
2. 再从剩余未排序元素中继续寻找最小元素，然后放到已排序序列的末尾
3. 重复以上过程，直到所有元素都排序完毕。
## 3. 代码：
``` c++
void SelectSort(vector<int>& vec){
    for(int i = 0; i < vec.size();i++){
        int min = i;
        for(int j = i + 1; j < vec.size;j++){
            if(vec[min] > vec[j]){
                min = j;
            }
        }
        if(min != i){
            int temp = vec[min];
            vec[min] = vec[i];
            vec[i] = temp;
        }
    }
}
```
## 4. 分析：
1. 时间复杂度：
* 最优： O(n ^ 2)
* 最差：O(n ^ 2)
* 平均： O(N ^ 2)
2. 空间复杂度：  O(n)
3. 稳定性（不稳定）
