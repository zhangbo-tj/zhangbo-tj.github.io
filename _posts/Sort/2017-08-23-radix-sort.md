---
layout: post
title: 基数排序
category: 排序算法
tags: 排序 基数排序
Keywords: 基数排序
description:
---
## 1. 原理：
基数排序是一种非比较型整数排序算法，其原理是将整数按照位数切割成不同的数字，然后按照每个位数进行比较。
## 2. 步骤：
1. 获取数组a中的最大值，并用其计算数组的最大指数
2. 从指数1开始，根据位数对应数组中的数字进行桶排序
3. 对数组a按照指数进行排序
## 3. 代码：
``` c++
void RadixSort(vector<int>& vec){
    int max = vec[0];
    for(int i = 1; i < vec.size(); i++){
        max = vec[i] > max ? vec[i]:max;
    }
    int d = 1;
    while(max / 10 != 0){
        max = max / 10;
        d++;
    }
    vector<int> temp(vec.size());
    vector<int> count(10);
    int len = vec.size();
    int radix = 1;
    for(int i = 1; i <= d; i++){
        count.assign(10,0);
        for(auto num:vec){
            int k = (num / radix) % 10;
            count[k]++;
        }
        partial_sum(count.begin(), count.end(), count.begin());
        for(int j = len - 1; j >= 0; j--){
            int k = (vec[j] / radix) % 10;
            temp[count[k]-1] = vec[j];
            count[k]--;
        }
        for(int j = 0; j < len; j++){
            vec[j] = temp[j];
        }
        radix *= 10;
    }
}
```
## 4. 分析：
1. 时间复杂度： O(k * N)
2. 空间复杂度：O(k + N)
3. 稳定性：稳定
