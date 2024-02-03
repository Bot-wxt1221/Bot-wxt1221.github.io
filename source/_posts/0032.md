---
title: 2.1 模拟赛总结
date: 2024-02-01
tags: [OI,模拟赛]
toc: true
mathjax: true
---

# 2.1 模拟赛总结

~~又是挂分的一天。~~

## A 

给定可重集，找出一个子集使得严格大于选出数的平均数尽可能多，报告这个最大值。

升序排序。

可以发现选的是一段前缀，搞一搞就好了。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#define int long long
inline int read();
int a[1000005];
int sum[1000005];
signed main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen("num.in","r",stdin);
  freopen("num.out","w",stdout);
  #endif
  int n=read();
  for(int i=1;i<=n;i++){
    a[i]=read();
  }
  std::sort(a+1,a+n+1);
  for(int i=1;i<=n;i++){
    sum[i]=a[i]+sum[i-1];
  }
  int j=0;
  int sum1=0,sum2=0;
  int ans=0;
  for(int i=1;i<=n;i++){
    sum2-=a[i];
    while(j+1<=n&&(((sum1+sum2+a[j+1]+a[i])<a[i]*(j+1))||(j<i))){
      j++;
      sum2+=a[j];
    }
    if(sum1+sum2+a[i]<a[i]*(j)){
      ans=std::max(ans,j-i+1);
    }
    sum1+=a[i];
  }
  printf("%lld",ans);
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

## B

给定一颗树以及多条非树边，有编号，你需要为每条边安排一个权值，权值两两不同，保证新的图的最小生成树是原来的树。

很明显，我们应该从前向后考虑，遇到树边就直接赋值，遇到非树边 $u_i,v_i$ 应将树上此二点的路径中未赋值的边赋值，然后给他赋值。

我们可以采用高级数据结构的方式来维护，~~我不会~~ 。

我们可以采用类似于并查集的方式维护，只要访问过这条边就可以向上合并。时间复杂度同并查集但是没有启发式合并，所以是 $\mathcal{O}(n\log n)$

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
inline int read();
int fa[500005];
int edge[500005];
std::priority_queue<int>qu;
int num[500005];
int fir[500005];
int nxt[1000005];
int v[1000005];
int w[1000005];
int now;
void add(int x,int y,int z){
  v[++now]=y;
  w[now]=z;
  nxt[now]=fir[x];
  fir[x]=now;
  return ;
}
bool del[500005];
int dep[500005];
void dfs(int now,int fa){
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(fa==v[i]){
      continue;
    }
    ::fa[v[i]]=now;
    edge[v[i]]=w[i];
    dep[v[i]]=dep[now]+(1);
    dfs(v[i],now);
  }
  return ;
}
namespace ans{
  int u[500005];
  int v[500005];
  int c[500005];
};
int tt;
int tt2;
int getfa(int x){
  if(x==1){
    tt=0;
    return 1;
  }
  if(edge[x]!=0){
    tt=edge[x];
    tt2=x;
    return fa[x];
  }
  return fa[x]=getfa(fa[x]);
}
int getfa2(int x){
  if(x==1){
    return 1;
  }
  if(edge[x]!=0){
    return x;
  }
  return fa[x]=getfa2(fa[x]);
}
int main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen("tree.in","r",stdin);
  freopen("tree.out","w",stdout);
  #endif
  int n=read();
  int m=read();
  for(int i=1;i<=n;i++){
    fir[i]=-1;
    fa[i]=i;
  }
  for(int i=1;i<=m;i++){
    ans::u[i]=read();
    ans::v[i]=read();
    ans::c[i]=read();
    if(ans::c[i]==1){
      add(ans::u[i],ans::v[i],i);
      add(ans::v[i],ans::u[i],i);
    }
  }
  dfs(1,1);
  int now=0;
  for(int i=1;i<=m;i++){
    if(ans::c[i]==1&&del[i]==0){
      num[i]=++now;
      del[i]=1;
    }
    if(ans::c[i]==0){
      int u=ans::u[i];
      int v=ans::v[i];
      while(u!=v){
        u=getfa2(u);
        v=getfa2(v);
        if(u==v){
          continue;
        }
        if(dep[u]<dep[v]){
          std::swap(u,v);
        }
        int temp=getfa(u);
        if(tt!=0){
          if(!del[tt]){
            qu.push(-tt); 
          }
          del[tt]=1;
          edge[tt2]=0;
        }
        u=temp;
      }
      while(qu.size()>0){
        num[-qu.top()]=++now;
        qu.pop();
      }
      num[i]=++now;
    }
  }
  for(int i=1;i<=m;i++){
    printf("%d ",num[i]);
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

## C

给定若干区间，要求选权值和最大的区间，这些区间相交就划到一个集合内，要求所有集合大小不大于 $k$ 。

我们的选择方式一定是把经过某个点的区间全部搞掉。

$dp_i$ 表示切了 $i$ 的位置，转移实际就是枚举上一次切的位置，然后中间的只能保留 $k$ 个，转移从后往前我们就可以把中间的区间甩进堆从而让时间复杂度达到 $\mathcal {O}(n^2\log n)$ 。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue>
#define int long long
inline int read();
std::priority_queue<int>qu;
class node{
  public:
    int l,r,w;
    node(int x=0,int y=0,int z=0){
      l=x;
      r=y;
      w=z;
      return ;
    }
}seq[2505];
bool operator < (node a,node b){
  if(a.l!=b.l){
    return a.l<b.l;
  }
  if(a.r<b.r){
    return a.r<b.r;
  }
  return a.w<b.w;
}
int dp[10005];
int X[10005];
int lst[10005];
signed main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen("interval.in","r",stdin);
  freopen("interval.out","w",stdout);
  #endif
  int n=read();
  int k=read();
  int now=0;
  int sum=0;
  for(int i=1;i<=n;i++){
    seq[i].l=read();
    seq[i].r=read();
    seq[i].w=read();
    sum+=seq[i].w;
    X[++now]=seq[i].l;
    X[++now]=seq[i].r;
  }
  std::sort(X+1,X+now+1);
  now=std::unique(X+1,X+now+1)-X-1;
  std::sort(seq+1,seq+n+1);
  for(int i=1;i<=n;i++){
    seq[i].l=std::lower_bound(X+1,X+now+1,seq[i].l)-X;
    seq[i].r=std::lower_bound(X+1,X+now+1,seq[i].r)-X;
    lst[seq[i].l]=i;
  }
  for(int i=1;i<=now;i++){
    lst[i]=std::max(lst[i],lst[i-1]);
  }
  for(int i=1;i<=now;i++){
    qu=std::priority_queue<int>();
    int ans=0;
    int p=lst[i]+1;
    for(int j=i;j>=1;j--){
      while(seq[p-1].l>=j){
        p--;
        if(seq[p].r<=i){
          ans+=seq[p].w;
          qu.push(-seq[p].w);
        }
        if(qu.size()>k){
          ans+=qu.top();
          qu.pop();
        }
      }
      dp[i]=std::max(dp[i],dp[j-1]+ans);
    }
  }
  printf("%lld\n",sum-dp[now]);
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

## D

实际上你把向不同方向的分开计算就可以套高级数据结构进行维护，但是我死也不写这道题。

写这道题的都是 **河童重工** 。
