---
title: "LeetCode 35 Search Insert Position"
excerpt: 'leetcode'
toc: true
toc_sticky: true
categories: 
    - leetcode
tags:
    - array
    - insert
---

## Problem
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with $O(log n)$ runtime complexity.




## My Solution

### code
```python
class Solution:
def searchInsert(self, nums: List[int], target: int) -> int:
    if target < nums[0]:
        return 0
    elif target > nums[-1]:
        return len(nums)
    left = 0
    right = len(nums) - 1
    mid = left + (right - left) //2
    while mid < right and left < mid:
        if target == nums[mid]:
            return mid
        elif target < nums[mid]:
            right = mid
            mid = left + (right - left) //2
        else:
            left = mid
            mid = left + (right - left) //2
    if target == nums[mid]:
        return mid
    elif nums[left] < nums[mid]:
        return left + 1
    else:
        return mid + 1
```

### 思路
1. 先避免Conner case 如果target 比最左边的还小 或者 最右边的还大 直接return 0 或者 len(nums)
2. 确定target在nums中后，按照二分法去逼近target。在while循环结束后，可以保证target在[left, right]内，且区间长度为2。
3. 如果  `target == nums[mid]`，则直接return mid。 利用mid来判断target是被夹在left和mid间 还是 mid和right间。
* left < mid: target 插入 mid (left+1)；
* mid < right: target 插入 mid+1（right）。

### 时间复杂度分析
$O(log(n))$ where $n= `len(nums)` $

### 空间复杂度分析
$O(1)$


