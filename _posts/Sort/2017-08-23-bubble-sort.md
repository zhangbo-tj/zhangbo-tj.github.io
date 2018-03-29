---
layout: post
title: 冒泡排序
category: 排序算法
tags: 排序 冒泡排序
Keywords: 冒泡排序
description:
---
## 1. 原理：
遍历若干次数组，每次遍历时都要从左向右依次比较两个相邻的元素的大小，如果前者比后者大，那么交换它们的位置。这样在每次遍历之后，最大的元素一定位于区间的末尾。

然后再将区间范围限定为[0...n-2]，再进行一次遍历，这样第二大元素就位于该区间的末尾，即n-2位置。

重复以上过程，直到整个数组都有序为止。
## 2. 代码：
``` c++
void BubbleSort(vector<int>& vec){
    for(int i = vec.size() - 1; i > 0; i--){
        bool exchanged = false;
        for(int j = 0; j < i; j++){
            if(vec[j] > vec[j+1]){
                exchanged = true;
                int temp = vec[j];
                vec[j] = vec[j+1];
                vec[j+1] = temp;
            }
        }
        if(!exchanged){
            break;
        }
    }
}
```
## 3. 步骤：
1. 比较相邻的两个元素，如果前者大于后者，则交换它们两个
2. 对每一对相邻的与元素做以上比较操作，直到数组的末尾
3. 重复以上遍历操作，直到没有数字需要比较为止
## 4. 分析：
1. 时间复杂度：
* 最优： O(n)
* 最差：O(n ^ 2)
* 平均：O(n ^ 2)
2. 空间复杂度： O(n)
3. 稳定性： 稳定

