---
layout: post
title: LeetCode House Rodder问题
category: 动态规划
tags: Dynamic-Programming LeetCode
Keywords: Dynamic-Programming
description:
---

# House Robber
## 1. LeetCode 198. [House Robber](https://leetcode.com/problems/house-robber/description/)
## 2. 问题描述
一个强盗要抢劫一条街道旁边的房子，每一个房子都存了一部分钱。要求不能同时抢劫两个相邻的房子，因为这样会触发自动报警装置。

给定一组非负整数表示每一个房子内可以抢到的钱数，计算在不触发报警机制的前提下最多能抢到多少钱。
## 3. 解题思路
是否抢劫第i个房子要取决于i-1个房子是否被抢劫，因此只有两种状态：
+ 抢劫第 i-1个房子，不抢第i个房子;
+ 不抢第i-1个房子，抢第i个房子;

因此前i个房子中能抢劫到的最大金额为 dp[i]  = max(dp[i-2] + nums[i], dp[i-1]);

## 4. 解决方案
```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int pre1 = 0;
        int pre2 = 0;
        for(int i = 0; i < nums.size(); i++){
            int cur = max(pre2 + nums[i],pre1);
            pre2 = pre1;
            pre1 = cur;
        }
        return pre1;
    }
};
```

# House Robber II
## 1. LeetCode 213. [House Robber II](https://leetcode.com/problems/house-robber-ii/description/)
## 2. 问题描述
一个抢到要抢劫一条街道旁边的房子，每个房子里都有一些钱。要求不能同时抢劫两个相邻的房子，因为这样会触发自动报警装置。而且这些房子是环形的，即第一个房子和最后一个房子是相邻的。

给定一组非负整数表示每一个房子中可以抢到的钱数，计算在不触发自动报警机制的前提下最多能抢到多少钱。
## 3. 解题思路

可以将该问题分解为两个House Robber问题，即假设有n个房子（编号为0至n-1），由于0和n-1这两个房子是邻居，因此不能同时抢劫这两个，所以可以将环形分解为两个部分：
+ 0至n-2（包括0但是不包括n-1)
+ 1至n-1(包括n-1但是不包括0)

因此分别计算这两个部分的动态规划结果的最大值即可。
## 4. 解决方案
```c++
class Solution {
public:
    int rob(const vector<int>& nums,int left, int right){
        int pre2 = 0;
        int pre1 = 0;
        for(int i = left; i <= right;i++){
            int cur = max(pre2+nums[i],pre1);
            pre2 = pre1;
            pre1 = cur;
        }
        return pre1;
    }
    
    int rob(vector<int>& nums) {
       if(nums.size() < 2){
           return nums.size() == 1? nums[0]:0;
       }
        return max(rob(nums,0,nums.size()-2),rob(nums,1,nums.size()-1));
    }
};
```

# Bad Neighbors
## 1. 问题描述
[TopCoder BadNeighbors](https://community.topcoder.com/stat?c=problem_statement&pm=2402&rd=5009)

小镇上的每一位住户都想要捐献一部分钱(保存在int数组中)，但是每一位住户都不想给其邻居捐献过的基金捐钱，而且第一位住户和最后一位住户是邻居。需要计算最多能收到多少捐献。
## 2. 解题思路
该问题与House Robber II的问题相同，即每一个住户是否捐献取决于其前一个住户是否捐款，因而只有两种情况：前一位邻居捐钱或者不捐钱，用dp[i]表示前i个住户最多能收到多少捐献金额，即可得到dp[i] = max(dp[i-2] + nums[i], dp[i-1])。

而第一位住户和最后一位住户相邻的话，则说明第一位住户和最后一位住户不可能同时捐款，因此就可以将原问题划分为两个子问题的最大值：
+ 包括第一户，不包括最后一户： 即求[0, n-2]位住户最多能收到多少捐款
+ 包括最后一户，不包括第一户：即求[1, n-1]户最多能收到多少捐款

## 3. 解决方案
```c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;
int MaxDonation(const vector<int>& nums, int left, int right) {
	int pre2 = 0;
	int pre1 = 0;
	for (int i = left; i <= right; i++) {
		int cur = max(pre2 + nums[i], pre1);
		pre2 = pre1;
		pre1 = cur;
	}
	return pre1;
}

int MaxDonations(const vector<int>& nums) {
	if (nums.size() < 2) {
		return nums.size() == 1 ? nums[0] : 0;
	}
	return max(MaxDonation(nums, 0, nums.size() - 2), MaxDonation(nums, 1, nums.size() - 1));

}
```