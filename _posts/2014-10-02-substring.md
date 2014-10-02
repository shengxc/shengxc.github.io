---
layout: post
title: "各种子序列和子串"
description: ""
category:
tags: ["算法"]
---
#最长公共子序列#

##问题描述##

子序列：一个给定序列的子序列就是该给定序列中去掉零个或多个元素，剩下的部分。
公共子序列：给定两个序列$X$和$Y$，如果序列$Z$即是$X$的子序列，也是$Y$的子序列，那么称序列$Z$是$X$和$Y$的公共子序列。
最长公共子序列（LCS）问题是给定两个序列，求这两个序列最长的公共子序列。

##分析##

该问题可用动态规划解决，最优子结构描述如下：

设$X=<x_1,x_2,...,x_m>$和$Y=<y_1,y_2,...,y_n>$为两个序列，并设$Z=<z_1,z_2,...z_k>$是$X$和$Y$的任意一个最长子序列。

1. 如果$x_m=y_n$，那么$z_k=x_m=y_n$而且$$Z_{k-1}$$是$$X_{m-1}$$和$$Y_{n-1}$$的LCS。
2. 如果$x_m\neq y_n$，那么$z_k\neq x_m$蕴含$Z$是$X_{m-1}$和$Y$的LCS。
3.  如果$x_m\neq y_n$,那么$z_k\neq y_n$蕴含$Z$是$X$和$Y_{n-1}$的一个LCS。

由上述最优子结构可以很容易看出LCS问题中得重叠子问题性质。为找出$X$和$Y$的一个LCS,可能需要找出$X$和$$Y_{n-1}$$的一个LCS以及$$X_{m-1}$$和$Y$的一个LCS。但这两个子问题都包含着找出$$X_{m-1}$$和$$Y_{n-1}$$的一个LCS的子子问题。
定义$c[i,j]$为序列$X_i$和$y_i$的一个LCS的长度，由上述最优子结构可得递归式
$$c[i,j]=\left\{
\begin{array}{rcl}
0 & & {i = 0 或 j = 0}\\
c[i-1,j-1]+1 & & {i,j>0且x_i = y_j}\\
max(c[i,j-1],c[i-1,j]) & & {i,j>0且x_i \neq y_j}
\end{array}\right.$$

###代码###

	def LCSubsequenceLength(x,y):
	    m = len(x)
	    n = len(y)
	    c = [[0 for col in range(n + 1)] for row in range(m + 1)]
	    for i in range(m):
	        for j in range(n):
	            if x[i] == y[j]:
	                c[i + 1][j + 1] = c[i][j] + 1
	            else:
	                if c[i][j + 1] >= c[i + 1][j]:
	                    c[i + 1][j + 1] = c[i][j + 1]
	                else:
	                    c[i + 1][j + 1] = c[i + 1][j]
	    return c[m][n]

<br/><br/>

#最长公共子串#

##问题描述##

 给定两个序列，求出这两个序列的最长公共子串。其中子串是指连续的子序列。

##分析##

动态规划方法：
设两个序列$X$和$Y$长度分别为$m$和$n$；他们的前缀字符串分别为$X[0:i],Y[0:j]$；$X[0:i]$和$Y[0:j]$的最长公共后缀为$LCSuffix(X[0:i],Y[0:j])$。那么$X$和$Y$的最长公共子串$$LCStr(X,Y)=max_{0 \leq i \leq m,0 \leq j \leq n}LCSuffix(X[0:i],Y[0:j])$$
上述方法中，最长公共后缀可由动态规划方法求得，其递推方法如下
$$LCSuffix(X[0:i],Y[0:j])=\left\{
\begin{array}{rcl}
LCSuffix(X[0:i-1],Y[0:j-1])+X[i] & & X[i]=Y[j]\\
"" & & X[i] \neq Y[j]
\end{array} \right. $$

###代码###

	def LCSubstringLength(x,y):
	    m = len(x)
	    n = len(y)
	    c = [[0 for col in range(n + 1)] for row in range(m + 1)]
	    for i in range(m):
	        for j in range(n):
	            if x[i] == y[j]:
	                c[i + 1][j + 1] = c[i][j] + 1
	            else:
	                c[i + 1][j + 1] = 0
	    ret = 0
	    for i in range(m + 1):
	        for j in range(n + 1):
	            if c[i][j] > ret:
	                ret = c[i][j]
	    return ret

<br/><br/>

#最长递增子序列#

##问题描述##

给定一个序列，求该序列中长度最大且所有元素以递增方式排序的子序列。

##分析##

###方法一###

先对输入序列排序进行排序，然后对排序后的序列以及原序列求最长公共子序列。

###方法二###

动态规划，定义$L(j)$为序列$X$的以第$j$个元素结尾的子序列的最长递增子序列，那么$$L(j)=max_{i<j,X_i<X_j}L(i)+1$$。如果序列$X$的长度为n，那么，$X$的最长递增子序列(lis)为$max_{0\leq i<n}L(i)$。

####代码####

	def lis(x):
	    n = len(x)
	    l = [1 for i in range(n)]
	    for j in range(1,n):
	        for i in range(j):
	            if(x[j] >= x[i] and l[j] < l[i] + 1):
	                l[j] = l[i] + 1
	    return max(l)

<br/><br/>

#最大连续子序列和#

##问题描述##

输入一个整数序列，求这个整数序列所有连续子序列中的最大和。

##分析##

动态规划：对于长度为$n$的序列$X$，定义函数$f(k)$为$X[1:k]$的最大连续后缀和。则对于$f(k)$有
$$f(k)=\left\{ 
\begin{array}{rcl} 
X_k & & f(k-1)+X_k<X_k即f(k-1)<0\\
f(k-1)+X_k & & f(k-1)+X_k\geq X_k即f(k-1)\geq 0
\end{array} \right. $$。最大连续子序列和为$mss=max_{1\leq k\leq n}f(k)$

###代码###

	def mss(x):
	    n = len(x)
	    l = [0 for i in range(n + 1)]
	    for i in range(n):
	        if l[i] < 0:
	            l[i + 1] = x[i]
	        else:
	            l[i + 1] = l[i] + x[i]
	    return max(l)

上述代码中得l数组可以由两个变量代替以节省空间，代码如下：

	def mss(x):
	    n = len(x)
	    maximum = 0
	    tmpmax = 0
	    for i in range(n):
	        if tmpmax < 0:
	            tmpmax = x[i]
	        else:
	            tmpmax += x[i]
	        if tmpmax > maximum:
	            maximum = tmpmax
	    return maximum

<br/><br/>

#最短连续子序列#

##问题描述##

给定长度为$n$的正数列$X$，以及整数$S$。求出总和不小于$S$的$X$的连续子序列的长度的最小值。如果解不存在，则输出0。

##分析##

###解法一###

令$sum(i)$表示$X$的长度为$i$的前缀序列的和，即$$sum(i)=\sum_{0\leq j<i}X_j$$，那么，$X$的连续子序列$$X_s+X_{s+1}+...+X_{t-1}=sum(t)-sum(s)$$。因此，预先以$O(n)$的时间计算好$sum$的话，就可以以$O(1)$的时间计算区间上得总和，这样一来，由于$X$的所有元素都大于零，起点$s$确定后，对于所有$s<t<t'$，有$sum(t)-sum(s)<sum(t')-sum(s)$，因此，可以用二分搜索快速地确定使序列和不小于$S$的结尾$t$的最小值。时间复杂度$O(nlogn)$

####代码####

	def lower_bound(arr,s):
	    lb = -1
	    ub = len(arr)
	    while ub - lb > 1:
	        mid = (lb + ub) / 2
	        if arr[mid] >= s:
	            ub = mid
	        else:
	            lb = mid
	    return ub
	def Subsequence(X,S):
	    n = len(X)
	    sum = range(n + 1)
	    sum[0] = 0
	    for i in range(n):
	        sum[i+1] = sum[i] + X[i]
	    if sum[n] < S:
	        return 0
	    res = n
	    s = 1
	    while sum[n] - sum[s-1] >= S:
	        t = lower_bound(map(lambda x:x-sum[s-1],sum[s:n + 1]),S)
	        res = min(res,t + 1)
	        s = s + 1
	    return res

###解法二###

尺取法：设以$X_s$开始的总和大于$S$的连续子序列为$$X_s,X_{s+1},...,X_{t-1}$$，此时$$X_{s+1}+...+X_{t-2}<X_{s}+...+X_{t-2}<S$$，所以，从$$X_{s+1}$$开始，总和最初超过$S$的连续子序列如果是$$X_{s+1},X_{s+2},...,X_{t'-1}$$的话，则必然有$$t\leq t'$$。利用这一性质可以设计出如下算法：

> 1）以$s=t=sum=0$初始化<br/>
> 2）只要依然有$sum<S$，就不断将$sum$增加$X_t$，并将$t$加1<br/>
> 3）如果2）中无法满足$sum\geq S$，则终止。否则，更新$res=min(res,t-s)$<br/>
> 4）将$sum$减去$X_s$，$s$增加1，然后回到2）

由于这个算法中$t$最多变化$n$次，因此，只需$O(n)$的复杂度就可以求解。

####代码####

	def Subsequence(X,S):
	    n = len(X)
	    res = n + 1
	    s = 0
	    t = 0
	    sum = 0
	    while True:
	        while t < n and sum < S:
	            sum += X[t]
	            t += 1
	        if sum < S:
	            break
	        res = min(res,t - s)
	        sum -= X[s]
	        s += 1
	    if res > n:      #解不存在
	        res = 0
	    return res

