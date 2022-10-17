---
title: 数据结构02
date: 2022-10-08 22:36:27
tags: 数据结构
index_img: /img/cover/ds.jpg
math: true
---

# 空间复杂度

## 组成

- **指令空间**：不同的编译器将产生不同的程序代码
- **数据空间**：简单变量和常量、结构变量空间、数组空间、动态分配的空间。
- **环境栈空间**：返回地址，形参，变量

## 度量

$$
S(p)=c+S_p
$$

### 固定部分（c）

包含指令空间(即代码空间)、简单变量及定长复合变量所占用空间、常量所占用空间等等。

### 实例特征（Sp）

- **动态分配**的空间

- **递归栈空间**（主要依赖于：局部变量及形式参数所需要的空间。递归的最大深度）

分析程序的空间复杂性的步骤是：先找到实例的特征（m,n之类的），然后估算Sp.

## 注意事项

局部变量及形参的空间不难确定，递归的最大深度，循环的最多次数经常出错。

递归的时候往往多一次，因为最后一次递归是执行退出条件的。

循环有的时候会少一次，因为初值已经赋值一次了，只要进行n-1次就行了。

# 时间复杂度

## 组成

$$
T_p=编译时间+运行时间
$$

## 估算方法

操作计数(operation counts) :找出一个或多个关键操作，确定这些关键操作所需要的执行时间；

步数(step counts):确定程序总的步数。 

其实不准，一个步骤执行n次的时间不一定等于该步骤执行时间的n倍。比如，计算机可以同时进行整数计算和浮点计算。

## Horner法则多项式优化

$$
P(x)=\underset{i=0}{\overset{n}{\sum}}c_ix^i
$$

直接计算需要执行2n次乘法，n次加法；使用Horner法则进行优化
$$
P(x)=(\cdots(c_nx+c_{n-1})x+c_{n-2})*x+\cdots)x+c_0
$$
只需要执行n次乘法，n次加法

```c++
template <class T>
T  horner(T coeff[], int n, const T& x)
{//计算n阶多项式在x处的值，系数为coeff[0:n]
  T value=coeff[n] ; 
  for(int i=1; i<=n; i++) 
    value=value * x + coeff[n-i] ; 
  return value; 
}
```

## 名次排序

先名次计算，再（原地）重排

### 名次计算

算法：循环进行$$C_n^2$$次两两比较，较大的名次加一

## 选择排序（及时终止）

```c++
template<class T>
void selectionSort(T a[], int n){	 
    bool sorted=false;
    for (int size=n; !sorted && (size > 1); size--) {
        int indexOfMax=0;
        sorted=true;
        for (int i=1; i < size; i++){
            //找1:size的最大元素 
            if (a[indexOfMax]<=a[i]) indexOfMax=i;
            else sorted=false;  //无序
        }
        swap(a[indexOfMax], a[size - 1 ]);
    }
}
```

## 冒泡排序（及时终止）

```c++
template<class T>
bool bubble(T a[], int n)
{ 	 //把a[0:n-1]中最大元素冒泡至右端 
  bool swapped=false; //尚未发生交换 
  for (int i=0; i<n-1; i++){
      if (a[i] > a[i+1]) {
      swap(a[i], a[i+1]);
      swapped=true; //发生了交换 
    }
  }
  return swapped;
}
template<class T>
void bubbleSort(T a[], int n)
{//及时终止的冒泡排序 
  for (int i=n; i>1 && bubble(a, i); i--) ;
}
```

还有一些步数什么的不想写了

# 渐进记法

![渐进记法](/img/data-structure-02/渐进记法.png)

1 < logn < n < nlogn < n2 < n3 < 2n < n!

![大O](/img/data-structure-02/O.png)

![结论](/img/data-structure-02/omega.png)

![小o](/img/data-structure-02/小o.png)

![推理规则](/img/data-structure-02/1.png)

折半搜索是O(logn)
