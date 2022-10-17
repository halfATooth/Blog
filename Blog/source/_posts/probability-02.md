---
title: 概率论02
date: 2022-10-08 22:53:51
tags: 概率论
index_img: /img/cover/probability.png
math: true
---

# 条件概率

![条件概率](/img/probability-02/条件概率.png)

# 乘法公式

$$
P(AB) = P(B)P( A|B),(P(B)>0)
$$

$$
P(AB) =  P(A)P(B|A),(P(A)>0)
$$

$$
P(A_1A_2… A_n)=P(A_1)P(A_2|A_1)P(A_3|A_1A_2)…P(A_n|A_1A_2…A_{n−1}),
$$

$$
(P(A_n|A_1A_2…A_{n−1}>0)
$$

# 事件的独立性

## 定义

如果事件A发生对事件B发生的概率没有影响，事件 B发生对事件A发生的概率也没有影响，把这种现象称为 A与B相互独立(Independent).

设A和B是样本空间中的两个事件，如果有 
$$
P(AB) = P(A)P(B)
$$
则称事件A与事件B相互独立，简称独立( 记 为 A ∐ B). 否则称A与B不独立或相依.

## 独立性的判断方法

- 方法1：根据试验实际情况判断。
- 方法2：验证P(AB) = P(A)P(B)

## 定理

- 若事件A与B相互独立，那么$\overset{-}{A}$$与B，$$\overset{-}{B}$$与A，$$\overset{-}{A}$$与$$\overset{-}{B}$也相互独立。
- 若P(A) >0,P(B)>0，可以证明：独立与互斥不能同时成立。

## 三个事件的独立性

![三个事件的独立性](/img/probability-02/3独立.png)

两两独立不一定相互独立

## n个事件的独立性

![n独立](/img/probability-02/n独立.png)

## 独立事件的概率计算

设$A_1 , A_2 ,…, A_n$$是样本空间$$\Omega$中的n个相互独立的事件，则

(1)至少有一个发生的概率为
$$
P(\underset{i=1}{\overset{n}{\bigcup}}A_i)=1-\underset{i=1}{\overset{n}{\Pi}}(1-P(A_i))
$$
(2)都发生的概率为
$$
P(\underset{i=1}{\overset{n}{\bigcap}}A_i)=\underset{i=1}{\overset{n}{\Pi}}P(A_i)
$$

#  全概率公式

## 定义

![全概率公式](/img/probability-02/全概率公式.png)

## 解释

每一原因都可能导致B发生，故B发生的概率是各原因引起 B发生概率的总和，即全概率公式（**由因到果**）.

# 贝叶斯公式



# 伯努利概型
