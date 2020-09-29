---
title: "3_binary_search"
date: 2020-02-06T21:03:35-05:00
draft: true
pre: "3. "
weights: 3
tags: ["binary search", "O(logn)"]
---

#### 二分法的通用模版：

```python
# find first occurance
def binarySearch(self, nums, target):

    if not nums:
        return -1
        
    start, end = 0, len(nums) - 1
    
    while (start + 1) < end:
        mid = start + (end - start) // 2
        
        if nums[mid] == target:
            end = mid
        elif nums[mid] > target:
            end = mid
        else:
            start = mid
    
    if nums[start] == target:
        return start
    
    if nums[end] == target:
        return end
    
    return -1

# find last occurance
def binarySearch(self, nums, target):

    if not nums:
        return -1
        
    start, end = 0, len(nums) - 1
    
    while (start + 1) < end:
        mid = start + (end - start) // 2
        
        if nums[mid] == target:
            start = mid
        elif nums[mid] > target:
            end = mid
        else:
            start = mid
  
    if nums[end] == target:
        return end

    if nums[start] == target:
        return start
    
    return -1
```