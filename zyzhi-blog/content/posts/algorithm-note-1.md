---
title: 86. 分隔链表
subtitle:
date: 2024-07-22T21:19:02+08:00
slug: 6fc9142
draft: false
author:
  name: zyz
  link:
  email:
  avatar:
description: 使用双指针进行单链表的分解
keywords: 双指针, 链表
license:
comment: true
weight: 0
tags:
  - 双指针
  - 链表
categories:
  - 算法笔记
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
**题目描述([力扣](https://leetcode.cn/problems/partition-list/)):**  

![图1](/algorithm_note_1_imgs/picture1.png)

**思路:** 创造两个指针，分别指向大于x的链表和小于x的链表，然后依次遍历初始链表的每个节点进行判断将其添加到对应的新链表中，最后将两个链接进行连接后返回。


**代码:**
```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x)
    {
        ListNode *h1 = new ListNode(-1); //指向小于x的节点组成的链表的头节点
        ListNode *h2 = new ListNode(-1); //指向大于x的节点组成的链表的头节点
        ListNode *t1, *t2; //分别指向各自链表的尾结点
        t1 = h1;
        t2 = h2;

        while(head != nullptr) //遍历每个节点，与x进行比较
        {
            if(head->val < x)
            {
                t1->next = head;
                t1 = head;
            }
            else
            {
                t2->next = head;
                t2 = head;
            }
            head = head->next;
        }

        t1->next = h2->next; //将两个链表进行连接
        t2->next = nullptr;
        return h1->next;
    }
};
```
