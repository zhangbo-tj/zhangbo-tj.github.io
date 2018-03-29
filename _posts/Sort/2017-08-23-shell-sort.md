---
layout: post
title: 希尔排序
category: 排序算法
tags: 排序 希尔排序
Keywords: 希尔排序
description:
---
## 1. 原理：
1. 对插入排序的改进：
* 插入排序对几乎已经排好序的数据进行操作时，效率高，几乎可以达到线性排序的效率
* 但是插入排序一般是低效的，因为插入排序每次只能将数据移动一位
2. 将全部元素分为几个区域来提升插入排序的性能，这样就能让一个元素朝最终位置前进一大步。然后算法再逐步取越来越小的步长进行排序，算法的最后一步就是插入排序，但是此时数组几乎已经是排好序了。
3. 步长序列：建议最初步长为 n / 2，然后每次对步长取半，直到步长为1位置
## 2. 代码：
``` c++
void ShellSort(vector<int>& vec){
    for(int group = vec.size() / 2; group >= 1;group /= 2){
        for(int i = group; i < vec.size(); i++){
            int key = vec[i];
            for(int j = i - group; j >= 0 && vec[j] > key; j -= group){
                vec[j + group] = vec[j];
            }
            vec[j + group] = key;
        }
    }
}
```
## 3. 分析：
1. 时间复杂度：
* 最优： O(n)
* 最差：根据步长不同而不同，
* 平均：根据步长不同而不同，O( n * logn ^ 2)
2. 空间复杂度：O(n)
3. 稳定性：不稳定
