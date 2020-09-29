---
title: "Bubble Sort"
date: 2020-02-05T21:16:08-05:00
draft: true
tags:  ["sorting", "bubble sort"]
weight: 1
---

Bubble sort, sometimes referred to as sinking sort, is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. The algorithm, which is a comparison sort, is named for the way smaller or larger elements "bubble" to the top of the list. This simple algorithm performs poorly in real world use and is used primarily as an educational tool. 

Time Complexity:

+ Average Case: `$O(n^2)$`
+ Worst Case: `$O(n^2)$`
+ Best Case: `$O(n)$`

Space Complexity (Auxillary Space): `$O(1)$`

```python
def bubble_sort(nums):
    if len(nums) <= 1:
        return nums

    for loop_round in range(1, len(nums)):
        swaps = False
        for i in range(len(nums) - loop_round):
            j = i + 1
            if nums[i] > nums[j]:
                nums[j], nums[i] = nums[i], nums[j]
                swaps = True

            # if already sorted, break
            if not swaps:
                break

    return nums
```