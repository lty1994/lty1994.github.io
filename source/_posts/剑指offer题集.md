---
title: 剑指offer题集
date: 2019-04-13 11:25:30
tags:
- 算法题目
categories:
- 学习
- 算法与数据结构
---

[TOC]



## 剑指offer题集笔记：

### 题目16：数值的整数次方

- **题目描述**

给定一个double类型的浮点数base和int类型的整数exponent，求base的exponent次方。

- **分析**

此题可以使用公式：
$$
{a^n}=\left\{\begin{matrix}{a^\frac{n}{2}}*{a^\frac{n}{2}},n为偶数\\
{a^\frac{n-1}{2}}*{a^\frac{n-1}{2}}*a,n为奇数
\end{matrix}\right.
$$
来求解a的n次幂，目的是可以减小乘法的次数，另外需要判断当幂指数不大于0和底数为0时的边界条件。

- **参考实现**

```java
public class Solution {
    public double Power(double base, int exponent) {
        double res=1.0;
        if((exponent&1)==1)res=base;
        boolean flag=true; //标识位幂指数是否为负
        if(exponent<0){//转为正幂指数
            if(base==0)return 0;
            flag=false;
            exponent*=-1;
        }        
        double tmp=base*base;
        while(exponent>1){//求乘法
            res*=tmp;
            tmp=res;
            exponent = exponent>>1;
        }
        return flag?res:1/res;
  }
}
```

------

### 题目21：调整数组数顺序使奇数位于偶数前面

- **题目描述**

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

- **分析**

该题和原书中的题有一点不一样，本题要求保证数组元素的稳定。书中最简单的方法是从头到尾扫描数组，碰到一个偶数，拿出来放到数组末尾，其他元素向前移动一位。没碰到一个偶数我们就要移动$O\left ( n \right )$该方法时间复杂度为$O\left ( n^{2} \right )$。书中改进的方法是设置两个指针分别指向数组两端，如果碰到奇数偶数对，那么交换。这种方法显然是不稳定的。

- **参考实现**

```c++
void ReorderOddEven_1(int *pData, unsigned int length)
{
    if(pData == nullptr || length == 0)
        return;

    int *pBegin = pData;
    int *pEnd = pData + length - 1;

    while(pBegin < pEnd)
    {
        // 向后移动pBegin，直到它指向偶数
        while(pBegin < pEnd && (*pBegin & 0x1) != 0)
            pBegin ++;

        // 向前移动pEnd，直到它指向奇数
        while(pBegin < pEnd && (*pEnd & 0x1) == 0)
            pEnd --;

        if(pBegin < pEnd)
        {
            int temp = *pBegin;
            *pBegin = *pEnd;
            *pEnd = temp;
        }
    }
}
```



上述书中的方法可以将while中对奇偶数的判断条件改成判定函数，则可以拓展代码的重用性。实现在给定的条件下，将数组划分成两部分。针对本题，考虑到稳定性，可以建一个队列，循环3次，分别将奇数、偶数打入队列，然后将队列输出修改原数组。时间复杂度是$3O\left( n\right)$，空间复杂度是$O\left(n\right)​$。

```java
public class Solution {
    public void reOrderArray(int [] array) {
        if(array==null||array.length==0)return;
        int len = array.length;
        Queue<Integer> queue = new LinkedList<>();
        for(int i=0;i<len;i++){
            if((array[i]&1)==1)queue.offer(array[i]);
        }
        for(int i=0;i<len;i++){
            if((array[i]&1)==0)queue.offer(array[i]);
        }
        for(int i=0;i<len;i++){
            array[i]=queue.poll();
        }
    }
}
```

------

### 题目22：链表中倒数第k个结点

- **题目描述**

输入一个链表，输出该链表中倒数第k个结点。

- **分析**

该题最简单的思路是遍历两次，一次求得链表长度，第二次找到倒数第k个点。优化的办法是，设置两个指针，第一个指针遍历到第k-1个节点，第二个指针再从头开始，两个指针同时扫描直至第一个指针到达链表末尾。此时第二个指针指向的就是倒数第K个节点。

这中间有几个边界条件需要特别注意：

1. 输入链表为空
2. 输入k为0
3. 链表节点个数小于K

- **参考实现**

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if(head==null||k==0)return null;
        ListNode front=head;
        ListNode last;
        for(int i =1;i<k;i++){
            if(front.next!=null){
                front=front.next;
            }
            else{
                return null;
            }
        }
        last=head;
        while(front.next!=null){
            last=last.next;
            front=front.next;
        }
        return last;

    }
}
```

------

### 题目24：反转链表

- **题目描述**

输入一个链表，反转链表后，输出新链表的表头。

- **分析**

a->b->c->d...h->i->j->...

a<-b<-c<-d...h<-i   j->...

该问题关键在解决反转链表后的断裂怎么处理，我们用三个指针来处理。

- **参考实现**

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode now = null;     \\指向当前操作的节点
        ListNode front = head;   \\指向下一个操作的节点
        ListNode res=null;       \\指向反转链表的尾结点
        while(front!=null){      \\判断是否到达尾结点
            now=front;           \\获得本轮操作节点
            front=front.next;    \\front指向下一个节点，解决链表断裂
            \\反转节点
            now.next=res;
            res=now;
        }
        return res;
    }
}
```

------

### 题目25：合并两个排序的链表

- **题目描述**

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

- **分析**

链表合并的过程如下图所示：

![链表合并过程](https://qtz5aq.ch.files.1drv.com/y4ma1HbikeYEQ_RqZxLokCR41f8eSRTJaqAI6BYlOpCxAnI6ZJRRCBurti5F9zfWZvVp8rGrjawlWw2AHN1VWft2Ld9CnLIzbZtrv30EduoGdUQjqVQfdA0jlQKdbfvIelfBZzMN9ys0az5YazAB3wKcSCXK2QQwwONxpTCOsrSixzMksQBd1iert12RyyuNT1R2AlL4G3Opv8aCxsh_IiqCg?width=2691&height=2355&cropmode=none)

a<-b<-c<-d...h<-i   j->...可以用递归实现

每一次都是选择两个链表中较小值的节点作为合并链表的头结点，然后我们再将上一次合并链表的尾结点与本次的头结点链接起来，就可以得到合并的链表。具体实现，用递归是最简单的。时间复杂度为$$O(list1.length+list2.lenght)$$

- **参考实现**

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1==null)return list2;
        else if(list2==null)return list1;
        ListNode res=null;
        if(list1.val<list2.val){
            res=list1;
            res.next=Merge(list1.next,list2);
        }else{
            res=list2;
            res.next=Merge(list1,list2.next);
        }
        return res;
    }
}
```

------

### 题目26：树的子结构

- **题目描述**

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

- **分析**

本题实际上考察的是树的遍历和树指针的操作，同时还有遍历过程中边界条件的处理。问题可以分成两步：

- 在A中找到与B根节点相同的节点；此步骤我们只需要遍历A树即可，我们任意选择一种树的遍历的方法即可，此处我们选择实现较为简单的递归遍历。遍历的边界条件是不能为空树；
- 判断A中该节点的子树和B是否有相同的结构；当我们能够遍历到B树的子节点的时候，代表两个子树有相同的结构。我们分别遍历左子树和右子树，当两个子树都有相同结构时，返回true，否则返回false。

- **参考实现**

```java
/**
public class TreeNode {    
	int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        boolean res = false;
        if(root1!=null&&root2!=null){
            if(root1.val==root2.val)res=help(root1,root2);
            if(!res)res=HasSubtree(root1.left,root2);
            if(!res)res=HasSubtree(root1.right,root2);
        }
        return res;
    }
    public boolean help(TreeNode root1,TreeNode root2){
        if(root2==null)return true;
        if(root1==null)return false;
        if(root1.val!=root2.val)return false;
        return help(root1.left,root2.left)&&help(root1.right,root2.right);
    }
}
```

------

### 未完待续...