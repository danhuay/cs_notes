---
title: "Merge Sort"
date: 2020-02-05T21:16:28-05:00
draft: true
tags:  ["sorting", "merge sort", "divide and conquer"]
weight: 2
---

Conceptually, a merge sort works as follows:

1. Divide the unsorted list into n sublists, each containing one element (a list of one element is considered sorted).
2. Repeatedly merge sublists to produce new sorted sublists until there is only one sublist remaining. This will be the sorted list.


Time Complexity:

Counts of comparasions `$n$` times steps (breakdowns) `$logn$`.

+ Average Case: `$O(nlogn)$`
+ Worst Case: `$O(nlogn)$`
+ Best Case: `$O(nlogn)$`

Space Complexity (Auxillary Space): `$O(n)$`

```python
# this method returns a new copy, instead of changing it in place.
def merge_sort(nums):
    if len(nums) <= 1:
        return nums

    mid = len(nums) // 2
    # recursively sort each half of the array
    left = merge_sort(nums[:mid])
    right = merge_sort(nums[mid:])

    ll = len(left)
    lr = len(right)
    merged = []
    l_i = r_i = 0

    while l_i < ll and r_i < lr:
        if left[l_i] < right[r_i]:
            merged.append(left[l_i])
            l_i += 1
        else:
            merged.append(right[r_i])
            r_i += 1

    merged = merged + left[l_i:] + right[r_i:]

    return merged

# 进行原位修改的方法
def merge_sort(self, A, start, end):
    if start >= end:
        return

    mid = (start + end) // 2

    self.merge_sort(A, start, mid)
    self.merge_sort(A, mid + 1, end)  # 需要mid + 1保证index不为负

    l, r, tmp = start, mid + 1, list()

    while l < mid + 1 and r < end + 1:
        if A[l] < A[r]:
            tmp.append(A[l])
            l += 1
        else:
            tmp.append(A[r])
            r += 1

    tmp = tmp + A[l:mid + 1] + A[r:end + 1]
    
    # 需要一一赋值进行原位修改
    for i, v in enumerate(tmp):
        A[start + i] = v

    return
```