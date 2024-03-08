---
title: 树分治
date: 2024-03-08
tags: [OI,图论,树形结构]
toc: true
mathjax: true
---


## 点分治

### 引入

[P3806](https://www.luogu.com.cn/problem/P3806)

题意：给定一颗具有 $n$ 个节点的树，边上有权值，一共给定 $m$ 次询问，每次询问给定 $k$，要求求出树上距离为 $k$ 的点对是否存在。

树上任意两点的路径是可以分为两条从一个节点到其某个祖先节点的路径。考虑递归到节点 $i$ ，对于此节点，为了进行统计，可以划分为两个部分，一种是经过 $i$ 号节点，另一种是没有经过 $i$ 号节点，很明显没有经过 $i$ 号节点的路径一定在 $i$ 号节点某个儿子的子树内，可以进行递归处理，接下来考虑如何处理，很明显经过 $i$ 号节点的路径一定是经过两个儿子的，定义 $dis_b$ 表示 $b$ 号节点到 $i$ 的距离，很明显这是一条树上从一个节点到其某个祖先节点的路径，我们要找的是二元组 $(a,b)$ ，使得有 $dis_a + dis_b =k $ ，这样直接处理是 $O(n^2)$ ，但是可以开数组 $tf_b$ 表示是否存在 $v$ 使得 $dis_v=b$ 。

### 时间复杂度分析

很明显每一层递归，设以 $i$ 节点为根的子树大小为 $siz_i$ ，很明显每次递归是 $O(siz_i \times k)$ ，总时间复杂度不难得出，设树深度为 $h$ ，则时间复杂度为 $O(nhk)$ 。

这个时间复杂度依赖于数据，深度最坏可能达到 $n$ ，于是最坏时间复杂度变成了 $O(n^2k)$。

所以是否可以适当变换树的结构使得深度可以减小到 $log_n$ 呢？

### 引入重心

#### 定义

$log$ 的时间复杂度关键在于分成一半，如果可以在递归过程中更换一个根节点，即可，因为这样更改根节点实际上只是更改了一条边，也就是从原来的根节点到他的父亲的边，所以子树内点间距离不会改变，所以怎么选呢？分成一半，在于相等，如果能确保所有儿子的子树大小尽可能相等，即可，换句话说也就是子树大小的最大值最小，注意这里讨论无向边，可以理解为以这个节点作为根节点的时候，求出一个 $W_i$ 是 $\max(siz_j)~{j\in son_i}$ ，求出最小的 $W_i$ ，这个点即为重心。

$mss(u)$ 表示 $u$ 的最大子树大小。

形式化的说，找到一个 $u$ 使得 $mss(u)$ 最小。

#### 性质

##### 引理1：

###### 结论：

某个树的重心一定满足他的最大子树大小不大于整棵树的一半，反命题同样成立。

###### 论证：

充分性：

$siz_u(v)$ 表示以 $u$ 为根，$v$ 子树大小。

$mss(u)$ 表示 $u$ 的最大子树大小。

反证法证明，$u$ 为树的重心，假设存在与他相邻的一个节点 $v$ 满足 $siz_u(v)>\frac n 2$ ，于是有 $siz_v(u)=n-siz_u(v)< \frac n 2 $ ，于是 $siz_v(u) <siz_u(v) =mss(u)$ ，因为 $siz_u(v)=1+\sum siz_v(w) ~(w\in son_v)$，所以有 $siz_v(w)<siz_u(v) ~ (w\in son_v)$ ,

推出 $mss(v)<siz_u(v)=mss(u)$ ，与重心定义不符。

 必要性：

如果 $mss(u) < \frac 2 n$ ，则说明对于每个儿子$v$都存在 $siz_u(v) < \frac 2 n$ ，于是有 $\sum siz_v(w) < \frac 2 n ~(w \in son_v ~\&~ w \neq u )$ ，可以推出$siz_v(w) < \frac 2 n (w \in son_v ~~\&~~ w \neq u)$。

##### 引理2：

###### 结论：

如果一棵树有两个重心，则这两个重心一定相邻，并且树有偶数个节点，可以被划分为两个大小相等的分支，每个分支各自包含一个重心。

###### 论证：

很明显对于两个重心 $u,v$ 有 $mss(u)=mss(v)$，$u$ 的最大子树肯定包含 $v$ ，假设法证明，假设 $w$ 是 $u$ 的最大子树，则有 $mss(u)=siz_u(w)<siz_v(u)$ ，很明显 $siz_v(u) \leq mss(v) $ ，则有 $mss(u)<mss(v)$，矛盾。

设 $u$ 到 $v$ 经过 $k$ 个中间节点，于是有 $wss(u)=siz_u(v)=k+mss(v)$，明显有 $k=0$。

因为 $siz_u(v)=siz_v(u)$ ，又因为 $siz_u(v)+siz_v(u)=n$ ，所以 $siz_u(v)=siz_v(u)=\frac 2 n $ ，结论不证自明。

##### 引理3：

###### 结论：

树至少一个重心，至多有两个重心。

###### 论证：

求重心在做求最小值操作，肯定存在最小值。不可能存在三个重心，因为他们两两相邻，会形成环，树怎么可能有环？

##### 引理4：

###### 结论：

假设树上所有边权为 $1$，则记 $dis_i $ 表示所有节点到 $i$ 号节点的距离和，则重心 $u$ 的 $dis_u$ 最小，反命题成立。

###### 论证：

很明显对于非重心点 $u$ ，有 $wss(v) > \frac 2 n~ (v \in son_v) $ ，那向 $v$ 移动会使 $dis$ 减少 $siz_u(v)-(n-siz_u(v)=2siz_u(v)-n>0$ ，结论得证。 

##### 求法：

根据定义以及引理1即可

我们先统计子树内的答案然后，每次移动到子树中的重心即可。

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
inline int read();
int fir[100005];
int nxt[200005];
int v[200005];
int w[200005];
int now,n,m;
int ques[1005];
bool vised[100005];
void add(int a,int b,int c){
    now++;
    v[now]=b;
    w[now]=c;
    nxt[now]=fir[a];
    fir[a]=now;
    return ;
}
int cnt,sum;
int dis[100005];
int dd[100005];
bool df[10000005];
int siz[100005];
int we[100005];
int he=0;
std::queue<int>a;
void calcsiz(int now,int fa){
    siz[now]=1;
    we[now]=0;
    for(int i=fir[now];i!=-1;i=nxt[i]){
        if(vised[v[i]]||fa==v[i]){
            continue;
        }
        calcsiz(v[i],now);
        we[now]=std::max(we[now],siz[v[i]]);
        siz[now]+=siz[v[i]];
    }
    we[now]=std::max(we[now],sum-siz[now]);
    if(we[now]<=sum/2){
        he=now;
    }
    return ;
}
void calcdis(int now,int fa){
    dd[++cnt]=dis[now];
    for(int i=fir[now];i!=-1;i=nxt[i]){
        if(vised[v[i]]||fa==v[i]){
            continue;
        }
        dis[v[i]]=dis[now]+w[i];
        calcdis(v[i],now);
    }
    return ;
}
bool res[1005];
void dfs(int now,int fa){
    vised[now]=1;
    df[0]=1; 
    a.push(0);
    for(int i=fir[now];i!=-1;i=nxt[i]){
        if(vised[v[i]]||fa==v[i]){
            continue;
        }
        cnt=0;
        dis[v[i]]=w[i];
        calcdis(v[i],now);
        for(int j=1;j<=cnt;j++){
            for(int kk=1;kk<=m;kk++){
                if(ques[kk]>=dd[j]){
                    res[kk]|=df[ques[kk]-dd[j]];
                }
            }
        }
        for(int j=1;j<=cnt;j++){
            if(dd[j]<=10000001){
                df[dd[j]]=1;
                a.push(dd[j]);
            }
        }
    }
    while(a.size()>0){
        df[a.front()]=0;
        a.pop();
    }
    for(int i=fir[now];i!=-1;i=nxt[i]){
        if(vised[v[i]]||fa==v[i]){
            continue;
        }
        sum=siz[v[i]];
        calcsiz(v[i],now);
        int hee=he;
        calcsiz(hee,now);
        dfs(hee,now);
    }
}
int main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen("P3806_2.in","r",stdin);
	freopen(".out","w",stdout);
	#endif
    n=read();
    m=read();
    for(int i=1;i<=n;i++){
        fir[i]=-1;
    }
    for(int i=1;i<n;i++){
        int u=read();
        int v=read();
        int w=read();
        add(u,v,w);
        add(v,u,w);
    }
    for(int i=1;i<=m;i++){
        ques[i]=read();
    }
    sum=n;
    calcsiz(1,-1);
    int hee=he;
    calcsiz(hee,-1);
    dfs(hee,-1);
    for(int i=1;i<=m;i++){
        if(res[i]){
            printf("AYE\n");
        }else{
            printf("NAY\n");
        }
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

[P4149](https://www.luogu.com.cn/problem/P4149)

## 点分树

[P6329](https://www.luogu.com.cn/problem/P6329)


对于此类带修的问题，直接点分治显然炸了，考虑采用点分治中形成的重构树进行求解。

不同的是，对于一个子树，我们将其整体的答案存储在根上，一般采用高级数据结构，具体来说，结点 $i$ 需要存储原树的信息，对于模板题来说，就是维护原树中这个子树内所有点到这个点的距离和，然后暴力向上跳求解或修改答案，在此过程中会发现一个点会被这个点在重构树上的每一个父亲都统计一遍，所以再统计一个这个点 $i$ 的子树内所有点到这个点 $i$ 在重构树上的父亲的信息。

时间复杂度取决与维护信息的方式，典型的时间复杂度是 $\mathcal{O}(n\log n)$ 以及 $\mathcal{O}(n\log^2 n)$ 。需要注意空间复杂度每个点维护的信息量一般与这个点的子树大小成正比，这样的空间复杂度仍然是 $\mathcal{O}(n \log n)$ 但是要注意实现，不能开多了。

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
inline int read();
int fir[100005];
int nxt[200005];
int v[200005];
int now;
inline int lowbit(int x){
  return x&(-x);
}
inline void print(int n){
  static char tt[35];
  int ttt=0;
  while(n){
    tt[++ttt]=n%10;
    n/=10;
  }
  if(ttt==0){
    tt[++ttt]=0;
  }
  while(ttt){
    putchar('0'+tt[ttt]);
    ttt--;
  }
  putchar('\n');
  return ;
}
class BIT{
public:
  std::vector<int>mp;
  void add(int x,int y){
    x++;
    while(x<mp.size()){
      mp[x]+=y;
      x+=lowbit(x);
    }
    return ;
  }
  int query(int x){
    x++;
    if(x<=0){
      return 0;
    }
    x=std::min(x,int(mp.size()-1));
    int ans=0;
    while(x>0){
      ans+=mp[x];
      x-=lowbit(x);
    }
    return ans;
  }
}dist[100005],ch[100005];
void add(int x,int y){
  v[++now]=y;
  nxt[now]=fir[x];
  fir[x]=now;
  return ;
}
int val[100005];
namespace nw{
  int fir[100005];
  int nxt[100005];
  int v[100005];
  int now;
};
int siz[100005];
int maxx[100005];
bool vised[100005];
int sum;
int rt;
void calcsiz(int now,int fa){
  siz[now]=1;
  maxx[now]=0;
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(v[i]==fa||vised[v[i]]){
      continue;
    }
    calcsiz(v[i],now);
    maxx[now]=std::max(maxx[now],siz[v[i]]);
    siz[now]+=siz[v[i]];
  }
  maxx[now]=std::max(maxx[now],sum-siz[now]);
  if(maxx[now]<maxx[rt]||rt==0){
    rt=now;
  }
  return ;
}
int top;
int dis[100005];
int tp[100005];
void calcdis1(int now,int fa){
  dist[top].add(dis[now],val[now]);
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(v[i]==fa||vised[v[i]]){
      continue;
    }
    dis[v[i]]=dis[now]+1;
    calcdis1(v[i],now);
  }
  return ;
}
void calcdis2(int now,int fa){
  ch[top].add(dis[now],val[now]);
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(v[i]==fa||vised[v[i]]){
      continue;
    }
    dis[v[i]]=dis[now]+1;
    calcdis2(v[i],now);
  }
  return ;
}
int dep[100005];
int ccc;
void dfs(int now,int fa){
  vised[now]=1;
  top=now;
  dis[now]=0;
  calcdis1(now,fa);
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(v[i]==fa||vised[v[i]]){
      continue;
    }
    rt=0;
    sum=siz[v[i]];
    calcsiz(v[i],now); 
    calcsiz(rt,0);
    top=rt;
    dist[rt].mp.resize(siz[rt]+2);
    ch[rt].mp.resize(siz[rt]+2);
    dis[v[i]]=1;
    calcdis2(v[i],-1);
    tp[rt]=now;
    dep[rt]=dep[now]+1;
    dfs(rt,now);
  }
  return ;
}
int to[100005][35];
int tdis[100005][35];
int lcadep[100005];
void dfs2(int now,int fa){
  lcadep[now]=lcadep[fa]+1;
  to[now][0]=fa;
  if(now!=fa){
    tdis[now][0]=1;
  }else{
    tdis[now][0]=0;
  }
  for(int i=1;i<=16;i++){
    to[now][i]=to[to[now][i-1]][i-1];
    tdis[now][i]=tdis[now][i-1]+tdis[to[now][i-1]][i-1];
  }
  for(int i=fir[now];i!=-1;i=nxt[i]){
    if(v[i]==fa){
      continue;
    }
    dfs2(v[i],now);
  }
  return ;
}
inline int dislca(int x,int y){
  if(lcadep[x]>lcadep[y]){
    std::swap(x,y);
  }
  int dis=0;
  int i=16;
  while(lcadep[x]<lcadep[y]){
    for(;i>0;i--){
      if(lcadep[x]<lcadep[to[y][i]]){
        break;
      }
    }
    dis+=tdis[y][i];
    y=to[y][i];
  }
  i=16;
  while(x!=y){
    for(;i>0;i--){
      if(to[x][i]!=to[y][i]){
        break;
      }
    }
    dis+=tdis[y][i];
    dis+=tdis[x][i];
    x=to[x][i];
    y=to[y][i];
  }
  return dis;
}
int dd[100005][55];
int main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen("P6329_1.in","r",stdin);
  freopen(".out","w",stdout);
  #endif
  int n=read(),m;
  m=read();
  for(int i=1;i<=n;i++){
    fir[i]=-1;
    val[i]=read();
  }
  for(int i=1;i<n;i++){
    int u=read();
    int v=read();
    add(u,v);
    add(v,u);
  }
  sum=n;
  calcsiz(1,1);
  calcsiz(rt,-1);
  int rtt=rt;
  ch[rt].mp.resize(n+2);
  dist[rt].mp.resize(n+2);
  dfs(rt,-1);
  int lstans=0;
  dfs2(1,1);
  for(int i=1;i<=n;i++){
    int j=i;
    while(j!=rtt){
      dd[i][dep[i]-dep[j]]=dislca(i,j);
      j=tp[j];
    }
    dd[i][dep[i]-dep[j]]=dislca(i,j);
    j=tp[j];
  }
  for(int i=1;i<=m;i++){
    int op=read();
    int x=read()^lstans;
    int y=read()^lstans;
    switch(op){
      case 0:{
        lstans=dist[x].query(y);
        int j=x;
        while(j!=rtt){
          int &diss=dd[x][dep[x]-dep[tp[j]]];
          lstans+=dist[tp[j]].query(y-diss);
          lstans-=ch[j].query(y-diss);
          j=tp[j];
        }
        int &diss=dd[x][dep[x]-dep[tp[j]]];
        lstans-=ch[j].query(y-diss);
        j=tp[j];
        print(lstans);
        break;
      }
      case 1:{
        dist[x].add(0,y-val[x]);
        int j=x;
        while(j!=rtt){
          int &diss=dd[x][dep[x]-dep[tp[j]]];
          dist[tp[j]].add(diss,y-val[x]);
          ch[j].add(diss,y-val[x]);
          j=tp[j];
        }
        val[x]=y;
        break;
      }
    }
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

[P3241](https://www.luogu.com.cn/problem/P3241)
