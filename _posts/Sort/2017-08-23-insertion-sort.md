---
layout: post
title: 插入排序
category: 排序算法
tags: 排序 插入排序
Keywords: 插入排序 
description:
---
## 1. 原理：
将数组划分为有序区间vec[0 ... i-1]和无序区间vec[i ... n-1]，对于未排序的vec[i], 在已排序的区间内从后往前扫描，直到找到相应的位置并插入。
## 2. 代码：
``` c++
void InsertionSort(vector<int>& vec){
    for(int i = 1; i < vec.size();i++){
        int key = vec[i];
        int j = i - 1;
        while(j >= 0 && vec[j] > key){
            vec[j+1] = vec[j];
            j--;
        }
        vec[j+1] = key;
    }
}
```
## 3. 算法步骤：
1. 从第二个元素开始，即vec[0]为有序区间， vec[1 ... n-1]为无序区间
2. 即将第i个数vec[i]插入到有序区间 vec[0 ... i-1]中
3. i++并重复计算第二步，直到 i = n - 1，计算完成
## 4. 分析：
1. 时间复杂度：
* 最优： O(n)
* 最差：O(n ^ 2)
* 平均：O(n ^ 2)
2. 空间复杂度： O(n)
3. 稳定性：稳定
## 5. 适用情况：
不适合数据量比较大的排序应用，如果数据量比较小的话，效果比较好。
## 6. 应用情况：
在STL的sort和stdlib的qsort算法中，都将插入排序作为快速排序的补充，用于少量元素的排序（通常为8个或以下）。
