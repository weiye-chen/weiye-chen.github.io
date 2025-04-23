# Recursion  Optimization 

## Recursive Algorithm

&emsp;Generally speaking, recursive algorithms are inefficient and easy to cause stack overflow.  
For a detailed introduction of recursive algorithm, see [Recursion Details](https://thinkerall.github.io/recursion/)

## Recursive algorithm optimization strategy

&emsp;There are usually two recursive optimization strategies.

1. Optimization of time complexity;
2. Optimization of spatial complexity.

This article takes Fibonacci sequence as an example to explain.

### Optimization of time complexity 

&emsp;It is not difficult to see that when calculating f(n)=f(n-1)+f(n-2), we should first calculate f(n-1) and f(n-2), 
including f(n-1)=f(n-2)+f(n-3)and f(n-2)=f(n-3)+f(n-4). In the calculation process, f(n-2) is calculated twice. 
Therefore, there are a lot of redundant calculations in the calculation process of Fibonacci sequence. 

&emsp;In order to eliminate the double calculation in the above cases, 
one idea is to store the intermediate results in the cache 
so that we can reuse them later without recalculation. 
This idea is also known as **memorization**, which is a technology often used with recursion. 
Therefore, we can use arrays or hash tables to store intermediate calculation results to reduce the number of calculations.(Space for Time)

```
int fibonacci(int n){
	if(n < 0)
		return 0;
	if(n=0 || n=1)
		return n;
	int ret[n];
    ret[0] = 0;
    ret[1] = 1;
	for(int i=2; i<=n; i++){
		ret[i]=ret[i-1]+ret[i-2];
	}
	return ret[n];
}
```

&emsp;Time complexity is from O(2^n) to O(n). 
The essence of the optimization algorithm is actually the idea of **dynamic programming**.

### Optimization of spatial complexity

&emsp;Recursive invocations will generate extra space on the system call stack. 
If the recursive call level is very deep, it is likely to cause stack overflow during program execution. 
In this case, there is a special recursion called **tail recursion**, 
which can control the impact of space overhead caused by recursion. 

> **Tail recursion**
>
> &emsp;Tail recursion is a special way of recursion. 
As the name implies, tail recursion is that the recursive call is the last instruction in the recursive function, 
and there must be only one recursive call in the function.

> **Tail call**
>
> &emsp;The function call will form a "**call record**"(or "**call frame**" ) in memory,
and save **call location** and **internal variables** and other information.
If function B is called inside function A, a call record of B will be formed above the call record of A.
The call record of B will not disappear until B finishes running and returns the result to A.
If function B calls function C internally, there is also a call record stack for C.
By analogy, all call records form a "**call stack**".
>
> &emsp;Because it is the last step of the function, tail call is not necessary to keep the call record of the outer function. 
Because the call location, internal variables and other information will not be used, 
it just replace the call record of the outer function with the call record of the inner function.

```
int fibonacciTail(int n, int acc, int cal){
//acccollect the return value of the last run of the stack, 
//because the stack space will be reclaimed later.
//cal is every recursive calculation.
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
```

&emsp;The above function has two additional parameters,  
one of which acts as an accumulator and records the return value of each previous stack, because the space of the original stack will be covered by the next layer of recursion. 
Another parameter is the operation of each recursion.  
Because it is Fibonacci, here is the addition. 
The calling method of tail recursion is also different from the original one.Because the initial value of Fibonacci is f(0,1,1,2,…), 
for the following calls, acc is 0 and cal is 1 here. 
The method called is fibonacciTail(n,0,1). 

#### Function rewrite

&emsp;The required parameters after the optimization of tail recursion are not intuitive enough. 
Therefore, we usually use the following two methods to rewrite.

&emsp;1.Currying (Convert multi-parameter functions to single-parameter forms)

```
int fibonacciTail(int n, int acc, int cal){
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
int fibonacci(int n){
	return fibonacciTail(n, 0, 1);
}
```

&emsp;2.Function parameter initialization

```
int fibonacciTail(int n, int acc=0, int cal=1){
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
```
## Reference

&emsp;[Comprehend recursion](https://zhuanlan.zhihu.com/p/150562212)

&emsp;[Two optimization methods of Recursion](https://blog.csdn.net/HEYUJIEBOY/article/details/76692870)

&emsp;[How to optimize recursion - Tail Recursion Optimization](https://cloud.tencent.com/developer/article/1694405)
