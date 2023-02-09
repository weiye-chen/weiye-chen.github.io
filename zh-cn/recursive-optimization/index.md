# 递归优化

## 递归算法

通常来说，递归算法的运行效率很低，容易造成栈溢出。有关递归算法的详细介绍见[递归详解](https://thinkerall.github.io/zh-cn/recursion/)。

## 递归算法优化策略

递归优化策略通常有两种。

1. 时间复杂度的优化；
2. 空间复杂度的优化。

本文以斐波那契数列为例进行讲解。

### 时间复杂度的优化 

&emsp;&emsp;不难看出，在计算f(n)=f(n-1)+f(n-2)时，要先计算f(n-1)和f(n-2),其中
f(n-1)=f(n-2)+f(n-3)，f(n-2)=f(n-3)+f(n-4)。在计算过程中，f(n-2)被计算了两次。
因此在，斐波那契数列的计算过程中存在着大量的冗余计算。

&emsp;&emsp;为了消除上述情况中的重复计算，其中一个想法是将中间结果存储在缓存中，以便我们以后可以重用它们，而不需要重新计算。
这个想法也被称为**记忆化**，这是一种经常与递归一起使用的技术。
因此，我们可以使用数组或者哈希表来存储中间计算结果，以减少计算次数。(空间换时间)

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

时间复杂度O(2^n)-\>O(n)，该优化算法的本质其实是**动态规划**的思想。

### 空间复杂度的优化

&emsp;&emsp;递归调用在系统调用栈上会产生额外空间，如果递归调用层级很深，程序执行过程中很可能导致栈溢出。
针对这种情况，有一种称为**尾递归**的特殊递归，它可以控制递归导致空间开销的影响。

> **尾递归**
>
> &emsp;&emsp;尾递归是一种特殊的递归方式。
> 顾名思义，尾递归就是递归调用为递归函数中的最后一条指令，并且在函数中应该只有一次递归调用。

> **尾调用**
>
> &emsp;&emsp;函数调用会在内存形成一个"**调用记录**"，又称"**调用帧**"**（call frame）**，保存**调用位置**和**内部变量**等信息。
> 如果在函数A的内部调用函数B，那么在A的调用记录上方，还会形成一个B的调用记录。
> 等到B运行结束，将结果返回到A，B的调用记录才会消失。
> 如果函数B内部还调用函数C，那就还有一个C的调用记录栈。
> 以此类推，所有的调用记录，就形成一个"**调用栈**"**（call stack）**。
>
> &emsp;&emsp;尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用记录，
> 因为调用位置、内部变量等信息都不会再用到了，
> 只要直接用内层函数的调用记录，取代外层函数的调用记录就可以了。

```
int fibonacciTail(int n, int acc, int cal){
//acc充当收集器的左右，收集上一次运行栈的返回值，因为之后栈空间会被回收
//cal是每一次递归的计算
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
```

&emsp;&emsp;上面的函数多了2个参数，一个起到收集器（accumulator)的作用，记录每上一次栈的返回值，因为原来栈的空间会被下一层递归覆盖。
还有一个参数就是每次递归的操作了，因为是斐波那契，所以这里是相加。
尾递归的调用方法也与原来的不一样。因为斐波那契的初始值 ( 0，1 ，1，2 ，……)，
所以以下的调用，acc 要用初始值 0，cal 也要用第二位的值这里也是1，
调用的方法就是 fibonacciTail(n, 0, 1）。

#### 函数改写

&emsp;&emsp;尾递归函数优化后函数需要的参数不够直观。因此，我们通常使用下面两种方法进行改写。

&emsp;&emsp;1.柯里化（currying），将多参数的函数转换成单参数的形式

```
int fibonacciTail(int n, int acc, int cal){
//acc充当收集器的左右，收集上一次运行栈的返回值，因为之后栈空间会被回收
//cal是每一次递归的计算
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
int fibonacci(int n){
	return fibonacciTail(n, 0, 1);
}
```

&emsp;&emsp;2.函数参数初始化

```
int fibonacciTail(int n, int acc=0, int cal=1){
//acc充当收集器的左右，收集上一次运行栈的返回值，因为之后栈空间会被回收
//cal是每一次递归的计算
	if(n==0) return acc;
	if(n==1) return cal;
	retrun fibonacciTail(n-1, cal, acc+cal);
}
```
## 参考

&emsp;&emsp;[全面理解递归](https://zhuanlan.zhihu.com/p/150562212)

&emsp;&emsp;[递归（Recursion）的两种优化方法](https://blog.csdn.net/HEYUJIEBOY/article/details/76692870)

&emsp;&emsp;[递归如何优化-尾递归优化](https://cloud.tencent.com/developer/article/1694405)
