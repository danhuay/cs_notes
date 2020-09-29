---
title: "Breadth First Search"
date: 2020-02-06T21:03:08-05:00
draft: true
pre: "2. "
weights: 2
tags: ["bfs", "queue"]
---

#### Breadth First Search (BFS) 宽度优先搜索

#### 先修内容

- 什么是队列，如何自己实现一个[队列]({{<ref "data_structures/queue.md">}})
- LinkedList 和 Queue 之间的关系是什么？
    - [LinkedList]({{<ref "data_structures/linked_list.md">}})
- 什么是拓扑排序 Topological Sorting
- 如何定义一个图的数据结构

---

#### 什么时候用BFS

**图的遍历 Traversal in Graph**

比如给出无向连通图(Undirected Connected Graph)中的一个点，找到这个图里的所有点。这就是一个常见的场景。
LintCode 上的 [Clone Graph](https://www.lintcode.com/problem/clone-graph/description) 就是一个典型的练习题。
更细一点的划分的话，这一类的问题还可以分为：

1. 层级遍历 Level Order Traversal: 也就是说我不仅仅需要知道从一个点出发可以到达哪些点，还需要知道这些点，分别离出发点是第几层遇到的，比如 [Binary Tree Level Order Traversal](https://www.lintcode.com/problem/binary-tree-level-order-traversal/description) 就是一个典型的练习题。
2. 由点及面 Connected Component
3. 拓扑排序 Topological Sorting

**最短路径 Shortest Path in Simple Graph**

最短路径算法有很多种，BFS 是其中一种，但是他有特殊的使用场景，即必须是在简单图中求最短路径。
大部分简单图中使用 BFS 算法时，都是无向图。当然也有可能是有向图，但是在面试中极少会出现。(简单图 Simple Graph，即，图中每条边长度都是1（或边长都相同）。)

最短路径模版：

要点：

1. 每当一个点加入`queue`，就在`seen`里面记录。 
2. 判断条件：是否到达目标？(简单图适用) -> （对于每个下一步） 是否越界？ -> 是否已经访问过？
3. 在计算距离和层级的时候，需要加一层循环保证distance的大小。如果直接在`seen`字典里面记录距离则不需要。

```python
# on a matrix
from collections import deque

def shortest_path(grid, source, destination):
    # edge cases
    if not grid or not grid[0]:
        return -1  # any default return values

    seen = set()
    shortest_path = bfs(source, destination, grid, seen)
    return shortest_path

def bfs(source, destination, grid, seen):
    direction_x = [1, -1, 0, 0]
    direction_y = [0, 0, 1, -1]

    x, y = source.x, source.y
    queue = deque([(x, y)])
    seen.add((x, y))
    distance = 0

    while queue:
        for _ in range(len(queue)):  # guarantee levels
            x, y = queue.popleft()

            if (x, y) == (destination.x, destination.y):
                return distance

            for dx, dy in zip(direction_x, direction_y):
                next_x, next_y = x + dx, y + dy 

                if not is_valid(next_x, next_y, grid):
                    continue

                if (next_x, next_y) in seen:
                    continue

                queue.append((next_x, next_y))
                seen.add((next_x, next_y))

        # level of
        distance += 1


def is_valid(x, y, grid):
    # x is the first element, bound to rows
    # y is the second element, bound to cols
    r, c = len(grid), len(grid[0])
    if (0 <= x) and (x < r) and (0 <= y) and (y < c):
        if grid[x][y]:  # can be replaced by any other statement:
            return True
    return False
```

**拓扑排序 Topological Sort**

只有Directed-Acyclic-Graph (DAG)才有拓扑排序。每个node有入度in-degrees的概念来表示有多少dependencies。

拓扑排序模版：

要点：

1. 入度degrees存储着当前node的入度值
2. edges存储着node的所有后续nodes（后续课程）
3. 每次把入度为0的点同时放入queue（不能保证顺序）

```python
from collections import deque

def findOrder(self, numCourses, prerequisites):
    """
    @param: numCourses: a total of n courses
    @param: prerequisites: a list of prerequisite pairs
    @return: the course order

    e.g.
    numCourses = 4
    prerequisites = [[1,0],[2,0],[3,1],[3,2]]  # 0 -> 1
    """    
    if numCourses == 0:
        return []
    
    # Initialize in-degree and edges dict
    degrees = {n: 0 for n in range(numCourses)}
    edges = {n: [] for n in range(numCourses)}
    course_order = []
    
    for to_course, from_course in prerequisites:
        degrees[to_course] += 1
        edges[from_course].append(to_course) # edges follow the arrow direction
        
    # put in-degree as 0's courses to queue
    queue = deque([k for k, v in degrees.items() if v == 0])
    
    while queue:
        curr_course = queue.popleft()
        # remove 0s
        degrees.pop(curr_course)
        course_order.append(curr_course)
        
        # all connected points degrees -1
        for next_course in edges[curr_course]:
            degrees[next_course] -= 1
            if degrees[next_course] == 0:
                queue.append(next_course)
    
    # if len(degrees) != 0 means it's not a DAG, exists a circle
    if len(degrees) == 0:
        return course_order
    else:
        return []
```













