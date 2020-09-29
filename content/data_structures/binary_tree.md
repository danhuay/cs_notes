---
title: "Binary Tree"
date: 2020-02-05T21:23:14-05:00
draft: true
---

#### Binary Search Tree

二叉搜索树（Binary Search Tree，又名排序二叉树，二叉查找树，通常简写为BST）定义如下：
空树或是具有下列性质的二叉树：

1. 若左子树不空，则左子树上所有节点值均小于或等于它的根节点值；
2. 若右子树不空，则右子树上所有节点值均大于根节点值；
3. 左、右子树也为二叉搜索树；

特性：

1. 按照中序遍历（in-order traversal）打印各节点，会得到由小到大的顺序。
2. 在BST中搜索某值的平均情况下复杂度为`$O(logN)$`，最坏情况下复杂度为`$O(N)$`，其中`N`为节点个数。将待寻值与节点值比较，若不相等，则通过是小于还是大于，可断定该值只可能在左子树还是右子树，继续向该子树搜索。
3. 在**平衡**BST中查找某值的时间复杂度为`$O(logN)$`。（平衡二叉树：左右子树高度差绝对值不超过1且左右子树都是平衡二叉树，或者空树。)
4. 插入新节点，保持BST特性平均情况下要耗时`$O(logN)$`。(对比有序数组，插入新节点需要耗时`$O(N)`)

#### Summary of BFS and DFS on a Binary Tree

```
    1
   / \
  2   3
 / \ 
4   5
```

##### 1. BFS, use **queue** (first in, first out, FIFO), use deque, append, popleft
1-2-3-4-5. Also known as **level-order**.

```
[1]
1 -> [2, 3]
2 -> [3, 4, 5]
3 -> [4, 5]
4 -> [5]
5 -> []
```

##### 2. DFS, use **stack** (first in, last out, FILO), regular list, append, pop

怎么理解先/中/后序遍历：指的是记录root的顺序。先序/先根表示先记录root，再左右；中序/中根表示先左，再记录root（root在中间），最后再右。以此类推。

1) 中序遍历 Inorder (Left, Root, Right) : 4-2-5-1-3

```python
# recursive
def traverse(root, result):
    if not root:
        return
    traverse(root.left, result)
    result.append(root.val) # 注意访问根节点放到了遍历左子树的后面
    traverse(root.right, result)

# Iterative
class Solution:
    """
    @param root: A Tree
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        stack = []
        result = []

        while root:
            stack.append(root)
            root = root.left

        while len(stack) > 0:
            node = stack[-1]
            result.append(node.val)

            if not node.right:
                node = stack.pop()
                while len(stack) > 0 and stack[-1].right == node:
                    node = stack.pop()
            else:
                node = node.right
                while node:
                    stack.append(node)
                    node = node.left
        
        return result
```

2) 先序遍历 Preorder (Root, Left, Right) : 1-2-4-5-3

```python
# recursive
# 将根作为root，空list作为result传入，即可得到整棵树的遍历结果
def traverse(root, result):
    if not root:
        return
    result.append(root.val)
    traverse(root.left, result)
    traverse(root.right, result)

# Iterative
class Solution:
    """
    @param root: A Tree
    @return: Preorder in ArrayList which contains node values.
    """
    def preorderTraversal(self, root):
        stack = []
        preorder = []

        if not root:
            return preorder

        stack.append(root)
        while len(stack) > 0:
            node = stack.pop()
            preorder.append(node.val)
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        
        return preorder
```

3) 后序遍历 Postorder (Left, Right, Root) : 4-5-2-3-1

```python
# recursive
def traverse(root, result):
    if not root:
        return
    traverse(root.left, result)
    traverse(root.right, result)
    result.append(root.val) # 注意访问根节点放到了最后

# Iterative
class Solution:
    """
    @param root: A Tree
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        result = []
        stack = []
        prev, curr = None, root

        if not root:
            return result

        stack.append(root)
        while len(stack) > 0:
            curr = stack[-1]
            if not prev or prev.left == curr or prev.right == curr:  # traverse down the tree
                if curr.left:
                    stack.append(curr.left)
                elif curr.right:
                    stack.append(curr.right)
            elif curr.left == prev:  # traverse up the tree from the left
                if curr.right:
                    stack.append(curr.right)
            else:  # traverse up the tree from the right
                result.append(curr.val)
                stack.pop()
            prev = curr

        return result
```