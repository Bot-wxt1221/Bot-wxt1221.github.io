---
title: 多项式
date: 2024-03-21
tags: [OI,多项式]
toc: true
mathjax: true
---

# 多项式

## 基础

多项式常作为生成函数的形式，学会多项式相关操作可以直接快速操纵生成函数。

多项式环可以在一个环上定义，元素 $f$ 成为环 $R$ 上的多项式，表示为一个序列，表示系数。

$$
f(x)=f_0+f_1x+f_2x^2+...+f_nx^n
$$

$x$ 在式子内无实际意义，用于标识系数位置。

部分记号：记 $[x^k]f(x)$ 为 $f(x)$ 在 $k$ 次项上的系数。

我们称一个多项式的次数或度为最高此项的次数。

幂级数的定义与上式类似，项数为无穷项，可以认为多项式为高次项为 $0$ 的幂级数。

多项式乘法的定义为：

$$
f(x)\times g(x)=\sum\limits_{i=0}^{m}\sum\limits_{j=0}^n a_ib_jx_{i+j}
$$

多项式乘法根据环 $R$ 的性质，若其为交换环，则满足交换律。若其为幺环，则多项式存在单位元。根据环的性质同样满足结合律，对加法满足分配律。

接下来对一些简单概念概括一下：

**复合**：展开  $f(g(x))$ 的形式，在存在的情况下满足结合律。若 $R$ 为幺环，存在单位元 $f(x)=x$

**乘法逆元**：可证多项式的倒数能展开当且仅当其常数项非 $0$ ，形式为一个幂级数，一般求其前 $n$ 项的系数。

**复合逆**：对复合运算定义逆运算，简单来说是满足 $g(f(x))=f(g(x))=x$ 的幂级数。

**多项式除法**：对于多项式 $f(x),g(x)$ 找到 $Q(x),R(x)$ 满足$\deg R<\deg g$ 且满足 $f(x)=g(x)Q(x)+R(x)$ 。 $Q(x)$ 为商，$R(x)$ 为余数，若余数 $R(x)=0$ 则说明 $g(x)$ 能整除 $f(x)$

**模多项式**：多项式除法满足如下性质：$f(x)\equiv R(x)~(\bmod ~ g(x))$，也就是说对于多项式的任意一根，也就是其 $0$ 点上，$f(x),R(x)$ 在此处点值相同。

多项式和数论息息相关，因为多项式的性质和整数域不谋而合。

生成函数应用并不在本文讨论范围内。

但是给出定义：


1.  普通生成函数：$k_n(x)=x^n$。
2.  指数生成函数：$k_n(x)=\dfrac{x^n}{n!}$。
3.  狄利克雷生成函数：$k_n(x)=\dfrac{1}{n^x}$。

接下来是常见算法介绍：

## 快速傅里叶变换(FFT)

考虑计算二函数 $f(x),g(x)$ 离散意义下卷积：

$$
(f*g)(x)=\sum\limits_{i=0}^x f(i)*g(x-i)
$$

是不是和多项式乘法很像，实际上 $f(x),g(x)$ 的生成函数的乘积等于 $(f*g)(x)$ 的生成函数。

但是无论怎么说你的时间复杂度是 $\mathcal{O}(n^2)$ 的，~~你怎么做？~~。

傅里叶变换：傅里叶变换是处理信号的方法，我们不多赘述，但是傅里叶变换的卷积定理告诉函数我们变换前的卷积等于变换后的乘积。

对于函数 $f(x)$ ，其离散傅里叶变换（函数值域是离散的）为：

$$
\mathcal{F}f(x)=\sum\limits_{i=0}^nf(i)e^{-i\frac{2\pi}{n+1}x}
$$

你会发现实际上是在算 $f(x)$ 的生成函数在特定的也就是一坨坨关于 $e$ 的一个次幂处的点值，还是 $\mathcal{O}(n^2)$ ，其中 $i$ 为任意实数。

进一步优化，考虑所带入点值的特殊性。

发现采用单位根正好符合以上形式，并且大部分性质可以在复平面上理解。

举出需要用的性质。

$$
\begin{align}
\omega_n^n&=1\\
\omega_n^k&=\omega_{2n}^{2k}\\
\omega_{2n}^{k+n}&=-\omega_{2n}^k
\end{align}
$$

考虑分治，根据项次数划分为两个部分。

设原多项式为 
$$
f(x)=a_0 + a_1x + a_2x^2+a_3x^3+a_4x^4+a_5x^5+a_6x^6+a_7x^7
$$

划分为两个部分，建立两个新函数：

$$
\begin{aligned}
G(x) &= a_0+a_2x+a_4x^2+a_6x^3\\
H(x) &= a_1+a_3x+a_5x^2+a_7x^3
\end{aligned}
$$

则 $f(x)=G(x^2) + x  \times  H(x^2)$

发现根据 $(3)$ 可以得到：

$$
\begin{aligned}
f(\omega_k^n) &=G((\omega_n^k)^2)+\omega_n^k\times H((\omega_n^k)^2)\\
&=G((\omega_n^{2k}))+w_n^k\times H((\omega_n^{2k}))\\
&= G(\omega_{n/2}^k) + \omega_n^k  \times H(\omega_{n/2}^k)
\end{aligned}
$$

$$
\begin{aligned}
f(\omega_n^{k+n/2}) &= G(\omega_n^{2k+n}) + \omega_n^{k+n/2}  \times H(\omega_n^{2k+n}) \\
&= G(\omega_n^{2k}) - \omega_n^k  \times H(\omega_n^{2k}) \\
&= G(\omega_{n/2}^k) - \omega_n^k  \times H(\omega_{n/2}^k)
\end{aligned}
$$

递归下去即可，注意长度必须一致，所以最开始多项式即补齐到 $2^m$ ，高次项补 $0$ 即可。

看着一大堆复数 `double` 运算，你不认为这个东西常数大吗？

考虑改非递归，发现按奇偶排序，实际上是重复比较二进制最后一位，按翻转对称二进制位即可解决顺序问题。

另外发现计算过程中不需要辅助数组因为访问后后面不会用了，考虑上文的计算过程既可以了。

然后发现还要转换回来，也就是逆变换。

管他三七二十一，考虑转换后的序列或者说函数的生成函数 $A(x)$，直接代入单位根的倒数（这群人怎么想到这么天才的办法），直接代入表达式计算其展开形式：

$$
\begin{aligned}
A(w_n^{-k})&=\sum_{i=0}^{n-1}f(\omega_n^i)\omega_n^{-ik}=\sum_{i=0}^{n-1}\omega_n^{-ik}\sum_{j=0}^{n-1}a_j(\omega_n^i)^{j}\\
&=\sum_{i=0}^{n-1}\sum_{j=0}^{n-1}a_j\omega_n^{i(j-k)}=\sum_{j=0}^{n-1}a_j\sum_{i=0}^{n-1}\left(\omega_n^{j-k}\right)^i
\end{aligned}
$$

考虑最后那坨求和，发现当且精当 $j-k$ 为 $0$ 是后面为 $n$ 其他情况都为 $0$，考虑有限等比级数求和即可得，所以发现结论就是：

$$
A(w_n^{-k})=a_k\times n
$$

一坨复杂的推导得到简洁的结果，大概是数学的常态了，而数学的精髓或许是发现有更简单的符合直觉的推导方式，但是关于这个结论，貌似是没有更简洁的证明被发现了。

所以最后又非常简单了，逆变换与变换基本相同，只用最后翻转一下序列然后除以 $n$ 即可。

代码实现反而比较简洁。

## 快速数论变换(NTT)

在模意义下讨论这些同样得到相同结果。

单位根对应原根，方法推理类似，省略。

NTT 一般意义上更快，精度更高，但是受限于取模，不过据说有搞 $3$ 个不同模数 NTT ，用 `CRT` 合并，还能跑得更快。

```
请务必实现一个常数小的板子适应不同题目的离谱常数。--Unknown
```

## 多项式求逆

请注意一般情况多项式逆元次数为无穷次，我们一般求其在模 $x^n$ 的值，也就是舍弃更高位。

若已求出 $f(x)$ 在模 $x^{\lfloor \frac n 2\rfloor}$ 意义下的逆元 $f_2^{-1}(x)$，要求模 $x^n$ 意义下逆元 $f^{-1}(x)$

tips: $f_2^{-1}(x)$ 与 $f^{-1}(x)$  在模 $x^{\lfloor \frac n 2\rfloor}$ 意义下相同。

所以：

$$
f_2^{-1}(x)-f^{-1}(x)\equiv0 ~~(\bmod ~x^{\lfloor \frac n 2\rfloor})
$$

把模数搞成正确的，同时平方，变换变换：

$$
f^{-1}(x)\equiv f_2^{-1}(x)\times (2-f(x)f_2^{-1}(x)) ~~ (\bmod ~ x^n)
$$

递归计算即可，为减小常数， NTT 变换过来后一下把运算搞完再变换回去，不然你就会 TLE。

时间复杂度： 

$$
T(n)=T(\frac n 2)+\mathcal{O}(n\log n)=\mathcal{O}(n\log n)
$$

给一个多项式全家桶板子（未完成，已完成部分可用）：

```cpp
#include <cstring>
#include <cmath>
#include <algorithm>
#include <complex>
#define CONFIG_POLY_DATA int
//#define CONFIG_FFT is not set
#define mod 998244353
#define g 3
#define invg 332748118
#define CONFIG_NTT y
inline int pow(int a,int k){
  long long res=1;
  while(k){
    if(k&1){
      #ifdef mod
      res=1ll*res*a%mod;
      #else
      res*=a;
      #endif
    }
    #ifdef mod
    a=1ll*a*a%mod;
    #else
    a*=a;
    #endif
    k>>=1;
  }
  return res;
}
class Poly{
  public:
    CONFIG_POLY_DATA xi[400005];
    int ci;
    Poly(int n){
      ci=n;
      memset(xi,0,sizeof(CONFIG_POLY_DATA)*n);
      return ;
    }
    Poly(){
      ci=0;
      xi[0]=0;
      return ;
    }
    Poly(CONFIG_POLY_DATA *seq,int len){
      ci=len;
      memcpy(xi,seq,len*sizeof(CONFIG_POLY_DATA));
      return ;
    }
};
double PI;
namespace warn{
  #ifdef CONFIG_FFT
  static void fft(std::complex <double> *f,int lim,int type,int *r){
    for(int i=0;i<=lim;i++){
      if(i<r[i]){
        std::swap(f[i],f[r[i]]);
      }
    }
    for(int len=2;len<=lim;len<<=1){
      std::complex <double> omega(std::cos(PI*2/len),std::sin(type*PI*2/len));
      for(int i=0;i<=lim;i+=len){
        std::complex<double>wn1(1,0);
        for(int k=i;k<len/2+i;k++,wn1*=omega){
          std::complex<double>x=f[k],y=f[k+len/2]*wn1;
          f[k]=x+y;
          f[k+len/2]=x-y;
        }
      }
    }
    return ;
  }
  #endif
  #ifdef CONFIG_NTT
  static void ntt(int *a,int lim,int type,int *r){
    for(int i=0;i<=lim;i++){
      if(i<r[i]){
        std::swap(a[i],a[r[i]]);
      }
    }
    for(int i=1;i<lim;i<<=1){
      int omega=pow(type?g:invg,(mod-1)/(i<<1));
      for(int j=0;j<lim;j+=(i<<1)){
        int g0=1;
        for(int k=0;k<i;k++){
          int x=a[j+k];
          int y=1ll*a[i+j+k]*g0%mod;
          a[j+k]=(x+y)%mod;
          a[i+j+k]=(((x-y)%mod)+mod)%mod;
          g0=(1ll*g0*omega)%mod;
        }
      }
    }
    return ;
  }
  #endif
}
#ifdef CONFIG_FFT
Poly FFT(const Poly &a,const Poly &b){
  PI=acos(-1);
  int tol=1;
  int k=0;
  while(tol<(a.ci+b.ci+2)){
    tol<<=1;
    k++;
  }
  int *r=new int[2*tol+5];
  r[0]=0;
  std::complex <double> *aa=new std::complex<double> [2*tol+5],*bb=new std::complex<double> [2*tol+5];
  for(int i=0;i<=a.ci;i++){
    aa[i]=a.xi[i];
  }
  for(int i=0;i<=b.ci;i++){
    bb[i]=b.xi[i];
  }
  Poly ans(tol+5);
  ans.ci=0;
  for(int i=0;i<=tol;i++){
    r[i]=(r[i>>1]>>1)|((i&1)<<(k-1));
  }
  warn::fft(aa,tol,1,r);
  warn::fft(bb,tol,1,r);
  for(int i=0;i<=tol;i++){
    aa[i]*=bb[i];
  }
  warn::fft(aa,tol,-1,r);
  for(int i=0;i<=a.ci+b.ci;i++){
    ans.xi[i]=(aa[i].real()/tol+0.5);
  }
  ans.ci=a.ci+b.ci;
  delete[] r;
  delete[] aa;
  delete[] bb;
  return ans;
}
#endif
#ifdef CONFIG_NTT
Poly ans;
Poly NTT(const Poly &a,const Poly &b){
  int tol=1;
  int k=0;
  while(tol<std::min(a.ci+b.ci+2,400002)){
    tol<<=1;
    k++;
  } 
  ans.ci=0;
  int *r=new int[2*tol+5];
  r[0]=0;
  int *aa=new CONFIG_POLY_DATA [2*tol+5],*bb=new CONFIG_POLY_DATA[2*tol+5];
  for(int i=0;i<=a.ci;i++){
    aa[i]=a.xi[i];
  }
  for(int i=a.ci+1;i<=tol*2;i++){
    aa[i]=0;
  }
  for(int i=0;i<=b.ci;i++){
    bb[i]=b.xi[i];
  }
  for(int i=b.ci+1;i<=tol*2;i++){
    bb[i]=0;
  }
  for(int i=0;i<=tol;i++){
    r[i]=(r[i>>1]>>1)|((i&1)<<(k-1));
  }
  warn::ntt(aa,tol,1,r);
  warn::ntt(bb,tol,1,r);
  for(int i=0;i<=tol;i++){
    aa[i]=1ll*aa[i]*bb[i]%mod;
  }
  warn::ntt(aa,tol,0,r);
  int inv=pow(tol,mod-2);
  ans.ci=a.ci+b.ci;
  for(int i=0;i<=a.ci+b.ci;i++){
    ans.xi[i]=1ll*aa[i]*inv%mod;
  }
  delete [] aa;
  delete [] bb;
  delete [] r;
  return ans;
}
#endif
const Poly operator * (const Poly &a,const Poly &b){
  #ifdef CONFIG_FFT
  return FFT(a,b);
  #endif
  #ifdef CONFIG_NTT
  return NTT(a,b);
  #endif
}
#ifdef CONFIG_NTT
static void inv_work(Poly &b,Poly &a,int len){
  if(len==1){
    b.xi[0]=pow(a.xi[0],mod-2);
    b.ci=0;
    return ;
  }
  inv_work(b,a,((len+1)>>1));
  int tt=a.ci;
  a.ci=len-1;
  int tol=1;
  int k=0;
  while(tol<(len*2)){
    tol<<=1;
    k++;
  }
  ans.ci=0;
  int *r=new int[2*tol+5];
  r[0]=0;
  int *aa=new CONFIG_POLY_DATA [2*tol+5],*bb=new CONFIG_POLY_DATA[2*tol+5];
  for(int i=0;i<=a.ci;i++){
    aa[i]=a.xi[i];
  }
  for(int i=a.ci+1;i<=tol*2;i++){
    aa[i]=0;
  }
  for(int i=0;i<=b.ci;i++){
    bb[i]=b.xi[i];
  }
  for(int i=b.ci+1;i<=tol*2;i++){
    bb[i]=0;
  }
  for(int i=0;i<=tol;i++){
    r[i]=(r[i>>1]>>1)|((i&1)<<(k-1));
  }
  warn::ntt(aa,tol,1,r);
  warn::ntt(bb,tol,1,r);
  for(int i=0;i<=tol;i++){
    aa[i]=1ll*((((2-1ll*aa[i]*bb[i])%mod)+mod)%mod)*bb[i]%mod;
  }
  warn::ntt(aa,tol,0,r);
  int inv=pow(tol,mod-2);
  ans.ci=len-1;
  for(int i=0;i<len;i++){
    ans.xi[i]=1ll*aa[i]*inv%mod;
  }
  delete [] aa;
  delete [] bb;
  delete [] r;
  b=ans;
  a.ci=tt;
  return ;
}
Poly inv(Poly &a){
  Poly ans(a.ci);
  inv_work(ans,a,a.ci+1);
  return ans;
}
void Dao(Poly &b){
  if(b.ci==0){
    b.xi[0]=0;
    b.ci=0;
    return ;
  }
  for(int i=1;i<=b.ci;i++){
    b.xi[i-1]=(1ll*(i)*b.xi[i])%mod;
  }
  b.ci--;
  return ;
}
void Ji(Poly &b){
  b.ci++;
  for(int i=b.ci;i>=0;i--){
    b.xi[i+1]=(1ll*(pow(i+1,mod-2))*b.xi[i])%mod;
  }
  b.xi[0]=0;
  return ;
}
Poly ln(Poly a){
  Poly b=inv(a);
  Dao(a);
  b=b*a;
  Ji(b);
  return b;
}
Poly a,temp;
void exp_work(Poly &ans,Poly &b,int len){
  if(len==1){
    ans.ci=0;
    ans.xi[0]=1;
    return ;
  }
  exp_work(ans,b,(len+1)>>1);
  for(int i=ans.ci+1;i<=(len)*2;i++){
    ans.xi[i]=0;
  }
  ans.ci=len*2;
  a=ln(ans);
  temp=Poly(len-1);
  temp.xi[0]=1;
  for(int i=0;i<len;i++){
    temp.xi[i]+=b.xi[i];
    temp.xi[i]%=mod;
  }
  for(int i=0;i<len;i++){
    temp.xi[i]+=mod-a.xi[i];
    temp.xi[i]%=mod;
  }
  ans=temp*ans;
  for(int i=len;i<=ans.ci;i++){
    ans.xi[i]=0;
  }
  ans.ci=len-1;
  return ;
}
Poly exp(Poly &a){
  Poly ans(a.ci);
  exp_work(ans,a,a.ci+1);
  return ans;
}
#endif

```
