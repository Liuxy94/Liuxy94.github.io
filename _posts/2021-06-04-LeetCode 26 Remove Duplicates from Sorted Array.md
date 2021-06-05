---
title: "LeetCode 26 Remove Duplicates from Sorted Array"
excerpt: 'leetcode'
toc: true
toc_sticky: true
categories: 
    - leetcode
tags:
    - array
    -sorted array
---

## Problem
Given a sorted array nums, remove the duplicates in-place such that each element appears only once and returns the new length.

Do not allocate extra space for another array, you must do this by __modifying the input array in-place__ with O(1) extra memory.


## My Solution

### code
```python

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) < 2:
            return len(nums)
        i = 0
        while i < len(nums)-1:
            if nums[i] == nums[i+1]:
                nums.pop(i+1)
            else:
                i += 1
        return len(nums)
```

### 思路
依次比较current和current+1，重复的直接pop掉。

### 时间复杂度分析
$O(n)$ where $n=`len(nums)`$.

### 空间复杂度分析
$O(1)$ since the operation is in place

## Better Solution
```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        i = 0
        for j in range(len(nums)):
            if nums[j] != nums[i]:
                i += 1
                nums[i] = nums[j]
        return i + 1
```
对比的话，这个快慢指针的判断比较简洁 前$i+1$个都是需要的。
