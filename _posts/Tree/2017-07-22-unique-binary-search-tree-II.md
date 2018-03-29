---
layout: post
title: 计算不同二叉搜索树II
category: 二叉树
tags: Tree LeetCode BST
Keywords: Tree BST
description:
---
## 1. LeetCode 95. [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/description/)
## 2. 描述：
给定整数n, 生成所有可以存储值 1 . . . n的BST，并返回这些BST
## 3. 示例
n = 3时，存在以下5个unique BST:
```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
## 4. 解题思路：
假设已知可以存储 1...n-1的所有BST，那么将 节点n插入这些BST就可以组成存储 1。。。n的BST，插入情况如下：
1. 添加到根节点：将新节点n作为根节点，原根节点作为其左子节点
2. 插入到右子节点：如果节点存在右子节点，则将新增节点作为其右子节点，原右子节点作为新节点的左子节点
3. 添加到右叶节点：如果到达叶节点，则将新增节点n作为其右子节点
## 5. 解决方案：
``` c++
TreeNode* Clone(TreeNode* root){
    if(root == nullptr){
        return nullptr;
    }
    
    TreeNode* new_root = new TreeNode(root->val);
    new_root->left = Clone(root->left);
    new_root->right = Clone(root->right);
    return new_root;
}
vector<TreeNode*> generateTrees(int n) {
    if(n == 0){
        return vector<TreeNode*>();
    }
    vector<TreeNode*> res(1, nullptr);
    
    for(int i = 1; i <= n; i++){
        vector<TreeNode*> tmp;
        
        for(int j = 0; j < res.size(); j++){
            TreeNode* old_root = res[j];
            TreeNode* new_root = new TreeNode(i);
            TreeNode* cloned_root = Clone(old_root);
            new_root->left = cloned_root;
            tmp.push_back(new_root);
            
            if(old_root != nullptr){
                TreeNode* cur_node = old_root;
                while(cur_node->right != nullptr){
                    TreeNode* added_node = new TreeNode(i);
                    TreeNode* right_node = cur_node->right;
                    cur_node->right = added_node;
                    added_node->left = right_node;
                    TreeNode* target = Clone(old_root);
                    tmp.push_back(target);
                
                    cur_node->right = right_node;
                    cur_node = cur_node->right;
                }
            
                cur_node->right = new TreeNode(i);
                TreeNode* target = Clone(old_root);
                tmp.push_back(target);
                cur_node->right = nullptr;
            }
            
        }
        res = tmp;
    }
    return res;
}
```