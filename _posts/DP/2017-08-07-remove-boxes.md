---
layout: post
title: 移除盒子  奇怪的打印机
category: 动态规划
tags: Dynamic-Programming LeetCode
Keywords: Dynamic-Programming
description:
---
# 移除盒子
## 1. LeetCode 546. [Remove Boxes](
https://leetcode.com/problems/remove-boxes/description/
## 2. 描述
给定一组盒子，用不同的整数分别代表盒子不同的颜色。每次都从中移除几个盒子，直到完全被移除为止。每一次可以选择一组相同颜色的盒子，如果一次移除K个相同颜色的盒子，则得分为：K * K。
计算最多能够得到多少分。
## 3. 示例
```
输入：[1, 3, 2, 2, 2, 3, 4, 3, 1]
输出：23
解释：[1, 3, 2, 2, 2, 3, 4, 3, 1]
第一轮：移除[2, 2, 2]，剩余[1, 3, 3, 4, 3, 1]，得分为 3 * 3 = 9
第二轮：移除4，剩余[1,3,3,1],得分为 1 * 1 = 1
第三轮：移除[3,3],剩余[1,1,1],得分为 2 * 2 = 4
第四轮：移除[1,1,1],剩余[],得分为 3 * 3 = 9
```
## 4. 解题思路
用矩阵dp[left][right][k] 表示[left,  right]范围内的盒子，而且后面有k个相同盒子的序列最多能获得的分数，例如dp[l][r][3]就表示序列 [b_l, b_l+1, b_l+2,,, b_r, A, A, A]，且b_r = A，这样的序列最多能获得的分数。

因此如果存在整数 i 满足： b[i] = b[r], l < i < r时，就可以考虑是否要把序列(b[l], b[l+1], b[l+2]....b[r], A,,,A)分割为两个部分： (b[l], b[l+1], ... b[i], b[r], A,,,A) 和[b[i+1], b[i+2],,,,b[r-1]]，由此就可以得到以下优化公式： dp[l][r][k] = max(dp[l][r][k], dp[l][i][k+1] + dp[i+1][r-1][0])

而计算dp[l][r][k] = dp[l][r][0] + k * k，最后的计算结果存储在dp[0][len - 1][0]中
## 5. 解决方案
``` c++
int nums[100][100][100];
int dfs(vector<int>& boxes,int left, int right, int k){
    if(left > right){
        return 0;
    }
    if(nums[left][right][k] > 0){
        return nums[left][right][k];
    }
    while(right > left && boxes[right] == boxes[right-1]){
        right--;
        k++;
    }
    nums[left][right][k] = dfs(boxes,left,right-1,0) + (k+1)*(k+1);
    
    for(int i = left; i < right; i++){
        if(boxes[i] == boxes[right]){
            nums[left][right][k] = max(nums[left][right][k], 
                                     dfs(boxes,left,i,k+1) + dfs(boxes, i+1, right-1,0));
        }
    }
    return nums[left][right][k];
}
int removeBoxes(vector<int>& boxes) {
    if(boxes.empty()){
        return 0;
    }
    int len = boxes.size();
    memset(nums,-1,sizeof(nums));
    return dfs(boxes, 0, len-1, 0);
}
```

# 奇怪的打印机
## 1. LeetCode 664. [Strange Printer](https://leetcode.com/contest/leetcode-weekly-contest-46/problems/strange-printer/)
## 2. 描述
有一个奇怪的打印机有一下要求：
1. 打印机每次都只能打印一段相同字符的序列
2. 每次打印时，打印机都能打印一段范围内的所有空位，可以覆盖之前打印的字符，但是打印中间不能有空隙

给定一个英文字符串（只包含小写英文字母），计算打印该字符串最小需要多少次打印。
## 3. 示例
* 输入：“aaabbb”
* 输出：2
* 解释：先打印“aaa”,再打印“bbb”
## 4. 解题思路
可以将其与LeetCode564 移除盒子的问题是相似的，但是因为和后面有多少个重复字符不无关，所以只需要用一个二维数组，dp[i][j] 表示打印范围[i, j]内的所有字符最少需要打印多少次。

如果存在 str[k] = str[j]，i < k < j，则可以将原字符串(s[i], s[i+1], s[i+2],,,s[j])分割为两个部分：```(s[i], s[i+1], ...s[k])```和 ```(s[k +1], s[k + 2],,,s[j-1])```, 注意此时没有s[j], 因为在打印前一个子串时，可以将打印范围设置为(s[i]...s[j])。
因此可以得到的子问题为： ```dp[i][j] = min(dp[i][j],  dp[i][k] + dp[k+1][j-1])，其中 i < k < j, 且str[k] = str[j]```
## 5. 解决方案
``` c++
int dfs(string& s, vector<vector<int>>& turns, int left, int right){
    if(left > right){
        return 0;
    }
    if(turns[left][right] != 0){
        return turns[left][right];
    }
    turns[left][right] = dfs(s, turns, left, right - 1) + 1;
    for(int i = left ; i < right; i++){
        if(s[i] == s[right]){
            turns[left][right] = min(turns[left][right], dfs(s,turns,left,i) + dfs(s,turns,i+1, right-1));
        }
    }
    return turns[left][right];
    
}
int strangePrinter(string s) {
    vector<vector<int>> turns(s.size(), vector<int>(s.size(),0));
    int res = dfs(s, turns, 0, s.size()-1);
    return res;
}
```