# binary tree level order traversal

一般找最短用BFS，找所有用DFS。如果搜索树某一层节点特别多，适合DFS。如果搜索树很深，适合BFS

Q: 在一个有向图中，有拓扑序 是否完全等价于 无环？
A: 是的👍。反过来可以用拓扑排序检测是否有环

Q: 看到答案中用了PriorityQueue，请问这是必要的吗？ (Alien Dictionary)
A: 是的，因为如果存在多种，要返回字典序最小的一个



# 二分法

要点: 1. start + 1 < end        数组长度
2. start + (end+start) / 2     INT_MAX
3. start = mid // or start = mid + 1
4. end = mid // or end = mid - 1
5. 缩小范围 + 找答案

A:start<=end也是可以的嘛?
Q:不要用，这样很容易死循环！ # 在只有一个数字的时候会死循环


BFS需要总结的点：
什么时候需要记录见过的点？
什么时候需要分层？


# 二叉树上的分治法和遍历法

全局变量可以用数据结构存下来当做parameter传入helper吗
可以的，这样就可以避免使用全局变量了

面试的时候写分治好还是写遍历好？
都可以，看更熟悉哪种吧，不过面试官有可能要求都写

















