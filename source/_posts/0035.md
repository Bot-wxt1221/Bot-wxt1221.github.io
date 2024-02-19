---
title: 高斯消元
date: 2024-02-19
tags: [OI,数学]
toc: true
mathjax: true
---

# 高斯消元

为了求解线性方程组，我们可以采用增广矩阵的形式对方程组进行考察，采用消元法是很容易被想到的。

消元法的核心是三个不变：

* 任意一方程乘任意非 $0$ 系数加上另一个方程，解不变。

* 任意一方程乘任意非 $0$ 系数，解不变。

* 任意二方程交换，解不变。

高斯消元的主要思路是根据以上性质将增广矩阵先初等变换为行最简形。也就是每一非 $0$ 行第一个元素为 $1$ ，此列没有其他元素非 $0$ 。

容易发现，若方程组无解或无数解是无法化为行最简形。然后可以发现一定存在某一行只有一个未知数，然后不停向上代回求解。

具体地，我们可以从每一行第一个非 $0$ 元素开始，消掉其他方程相同的元，并且化系数为 $1$ 。

若没有消掉每一个元，则说明无解或无数解。我们判定所有 $0x_i=a$ 的 $a$ 是否为 $0$ 即可区分无解或无数解。

为了精度起见，我们选择系数绝对值最大的进行消元。

```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#define int long long
inline int read();
double mp[105][105];
signed main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen(".in","r",stdin);
  freopen(".out","w",stdout);
  #endif
  int n=read();
  for(int i=1;i<=n;i++){
    for(int j=1;j<=n+1;j++){
      scanf("%lf",&mp[i][j]);
    }
  }
  int r=1;
  for(int i=1;i<=n;i++){//变换
    int ni=r;
    for(int j=r+1;j<=n;j++){
      if(std::abs(mp[j][i])>std::abs(mp[ni][i])){
        ni=j;
      }
    }
    for(int j=1;j<=n+1;j++){
      std::swap(mp[ni][j],mp[r][j]);
    }
    if(mp[r][i]==0){
      continue;
    }
    double te=mp[r][i];
    for(int j=1;j<=n+1;j++){
      mp[r][j]/=te;
    }
    for(int j=r+1;j<=n;j++){
        double te2=mp[j][i];
        for(int k=1;k<=n+1;k++){
          mp[j][k]-=mp[r][k]*te2;
        }
    }
    r++;
  }
  if(r<=n){
    for(int i=r;i<=n;i++){
      if(std::abs(mp[i][n+1])>1e-6){
        printf("-1");
        return 0;
      }
    }
    printf("0");
    return 0;
  }

  for(int i=n-1;i>=1;i--){
    for(int j=i+1;j<=n;j++){
      mp[i][n+1]-=mp[j][n+1]*mp[i][j];
    }
  }
     for(int i=1;i<=n;i++){
    printf("x%lld=%.2lf\n",i,mp[i][n+1]);
  }
  return 0;
}
inline int read(){
  int x=0,f=1;char c=getchar();
  while(c<'0'||c>'9'){
    c=='-'?f=-1:1;
    c=getchar();
  }
  while(c>='0'&&c<='9'){
    x=(x<<3)+(x<<1)+(c^48);
    c=getchar();
  }
  return f*x;
}
/*
Anything about this program:
Type:

Description:

Example:
	1:
		In:

		Out:
More:

*/

```
