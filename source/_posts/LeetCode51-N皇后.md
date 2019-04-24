---
title: LeetCode51-N皇后
date: 2019-04-24 11:12:36
tags:
- 算法题目
- LeetCode-hard
categories:
- 学习
- 算法与数据结构

---

## 题目：[51-N皇后问题](<https://leetcode-cn.com/problems/n-queens/>/)

### **题目描述**

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。这个问题的由来其实是[八皇后问题](https://zh.wikipedia.org/zh-hans/%E5%85%AB%E7%9A%87%E5%90%8E%E9%97%AE%E9%A2%98)，这个问题非常经典。

![8皇后问题](https://odyc3g.ch.files.1drv.com/y4mQRaqVnNn-u0-EdEAPXFFzfKa0ni8fJOmRoG8BEptM7CvwDqnczU095pweUYBpTToAmEPhntHt9Oliq7Yv6ahJfl_X0hvTcEOudvmjINbbojrFHdEKfxJqYSyVMnQDGD_MpRdPADSrFz05iWq3SD4eXms83-hKGBh1UkWDHMqeX7Fe2k-V_aNbAsgBnLgVkXVxL33uQWJRdgqH-GQiBpHFw?width=258&height=276&cropmode=none)

上图为 8 皇后问题的一种解法。

给定一个整数 *n*，返回所有不同的 *n* 皇后问题的解决方案。每一种解法包含一个明确的 *n* 皇后问题的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

### **示例**

> 输入: 4
> 输出: [
>  [".Q..",  // 解法 1
>   "...Q",
>   "Q...",
>   "..Q."],
>
>  ["..Q.",  // 解法 2
>   "Q...",
>   "...Q",
>   ".Q.."]
> ]
> 解释: 4 皇后问题存在两个不同的解法。

### **思路**

在理解了八皇后问题之后，我们不难发现，每行放置一个且仅一个皇后。每放置一个皇后，则棋盘上与该皇后在同列和斜线上（与皇后位置行差和列差的绝对值相等）的位置无法再放置皇后。基于此，我们可以遍历每一行所有的列，如果该列可以放置，则标记（这个标记我们可以用一位数组来表示，索引代表行，值代表该行皇后放置的列），然后放置下一行的皇后。可见子问题是重复的，我们可以用回溯的方法来实现。同时不难发现，当n<4且不等于1时，问题是无解的。

### **参考实现**

```java
class Solution {
    static List<List<String>> res= new ArrayList<>();
    public List<List<String>> solveNQueens(int n) {
        res.clear();
        List<String> sol=new ArrayList<>();
        if(n==2||n==3){return res;}
        else if(n==1){sol.add("Q");res.add(sol);return res;}
        else{
            List<Integer> tmp = new ArrayList<>();
            getsol(n,0,tmp);
            return res;
        }
    }
    public static void getsol(int n, int row, List<Integer> tmp) {
        if(n==row){\\如果遍历完所有行,则为一种解法
            List<String> path = new ArrayList<>();
            for (Integer i :
                    tmp) {
                String string="";
                for (int j = 0; j < n; j++) {
                    if(j==i){
                        string+="Q";
                    }else{
                        string+=".";
                    }
                }
                path.add(string);
            }
            res.add(path);
        }
        A:for (int i = 0; i < n; i++) {
            if(!tmp.contains(i)){//判断该列是否有皇后
                for (int j = 0; j < row; j++) {//判断该点斜线上是否有皇后,如果有,则继续下一列
                    if(row-j==Math.abs(i-tmp.get(j))){
                        continue A;
                    }
                }
                tmp.add(i);
                getsol(n, row + 1, tmp);//查找下一行皇后的位置
                tmp.remove(tmp.size() - 1);//下一行所有列都无法放置,则回溯,进入当前行下一列
            }
        }
    }
}
```