---
title: 概率论错题01
date: 2022-10-08 22:54:57
tags: 概率论错题
index_img: /img/cover/probability.png
math: true
---

第一章：随机事件及其概率

# 随机事件及其运算

## 事件的表示

Q:事件A,B,C中至少有两个发生，表示为

A:
$$
AB\cup BC\cup AC
$$
把两个发生的情况列出来，“至少”的话就是他们的组合啦

## 判断事件的关系及运算

![](/img/probability-011/1-5.png)

法一：排除法，显然

法二：
$$
A\cup B=(A\cup B)\cup AB=(A\cup B)\cup (\overline{A}\cap\overline{B})
$$

$$
=(A\cup B)\cup (\overline{A\cup B})=\Omega
$$

![](/img/probability-011/6-5.png)
$$
\because P(AB)=P(A)P(B|A)\leq P(A)
$$

$$
同理，P(AB)\leq P(B)
$$

$$
\therefore 2P(AB)\leq P(A)+P(B)
$$

$$
\therefore C正确
$$



# 随机事件的概率

## 古典概型

![](/img/probability-011/古典概型.png)

样本空间含有的样本数为$3^4$，该事件意味着：

1. 前三次共摸到2种颜色的球，第四个球也随之固定：$C_3^2$
2. 这两种颜色的球，一种颜色2个，另一种颜色1个：$C_2^1$
3. 前三个球有顺序：$C_3^1$

所以，概率为$\frac{C_3^2C_2^1C_3^1}{3^4}=\frac 2 9$



# 概率基本运算法则

## 利用性质求概率

![](/img/probability-011/性质求概率.png)
$$
由题意知，AB\subset C\therefore P(C)\geq P(AB)
$$

$$
又\because P(A+B)=P(A)+P(B)-P(AB)
$$

$$
\therefore P(AB)=P(A)+P(B)-P(A+B)
$$

$$
\therefore P(C)\geq P(A)+P(B)-P(A+B)\geq P(A)+P(B)-1
$$



![](/img/probability-011/3-5.png)
$$
(\overline A+B)(A+B)(\overline A+\overline B)(A+\overline B)
$$

$$
=(A+B)(\overline A+\overline B)(A+\overline B)(\overline A+B)
$$

$$
=(A\overline B+\overline AB)(\overline A\overline B+AB)=\emptyset
$$

$$
\therefore 原式=0
$$



## 利用公式求概率

![](/img/probability-011/6-8.png)

设A={数字以0结尾}，B={数字以5结尾}，C={该数字被5整除}，样本总数n为$9\times 9\times 8\times 7$，

|A|=$9\times 8\times 7$$，|B|=$$8\times 8\times 7$.

$\therefore P(C)=\frac{|A|+|B|}{n}$



![](/img/probability-011/6-9.png)

样本总数n为$4^3$，<font color="red">这是一次次放球推演出的样本总数，是有次序的，此时认为球是不同的</font>，

设$B_i=\{杯子中球的最大个数为i\}$，

$|B_1|=C_4^1A_3^3$，4个杯子选一个空着，3个球全排列；

$|B_2|=C_4^1C_3^1C_3^2$，4个杯子选一个放2个球，3个球选2个放进去，剩下3个杯子选一个放最后一个球；

$|B_3|=C_4^1$，4个杯子选一个放3个球。

$\therefore P(B_i)=\frac{|B_i|}{n}$.

![](/img/probability-011/6-13.png)

设$A=\{至少有2只配成一双\}$$，则$$\overline A=\{任意2只都不能配成一双\}$,

从对立事件考虑，那么首先这4只必然来自4双鞋：$C_5^4$,

然后，这4只鞋每一只都有“左或右”两种可能：$2^4$,

$\therefore P(A)=1-P(\overline A)=1-\frac{C_5^42^4}{C_{10}^4}=\frac{13}{21}$.

## 概率的综合应用

![](/img/probability-011/6-23.png)

<font color="red">这题可以把n看作已知量，然后问当时选出的是正品的可能性，那就是典型的贝叶斯公式了，这题拐了个小弯，我第一次做的时候有点懵</font>

设A={选出的是正品}，B={使用n次未发生故障}，

全概率公式：$P(B)=P(A)P(B|A)+P(\overline A)P(B|\overline A)=0.1\times 1+0.9\times (0.9)^n$,

贝叶斯公式：$P(A|B)=\frac{P(A)P(B|A)}{P(A)P(B|A)+P(\overline A)P(B|\overline A)}=\frac{0.1\times 1}{0.1\times 1+0.9\times (0.9)^n}\geq0.7$,

$\therefore n\geq 29$.

![](/img/probability-011/6-26.png)

<font color="blue">没啥好说的，就典型的贝叶斯，可能需要注意的是</font><font color="red">随机选取(三分之一）和选的时候是有顺序的</font>

![](/img/probability-011/6-25.png)

也没啥好说的，就是注意一下约束条件，例如：只有甲炮射中的概率为$0.4\times (1-0.5)\times(1-0.7)$,而不是0.4，容易忽略。

# 独立性

## 独立性判断

![](/img/probability-011/5-1a.png)

![](/img/probability-011/5-1b.png)

$$
P(A|B)+P(\overline A|\overline B)=\frac{P(AB)}{P(B)}+\frac{P(\overline A\overline B)}{P(\overline B)}
$$

$$
=\frac{P(AB)}{P(B)}+\frac{1-P(A+B)}{1-P(B)}
$$

$$
=\frac{P(AB)}{P(B)}+\frac{1-P(A)-P(B)+P(AB)}{1-P(B)}
$$

$$
=\frac{P(AB)}{P(B)}+1+\frac{P(AB)-P(A)}{1-P(B)}=1
$$

$$
\therefore \frac{P(AB)}{P(B)}=\frac{P(A)-P(AB)}{1-P(B)}
$$

$$
\therefore P(AB)-P(B)P(AB)=P(A)P(B)-P(B)P(AB)
$$

$$
\therefore P(AB)=P(A)P(B)
$$



## 关于独立性与独立重复实验的问题

![](/img/probability-011/6-30.png)

这里用到一个重要（冷门）公式，经常忘
$$
P(\overline AB)=P(B)-P(AB)
$$
1.必要性

$\because A,B相互独立\therefore \overline A也与B相互独立$,

$\therefore P(B|A)=P(B),P(B|\overline A)=P(B)\therefore P(B|A)=P(B|\overline A)$.

2.充分性

$\because P(B|A)=P(B|\overline A)\therefore \frac{P(AB)}{P(A)}=\frac{P(\overline AB)}{P(\overline A)}=\frac{P(B)-P(AB)}{1-P(A)}$,

$\therefore P(A)P(B)-P(A)P(AB)=P(AB)-P(A)P(AB)$,

$\therefore P(AB)=P(A)P(B)$.

$\therefore A,B相互独立$.



![](/img/probability-011/6-33.png)

设A={甲获胜},$\overline A=\{乙获胜\}$$，$$B_i=\{甲第i轮射中\}$$,$$C_i=\{乙第i轮射中\}$，

$A=B_1+\overline{B_1}\overline{C_1}B_2+\overline{B_1}\overline{C_1} \overline{B_2}\overline{C_2}B_3+...$,

$\therefore P(A)=p_1+(1-p_1)(1-p_2)p_1+(1-p_1)^2(1-p_2)^2p_1+...$,

$=\frac{p_1}{1-(1-p_1)(1-p_2)}=\frac{p_1}{p_1+p_2-p_1p_2}$,

$\therefore P(\overline A)=1-P(A)$.



# 证明题

![](/img/probability-011/6-38.png)
$$
P(A\overline B+\overline AB)=P(A\overline B)+P(\overline AB)-0
$$

$$
=P(A)-P(AB)+P(B)-P(AB)=P(A)+P(B)-2P(AB)
$$

