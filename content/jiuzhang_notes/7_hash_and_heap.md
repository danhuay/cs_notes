---
title: "7_hash_and_heap"
date: 2020-02-06T21:04:48-05:00
draft: true
---


#### Hash
- Open Hashing（常见）
	用Linked-List来储存冲突
- Close Hashing （工业界很少用）
	将冲突放入下一位
- Rehashing
	在当前size被占用10%～50%的时候，对哈希表进行扩容。
	需要将已有的数值遍历重新赋予哈希值。

#### Heap

Heap的特点：

- `$O(logn)$`的时间内`add(element)`
- `$O(logn)$`的时间内`poll()`删除元素
- `$O(1)$`的时间内求`min/max`

Heap是一个二叉树：

结构特性：从上到下，从左到右依次添加节点。
值特性：如果是minHeap，父亲节点 <= 儿子节点，maxHeap相反。和二叉搜索树不同，两个儿子节点之间没有大小关系。

堆的插入操作（minHeap为例）：

1. 将新的值放入最下层最左边的空位（保证结构特性）。
2. 将新插入的值和父亲节点调换，直至保证值特性 - Sift Up。 

堆的删除操作`poll`：

1. 先将要删除的点的值和顶点互换
2. 再通过调换保证值特性（和小的一边互换）
3. 普通`delete/remove`操作是`$O(n)$`复杂度。
4. 如果要支持快速删除，需要Hashmap存储index。

堆的实现：

用普通数组array即可。在array[0]存储当前heap内的值的个数。
当前节点：`k`
父节点：`k // 2`
左节点：`k * 2`
右节点：`k * 2 + 1`