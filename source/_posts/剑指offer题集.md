---
title: 剑指offer题集笔记
date: 2019-04-13 11:25:30
tags:
- 算法题目
categories:
- 学习
- 算法与数据结构
---

### 题目12：矩阵中的路径

- **题目描述**

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。 例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

- **分析**

该类问题可以用[回溯法](<https://baike.baidu.com/item/%E5%9B%9E%E6%BA%AF%E6%B3%95>)来求解，回溯法非常适合由多个步骤组成的问题，并且每个步骤都有多个选项。当我们在某一步选择了其中一个选项时，我们进入下一步，然后又面临新的选项。重复直至到达最终的状态。

该题过程主要如下：

遍历矩阵中的所有节点（因为题目允许从任何一个格子开始），对于其中一个节点作为起始节点，然后向四周（符合回溯的条件）寻找下一个和字符串匹配的节点，如果有，那么继续向后寻找路径，如果没有，则遍历下一个节点作为起始节点。直至遍历完所有节点。

过程中我们需要一个标志位来标记节点是否已为路径上的节点，和一个整数保存路径的长度。要注意的是如果该节点四周没有可以继续的路径选项，那么，需要对当前节点进行复位操作（路径长度-1，取消路径节点的标记）。

- **参考实现**

```java
public class Solution {
    public boolean hasPath(char[] matrix, int rows, int cols, char[] str)
    {
        if(matrix==null||str==null||rows<1||cols<1)return false;
        boolean[] visit=new boolean[rows*cols];
        for(int i=0;i<rows*cols;i++)
            visit[i]=false;
        int pathlen=0;
        for(int row=0;row<rows;row++){
            for(int col=0;col<cols;col++){
                if(has(matrix,rows,cols,row,col,str,visit,pathlen))
                    return true;
            }
        }
        return false;
    }
    public boolean has(char[] matrix, int rows, int cols,int row,int col, char[] str,boolean[]visit,int pathlen){
        if(pathlen==str.length)return true;
        boolean haspath=false;
if(row<rows&&col<cols&&row>=0&&col>=0&&matrix[row*cols+col]==str[pathlen]&&!visit[row*cols+col]){
            pathlen++;
            visit[row*cols+col]=true;
            haspath = has(matrix,rows,cols,row-1,col,str,visit,pathlen)
                    ||has(matrix,rows,cols,row,col-1,str,visit,pathlen)
                    ||has(matrix,rows,cols,row,col+1,str,visit,pathlen)
                    ||has(matrix,rows,cols,row+1,col,str,visit,pathlen);
            if(!haspath){
                pathlen--;
                visit[row*cols+col]=false;
            }
        }
        return haspath;
    }
}
```

------

### 题目13：机器人的运动范围

- **题目描述**

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

- **分析**

回溯法求解，该题过程主要如下：

用count表示机器人能到达的格子的个数，机器人一开始从（0,0）开始移动，当它准备进入（i，j）时，首先检查是否能到达，如果能到达，count+1。再判断（i，j）周围相邻的格子（需要注意边界上的格子没有4个相邻格子）是否能够到达。使用一个数组来标记格子是否已达，避免重复累加。同时，定义函数返回某一格是否可达，这个函数主要是判断格子是否满足相应的约束条件。

- **参考实现**

```java
public class Solution {
    public int movingCount(int threshold, int rows, int cols)
    {
        if(threshold<0||rows<=0||cols<=0)return 0;
        boolean[] visit = new boolean[rows*cols];
        for(int i =0;i<rows*cols;i++){
            visit[i]=false;
        }
        return help(threshold,rows,cols,0,0,visit);
    }
    public int help(int threshold,int rows,int cols,int row,int col,boolean[]visit){
        int count=0;
        if(check(threshold,rows,cols,row,col,visit)){
            visit[row*cols+col]=true;
            count=1+help(threshold,rows,cols,row-1,col,visit)
                +help(threshold,rows,cols,row,col-1,visit)
                +help(threshold,rows,cols,row+1,col,visit)
                +help(threshold,rows,cols,row,col+1,visit);
        }
        return count;
    }
    public boolean check(int threshold,int rows,int cols,int row,int col,boolean[]visit){
        if(row>=0&&row<rows&&col>=0&&col<cols&&getNum(row)+getNum(col)<=threshold&&!visit[row*cols+col])return true;
        return false;
    }
    public int getNum(int num){
        int sum =0;
        while(num>0){
            sum+=num%10;
            num/=10;
        }
        return sum;
    }
}
```

------

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

### 题目27：二叉树的镜像

- **题目描述**

操作给定的二叉树，将其变换为源二叉树的镜像。

输入描述：

> 二叉树的镜像定义：源二叉树 
>     	    8
>     	   /  \
>     	  6   10
>     	 /  \   /   \
>     	5  7 9   11
>     镜像二叉树
>     	    8
>     	   /  \
>     	10   6
>             /  \   /  \
>           11  9 7  5

- **分析**

本题实际上考察的是树的遍历，问题大致过程如下：

先前序遍历树的每个节点，当节点不是叶节点的时候，就交换两个子节点。要注意的是，虽然在遍历过程中对子节点进行了交换，但是递归的调用栈的顺序为前序遍历的顺序，不会改变。可以用递归直接实现，也可以借助队列循环实现。

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
    public void Mirror1(TreeNode root) {  \\递归实现
        if(root==null)return;
        if(root.left!=null||root.right!=null){
            TreeNode tmp=root.left;
            root.left=root.right;
            root.right=tmp;
            if(root.left!=null)Mirror(root.left);
            if(root.right!=null)Mirror(root.right);
        }
        return;
    }    
    public void Mirror2(TreeNode root) {\\循环实现
        if(root==null)return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode now=queue.peek();
            if(now.left!=null||now.right!=null){
                TreeNode tmp=now.left;
                now.left=now.right;
                now.right=tmp;
                queue.poll();
                if(now.left!=null)queue.offer(now.left);
                if(now.right!=null)queue.offer(now.right);
            }else{
                queue.poll();
            }
            
        }
    }
}
```

------

### 题目28：对称的二叉树

- **题目描述**

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

- **分析**

本题实际上考察的是树的遍历过程。树的前序、中序和后序遍历都是先访问左子树，然后再访问右子树。在本题中，我们不难发现，先访问右子树再访问左子树遍历得到的序列和原来先左后右遍历得到的序列应该相同。但是有一种特殊情况，就是树的所有节点的值相同，他们的输出序列永远相同。这个时候考虑树中空节点的情况，两种遍历方式中，遍历到某个位置的时候，两个位置要么相同，要么同时为空，否则就不对称。

- **参考实现**

```java
/*
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
    boolean isSymmetrical(TreeNode pRoot)
    {
        return isSymmetrical(pRoot,pRoot);\\让树分别从左子树和右子树两个方向遍历
    }
    boolean isSymmetrical(TreeNode p1,TreeNode p2){
        if(p1==null&&p2==null)return true;
        if(p1==null||p2==null)return false;
        if(p1.val!=p2.val)return false;
        return isSymmetrical(p1.left,p2.right)&&isSymmetrical(p1.right,p2.left);
    }
}
```

------

### 题目30：包含min函数的栈

- **题目描述**

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

- **分析**

题干中我们需要定义一个栈，并且实现得到栈中最小元素的函数，并要求时间复杂度为$$O(1)$$。常规思路就是从栈顶通过迭代器搜索栈中最小的元素，但是这样做不符合时间复杂度的要求。因为题干没有限制空间复杂度，所以我们可以建一个辅助栈，使得栈顶元素始终是栈的最小元素。当然还有一种方法是建立最小堆，但是最小堆建立的时间复杂度是$$O(\log N)$$，也不符合题干要求。

- **参考实现**

```java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack=new Stack<>();
    Stack<Integer> min_stack=new Stack<>();
    public void push(int node) {
        stack.push(node);
        if(min_stack.isEmpty()||min_stack.peek()>node){
            min_stack.push(node);
        }
    }
    
    public void pop() {
        if(stack.isEmpty())return;
        else{
            if(stack.peek()==min_stack.peek())min_stack.pop();
            stack.pop();
        }
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int min() {
        return min_stack.peek();
    }
}
```

------

### 题目32：从上往下打印二叉树

- **题目描述**

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

- **分析**

本题实际上考察的是树的层序遍历。借住队列，每次将父节点的左节点和右节点依次送入队尾，然后从队首取出元素的序列即为层序遍历。

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
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        ArrayList<Integer> res=new ArrayList<>();
        if(root==null)return res;
        Queue<TreeNode> queue =new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode now=queue.peek();
            if(now.left!=null)queue.offer(now.left);
            if(now.right!=null)queue.offer(now.right);
            res.add(queue.poll().val);
        }
        return res;
        
    }
}
```

------

### 题目33：二叉搜索树的后序遍历序列

- **题目描述**

输入一个整数数组，判断该数组是不是某[二叉搜索树](<https://baike.baidu.com/item/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91>)的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

- **分析**

本题考查了二叉搜索树的结构，同时考查了数组的操作。

- **参考实现**

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence==null||sequence.length==0)return false;
        if(sequence.length<3)return true;
        return help(sequence,0,sequence.length-1);
    }
    public boolean help(int[] arr,int front,int last){
        if(front>=last)return true;
        int target=arr[last];
        int id1=front;
        if(arr[id1]<target){
            while(id1<last&&arr[id1]<target)id1++;
            int id2=id1;
            while(id2<last){
                if(arr[id2]<target)return false;
                id2++;
            }
            return help(arr,front,id1-1)&&help(arr,id1,last-1);
        }else{
            while(id1<last){
                if(arr[id1]<target)return false;
                id1++;
            }
            return help(arr,front,last-1);
        }
    }
}
```

上述代码存在冗余，检测右子树重复了。我们可以优化一下：

```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence==null||sequence.length==0)return false;
        if(sequence.length<3)return true;
        return help(sequence,0,sequence.length-1);
    }
    public boolean help(int[] arr,int front,int last){
        if(front>=last)return true;
        int i=front;
        while(i<last){
            if(arr[i]>arr[last])break;
            i++;
        }
        for(int j=i;j<last;j++){
            if(arr[j]<arr[last])return false;
        }
        return help(arr,front,i-1)&&help(arr,i,last-1);
    }
}
```

------

### 题目34：二叉树中和为某一值的路径

- **题目描述**

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

- **分析**

题干中的条件是找到从根节点到叶节点的路径，路径始终是从根节点开始，所以我们只能是前序遍历二叉树。当我们访问某一个子节点的时候，如果不是叶节点，那么就继续向下遍历，并将节点添加到路径，target为target减去当前接节点的值。如果是叶节点的话，判断是否符合条件，如果符合那么将路径添加。否则返回父节点，在返回时要将当前节点从路径中删去。

- **参考实现**

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target){
        ArrayList<ArrayList<Integer>> res=new ArrayList<>();
        if(root==null)return res;
        ArrayList<Integer> path = new ArrayList<>();
        FindPath(root,target,res,path);
        return res;
    }
    public void FindPath(TreeNode root,int target,ArrayList<ArrayList<Integer>> res,ArrayList<Integer> path){
        path.add(root.val);
        if(root.left==null&&root.right==null&&root.val==target){
            res.add(new ArrayList<Integer>(path));
        }else{
            if(root.left!=null)FindPath(root.left,target-root.val,res,path);
            if(root.right!=null)FindPath(root.right,target-root.val,res,path);
        }
        path.remove(path.size()-1);
        return;
    }
}
```

------

### 题目54：二叉搜索树的第K大节点

- **题目描述**

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

- **分析**

由二叉树的结构我们不难想到，二叉树的中序遍历序列是树节点的生序序列，如此我们只要得到中序遍历序列就可以找到第K大节点。主要考察的是树的遍历；

- **参考实现**

```java
public class Solution {
    TreeNode KthNode(TreeNode pRoot, int k)
    {
        if(pRoot==null||k<=0)return null;
        TreeNode res=null;
        ArrayList<TreeNode> tmp=new ArrayList<>();
        help(pRoot,tmp,k,res);
        if(tmp.size()<k)return null;
        return tmp.get(k-1);
    }
    public void help(TreeNode root,ArrayList<TreeNode> tmp,int k,TreeNode res){
        if(root==null||tmp.size()>=k)return;
        if(root.left!=null)help(root.left,tmp,k,res);
        tmp.add(root);
        if(root.right!=null)help(root.right,tmp,k,res);
        return;
    }
}
```

------

### 题目55：二叉树的深度

- **题目描述**

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

- **分析**

该题与题34有一点相似，都是从根节点到叶节点进行遍历。所以该题也只能是前序遍历这个树，我们采用递归的方式实现。题目要求树的深度，定义一个全局变量表示当前树的最大深度，定义一个局部变量表示当前树的深度，递归之后返回树的最大深度即可。下面给出两种解法：

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
解法一：
public class Solution {
    int depth=0;
    public int TreeDepth(TreeNode root) {
        int path=0;
        TreeDepth(root,path);
        return depth;
    }
    public void TreeDepth(TreeNode root,int path){
        if(root==null)return;
        else{
            if(++path>depth)depth=path;
            if(root.left!=null)TreeDepth(root.left,path);
            if(root.right!=null)TreeDepth(root.right,path);
        }
    }
}
解法二：
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root==null)return 0;
        int left_depth=TreeDepth(root.left);
        int right_depth=TreeDepth(root.right);
        return left_depth>right_depth?left_depth+1:right_depth+1;
    }
}
```

#### 延伸题：平衡二叉树的判定

- **题目描述**

输入一棵二叉树，判断该二叉树是否是[平衡二叉树](<https://baike.baidu.com/item/%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91>)。

> 平衡二叉搜索树（Self-balancing binary search tree）又被称为AVL树（有别于AVL算法），且具有以下性质：它是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。平衡二叉树的常用实现方法有[红黑树](https://baike.baidu.com/item/%E7%BA%A2%E9%BB%91%E6%A0%91/2413209)、[AVL](https://baike.baidu.com/item/AVL/7543015)、[替罪羊树](https://baike.baidu.com/item/%E6%9B%BF%E7%BD%AA%E7%BE%8A%E6%A0%91/13859070)、[Treap](https://baike.baidu.com/item/Treap)、[伸展树](https://baike.baidu.com/item/%E4%BC%B8%E5%B1%95%E6%A0%91/7003945)等。

- **分析**

该题最直接的做法，遍历每个结点，借助一个获取树深度的递归函数，根据该结点的左右子树高度差判断是否平衡，然后递归地对左右子树进行判断。但是这样会重复遍历节点，我们可以简化一下，在判断当前子树不是平衡二叉树之后我们就直接返回，可以借助一个标志（-1）。

- **参考实现**

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        return getdepth(root)!=-1;
    }
    public int getdepth(TreeNode root){
        if(root==null)return 0;
        int left=getdepth(root.left);
        if(left==-1)return -1;
        int right=getdepth(root.right);
        if(right==-1)return -1;
        return Math.abs(left-right)>1?-1:1+Math.max(left,right);
    }
}
```

------



### 未完待续...