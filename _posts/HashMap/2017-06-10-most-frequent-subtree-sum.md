---
layout: post
title: 出现最频繁的子树和
category: HashMap
tags: HashMap LeetCode
Keywords: HashMap LeetCode
description:
---
## 1. LeetCode 508: [Most Frequent Subtree Sum](https://leetcode.com/problems/most-frequent-subtree-sum/description/)
## 2. 描述：
给定一颗树的root, 找出其所有子树的和中，出现最频繁的数。 

子树的和等于其所有的子节点的值相加。
## 3. 示例 1：
```
Input:
  5
 / \
2  -3
return [2, -3, 4], since all the values happen only once, return all of them in any order.
```
## 示例 2：
```
Input:
  5
 / \
2  -5
return [2], since 2 happens twice, however -5 only occur once.
```
## 4. 解决方案 1:
``` c++
unordered_map<int,int> sum_count;
int RootSum(TreeNode* root){
    if(root == NULL) 
        return 0;
    int sum = root->val + RootSum(root->left) + RootSum(root->right);
    sum_count[sum]++;
    return sum;
}
vector<int> findFrequentTreeSum(TreeNode* root) {
    RootSum(root);
    int max_count = 0;
    for(auto& iter: sum_count){
        max_count = max(max_count, iter.second);
    }
    vector<int> result;
    for(auto& iter: sum_count){
        if(iter.second == max_count){
            result.push_back(iter.first);
        }
    }
    return result;
}
```
## 5.解决方案 2:
``` c++
int FindRootSum(TreeNode* root, unordered_map<int,int>& sum_count, int& max_count){
    if(root == nullptr) 
        return 0;
    int sum = root->val + FindRootSum(root->left, sum_count,max_count) + FindRootSum(root->right, sum_count, max_count);
    sum_count[sum]++;
    max_count = max(max_count, sum_count[sum]);
    return sum;
}
vector<int> findFrequentTreeSum(TreeNode* root) {
    unordered_map<int, int> sum_count;
    int max_count = 0;
    FindRootSum(root, sum_count, max_count);
    vector<int> results;
    for(auto& iter: sum_count){
        if(iter.second == max_count){
            results.push_back(iter.first);
        }
    }
    return results;
}
```