---
title: LeetCode82-已序链表中删除重复项
date: 2019-04-28 15:23:36
tags:
- 算法题目
- LeetCode-medium
categories:
- 学习
- 算法与数据结构

---

## 题目：[82-已序链表中删除重复项](<https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/>)

### **题目描述**

*给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 *没有重复出现* 的数字。

### **示例**

> 1、
>
> 输入: 1->2->3->3->4->4->4
> 输出: 1->2
>
> 2、
>
> 输入: 1->1->1->2->3
> 输出: 2->3

### **思路**

该题考察链表的遍历操作，可以添加两个辅助指针，用来分别指向重复项的起点和终点（这里我们不直接指向操作的节点，而是对指针的后继节点进行判断，这样可以避免尾结点空的问题，示例1）定位之后将重复节点丢弃即可。在头指针在遍历的时候要注意判断空值。链表指针一直向后遍历，所以时间复杂度为$$O(n)$$

### **参考实现**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if(head==null||head.next==null)return head;
        ListNode front = new ListNode(-1);
        front.next=head;
        ListNode last = front;
        while(head.next!=null){
            if(front.next.val!=head.next.val){//判断是否重复
                front=front.next;
            }else{
                while(head.next!=null&&front.next.val==head.next.val){//获取最后一个重复节点
                    head=head.next;
                }
                front.next=head.next;//丢弃重复节点
            }
            if(head.next!=null)head=head.next;//向后遍历链表
        }
        return last.next;
    }
}
```