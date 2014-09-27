---
layout: post
title: "各种子序列和子串"
description: ""
category:
tags: ["算法"]
---

#最长公共子串#

##问题描述##

 给定两个字符串，求出这两个字符串的公共子串。字符串的子串是指该字符串中连续的一段子字符串。

##分析##

动态规划方法：
设两个字符串$S$和$T$长度分别为$m$和$n$；他们的前缀字符串分别为$S[0:i],T[0:j]$；$S[0:i]$和$T[0:j]$的最长公共后缀为$LCSuffix(S[0:i],T[0:j])$。那么$S$和$T$的最长公共子串$$LCS(S,T)=max_{0 \leq i<m,0 \leq j<n}LCSuffix(S[0:i],T[0:j])$$
上述方法中，最长公共后缀可由动态规划方法求得，其递推方法如下
$$LCSuffix(S[0:i],T[0:j])=\left\{\begin{array}{}LCSuffix(S[0:i-1],T[0:j-1])+S[i] && if S[i]=T[j]\\"" && ifS[i] \neq T[j]\end{array} \right. $$