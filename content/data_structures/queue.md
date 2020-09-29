---
title: "Queue"
date: 2020-02-04T17:44:58-05:00
draft: true
tags: ["data structure", "queue"]
---

#### 什么是队列（Queue）
队列（queue）是一种采用先进先出（FIFO，first in first out）策略的抽象数据结构。比如生活中排队，总是按照先来的先服务，后来的后服务。队列在数据结构中举足轻重，其在算法中应用广泛，最常用的就是在**宽度优先搜索(BFS）**中，记录待扩展的节点。

队列内部存储元素的方式，一般有两种，数组（array）和链表（linked list）。两者的最主要区别是：

- 数组对随机访问有较好性能。
- 链表对插入和删除元素有较好性能。

Queue's are mentioned in [Python's documentation](https://docs.python.org/2/tutorial/datastructures.html#using-lists-as-queues). 
From a library called `collections`, you can import a package called `deque`. A deque is a double-ended queue, you can enqueue on either end.

Examine the code below:

```python
# add - 队尾追加元素
# poll - 弹出队首元素
# size - 返回队列长度
# empty - 判断队列为空

# Using collections.deque
class Queue:
    def __init__(self):
        from collections import deque
        self.queue = deque()

    def add(self, element):
        self.queue.append(element)

    def poll(self):
        return self.queue.popleft()

    def size(self):
        return len(self.queue)

    def empty(self):
        return self.size() == 0

# Using native list
class Queue:
    def __init__(self):
        self.queue = list()

    def add(self, element):
        self.queue.append(element)

    def poll(self):
        # slower than deque.popleft(), list will reconstruct
        return self.queue.pop(0)

    def size(self):
        return len(self.queue)

    def empty(self):
        return self.size() == 0
```


