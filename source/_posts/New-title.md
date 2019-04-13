---
title: Hexo中使用Latex公式
date: 2019-04-13 19:47:08
tags:
- 博客
categories:
- 工作
---

### 使用

公式插入格式：

```
$数学公式$ 行内 不独占一行
$$数学公式$$ 行间 独占一行
```

例如：

```
$f(x)=ax+b$  #行内显示
$$
f(x)=ax+b    #行间显示
$$  
```

显示效果为：$f(x)=ax+b​$

行间使用则为：

$$
f(x)=ax+b
$$

### 语法格式

#### 上标与下标

使用 ^ 表示上标，使用 _ 表示下标，如果上下标的内容多于一个字符，可以使用大括号括起来：

```
$$f(x) = a_1x^n + a_2x^{n-1} + a_3x^{n-2}$$
```

显示效果为：
$$
f(x) = a_1x^n + a_2x^{n-1} + a_3x^{n-2}
$$
如果左右两边都有上下标可以使用 \sideset 语法：

```
$$\sideset{^n_k}{^x_y}a$$
```

$$
\sideset{^n_k}{^x_y}a
$$

#### 括号

在 markdown 语法中，\, $, {, }, _都是有特殊含义的，所以需要加\转义。小括号与方括号可以使用原始的() [] 大括号需要转义\也可以使用\lbrace和 \rbrace

```
\{x*y\}
\lbrace x*y \rbrace
```

显示效果为：$\lbrace x*y \rbrace$

原始符号不会随着公式大小自动缩放，需要使用 \left 和 \right 来实现自动缩放：

```
$$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$$
```

效果(加上了\left 和 \right):$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$

没加的效果：$ \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \rbrace$

#### 分数与开方

可以使用\frac 或者 \over 实现分数的显示：

```
$\frac xy$
$ x+3 \over y+5 $
```

分别显示为：

$\frac xy$       $ x+3 \over y+5 $

开方使用\sqrt:

```
$ \sqrt{x^5} $
$ \sqrt[3]{\frac xy} $
```

显示为：$ \sqrt{x^5} $     $ \sqrt[3]{\frac xy} $

#### 求和与积分

求和使用\sum,可加上下标，积分使用\int可加上下限，双重积分用\iint:

```
$ \sum_{i=0}^n $
$ \int_a ^ b $
$ \iint_1 ^ \infty $
```

分别显示：$ \sum_{i=0}^n $   $ \int_a^b $  $ \iint_1^\infty $

#### 极限

极限使用\lim:       $ \lim_{x \to 0} $

```
$ \lim_{x \to 0} $
```

#### 表格与矩阵

表格样式lcr表示居中，|加入一条竖线，\hline表示行间横线，列之间用&分隔，行之间用\分隔：

$$\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}$$

```
$$\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}$$
```

矩阵:

```
$$\left[
\begin{matrix}
V_A \\\\
V_B \\\\
V_C \\\\
\end{matrix}
\right] =
\left[
\begin{matrix}
1 & 0 & L \\\\
-cosψ & sinψ & L \\\\
-cosψ & -sinψ & L
\end{matrix}
\right]
\left[
\begin{matrix}
V_x \\\\
V_y \\\\
W \\\\
\end{matrix}
\right] $$
```

显示效果：

$$\left[
\begin{matrix}
V_A \\\\
V_B \\\\
V_C \\\\
\end{matrix}
\right] =
\left[
\begin{matrix}
1 & 0 & L \\\\
-cosψ & sinψ & L \\\\
-cosψ & -sinψ & L
\end{matrix}
\right]
\left[
\begin{matrix}
V_x \\\\
V_y \\\\
W \\\\
\end{matrix}
\right] $$



参考：<http://stevenshi.me/2017/06/26/hexo-insert-formula/>