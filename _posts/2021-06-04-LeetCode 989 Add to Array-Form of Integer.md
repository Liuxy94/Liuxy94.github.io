---
title: "LeetCode 989 Add to Array-Form of Integer"
excerpt: 'leetcode'
toc: true
toc_sticky: true
categories: 
    - leetcode
tags:
    - array
    - sum of array
---

## Problem
The array-form of an integer num is an array representing its digits in left to right order.

* For example, for `num = 1321`, the array form is `[1,3,2,1]`.
Given `num`, the __array-form__ of an integer, and an integer `k`, return the __array-form__ of the integer `num + k`.




## My Solution

### code
```python
class Solution:
def addToArrayForm(self, num: List[int], k: int) -> List[int]:
    sum_list = []
    carry = 0 
    for i in range(len(num)):    
        res = k%10
        curr_sum = res + num[-(1+i)] + carry
        if curr_sum > 9:
        curr_sum = curr_sum%10
        carry = 1
        else:
        carry = 0
        sum_list.insert(0,curr_sum)
        k = k//10
    while k:
        res = k%10
        curr_sum = res + carry
        if curr_sum > 9:
        curr_sum = curr_sum%10
        carry = 1
        else:
        carry = 0
        sum_list.insert(0,curr_sum)
        k = k//10
    if carry:
        sum_list.insert(0, 1)
    return sum_list
```

### 思路
 1. 先遍历数组`num`， 利用carry 和整除 求余，计算干净 `num`。
 2. 如果遍历数组后， `k` 非零，继续将 `k` 转化为数组放到前面。
 3. 最后，如果 `carry`非零，再一次放到前面。

### 时间复杂度分析
$O(n)$ where $n= max(`len(nums)`, `len(k_array)`) $

### 空间复杂度分析
$O(n)$ where $n= max(`len(nums)`, `len(k_array)`) $

注意如果想转化为数字相加，注意浮点数问题。

## Better Solution
```python
class Solution: 
def addToArrayForm(self, A: List[int], K: int) -> List[int]:
    carry = 0
    for i in range(len(A) - 1, -1, -1):
        A[i], carry = (carry + A[i] + K % 10) % 10, (carry + A[i] + K % 10) // 10
        K //= 10
    B = []
    # 如果全部加完还有进位，需要特殊处理。 比如 A = [2], K = 998
    carry = carry + K
    while carry:
        B = [(carry) % 10] + B
        carry //= 10
    return B + A
```
### 思路

如果你没做出来这道题，不妨先试试 66. 加一，那道题是这道题的简化版，即 K = 1 的特殊形式。

这道题的思路是 从低位到高位计算，注意进位和边界处理。 细节都在代码里。

为了简化判断，我将 carry（进位） 和 K 进行了统一处理，即 carry = carry + K


### 复杂度分析
令 N 为数组长度。

时间复杂度：$O(N+max(0, K-N)^2) $
空间复杂度：$O(max(1,K−N))$

