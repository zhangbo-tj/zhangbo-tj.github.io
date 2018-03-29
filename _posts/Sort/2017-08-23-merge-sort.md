---
layout: post
title: 归并排序
category: 排序算法
tags: 排序 归并排序
Keywords: 归并排序
description:
---
## 1. 原理：
采用分治法的一种典型应用
## 2. 迭代法步骤：
1. 将序列上每相邻两个元素进行归并操作，形成floor(n / 2)个序列，每个序列包含两个元素
2. 将上述序列再次进行归并，使得每个序列有4个元素
3. 重复步骤2，直到所有元素排序完毕。
## 3. 迭代法代码：
``` c++
void IterativeMergeSort(vector<int>& vec){
    vector<int> temp(vec.size(),0);
    int len = vec.size();
    for(int seg = 1; seg < len; seg *= 2){
        for(int start = 0; start < len; start += 2 * seg){
            int low = start;
            int mid = min(start + seg, len);
            int high = min(start + 2 * seg, len);
            int index = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while(start1 < end1 && start2 < end2){
                temp[index++] = vec[start1]<vec[start2]?vec[start1++]:vec[start2++];
            }
            while(start1 < end1){
                temp[index++] = vec[start1++];
            }
            while(start2 < end2){
                temp[index++] = vec[start2++];
            }
        }
        swap(temp,vec);
    }
}
```
## 4. 递归法步骤：
1. 申请空间，其大小为两个已排序序列之和，用来存放合并后的序列
2. 设定两个指针，最初位置分别为两个已排序序列的开始位置
3. 比较两个指针所指向的元素，将较小的元素放入合并后的区间，并将其指针移动到下一个位置。
4. 重复以上步骤。
## 5. 递归法代码：
``` c++
void RecursiveMergeSort(vector<int>& vec, int left, int right){
    if(left >= right || left + 1 == right){
        return;
    }
    int left1 = left;
    int right1 = (left + right) / 2;
    int left2 = right + 1;
    int right2 = right;
    RecursiveMergeSort(vec, left1, right1);
    RecursiveMergeSort(vec, left2, right2);
    vector<int> temp(vec.size(),0);
    int index = left1;
    while(left1 <= right1 && left2 <= right2){
        temp[index++] = vec[left1] < vec[left2]? vec[left1++]:vec[left2++];
    }
    while(left1 <= right1){
        temp[index++] = vec[left1++];
    }
    while(left2 <= right2){
        temp[index++] = vec[left2++];
    }
    for(int i = left; i <= right; i++){
        vec[i] = temp[i]
    }
}
```
6. 分析：
1. 时间复杂度：
* 最优： O(n)
* 最差： O(nlgn)
* 平均：O(nlgn)
2. 空间复杂度：O(n)
3. 稳定性：稳定
4. 缺点：需要额外存储空间
