 ---
title: "Quick Sort"
date: 2020-02-05T21:16:36-05:00
draft: true
tags:  ["sorting", "quick sort", "divide and conquer"]
weight: 3
---

Like Merge Sort, QuickSort is a Divide and Conquer algorithm. It picks an element as pivot and partitions the given array around the picked pivot. When traversing the array, we will move an element larger than the pivot to the right of the pivot and leave smaller ones to the left.


Time Complexity:

Counts of comparasions `$n$` times (the **conquer** step) # of steps (breakdowns, the **divide** process) `$logn$`.

+ Average Case: `$O(nlogn)$`
+ Worst Case: `$O(n^2)$`
+ Best Case: `$O(nlogn)$`

Space Complexity (Auxillary Space): `$O(1)$`

```python
def quick_sort(A, start, end):
    if start >= end:
        return
    
    l, r = start, end
    pivot = A[(l + r) // 2]
    
    while l <= r:
        while l <= r and A[l] < pivot:
            l += 1 
        while l <= r and A[r] > pivot:
            r -= 1 
        if l <= r:
            A[l], A[r] = A[r], A[l]
            l += 1 
            r -= 1 
    
    quick_sort(A, start, r)
    quick_sort(A, l, end)
    
    return
```