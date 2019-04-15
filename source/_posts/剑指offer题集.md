---
title: 剑指offer题集
date: 2019-04-13 11:25:30
tags:
- 算法题目
categories:
- 学习
- 算法与数据结构
---

### 题目16：数值的整数次方

#### 题目描述：

给定一个double类型的浮点数base和int类型的整数exponent，求base的exponent次方。

#### 分析：

此题可以使用公式：
$$
{a^n}=\left\{\begin{matrix}{a^\frac{n}{2}}*{a^\frac{n}{2}},n为偶数\\
{a^\frac{n-1}{2}}*{a^\frac{n-1}{2}}*a,n为奇数
\end{matrix}\right.
$$
来求解a的n次幂，目的是可以减小乘法的次数，另外需要判断当幂指数不大于0和底数为0时的边界条件。

参考实现：

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

#### 题目描述：输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

#### 分析：

该题和原书中的题有一点不一样，本题要求保证数组元素的稳定。书中最简单的方法是从头到尾扫描数组，碰到一个偶数，拿出来放到数组末尾，其他元素向前移动一位。没碰到一个偶数我们就要移动$O\left ( n \right )​$该方法时间复杂度为$O\left ( n^{2} \right )​$。书中改进的方法是设置两个指针分别指向数组两端，如果碰到奇数偶数对，那么交换。这种方法显然是不稳定的。

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



上述书中的方法可以将while中对奇偶数的判断条件改成判定函数，则可以拓展代码的重用性。实现在给定的条件下，将数组划分成两部分。针对本题，考虑到稳定性，可以建一个队列，循环3次，分别将奇数、偶数打入队列，然后将队列输出修改原数组。时间复杂度是$3O\left( n\right)$，空间复杂度是$O\left(n\right)$。

```java
public class Solution {
    public void reOrderArray(int [] array) {
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

