---
title: "Introduction"
date: 2020-02-06T21:05:55-05:00
draft: true
pre: "1. "
weights: 1
---

#### Coding Style

几个技巧快速提高代码风格:

1. 二元运算符左右两边加空格
2. if, for 和括号之间加空格
3. 严格按照要求进行程序缩进
4. 即使 if / for 语句内部只有一句话，也要加上花括号
5. 变量名使用有意义的英文名，不要用a,b,c,s1,s2
6. 区分不同的逻辑块，逻辑块之间用空行隔开，简要注释每个部分做的事情
7. 多用 Helper Function 或子函数，不要所有程序都写在一个大函数里
8. [Python风格规范](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)

---

#### 贪心算法 Greedy Algorithms

面试中极少出现，因为一般是经典算法，或者需要严格的数学证明。
需要背诵的贪心算法：

- http://www.lintcode.com/problem/majority-number/
- http://www.lintcode.com/problem/create-maximum-number/
- http://www.lintcode.com/problem/jump-game-ii/
- http://www.lintcode.com/problem/jump-game/
- http://www.lintcode.com/problem/gas-station/
- http://www.lintcode.com/problem/delete-digits/
- http://www.lintcode.com/problem/task-scheduler/

---

#### 例题: 最长回文子串 Longest Palindromic Substring

题目 [LintCode 200](https://www.lintcode.com/problem/longest-palindromic-substring/description?_from=ladder&&fromId=1)

需要掌握的方法：

(1) 暴力Brute Force，需要的时间是`$O(n^3)$`。解析：遍历所有子串需要`$O(n^2)$`，每次判断是否是回文串`is_palindrome()`需要的时间是`$O(n)$`。

```python
class Solution:
    """
    @param s: input string
    @return: the longest palindromic substring
    """
    def longestPalindrome(self, s):
        # write your code here
        ls = len(s)

        if ls <= 1:
            return s
        
        for sub_len in range(ls, 0, -1):
            for i in range(ls - sub_len + 1):
                if self.is_palindromic(s[i:i+sub_len]):
                    return s[i:i+sub_len]

    # def is_palindromic(self, s):
    #     # take extra space O(n) comparasion, O(n) auxillary space
    #     return s == s[::-1]
        
    def is_palindromic(self, s):
        # in-place method, O(n) time complexity
        str_len = len(s)
        if str_len <= 1:
            return True
        
        for i in range(str_len // 2 + 1):
            if s[i] != s[-(i + 1)]:
                return False
        
        return True
```

(2) 从中心扩展的枚举法 Enumeration，需要的时间是`$O(n^2)$`。解析：这里的思路从”枚举判断“变成了从字符串中“生成”回文串。因此减少了每次判断是否为回文串的时间。从中心生成回文串总共有`2n - 1`个中心点（`n`个字符，以及`n - 1`个字符中心空位），以及`n / 2`次生成判断，所以总时间是`$O(n^2)$`。

```python
class Solution:
    """
    @param s: input string
    @return: the longest palindromic substring
    """
    def longestPalindrome(self, s):
        # write your code here
        ls = len(s)
        
        if ls <= 1:
            return s
        
        start_i = end_i = 0
        
        for i in range(ls):
            # character as symmetry axis
            len1 = self.expand_around_center(s, i, i)
            # space between characters as symmetry axis
            len2 = self.expand_around_center(s, i, i + 1)
        
            sub_len = max(len1, len2)
            print(i, len1, len2)
            if sub_len > end_i - start_i:
                # start index depends on whether sub_len is an odd number or even number, need to exclude one character represents s[i]
                start_i = i - (sub_len - 1) // 2
                # end index is always floor of half of the sub_len size
                end_i = i + sub_len // 2 

        return s[start_i:end_i + 1]


    def expand_around_center(self, s, left, right):
        str_len = len(s)
        
        while (left >= 0) and (right < str_len) and (s[left] == s[right]):
            right += 1
            left -= 1 
        
        # length of a string of left and right indices: right - left + 1
        # since the substring has been expanded by 2 characters, so the actual substring length is right - left  + 1 - 2
        return right - left - 1
```

(3) 基于动态规划 Dynamic Programming (DP)的解法，需要时间是`$O(n^2)$`，空间复杂度也为`$O(n^2)$`。解析：对于给定的字符串`s`，设一个判断字符是否为回文串的函数`lps(i,j)`。易知`lps(i,j) = lps(i + 1, j - 1) && s[i] == s[j]`，即首位字符相同且中间是回文串的字符为回文串。可以构造二维数组记录`lps(i,j)`的值。二位数组的初始值需要判断字符串为1位或为2位的情况（同解法2，对称轴可能在字符上，可能在字符间）。

```python
class Solution:
    """
    @param s: input string
    @return: the longest palindromic substring
    """
    def longestPalindrome(self, s):
        str_len = len(s)
    
        if str_len <= 1:
            return s
    
        lps = [[0] * str_len for _ in range(str_len)]
        max_sub_len = 1
        start_idx = 0
    
        for i in range(str_len):
            lps[i][i] = 1
    
        for i in range(str_len - 1):
            if s[i] == s[i + 1]:
                lps[i][i + 1] = 1
                max_sub_len = 2
                start_idx = i
    
        for i in range(str_len -1, -1, -1):
            for j in range(i + 2, str_len):
                lps[i][j] = int(lps[i + 1][j - 1] and s[i] == s[j])
                if lps[i][j] and j - i + 1 > max_sub_len:
                    max_sub_len = j - i + 1
                    start_idx = i
    
        return s[start_idx:start_idx + max_sub_len]
```


