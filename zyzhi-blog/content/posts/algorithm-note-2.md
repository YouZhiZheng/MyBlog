---
title: 543. 二叉树的直径
subtitle:
date: 2024-07-25T21:27:01+08:00
slug: b92556f
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: 使用后序遍历计算树的直径
keywords: 后序遍历, 二叉树
license:
comment: true
weight: 0
tags:
  - 二叉树
  - 后序遍历
categories:
  - 算法
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRss: false
hiddenFromRelated: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: true
lightgallery: force
password:
message:
repost:
  enable: false
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---
**题目描述([力扣](https://leetcode.cn/problems/diameter-of-binary-tree/)):**  

![图1](/PostsImgs/algorithm_note_2_imgs/picture1.png)

**思路:** 首先理解二叉树的直径指的是二叉树中任意两个节点之间的路径长度的最大值(**最长直径不一定经过根节点**)。想要找到二叉树的直径，就需要遍历每个节点，依次计算每个节点的左右子树深度之和，其中的最大值就是二叉树的直径。因为二叉树的直径大多数情况下就是根节点下的左右子树深度之和，但存在直径不经过根节点的情况，所以需要遍历每个节点，计算以该节点为根的二叉树的直径，然后取计算结果中的最大值。根据前面的分析，**我们需要计算左右子树深度之和，这就需要左右子树的信息，所以选择后序遍历**。


**代码:**
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root)
    {
        maxDepth(root);
        return maxDia;
    }

    int maxDepth(TreeNode* root)
    {
        if(root == nullptr) return 0;

        int lefth_h = maxDepth(root->left);
        int right_h = maxDepth(root->right);
        int Dia = lefth_h + right_h; //计算当前二叉树的直径
        maxDia = max(maxDia, Dia);

        return lefth_h > right_h? lefth_h + 1: right_h + 1; //返回子树高度
    }
private:
    int maxDia = -1; //用于存储二叉树直径
};
```