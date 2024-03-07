---
title: 数论算法
date: 2024-03-03
tags: [OI,数论,数学]
toc: true
mathjax: true
---

整数往往具有很优美的性质，尤其在一些计数题中，式子本身定义就与整除等概念密不可分。而这种题大量的需要数论知识才能进行处理。

**前置知识**：整除，因数，带余除法，最大公因数，最小公倍数，欧几里得算法（又称辗转相除法），素数，合数，同余，算术基本定理，乘法逆元，数论函数，积性函数的定义。

## 扩展欧几里得算法（EXGCD）

对于方程 $ax+by=c$ ，可以证明该方程有解当且仅当 $gcd(a,b)|c$ 。所以实际上问题转换为了求解 $ax+by=gcd(a,b)$ 的解，考虑求解最大公因数经典算法辗转相除法，其定义基本如下

$$
gcd(a,b)=gcd(b,a \bmod b)
$$

此算法本身是一个递归的过程，最深层即 $gcd(a,0)$ ，我们定义此时 $gcd(a,0)=a$ ，即可构造出初始解：

$$
\left\{
    \begin{aligned}
        x=1 \\
        y=0 
    \end{aligned}
\right.
$$

然后考虑如何从 $gcd(b,a \bmod b)$ 转移到 $gcd(a,b)$ ，$a \bmod b$ 可以表示为 $a-(\lfloor \frac a b \rfloor \times b)$ 。

所以设之前的解为 $x_1,y_1$ ，转换后的解为 $x_2,y_2$ ，可发现。

$$
ax_2+by_2=bx_1+(a \bmod b)y_2
$$

将以上取模的式子带入并进行一些化简后解得：

$$
\left\{
    \begin{aligned}
        &x_2=y_1 \\
        &x_2=x_1-\lfloor \frac a b \rfloor\times y_1 
    \end{aligned}
\right.
$$

注意计算过程中数据类型。

## 中国剩余定理（CRT）

若需要求解类似于如下的模线性方程组：

$$
\left\{
    \begin{aligned}
        x&\equiv a_1 (\bmod~ b_1) \\
        x&\equiv a_2 (\bmod~ b_2) \\
         &~~\vdots \\
        x&\equiv a_n(\bmod ~b_n) \\
    \end{aligned}
\right.
$$

我们可以考虑对于每一个方程构造一个数 $ans_i$ ，使得其对于所有的 $j$ 都满足 $ans_i \equiv [i=j]\times a_j \pmod {b_j}$ 。

实际上当且仅当所有的 $b$ 两两互质时，这样的做法是可行的，由于这个算法可完全被扩展中国剩余定理替代，故省略详细步骤。

## 扩展中国剩余定理（EXCRT）

对于模数不互质的情况，就需要更一般的做法了，我们先想办法能不能做 $n=2$ 的情况，即合并两个方程，然后采用这种方式求解一般情况。

对于方程 $i,j$ ，发现实际上是在求解：

$$
m_ip+a_i=m_jq+a_j
$$

稍作转换

$$
m_ip-m_jq=a_j-a_i
$$

exgcd即可。

注意判无解。

## 卢卡斯定理(Lucas)

求解组合数可以采用线性预处理阶乘以及逆元，然后 $\mathcal{O}(1)$ 查询，但是若模数较小，我们需要求的组合数大参数已经大于模数了，上述算法就会出问题，而我们引入卢卡斯定理即可解决：

$$
\binom {n} {m} \bmod p=\binom{\lfloor \frac n p \rfloor}{\lfloor \frac m p \rfloor}\times \binom{n \bmod p}{m \bmod p} \bmod p
$$

该公式仅在 $p$ 为质数时成立。

证明仅需考虑其 $\binom {n}{m}$ 在二项式上的意义。

我们可以发现 $\binom {p}{n} \bmod p$ 当且仅当 $n=0$ 或 $n=p$ 时为 $1$ ,其余时候都为 $0$ 。

$$
\begin{aligned}
(a+b)^p & = \sum\limits _ {n=0} ^ p a^n b^{p-n} \\
&\equiv   \sum\limits _ {n=0} ^ p [n=0 \vee n=p] a^n b^{p-n} (\bmod ~p)\\
&\equiv   a^p+b^p (\bmod ~p) 
\end{aligned}
$$

然后分解 $(1+x) ^ n$ 得到：

$$
\begin{aligned}
(1+x)^n & \equiv (1+x)^{p\lfloor \frac n p \rfloor} \times (1+x) ^{n \bmod  p} \\
&\equiv (1+x^p) ^{\lfloor \frac n p \rfloor} \times (1+x) ^{n \bmod  p} \\
\end{aligned}
$$

对应一下发现得证。

运用到质数的条件实际上是在证明 $\binom {p} {n} \bmod p$ 时的取值是用到的。

## 扩展卢卡斯定理(exLucas)

我们考虑分解非质数模数 $p$ ，考虑采用 CRT 进行合并答案。

考虑问题转换后变成求解 $\binom n m \bmod p^a$。

考虑定义式：

$$
\binom n m=\frac {n!} {m!(n-m)!} \bmod p^a 
$$

然而，分母的逆元不一定存在因为不能直接求。

然而我们可以乱搞，把 $p$ 的质因子全部提出来。

最终就在求这个东西了 $\frac {n!} {p^x} \bmod p^a$。

也就是说我们不考虑质因子 $p$ ，然后最后全部提出来单独计算。

~~还是不会算~~

考虑 $n!$

若 $n=15，p=2^2=9$

$$
n!=15\times 14\times 13\times 12\times 11\times 10\times 9\times 8\times \\7\times 6\times 5\times 4 \times 3 \times 2 \times 1
$$

我们直接提取所有与 $p$ 不互质的数：

$n!=2^7 \times (7!) \times \prod \limits _{i=1}^{n} [gcd_{2,i}=1]i$

前面一坨快速幂，中间一坨递归下去，烦的就是后面那坨。

发现模一下 $p^a$ 之后具有循环节，暴力求解循环部分和剩余部分。

所以`exLucas`不是多项式时间复杂度的。

时间复杂度看实现，询问较多，可以考虑预先处理暴力循环节和剩余部分以及预先分解模数。

## 杜教筛

考虑计算一些数论函数的前缀和问题，以低于线性时间复杂度的代价。

设我们要求的前缀和函数为 $f$ ，其前缀和记为 $S$

考虑狄利克雷卷积的一些性质：

$g$ 是任意一个数论函数。


$$
\begin{aligned}

\sum\limits _{i=1}^{n}(f*g)(i)& = \sum\limits _{i=1} ^{n} \sum\limits_{d|i} g(d)f(\frac i d) \\
&=\sum\limits _{i=1}^{n} g(i)S(\lfloor \frac n i \rfloor)

\end{aligned}
$$

考虑变换求和顺序，发现所有的 $g(x)f(y)$ 对 $xy\leq n$ 做贡献，所以枚举 $x,y$ 即可。

考虑把 $g$ 弄掉，发现：

$$
\begin{aligned}
g(1)S(n)&=\sum\limits_{i=1}^{n} g(i)S(\lfloor \frac n i \rfloor) -\sum\limits_{i=1}^{n} g(i)S(\lfloor \frac n i \rfloor) \\
&=\sum\limits_{i=1} ^{n} (f*g)(i)-\sum\limits_{i=1}^{n} g(i)S(\lfloor \frac n i \rfloor) 
\end{aligned}
$$

假如可以快速计算两个部分即可计算前缀和。

另外第二块一般利用数论分块进行求解，需要能快速求解 $g$ 的前缀和。

直接这样计算为 $\mathcal {O} (n^{\frac 3 4} )$ 。

一般 $1s$ 内能过掉 $2^{31}$ 左右的数据，这种筛的时间复杂度仅供建议，具体参考模板题速度。

对于莫比乌斯函数选择 $g(n)=1$。

对于欧拉函数选择 $g(n)=1$
