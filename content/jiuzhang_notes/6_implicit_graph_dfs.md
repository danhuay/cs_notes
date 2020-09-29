---
title: "6_implicit_graph_dfs"
date: 2020-02-06T21:04:23-05:00
draft: true
---

#### Combination DFS

```python
# without duplicates
class Solution:
    """
    @param nums: A set of numbers
    @return: A list of lists
    """
    def subsets(self, nums):
        # write your code here
        if nums is None:
            return []
        
        nums.sort()
        results = []
        self.dfs(nums, 0, [], results)
        
        return results
    
    def dfs(self, nums, index, subset, results):
        results.append(list(subset))  # deep-copy
        
        for i in range(index, len(nums)):
            subset.append(nums[i])
            self.dfs(nums, i + 1, subset, results)
            subset.pop()

# with duplicates
class Solution:
    """
    @param nums: A set of numbers.
    @return: A list of lists. All valid subsets.
    """
    def subsetsWithDup(self, nums):
        # write your code here
        if nums is None:
            return []
        
        nums.sort()
        results = []
        self.dfs(nums, 0, [], results)
        return results
    
    def dfs(self, nums, index, subset, results):
        results.append(list(subset))
        
        for i in range(index, len(nums)):
            # 判断要点，不与前一个数相同，且不是这一轮的第一个数(i > index)
            if (i > 0) and (nums[i] == nums[i - 1]) and (i > index):
                continue
            subset.append(nums[i])
            self.dfs(nums, i + 1, subset, results)
            subset.pop()
```


#### Permutation DFS

```python
# without duplicates
class Solution:
    """
    @param: nums: A list of integers.
    @return: A list of permutations.
    """
    def permute(self, nums):
        # write your code here
        if nums is None:
            return []
        
        visited, results = [False] * len(nums), []
        self.dfs(nums, visited, [], results)
        return results
    
    def dfs(self, nums, visited, permutation, results):
        if len(permutation) == len(nums):
            results.append(list(permutation))
            return
        
        for i in range(len(nums)):
            if visited[i]:
                continue
            
            permutation.append(nums[i])
            visited[i] = True
            
            self.dfs(nums, visited, permutation, results)
            
            visited[i] = False
            permutation.pop()

# with duplicates
class Solution:
    """
    @param str: A string
    @return: all permutations
    """
    def stringPermutation2(self, string):
        # write your code here

        str_arr = sorted(list(string))
        visited, results = [False] * len(str_arr), []
        self.dfs(str_arr, visited, [], results)
        return results
    
    def dfs(self, str_arr, visited, permutations, results):
        if len(permutations) == len(str_arr):
            results.append(''.join(list(permutations)))
            return
        
        for i in range(len(str_arr)):
            if visited[i]:
                continue
            
            # 前一个字符没有visited的时候，这个字符不能使用
            if i > 0 and str_arr[i] == str_arr[i - 1] and not visited[i - 1]:
                continue
            
            permutations.append(str_arr[i])
            visited[i] = True
            self.dfs(str_arr, visited, permutations, results)
            visited[i] = False
            permutations.pop()
```