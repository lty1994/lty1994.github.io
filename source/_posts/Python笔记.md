---
title: Python笔记
date: 2019-04-21 11:04:32
tags:
- Python
categories:
- 学习
- 计算机语言
---

### Python多元赋值

python中有一种赋值机制即多元赋值，采用这种方式赋值时，等号两边的对象都是元组并且元组的小括号是可选的。通常形式为：

> x, y, z = 1, 2, 'a string'     
>
> 等同于 (x, y, z) = (1, 2, 'a string') 

这种赋值类型最经常用到的环境是变量交换，形如

> x, y = y, x

这种交换方式无需中间变量即可交换两个变量的值。那么具体实现机制是怎样的呢？

运行时，首先构造一个元组(y, x)，然后构造另一个元组(x, y)，接着用元组(y, x)赋值给(x, y)，元组赋值过程从左到右，依次进行。假如x=1,y=2，先令x=y,此时x=2,然后令y=x,y应该等于2？那么就不能实现变量交换了？对于这个问题，应该从元组的特性说起。

> x, y, z = 1, 2, 'a string'
> tuple = (x, y, z)

变量名x, y, z都是引用，内存开辟除了三个空间分别存储1, 2, 'a string'，三个变量分别指向这三块地址。由这三个变量构造的元组tuple，它有三个元素，这三个元素并不是x,y,z这三个变量，而是这三个变量所指向的地址空间里的内容。如果此时再另x=4,此时在地址空间会另开辟出一块空间存储4，x进而指向这块空间，而元组内的三个值仍保持不变。所以对于 x, y = y, x 来说，首先由y,x所构成的元组(y,x)其实应该表示为(2,1),那么再从左到右赋值，就可以交换变量的值了。

对于此，LeetCode中有一道[链表反转](<https://leetcode-cn.com/problems/reverse-linked-list/>)的题目，借助多元赋值可以使代码非常简洁。给出实现代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        p, rev = head, None
        while p:
            rev, rev.next, p = p, rev, p.next
        return rev
```

