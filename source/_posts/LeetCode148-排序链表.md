---
title: LeetCode148-排序链表
date: 2018-11-05 17:03:36
tags:
- 算法题目
- LeetCode-medium
categories:
- 学习
- 算法与数据结构
---
题目来源：[148. 排序链表](https://leetcode-cn.com/problems/sort-list/description/)
**在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。**
**示例1:**

>输入: 4->2->1->3
  输出: 1->2->3->4

**示例2:**
>输入: -1->5->3->4->0
  输出: -1->0->3->4->5

思路：考查链表的排序，要求的时间复杂度可以联想到快速排序、堆排序、归并排序。由于是链表，我们选择二分归并排序比较容易实现。以下是Java的两种实现。

解法一：常规的插入排序，用时较多
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
    public ListNode sortList(ListNode head) {
        ListNode root=new ListNode(0);
        root.next=head;
        ListNode p=head;
        while(p!=null&&p.next!=null){
            if(p.val<=p.next.val){p=p.next;}
            else{
                ListNode tmp=root,q=p.next;
                while(tmp.next.val<=q.val){
                    tmp=tmp.next;
                }                
                p.next=q.next;
                q.next=tmp.next;
                tmp.next=q;                
            }
        }
        return root.next;  
    }
}
```
解法二：二分归并排序,用时较少
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
    public ListNode sortList(ListNode head) {
     if(head==null || head.next==null) return head;
        return mergeSort(head);
    }

	private static ListNode mergeSort(ListNode head){
		if(head==null || head.next==null) return head;
		//分治
		ListNode p1 = head;
		ListNode p2 = head;
		//遍历对链表二分
		while(p2.next!=null && p2.next.next!=null){
			p1 = p1.next;
			p2 = p2.next.next;
		}
		ListNode left = head; //左子链
		ListNode right = p1.next; //右子链
		p1.next = null;
		//排序子链表
		left = mergeSort(left);
		right = mergeSort(right);
		//合并
		if(left==null) return right;
		if(right==null) return left;
		
		ListNode h = new ListNode(-1);
		ListNode p = h;
		//对左右两数进行比较
		while(left!=null && right!=null){
			if(left.val<right.val){
				p.next = left;
				left = left.next;
				p = p.next;
			}else{
				p.next = right;
				right = right.next;
				p = p.next;
			}
		}
		if(left==null) p.next = right;
		else p.next = left;
		return h.next; //返回表头
	}
}
```