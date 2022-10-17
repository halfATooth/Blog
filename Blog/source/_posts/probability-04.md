---
title: 概率论04
date: 2022-10-08 22:55:03
tags: 概率论
index_img: /img/cover/probability.png
math: true
---

> 一维连续型随机变量及其分布

# cdf与pdf

## 定义

设F( x)是随机变量X的概率分布函数(cumulative distribution function, cdf)，若存在非负可积函数f ( x)≥0，对任意实数x，有$F(x)=P(X\leq x)=\int_{-\infty}^xf(t)dt\qquad -\infty<x<+\infty$.

称X为连续型随机变量，f ( x)为X的概率密度函数 (Probability Density Function，pdf ).

## 性质

- f(x)>0，且在部分点处有可能f(x)>1.
- $F(+\infty)=1$.
- $P(a<x\leq b)=F(b)-F(a)=\int_a^bf(x)dx$.
- 连续随机变量的cdf是连续函数，离散随机变量的cdf不一定连续.
- $P(X=a)=0$，a为任意指定值.
- <font color="red">$P(A)=0 \not\Rightarrow A=\emptyset$</font>.
- <font color="red">$P(B)=1\not\Rightarrow B=\Omega$</font>.

## Logistic distribution

作用：将在$(-\infty,+\infty)$上的变量映射到$(0,1)$上。

**sigmoid函数**：
$$
F(x)=\frac{e^x}{1+e^x},\quad x\in R
$$

# 均匀分布

## 定义

如果连续型随机变量X具有密度函数

![均匀分布密度函数](/img/probability-04/均匀分布密度函数.png)

在指定区间内的所有值 都具有相同的概率，其中a, b是有限数，则称X是[a,b]上的均匀分布,记作 <font color="red">X～U [a,b]</font>.

![均匀分布函数](/img/probability-04/均匀分布函数.png)

## 性质

均匀分布的**特点**：若X～U [a,b]，则X 取值落在 [a,b]中的 某一区域内的概率与这一区域的长度成正比，而与区域的位 置无关.

均匀分布的**重要性**：借助于均匀分布可以生成任意的分布， 在计算机上很容易生成均匀分布.

Unif(0,1)经常称为**标准分布**(standard Uniform)

# 指数分布

## 定义

如果连续型随机变量X具有密度函数
$$
f(x)=
\begin{cases}
\lambda e^{-\lambda x},\quad x>0 \newline
0,\qquad其他 
\end{cases}
$$
这里$\lambda>0$为常数,则称X服从参数为$\lambda$的指数分布． 简记为<font color="red">X～E($\lambda$)</font>. 其分布函数为
$$
F(x)=\begin{cases}
0,\qquad x<0 \newline
1-e^{-\lambda x},\qquad x\geq 0
\end{cases}
$$
指数分布常用来近似“寿命”问题，$\lambda $表示寿命的倒数。

![指数分布](/img/probability-04/指数分布.png)

## 指数分布的无记忆性

$$
P(X>s+t|X>s)=P(X>t)
$$

理解：<font color="red">忽略了前s阶段的损耗</font>.



# 正态分布

## 定义

如果连续型随机变量X的密度函数
$$
f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}},\qquad-\infty<x<+\infty
$$
其中$\mu,\sigma$ 为常数,且$\sigma>0$，则称随机变量X 服从参数$\mu,\sigma$ 的正态分布,记为<font color="red">$X～N (\mu,\sigma^2 )$</font>.

分布函数没有解析表达式

![正态分布](/img/probability-04/正态分布.png)

## 小结论

对于标准的正态分布$f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$,有如下结论成立
$$
\int_{-\infty}^\infty f(x)dx=\int_{-\infty}^\infty x^2f(x)dx=1
$$

$$
\int_{-\infty}^\infty xf(x)dx=\int_{-\infty}^\infty x^3f(x)dx=0
$$

$$
\int_{-\infty}^\infty xd(f(x))=-\int_{-\infty}^\infty x^2f(x)dx
$$

$$
\int_{0}^\infty xf(x)dx = \frac{1}{\sqrt{2\pi}}
$$

$$
\int_{-\infty}^\infty |x|f(x)dx = \sqrt{\frac{2}{\pi}}
$$

**冷知识**：在 x = $\mu±\sigma$ 时, 曲线 y = f (x) 在对应的点处有拐点

## 标准正态分布 X ～N (0,1)

- <font color="red">$\Phi(x)+\Phi(-x)=1$</font>
- 通过<font color="red">$Y=\frac{X-\mu}{\sigma}$</font>，将一般正态分布转化为标准正态分布。

# 离散型随机变量函数的分布

我们已经知道了X的分布，还想知道Y=g(X)的分布，需要将与Y 有关的事件转化成 X 的事件。

<font color="red" size=5>注意合并</font>：

![离散变量](/img/probability-04/离散变量.png)

# 连续型随机变量函数的分布

![法1](/img/probability-04/法1.png)

方法二：

![法2](/img/probability-04/法2.png)

## 反变换定理

我去，这定理太邪门了

![反变换](/img/probability-04/反变换.png)

只要知道随机变量X服从的 分布函数cdf以及其cdf的反 函数，就可以通过计算机所 生成的[0,1]上的均匀分布生 成所需要的分布。
$$
X=F^{-1}(U)
$$
F是已知的，U是计算机模拟的，所以就能产生X的分布

c++算法如下：

```c++
double randomExponential(double lambda){
  double pV = 0.0;
  while(true){ //避免pV=1的情况
    pV = (double)rand()/(double)RAND_MAX;
    if (pV != 1) break;
  }
  pV = (-1.0/lambda)*log(1-pV);
  return pV;
}
```

使用python实现$\lambda =1$时的指数分布：

```python
import numpy as np
import matplotlib.pyplot as plt
i = 0
y=[]
x=[]
while i<1000:
    a=np.random.uniform()
    y.append(-np.log(1-a))
    x.append(i)
    i=i+1
y.sort()
plt.scatter(x, y)
plt.show()
```

![结果图像](/img/probability-04/Figure_1.png)

.
