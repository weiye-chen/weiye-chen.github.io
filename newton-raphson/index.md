# Newton-Raphson method

## Introduction

&emsp;The Newton iteration method, as the name suggests, is a method proposed by Newton for solving approximate solutions to equations. 
This article mainly introduces the principle of Newton's iterative method and its application in solving square roots.

## Theory

&emsp;Assuming an approximate solution of equation $f(x)=0$ is $x_k$，the first-order Taylor expansion of $f(x)$ in $x_k$ is $f(x)=f(x_k)+f\'(x_k)(x-x_k)+o(x)$，so $f(x_k)+f\'(x_k)(x-x_k) \approx f(x)$.
In order to obtain $f(x_k)+f\'(x_k)(x-x_k)=0$，obtain a new approximate solution set to $x_{k+1}$, assum：

\begin{equation}
	x_{k+1}=x_k-\frac{f(x_k)}{f\'(x_k)}
\end{equation}

The above equation is the Newton iterative formula.

## Solve square root

&emsp;&emsp; Assuming $f(x)=x^2-n=0$，The solution to this equation is $\sqrt{n}$. According to Newton's iterative formula, we can obtain the following iterative formula:

\begin{equation}
	x_{k+1}=x_k-\frac{x_{k}^2-n}{2x_k}=\frac{x_k+\frac{n}{x_k}}{2}
\end{equation}

The code is as follows：

```cpp
double newton_sqrt(double n){
	const double eps = 1e-15;//precision
	double x = 1;
	while(true){
		double nx = (x+n/x)/2;
		if(abs(nx-x)<eps) break;
		x=nx;
	}
}
```

Of course, we can also solve through the dichotomy method. The code is as follows:

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
The algorithm complexity is $O(log_{2}n)$.


## Reference

&emsp;&emsp;[oi-wiki Newton](https://oi-wiki.org/math/numerical/newton/)

&emsp;&emsp;[citizendium Newton's method](https://en.citizendium.org/wiki/Newton%27s_method#Convergence_analysis)

