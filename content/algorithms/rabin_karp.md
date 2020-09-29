---
title: "Rabin Karp Algorithm"
date: 2020-02-06T21:41:37-05:00
draft: true
---

**From [WikiPedia](https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm):**

In computer science, the **Rabin–Karp algorithm** or Karp–Rabin algorithm is a **string-searching** algorithm created by Richard M. Karp and Michael O. Rabin (1987) that uses hashing to find an exact match of a pattern string in a text. It uses a **rolling hash** to quickly filter out positions of the text that cannot match the pattern, and then checks for a match at the remaining positions. Generalizations of the same idea can be used to find more than one match of a single pattern, or to find matches for more than one pattern.

To find a single match of a single pattern, the expected time of the algorithm is **linear in the combined length of the pattern and text**, although its worst-case time complexity is the product of the two lengths. To find multiple matches, the expected time is linear in the input lengths, plus the combined length of all the matches, which could be greater than linear.

**TL;NR:**

Rabin-Karp 算法是一种Rolling Hash的方法。
若文本长度为n，目标字符串长度为m，一般情况下时间复杂度为`$O(n+m)$`, 最差情况（当哈希函数选择不当）下时间复杂度为`$O(n*m)$`。
如果对每一段字符串取哈希值，时间复杂度为`$O(m*n)$`，因为每次取哈希值都为`$O(m)$`。但是考虑到每次挪动一位字符，中间的m-1位字符和上次运算相同，因此可以在`$O(1)$`时间内计算每一次的新哈希值，可以只用`$O(n)$`的时间的时间来遍历文本。
最后需要遍历目标字符串来最终确定找到字符串，时间为`$O(m)$`。


---

**例题 strStr**

LintCode 594. [strStr II](https://www.lintcode.com/problem/strstr-ii/description)

```python
def strStr2(source, target):
    # edge case
    if (source is None) or (target is None):
        return -1

    if len(target) == 0:
        return 0

    n = len(source)
    m = len(target)
    hash_exp = 31
    hash_base = 10**6
    # power will be used in below rolling hash calculation
    power = hash_exp ** m % hash_base

    # calculate hash value of the target O(m)
    target_hash = 0
    for char in target:
        target_hash = (target_hash * hash_exp + ord(char)) % hash_base

    source_hash = 0
    for i in range(n):

        # add the new digits to the hash e.g. abc + d
        source_hash = (source_hash * hash_exp + ord(source[i])) % hash_base

        # not until reaching the length of m, keep adding
        if i < m - 1:
            continue

        # after the 
        if i >= m:
            # abcd - a, if value less than 0, add hash_base to guarantee positive
            source_hash = (source_hash - ord(source[i-m]) * power) % hash_base
            if source_hash < 0:
                source_hash += hash_base

        # check value
        if source_hash == target_hash:
            if source[i-m+1:i+1] == target:
                return i-m+1
    return -1
```

Another implementation:

```python
def hash_function(string, hash_exp, hash_base):
    hash_value = 0
    for char in string:
        hash_value = (hash_value * hash_exp + ord(char)) % hash_base
    return hash_value


def rolling_hash(orig_hash, char_add, char_remove, power, hash_exp, hash_base):
    # add char_add
    hash_value = (orig_hash * hash_exp + ord(char_add)) % hash_base
    # remove char_remove
    hash_value = (hash_value - ord(char_remove) * power) % hash_base
    return hash_value


def strStr2(source, target):
    # edge cases
    if (source is None) or (target is None):
        return -1

    if len(target) == 0:
        return 0

    n = len(source)
    m = len(target)
    hash_exp = 31
    hash_base = 10**6
    power = hash_exp ** m % hash_base

    target_hash = hash_function(target, hash_exp, hash_base)

    for i in range(n-m+1):
        if i == 0:
            source_hash = hash_function(source[:m], hash_exp, hash_base)
        else:
            char_add = source[i+m-1]
            char_remove = source[i-1]
            source_hash = rolling_hash(source_hash, char_add, char_remove,
                                       power, hash_exp, hash_base)
        if source_hash == target_hash:
            if source[i:i+m] == target:
                return i
    return -1
```