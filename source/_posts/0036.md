---
title: 莫比乌斯反演
date: 2024-02-23
tags: [OI,数学]
toc: true
mathjax: true
---

# 前置知识

## 数论分块

简单来说，含有向下取整除法的求和式子如 $\sum\limits _{i \in \{x,y\}} f_i g{\lfloor \frac n i \rfloor}$，可以发现向下取整的式子取值较少，若对于 $f$ ，我们可以快速区间求和，则运用分配率可以计算。

小结论：使 $\lfloor \frac n i \rfloor = \lfloor \frac n j \rfloor$ 成立的最大的满足 $i \leq j \leq y$ 的值为 $\lfloor \frac n {\lfloor \frac n i \rfloor} \rfloor$。这说明了分块的区间。

## 狄利克雷生成函数（DGF）


对于一个无穷序列 $f$，我们这样定义其狄利克雷生成函数 

$$F_x=\sum\limits_{i \geq 1}\frac {f_i} {i^x}$$

若 $f$ 满足积性，只需要质数处的取值即可表示其DGF。

$$F_x=\prod \limits _ {p \in P} (\sum \limits _{i \geq 0} \frac {f_{p^i}} {p^{ix}})$$ 

感性理解一下积性函数的意义所有的质数的次幂实际上这样相乘后可以得到所有的数。

### 常见函数的DGF

#### 黎曼函数

序列 $[1,1,1,1,...]$ 的 DGF 是 $\zeta _x = \sum \limits _{i \geq 1} \frac 1 {i_x}$ 。

$\zeta$ 被称为黎曼函数。

tips: $\zeta _x$ 仅在 $x > 1$ 时收敛。

另外 $\zeta$ 是积性函数，于是 $\zeta$ 有着另外一种更具启发式以及和质数性质密切相关的表达式。

$$\zeta_x=\prod \limits _{p \in P} \frac 1 {1-p^{-x}}$$ 

#### 莫比乌斯函数

莫比乌斯函数的定义是


$$\mu _x=
\begin{cases}
0& \text{x含有平方因子}\\
(-1)^k & \text{x不含有平方因子，k 为本质不同质因子个数}\\
1& {x \in P}
\end{cases}$$

为积性函数。其 DGF 定义为

$$F_x=\prod \limits _{p \in P} (1-\frac 1 {p^x})=\prod \limits _{p \in P} (1-p^{-x})$$

所以 $\zeta_xF_x=1$，互为倒数。

#### 欧拉函数

$\varphi_n$ 定义为小于等于 $n$ 的整数中与 $n$ 互质的数的个数。

这玩意也是积性函数。

其 DGF 定义为 

$$F_x=\prod \limits _ {p \in P} \sum \limits _ {i\geq 1} (1+{\frac {(p-1)p^{i-1}}{p_{ix}}}) = \prod \limits _{p \in P} \frac {1-p^{-x}} {1-p^{1-x}}$$

回想 $\zeta$ 的定义，我们可以注意到（注意力惊人！）：

$F_x=\frac {\zeta_{x-1}} {\zeta _ {x}}$。

#### 幂函数

函数 $I(n,k) = n^k$ ，其 $DGF$ 为 $\prod \limits _ {p \in P} (\sum \limits _{i>0} \frac {p^{ik}} {p^{ix}})$ 


#### 狄利克雷卷积

~~没想到吧，还有~~。

对于两个数论函数，我们定义其离散意义下狄利克雷卷积为：

$h_x=\sum \limits _{ab=x} f(a) g(b)$ ，其中 $a,b$ 必须为整数。

狄利克雷卷积与狄利克雷生成函数密切相关，我们可以发现，对于两个数论函数 $f,g$ ，二者狄利克雷生成函数之积等价于二者的狄利克雷函数卷积（与**狄利克雷卷积**区分）后的狄利克雷生成函数。

其满足交换律、结合律、分配率、等式的性质，可以结合狄利克雷生成函数观点感性理解。

存在单位元、逆元，其单位元为 $\mu$ 与 $1$ 的狄利克雷卷积。

~~呜呜呜终于写完了~~

## 思路

注意到！如果 $f_n$ 与 $1$ 进行狄利克雷卷积后得到 $g_n$ ,$g_n$ 与 $\mu _n$ 进行狄利克雷卷积可得到 $f_n$ ，于是我们称与 $1$ 进行卷积为莫比乌斯变换，与 $\mu_n$ 卷积为反演（逆变换）。

使用方法：给你一个式子，实际上 $[gcd_{i,j}=1]$ 可以表示为 $\sum \limits _{d|gcd_{i,j}} \mu_d$ ，然后再提出来提出去就可以数论分块了。


