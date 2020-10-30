title: 算法题——833 Replace in String
author: hero576
tags:
  - 算法题
categories:
  - 数据结构与算法
date: 2018-02-13 18:52:00
---
> 
<!-- more -->


## 题目要求
To some string S, we will perform some replacement operations that replace groups of letters with new ones (not necessarily the same size).

Each replacement operation has 3 parameters: a starting index i, a source word x and a target word y.  The rule is that if x starts at position i in the original string S, then we will replace that occurrence of x with y.  If not, we do nothing.

For example, if we have S = "abcd" and we have some replacement operation i = 2, x = "cd", y = "ffff", then because "cd" starts at position 2 in the original string S, we will replace it with "ffff".

Using another example on S = "abcd", if we have both the replacement operation i = 0, x = "ab", y = "eee", as well as another replacement operation i = 2, x = "ec", y = "ffff", this second operation does nothing because in the original string S[2] = 'c', which doesn't match x[0] = 'e'.

All these operations occur simultaneously.  It's guaranteed that there won't be any overlap in replacement: for example, S = "abc", indexes = [0, 1], sources = ["ab","bc"] is not a valid test case.

**Example 1:**
```python
Input: S = "abcd", indexes = [0,2], sources = ["a","cd"], targets = ["eee","ffff"]
Output: "eeebffff"
Explanation: "a" starts at index 0 in S, so it's replaced by "eee".
"cd" starts at index 2 in S, so it's replaced by "ffff".
```
**Example 2:**
```python
Input: S = "abcd", indexes = [0,2], sources = ["ab","ec"], targets = ["eee","ffff"]
Output: "eeecd"
Explanation: "ab" starts at index 0 in S, so it's replaced by "eee". 
"ec" doesn't starts at index 2 in the original S, so we do nothing.
```
题目难度 Medium  
主要是更新完,字符串长度就变化了，一开始用指针，根据上一次的变化求索引，但是indexex不是按照顺序的，一直不通过。后来借助了列表，把索引先存起来，每次替换的时候再查找新的索引值。
```python
class Solution:
    def findReplaceString(self, S, indexes, sources, targets):
        change=0
        id_r=0  #定义待输出字符串指针
        result=S
        id_list=[i for i in range(len(S))]
        
        for i,index in enumerate(indexes):
            id_r=id_list.index(index) #初始化待输出字符串指针
            s1=sources[i]
            is_change=True
            for j,sor in enumerate(s1):
                if not sor==S[index+j]:
                    is_change=False
            if is_change:
                result=result[:id_r]+targets[i]+result[id_r+len(sources[i]):]
                for x in range(len(sources[i])):
                    id_list.pop(id_r)
                for x in range(len(targets[i])):
                    id_list.insert(id_r,-1)
        return result
```