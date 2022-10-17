---
title: 数据结构01
date: 2022-10-08 22:33:30
tags: 数据结构
index_img: /img/cover/ds.jpg
math: true
---

# 递归

## 定义

一个正确的递归函数必须包含：**基础部分**和**递归调用部分**

递归调用过程中：程序的状态(如局部变量、传值形式参数的值、引用形式参数绑定的值以及代码的执行位置等)被保留在递归栈中。

递归工作栈：**局部变量**，**参数**，**返回地址**

## 优点

递归简洁、易编写、易懂、易证明正确性

## 递归改为非递归

递归效率低，重复计算多；改为非递归的目的是提高效率；有些递归可直接用迭代实现非递归，其他情形必须借助栈实现非递归

# 全排列

## 分析

基础部分：n=1时，全排列是他本身

递归部分：n>1时，设n个元素组成集合E
$$
令E_i表示E-\{e_i\}
$$

$$
令e_i.perm(X)表示前缀为e_i，后缀为perm(X)的排列
$$

$$
\therefore perm(E)=\{e_1.perm(E_1),e_2.perm(E_2),\dots ,e_n.perm(E_n)\}
$$

那么问题来了，**如何实现ei.perm(Ei)**?

使用循环依次与前缀元素**交换**，也可以理解为使每个元素都有做前缀的机会。

## 代码

```c++
template<class T>
void permutations(T list[], int k, int m){
    if(k==m){
        for(int i=0;i<=m;i++)
            cout<<list[i];
        cout<<endl;
    }else{
        for(int i=k;i<=m;i++){
            swap(list[k], list[i]);
            permutations(list, k+1, m);
            swap(list[k], list[i]);
        }
    }
}
```

## 图解

![全排列图解](img/data-structure-01/全排列图解.png)

# 子集生成

## 分析

一个有n个元素的集合的子集，可以用含有n个0/1的序列来表示，0表示对应的元素不在子集中，1表示对应的元素在子集中，本质上是n个0/1的排列。

如，{a,b,c}的子集本质上是000，100，010，001，110，101，011，111

**基础部分：**n=0时，子集为空集；

**递归部分：**n>0时，subset(E)={e1+subset(E1),subset(E1)}，第一个元素在与不在两种情况。

## 图解

![生成子集图解](img/data-structure-01/生成子集图解.jpg)

## 代码

```c++
#include<iostream>
using namespace std;
void f(int* h, int* p, char* c, int n) {
	if (n == 0) {
		cout << '{';
		for (int i = 0; h < p; h++) {
			if (*h == 1) {
				cout << c[i]<<',';
			}
			i++;
		}
		cout<<'}' << endl;
		return;
	}
	*p=1; 
	f(h, p+1, c, n - 1);
	*p=0; 
	f(h, p+1, c, n - 1);
}
int main() {
//先输入元素个数n，再输入n个元素的代号
	int n; cin >> n;
	if (n < 0) {
		cout << "n不能为负";
		return 0;
	}
	int* a = new int[n];
	char* c = new char[n];
	for (int i = 0; i < n; i++)
		cin >> c[i];
	f(a, a, c, n);
	return 0;
}
```

