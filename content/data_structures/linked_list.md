---
title: "Linked List"
date: 2020-02-04T17:33:03-05:00
draft: true
tags: ["data structures", "linked list"]
---

#### LinkedList 和 List 的对比：
- 对于随机访问的`get`和`set`，List有绝对优势；
- 对于频繁新增/删除操作`add`和`remove`， LinkedList比较占优，因为只需要移动指针，而List需要移动数据。


#### How to build linked list in python:

```python
class Element(object):
    def __init__(self, value):
        self.value = value
        self.next = None
        
class LinkedList(object):
    def __init__(self, head=None):
        self.head = head
        
    def append(self, new_element):
        current = self.head
        if self.head:
            while current.next:
                current = current.next
            current.next = new_element
        else:
            self.head = new_element
            
    def get_position(self, position):
        """Get an element from a particular position.
        Assume the first position is "1".
        Return "None" if position is not in the list."""
        curr_pos = 1
        curr_node = self.head
        while curr_node and curr_pos <= position:
            if curr_pos == position:
                return curr_node
            curr_node = curr_node.next
            curr_pos += 1
        return None
    
    def insert(self, new_element, position):
        """Insert a new node at the given position.
        Assume the first position is "1".
        Inserting at position 3 means between
        the 2nd and 3rd elements."""
        
        previous_element = self.get_position(position-1)
        next_element = previous_element.next
        new_element.next = next_element
        previous_element.next = new_element
                
        return
    
    
    def delete(self, value):
        """Delete the first node with a given value."""
        prev_node = None
        curr_node = self.head
        while curr_node:
            if curr_node.value == value:
                if prev_node is None:
                    self.head = curr_node.next
                else:
                    prev_node.next = curr_node.next
                return
            prev_node = curr_node
            curr_node = curr_node.next
        return
```