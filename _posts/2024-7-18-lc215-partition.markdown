---
layout: post
title: Partition and Min-Heap -- leetcode215
date: 2024-07-18 11:37:14 +08:00
description: # Add post description (optional)
img: # Add image post (optional)
tags: [Alogorithm, Leetcode] # add tag
---

## 题干
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

示例 1:

输入: [3,2,1,5,6,4], k = 2

输出: 5

示例 2:

输入: [3,2,3,1,2,4,5,5,6], k = 4

输出: 4

## 题解1——快排变式-partition


## 题解2——小顶堆/优先队列
$$ Topk $$ 的问题一般都要会用到堆

复杂度为 $$ O(n \log k) $$