# 牛顿迭代法

## 概述

&emsp;&emsp; 牛顿迭代法顾名思义是由牛顿提出的一种用于解决方程近似解的方法。本文主要介绍牛顿迭代法的原理及其在求解平方根中的应用。

## 原理

&emsp;&emsp; 假设方程$f(x)=0$的一个近似解是$x_k$，$f(x)$在$x_k$处的一阶泰勒展开为$f(x)=f(x_k)+f\'(x_k)(x-x_k)+o(x)$，则$f(x_k)+f\'(x_k)(x-x_k) \approx f(x)$
从而得到$f(x_k)+f\'(x_k)(x-x_k)=0$，解得一个新的近似解设为$x_{k+1}$,即：

\begin{equation}
	x_{k+1}=x_k-\frac{f(x_k)}{f\'(x_k)}
\end{equation}

上式即为牛顿迭代公式。

## 求解平方根

&emsp;&emsp; 设$f(x)=x^2-n=0$，这个方程的解就是$\sqrt{n}$，根据牛顿迭代公式，我们可以得到如下迭代式：

\begin{equation}
	x_{k+1}=x_k-\frac{x_{k}^2-n}{2x_k}=\frac{x_k+\frac{n}{x_k}}{2}
\end{equation}

代码如下：

```cpp
double newton_sqrt(double n){
	const double eps = 1e-15;//确定精度
	double x = 1;
	while(true){
		double nx = (x+n/x)/2;
		if(abs(nx-x)<eps) break;
		x=nx;
	}
}
```

当然，我们也可以通过二分法进行求解。代码如下：

```cpp
double dichotomy_sqrt(double n){
	double left=0;
	double right=max(1,n);
	while(abs(left-right)>1e-5){
		x=(left+right)/2
		if(x*x>n) right=x;
		else left=x;
	}
	return x;
}
```
其算法复杂度都为$O(log_{2}n)$.


## 参考

&emsp;&emsp;[oi-wiki-牛顿迭代法](https://oi-wiki.org/math/numerical/newton/)

&emsp;&emsp;[牛顿法](https://en.citizendium.org/wiki/Newton%27s_method#Convergence_analysis)

