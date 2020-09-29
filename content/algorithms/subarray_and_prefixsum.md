---
title: "Subarray and PrefixSum"
date: 2020-02-11T21:29:43-05:00
draft: true
---

#### Introduction

定义数组`A`的前缀和数组`PrefixSum`，其中`PrefixSum[i] = A[0] + A[1] + ... A[i]`，特别定义`PrefixSum[0] = 0`。对于长度为`n`的数组，可知构造`PrefixSum`需要耗费`O(n)`的时间和`O(n)`的空间。如需要计算子数组从下标`i`到下标`j`之间的所有数之和，则有：

```math
sum(i~j) = PrefixSum[j+1] - PrefixSum[i]
```

---

**例题**

1. [Maximum Subarray](#maximum-subarray)
2. [Minimum Subarray](#minimum-subarray)
3. [Subarray Sum](#subarray-sum)
4. [Subarray Sum Closest](#subarray-sum-closest)

#### Maximum Subarray

LintCode 41. [Maximum Subarray](https://www.lintcode.com/problem/maximum-subarray/description)


```python
class Solution:
    """
    @param nums: A list of integers
    @return: A integer indicate the sum of max subarray
    """
    def maxSubArray(self, nums):
        # write your code here
        if not nums:
            return
        
        prefixsum = self.construct_prefixsum(nums)
        
        max_sum = nums[0]
        min_presum = prefixsum[0]
        for i in range(1, len(nums)):
            min_presum = min(prefixsum[i], min_presum)
            max_sum = max(max_sum, prefixsum[i+1] - min_presum)
        
        return max_sum
    
    def construct_prefixsum(self, nums):
        prefixsum = [0]
        for i in nums:
            prefixsum.append(prefixsum[-1] + i)
        return prefixsum
```

```python
class Solution:
    """
    @param nums: A list of integers
    @return: A integer indicate the sum of max subarray
    """
    def maxSubArray(self, nums):
        # write your code here
        if not nums:
            return
        
        max_sum = nums[0]
        min_presum = last_prefixsum = 0
        
        for n in nums:
            min_presum = min(min_presum, last_prefixsum)
            last_prefixsum += n
            max_sum = max(max_sum, last_prefixsum - min_presum)

        return max_sum
```

#### Minimum Subarray

LintCode 44. [Minimum Subarray](https://www.lintcode.com/problem/minimum-subarray/description)

```python
class Solution:
    """
    @param: nums: a list of integers
    @return: A integer indicate the sum of minimum subarray
    """
    def minSubArray(self, nums):
        # write your code here
        
        if not nums:
            return
        
        max_presum = 0
        last_presum = 0
        min_subsum = nums[0]
        
        for n in nums:
            max_presum = max(max_presum, last_presum)
            last_presum += n
            min_subsum = min(min_subsum, last_presum - max_presum)
        
        return min_subsum
```

#### Subarray Sum

LintCode 138. [Subarray Sum](https://www.lintcode.com/problem/subarray-sum/description)

```python
class Solution:
    """
    @param nums: A list of integers
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def subarraySum(self, nums):
        # write your code here
        prefixsum = self.prefixsum(nums)
        
        for i in range(len(prefixsum) - 1):
            for j in range(i, len(prefixsum)):
                if prefixsum[j] - prefixsum[i] == 0:
                    return [i, j - 1]
        return
        
    def prefixsum(self, nums):
        prefixsum = [0]
        for n in nums:
            prefixsum.append(prefixsum[-1] + n)
        return prefixsum
```

```python
class Solution:
    """
    @param nums: A list of integers
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def subarraySum(self, nums):
        # write your code here
        prefixsum = self.prefixsum(nums)
        
        result_idx = dict()
        for i, n in enumerate(prefixsum):
            result_idx[n] = result_idx.get(n, []) + [i]
            if len(result_idx[n]) == 2:
                return [min(result_idx[n]), max(result_idx[n]) - 1]
        return   
    
    def prefixsum(self, nums):
        prefixsum = [0]
        for n in nums:
            prefixsum.append(prefixsum[-1] + n)
        return prefixsum
```

#### Subarray Sum Closest

LintCode 139. [Subarray Sum Closest](https://www.lintcode.com/problem/subarray-sum-closest/description)

```python
class Solution:
    """
    @param: nums: A list of integers
    @return: A list of integers includes the index of the first number and the index of the last number
    """
    def subarraySumClosest(self, nums):
        import sys
        prefixsum = self.prefixsum(nums)
        prefixsum.sort(key=lambda x: x[1]) # sort by prefixsum values
        
        closest_value = sys.maxsize
        for i in range(len(prefixsum) - 1):
            if closest_value > prefixsum[i + 1][1] - prefixsum[i][1]:
                closest_value = prefixsum[i + 1][1] - prefixsum[i][1]
                left = min(prefixsum[i + 1][0], prefixsum[i][0]) 
                right = max(prefixsum[i + 1][0], prefixsum[i][0]) - 1
        return [left, right]
        
    def prefixsum(self, nums):
        prefixsum = [(0, 0)]
        for i, n in enumerate(nums):
            prefixsum.append((i + 1, prefixsum[-1][1] + n))
        return prefixsum
```

