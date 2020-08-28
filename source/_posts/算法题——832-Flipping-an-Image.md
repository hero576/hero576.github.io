title: 算法题——832 Flipping an Image
author: hero576
tags:
  - 算法题
categories:
  - 数据结构与算法
date: 2018-02-13 17:15:00
---
## 题目要求
Given a binary matrix A, we want to flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.  For example, flipping [1, 1, 0] horizontally results in [0, 1, 1].

To invert an image means that each 0 is replaced by 1, and each 1 is replaced by 0. For example, inverting [0, 1, 1] results in [1, 0, 0].
**Example 1:**
```python
Input: [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]
```
**Example 2:**
```python
Input: [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
```
难度级别：简单

这个题比较简单，用列表的方法就可以了。我的答案：
```python
class Solution:
    def flipAndInvertImage(self, A):
        for line in A:
            line.reverse()
            for i,val in enumerate(line):
                line[i]=0 if val==1 else 1
        return A
```


