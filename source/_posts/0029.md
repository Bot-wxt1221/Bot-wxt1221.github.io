---
title: CDQ分治
date: 2024-01-29
tags: [OI,CDQ分治,算法笔记]
toc: true
mathjax: true
---

# CDQ 分治

## 思路总结

考虑三维偏序问题，也就是给定有  $n$ 个元素的序列 $a,b,c$ ，要求出有多少对 $i,j$ 满足 $a_i \leq a_j,b_i \leq b_j , c_i \leq c_j$

我们先按 $a$ 为关键字排序，把问题转换为是否存在 $i \leq j ,b_i \leq b_j ,c_i \leq c_j$ 。

我们继续考虑消掉 $b$ 这一维，如果只剩 $c$ 这一维，则可以套高级数据结构直接求解。

经典的排序算法中，可以统计类似逆序对的算法有归并排序，我们考虑采用归并排序的办法，在对 $b$ 进行排序时，统计答案。

归并排序的特点是先左右再合在一起，我们考虑在合并时统计跨越两个区间的答案。

具体的，`solve(i,j)` 的过程是先分治为两个相等的区间，然后统计跨这两个区间的答案。

我们在归并排序的过程中，即可统计三维偏序问题答案。

先按 $a$ 排序，然后归并排序的过程中以 $b$ 为关键字，实际上我们在归并排序的并的过程中，是按照 $b$ 的大小进行合并的，所以简单的把 $c$ 放进树状数组查询所有已经加入的数小于 $c$ 的元素即可统计答案。

另外，这种方法统计答案实际上统计了对于每个 $i$ 的答案。


```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
inline int read();
class node{
    public:
        int a,b,c;
        int cnt;
        int i;
        node(int aa=0,int bb=0,int cc=0){
            a=aa;
            b=bb;
            c=cc;
            return ;
        }
}b[1000005],a[1000005],tem[1000005];
int tree[200005];
int cnt2[1000005];
int end[1000005];
int f[1000005];
int lowbit(int a){
    return a&-a;
}
void add(int a,int i){
    while(a<=200000){
        tree[a]+=i;
        a+=lowbit(a);
    }
    return ;
}
int query(int a){
    int ans=0;
    while(a>0){
        ans+=tree[a];
        a-=lowbit(a);
    }
    return ans;
}
int ans[1000005];
bool cmp(node a,node b){
    if(a.a!=b.a){
        return a.a<b.a;
    }
    if(a.b!=b.b){
        return a.b<b.b;
    }
    return a.c<b.c;
}
void cdq(int l,int r){
    if(l>=r){
        return ;
    }
    int mid=(l+r)/2;
    cdq(l,mid);
    cdq(mid+1,r);
    int ii1=l,ii2=mid+1;
    int da=l-1;
    while(ii1<=mid&&ii2<=r){
        if(a[ii1].b<=a[ii2].b){
            add(a[ii1].c,a[ii1].cnt);
            tem[++da]=a[ii1];
            ii1++;
        }else{
            ans[a[ii2].i]+=query(a[ii2].c);
            tem[++da]=a[ii2];
            ii2++;
        }
    }
    while(ii1<=mid){
        add(a[ii1].c,a[ii1].cnt);
        tem[++da]=a[ii1];
        ii1++;
    }
    while(ii2<=r){
        ans[a[ii2].i]+=query(a[ii2].c);
        tem[++da]=a[ii2];
        ii2++;
    }
    for(int i=l;i<=mid;i++){
        add(a[i].c,-a[i].cnt);
    }
    for(int i=l;i<=r;i++){
        a[i]=tem[i];
    }
    return ;

}
int main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
	int n=read(),k;
    k=read();
    for(int i=1;i<=n;i++){
        b[i].a=read();
        b[i].b=read();
        b[i].c=read();
    }
    std::sort(b+1,b+n+1,cmp);
    int cnt=0;
    int tot=0;
    for(int i=1;i<=n;i++){
        if(b[i].a!=b[i+1].a||b[i].b!=b[i+1].b||b[i].c!=b[i+1].c){
            cnt++;
            a[++tot]=b[i];
            end[i]=tot;
            cnt2[i]=cnt;
            a[tot].cnt=cnt;
            a[tot].i=i;
            cnt=0;
        }else{
            cnt++;
        }
    }
    cdq(1,tot);
    for(int i=1;i<=n;i++){
        if(end[i])
            f[ans[i]+cnt2[i]-1]+=cnt2[i];
    }
    for(int i=0;i<=n-1;i++){
        printf("%d\n",f[i]);
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

## 题目选做


### 二/三维偏序

CDQ分治可用于离线求解偏序问题，故可以将其他问题转换为三维偏序问题。

很多在线问题可以转换为离线问题因为我们可以把时间搞成一维，问题完美解决，另外实际上CDQ分治每个点可以附加权值。

#### [P2717](https://www.luogu.com.cn/problem/P2717)寒假作业

平均值大于问题全部减去 $k$ 转换为区间和大于 $0$ 问题。

##### 解1

前缀和后统计逆序对。

##### 解2

并时维护前缀以及后缀和，处理出区间数量即可。

此题较为模板，实际上都是利用了CDQ的原理排序来解决问题。

#### [P2163](https://www.luogu.com.cn/problem/P2163) 园丁的烦恼

离线询问坐标系中任意矩阵的元素个数。

元素个数、询问个数小于 $5 \times 10^5$ ，坐标非负且小于 $10^7$ 。

很明显要利用前缀和，但是数据范围即使离散化后也很难做，我们考虑把询问离线化，这样我们的问题全部转换为形似 $[0,0]-[l,r]$ 的矩阵的问题，之后就是一个简单的三维偏序问题。

这里要注意哪些能够统计进答案，哪些不行。

#### [P3157](https://www.luogu.com.cn/problem/P3157) 动态逆序对

满足要求的有两种情况:$val_i \leq val_j,time_i \leq time_j , pos_i \geq pos_j$ 或者 $time_i \leq time_j,val_i \geq val_j,pos_i \leq pos_j$。

三维偏序时处理两次即可。

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#define int long long
inline int read();
int pos[200005];
int tree[200005];
int lowbit(int x){
  return x&(-x);
}
void mod(int x,int y){
  while(x<=200000){
    tree[x]+=y;
    x+=lowbit(x);
  }
  return ;
}
int query(int x){
  int ans=0;
  while(x>0){
    ans+=tree[x];
    x-=lowbit(x);
  }
  return ans;
}
class node{
  public:
    int tim,a,opp,pos;
    node(int x=0,int y=0,int z=0,int aa=0){
      tim=x;
      a=y;
      opp=z;
      pos=aa;
    }
}seq[200005],temp[200005];
int ans[200005];
void cdq(int l,int r){
  if(l==r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  cdq(mid+1,r);
  int nl=l;
  int nr=mid+1;
  int nw=l;
  while(nl<=mid&&nr<=r){
    if(seq[nl].pos<=seq[nr].pos){
      temp[nw++]=seq[nl];
      mod(seq[nl].a,seq[nl].opp);
      nl++;
    }else{
      temp[nw++]=seq[nr];
      ans[seq[nr].tim]+=seq[nr].opp*(query(200000)-query(seq[nr].a));
      nr++;
    }
  }
  while(nl<=mid){
    temp[nw++]=seq[nl];
    mod(seq[nl].a,seq[nl].opp);
    nl++;
  }
  while(nr<=r){
    temp[nw++]=seq[nr];
    ans[seq[nr].tim]+=seq[nr].opp*(query(200000)-query(seq[nr].a));
    nr++;
  }
  for(int i=l;i<=mid;i++){
    mod(seq[i].a,-seq[i].opp);
  }
  nl=mid;
  nr=r;
  while(nl>=l&&nr>=mid+1){
    if(seq[nl].pos>=seq[nr].pos){
      mod(seq[nl].a,seq[nl].opp);
      nl--;
    }else{
      ans[seq[nr].tim]+=seq[nr].opp*(query(seq[nr].a-1));
      nr--;
    }
  }
  while(nl>=l){
    mod(seq[nl].a,seq[nl].opp);
    nl--;
  }
  while(nr>=mid+1){
    ans[seq[nr].tim]+=seq[nr].opp*(query(seq[nr].a-1));
    nr--;
  }
  for(int i=l;i<=mid;i++){
    mod(seq[i].a,-seq[i].opp);
  }
  for(int i=l;i<=r;i++){
    seq[i]=temp[i];
  }
  return ;
}
signed main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
  int n=read(),m;
  m=read();
  for(int i=1;i<=n;i++){
    seq[i].a=read();
    seq[i].tim=0;
    seq[i].opp=1;
    seq[i].pos=i;
    pos[seq[i].a]=i;
  }
  for(int i=1;i<=m;i++){
    int m=read();
    seq[i+n].a=m;
    seq[i+n].tim=i;
    seq[i+n].opp=-1;
    seq[i+n].pos=pos[m];
  }
  cdq(1,n+m);
  for(int i=1;i<=m;i++){
    ans[i]+=ans[i-1];
  }
  for(int i=0;i<m;i++){
    printf("%lld\n",ans[i]);
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

#### [P4169](https://www.luogu.com.cn/problem/P4169) 天使玩偶/SJY摆棋子

初始给定 $n$ 个点，动态加点并且查询距离某点最近的点的距离。

CDQ分治，搞一维时间，然后考虑转换为三维偏序，我们只考虑所有 $x_i \leq x_j$ 和 $y_i \leq y_j$ 的点对，其他点对可以旋转坐标系得到，就是一个简单三维偏序。

实测不需要卡常，你们就是人傻常数大。

最大时间啊2.03s稳过，根本不怕。


```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <queue>
inline int read();
class node{
  public:
    int x,y,t,quan,i;
}pos[600005],temp[600005];
int ans[600005];
bool cmp1(node a,node b){
  if(a.x!=b.x){
    return a.x<b.x;
  }
  if(a.y!=b.y){
    return a.y<b.y;
  }
  return a.t<b.t;
}
int tree[1000015];
int lowbit(int x){
  return (x)&(-x);
}
void add(int x,int y){
  while(x<=1000005){
    tree[x]=std::max(tree[x],y);
    x+=lowbit(x);
  }
  return ;
}
int query(int x){
  int ans=-0x3f3f3f3f;
  while(x>0){
    ans=std::max(ans,tree[x]);
    x-=lowbit(x);
  }
  return ans;
}
void clear(int x){
  while(x<=1000005){
    tree[x]=-0x3f3f3f3f;
    x+=lowbit(x);
  }
  return ;
}
void cdq(int l,int r){
  if(l==r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  cdq(mid+1,r);
  int nl=l,nr=mid+1;
  int now=l-1;
  while(nl<=mid&&nr<=r){
    if(pos[nl].y<=pos[nr].y){
      temp[++now]=pos[nl];
      if(pos[nl].quan){
        add(pos[nl].t,pos[nl].x+pos[nl].y);
      }
      nl++;
    }else{
      temp[++now]=pos[nr];
      ans[pos[nr].i]=std::min(ans[pos[nr].i],pos[nr].x+pos[nr].y-query(pos[nr].t));
      nr++;
    }
  }
  while(nl<=mid){
    temp[++now]=pos[nl];
    if(pos[nl].quan){
      add(pos[nl].t,pos[nl].x+pos[nr].y);
    }
    nl++;
  }
  while(nr<=r){
    temp[++now]=pos[nr];
    ans[pos[nr].i]=std::min(ans[pos[nr].i],pos[nr].x+pos[nr].y-query(pos[nr].t));
    nr++;
  }
  for(int i=l;i<=mid;i++){
    clear(pos[i].t);
  }
  for(int i=l;i<=r;i++){
    pos[i]=temp[i];
  }
  return ;
}
std::queue<int>qu;
int main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
  int n=read(),m;
  m=read();
  for(int i=1;i<=n;i++){
    pos[i].x=read()+1;
    pos[i].y=read()+1;
    pos[i].t=1;
    pos[i].quan=1;
    pos[i].i=i;
    ans[i]=0x3f3f3f3f;
  }
  for(int i=1;i<=1000005;i++){
    tree[i]=-0x3f3f3f3f;
  }
  for(int i=1;i<=m;i++){
    int t=read();
    ans[i+n]=0x3f3f3f3f;
    switch(t){
      case 1:{
        pos[i+n].x=read()+1;
        pos[i+n].y=read()+1;
        pos[i+n].t=i+1;
        pos[i+n].quan=1;
        pos[i+n].i=i+n;
        break;
      }
      case 2:{
        pos[i+n].x=read()+1;
        pos[i+n].y=read()+1;
        pos[i+n].t=i+1;
        pos[i+n].i=i+n;
        qu.push(i+n);
        pos[i+n].quan=0;
      }
    }
  }
  std::sort(pos+1,pos+n+m+1,cmp1);
  cdq(1,n+m);
  for(int i=1;i<=n+m;i++){
    pos[i].y=1000005-pos[i].y;
  }
  std::sort(pos+1,pos+n+m+1,cmp1);
  cdq(1,n+m);
  for(int i=1;i<=n+m;i++){
    pos[i].x=1000005-pos[i].x;
  }
  std::sort(pos+1,pos+n+m+1,cmp1);
  cdq(1,n+m);
  for(int i=1;i<=n+m;i++){
    pos[i].y=1000005-pos[i].y;
  }
  std::sort(pos+1,pos+n+m+1,cmp1);
  cdq(1,n+m);
  while(qu.size()>0){
    printf("%d\n",ans[qu.front()]);
    qu.pop();
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

#### [P4390](https://www.luogu.com.cn/problem/P4390) Mokia 摩基亚

动态加点并且询问矩阵元素和。

CDQ分治在处理重复元素的时候会出错，但是这道题很明显同一个点只有一个对答案产生贡献，适当重排顺序把可以贡献的放前面即可。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
inline int read();
long long ans[2000005];
class node{
  public:
    int x,y,num,i;
    node(int a=0,int b=0,int c=0,int d=0){
      x=a;
      y=b;
      num=c;
      i=d;
    }
}seq[2000005],temp[2000005];
int qx1[2000005],qx2[2000005],qy1[2000005],qy2[2000005];
long long tree[2000005];
int lowbit(int x){
  return x&(-x);
}
void mod(int x,int y){
  while(x<=2000000){
    tree[x]+=y;
    x+=lowbit(x);
  }
  return ;
}
int query(int x){
  int ans=0;
  while(x>0){
    ans+=tree[x];
    x-=lowbit(x);
  }
  return ans;
}
bool cmp(node a,node b){
  if(a.x!=b.x){
    return a.x<b.x;
  }
  if(a.y!=b.y){
    return a.y<b.y;
  }
  if(a.i!=b.i){
    return a.i<b.i;
  }
  return a.num>b.num;
}
void cdq(int l,int r){
  if(l>=r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  cdq(mid+1,r);
  int nl=l,nr=mid+1;
  int now=l;
  while(nl<=mid&&nr<=r){
    if(seq[nl].y<=seq[nr].y){
      temp[now++]=seq[nl];
      mod(seq[nl].i,seq[nl].num);
      nl++;
    }else{
      temp[now++]=seq[nr];
      ans[seq[nr].i]+=query(seq[nr].i);
      nr++;
    }
  }
  while(nl<=mid){
    temp[now++]=seq[nl];
    mod(seq[nl].i,seq[nl].num);
    nl++;
  }
  while(nr<=r){
    temp[now++]=seq[nr];
    ans[seq[nr].i]+=query(seq[nr].i);
    nr++;
  }
  for(int i=l;i<=mid;i++){
    mod(seq[i].i,-seq[i].num);
  }
  for(int i=l;i<=r;i++){
    seq[i]=temp[i];
  }
  return ;
}
signed main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen(".in","r",stdin);
  freopen(".out","w",stdout);
  #endif
  int op=read();
  int w;
  int now=0;
  int qq=0;
  while(op!=3){
    if(op==0){
      w=read();
    }else if(op==1){
      seq[++now].x=read();
      seq[now].y=read();
      seq[now].num=read();
      seq[now].i=now;
    }else{
      int x1=read();
      int y1=read();
      int x2=read();
      int y2=read();
      now++;
      seq[now]=node(x2,y2,0,now);
      qx1[++qq]=now;
      now++;
      seq[now]=node(x1-1,y2,0,now);
      qx2[qq]=now;
      now++;
      seq[now]=node(x2,y1-1,0,now);
      qy1[qq]=now;
      now++;
      seq[now]=node(x1-1,y1-1,0,now);
      qy2[qq]=now;
    }
    op=read();
  }
  std::sort(seq+1,seq+now+1,cmp);
  cdq(1,now);
  for(int i=1;i<=qq;i++){
    printf("%lld\n",ans[qx1[i]]-ans[qx2[i]]-ans[qy1[i]]+ans[qy2[i]]);
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

#### [P3658](https://www.luogu.com.cn/problem/P3658) Why Did the Cow Cross the Road III P

给定两个排列 $a,b$ ，求出所有数对$(i,j)$满足 $a_i \leq a_j , b_j \leq b_i , |i-j| > k$ 的数量，同样是三维偏序关系，统计答案时我们仍然可以发现 $|i-j| > k$ 的条件实际上就是值域 $[0,i-k-1] \cup [i+k+1,\infty]$ 的答案，树状数组仍然可以做。

读入方式有一点奇怪，请参考代码。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#define int long long
inline int read();
class node{
  public:
    int a,b,i;
    node(int x=0,int y=0,int z=0){
      a=x;
      b=y;
      i=z;
      return ;
    }
}seq[100005],temp[100005];
bool operator < (node a,node b){
  if(a.a!=b.a){
    return a.a<b.a;
  }
  return a.b>b.b;
}
int tree[100005];
int lowbit(int x){
  return x&(-x);
}
void add(int x,int y){
  while(x<=100000){
    tree[x]+=y;
    x+=lowbit(x);
  }
  return ;
}
int query(int x){
  int ans=0;
  while(x>0){
    ans+=tree[x];
    x-=lowbit(x);
  }
  return ans;
}
int ans=0;
int k;
void cdq(int l,int r){
  if(l>=r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  cdq(mid+1,r);
  int nl=l,nr=mid+1;
  int now=l-1;
  while(nl<=mid&&nr<=r){
    if(seq[nl].b>=seq[nr].b){
      temp[++now]=seq[nl];
      add(seq[nl].i,1);
      nl++;
    }else{
      temp[++now]=seq[nr];
      ans+=(query(100000)-(query(std::min(seq[nr].i+k,100000ll))-query(std::max(seq[nr].i-k-1,0ll))));
      nr++;
    }
  }
  while(nl<=mid){
    temp[++now]=seq[nl];
    add(seq[nl].i,1);
    nl++;
  }
  while(nr<=r){
    temp[++now]=seq[nr];
    ans+=(query(100000)-(query(std::min(seq[nr].i+k,100000ll))-query(std::max(seq[nr].i-k-1,0ll))));
    nr++;
  }
  for(int i=l;i<=mid;i++){
    add(seq[i].i,-1);
  }
  for(int i=l;i<=r;i++){
    seq[i]=temp[i];
  }
  return ;
}
signed main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen(".in","r",stdin);
  freopen(".out","w",stdout);
  #endif
  int n=read();
  k=read();
  for(int i=1;i<=n;i++){
    int x=read();
    seq[x].a=i;
  }
  for(int i=1;i<=n;i++){
    int y=read();
    seq[y].b=i;
    seq[y].i=y;
  }
  std::sort(seq+1,seq+n+1);
  cdq(1,n);
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

### DP转移


DP转移的条件非常像偏序问题的，同样可以CDQ分治。

具体思路是先 `solve(l,mid)` ，然后处理中间的，然后 `solve(mid+1,r)` 。

部分问题的转移只能从前面转移到后面，那CDQ分治中间处理完之后还要恢复到排序之前的状态。

#### [P4093](https://www.luogu.com.cn/problem/P4093) 序列

$dp_i=\max\limits_{j<i}\{dp_j+1\}(mx_j\leq a_i,a_j\leq mn_j)$ 。

条件非常的偏序，直接套CDQ分治即可。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
inline int read();
class node{
  public:
    int seq,mx,mn,i;
    node(int a=0,int b=0,int c=0,int t=0){
      seq=a;
      mx=b;
      mn=c;
      i=t;
      return ;
    }
}seq[100005];
bool cmp1(node a,node b){
  return a.mx<b.mx;
}
bool cmp2(node a,node b){
  return a.seq<b.seq;
}
bool cmp3(node a,node b){
  return a.i<b.i;
}
int tree[100005];
int lowbit(int x){
  return x&(-x);
}
int query(int x){
  int ans=-0x3f3f3f3f;
  while(x>0){
    ans=std::max(ans,tree[x]);
    x-=lowbit(x);
  }
  return ans;
}
void mod(int x,int y){
  while(x<=100000){
    tree[x]=std::max(tree[x],y);
    x+=lowbit(x);
  }
  return ;
}
void clear(int x){
  while(x<=100000){
    tree[x]=-0x3f3f3f3f;
    x+=lowbit(x);
  }
  return ;
}
int dp[100005];
void cdq(int l,int r){
  if(l>=r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  std::sort(seq+l,seq+mid+1,cmp1);
  std::sort(seq+mid+1,seq+r+1,cmp2);
  int nl=l,nr=mid+1;
  while(nl<=mid&&nr<=r){
    if(seq[nl].mx<=seq[nr].seq){
      mod(seq[nl].seq,dp[seq[nl].i]+1);
      nl++;
    }else{
      dp[seq[nr].i]=std::max(dp[seq[nr].i],query(seq[nr].mn));
      nr++;
    }
  }
  while(nl<=mid){
    mod(seq[nl].seq,dp[seq[nl].i]+1);
    nl++;
  }
  while(nr<=r){
    dp[seq[nr].i]=std::max(dp[seq[nr].i],query(seq[nr].mn));
    nr++;
  }
  for(int i=l;i<=mid;i++){
    clear(seq[i].seq);
  }
  std::sort(seq+l,seq+r+1,cmp3);
  cdq(mid+1,r);
  return ;
}
int main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
  int n=read(),m;
  m=read();
  for(int i=1;i<=100000;i++){
    tree[i]=-0x3f3f3f3f;
  }
  for(int i=1;i<=n;i++){
    dp[i]=1;
    seq[i].seq=seq[i].mx=seq[i].mn=read();
    seq[i].i=i;
  }
  for(int i=1;i<=m;i++){
    int x=read();
    int y=read();
    seq[x].mx=std::max(seq[x].mx,y);
    seq[x].mn=std::min(seq[x].mn,y);
  }
  cdq(1,n);
  int ans=-0x3f3f3f3f;
  for(int i=1;i<=n;i++){
    ans=std::max(ans,dp[i]);
  }
  printf("%d",ans);
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

#### [P3364](https://www.luogu.com.cn/problem/P3364) Cool loves touli

可以一眼瞪出按照等级排序之后只用 $dp_i=\max\limits_{j<i} \{dp_j+1\}$  即可，其中 $j$ 的限制就是题目中那坨不大于不小于，而这是偏序关系，CDQ分治优化转移即可。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
inline int read();
class node{
  public:
    int l,s,w,a;
    int i;
    node(int xx=0,int yy=0,int zz=0,int aa=0,int ii=0){
      l=xx;
      s=yy;
      w=zz;
      a=aa;
      i=ii;
      return ;
    }
}seq[100005];
bool cmp1(node a,node b){
  return a.a<b.a;
}
bool cmp2(node a,node b){
  return a.s<b.s;
}
bool cmp3(node a,node b){
  return a.i<b.i;
}
bool cmp4(node a,node b){
  return a.l<b.l;
}
int tree[1000005];
int lowbit(int x){
  return x&(-x);
}
int query(int x){
  int ans=-0x3f3f3f3f;
  while(x>0){
    ans=std::max(ans,tree[x]);
    x-=lowbit(x);
  }
  return ans;
}
void mod(int x,int y){
  while(x<=1000000){
    tree[x]=std::max(tree[x],y);
    x+=lowbit(x);
  }
  return ;
}
void clear(int x){
  while(x<=1000000){
    tree[x]=-0x3f3f3f3f;
    x+=lowbit(x);
  }
  return ;
}
int dp[1000005];
void cdq(int l,int r){
  if(l>=r){
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  std::sort(seq+l,seq+mid+1,cmp1);
  std::sort(seq+mid+1,seq+r+1,cmp2);
  int nl=l,nr=mid+1;
  while(nl<=mid&&nr<=r){
    if(seq[nl].a<=seq[nr].s){
      mod(seq[nl].w,dp[seq[nl].i]);
      nl++;
    }else{
      dp[seq[nr].i]=std::max(dp[seq[nr].i],query(seq[nr].a)+1);
      nr++;
    }
  }
  while(nl<=mid){
    mod(seq[nl].w,dp[seq[nl].i]);
    nl++;
  }
  while(nr<=r){
    dp[seq[nr].i]=std::max(dp[seq[nr].i],query(seq[nr].a)+1);
    nr++;
  }
  for(int i=l;i<=mid;i++){
    clear(seq[i].w);
  }
  std::sort(seq+l,seq+r+1,cmp4);
  cdq(mid+1,r);
  return ;
}
int X[1000005];
int main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
  int n=read();
  int m=0;
  for(int i=1;i<=n;i++){
    dp[i]=1;
    seq[i].l=read();
    X[++m]=seq[i].l;
    seq[i].s=read();
    X[++m]=seq[i].s;
    seq[i].w=read();
    X[++m]=seq[i].w;
    seq[i].a=read();
    X[++m]=seq[i].a;
    seq[i].i=i;
  }
  std::sort(X+1,X+m+1);
  for(int i=1;i<=n;i++){
    seq[i].l=std::lower_bound(X+1,X+m+1,seq[i].l)-X;
    seq[i].s=std::lower_bound(X+1,X+m+1,seq[i].s)-X;
    seq[i].w=std::lower_bound(X+1,X+m+1,seq[i].w)-X;
    seq[i].a=std::lower_bound(X+1,X+m+1,seq[i].a)-X;
  }
  std::sort(seq+1,seq+n+1,cmp4);
  cdq(1,n);
  int ans=-0x3f3f3f3f;
  for(int i=1;i<=n;i++){
    ans=std::max(ans,dp[i]);
  }
  printf("%d",ans);
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

tips: 请注意题目是否真的满足偏序关系，只有像这道题仅要求相邻的元素满足偏序关系才行，这是因为偏序关系没有传递性。


### 斜率优化/凸包维护

cdq分治的形式可以让我们维护很多DP转移，其中最经典的就是斜率优化，另外实际上这样的方式本质维护了凸包。

#### [P2497](https://www.luogu.com.cn/problem/P2497) 基站建设

可以发现，转移式为 $dp_i=\min\{dp_j+\frac {x_i-x_j} {2  \sqrt{r_j}}\}+v_i$ 。

整个式子非常的斜率优化，所有我们分离一下相关的函数。

$$y=dp_j-\frac {x_j} {2\sqrt r_j}$$

$$k=x_i$$

$$x=-\frac 1 {2\sqrt{r_j}}$$

$$b=dp_i-v_i$$

~~做毛线斜率优化，nm x 不单调~~

这样的凸包实际上维护方法很多，经典的办法有各路高级数据结构，当然也有CDQ分治。

原理很好理解，直接转移我们不能排序保证 $x$ 的单调性，实际上这个问题和三维偏序中为什么不能直接计算的原因很类似，而CDQ分治在分治的每一层只处理部分转移从而允许进行排序。形式和DP转移一样。

先是 `solve(l,mid)` ，然后进行排序， 先维护 `[l,mid]` 的凸包，这里排序后直接使用单调栈进行维护，然后再 `solve(mid+1,r)` 。

请注意，和维护DP转移一样，你可能需要处理完后按照最开始的下标再排一边序。


```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#include <deque>
#define int long long
#define double long double
inline int read();
class node{
    public:
        int x,r,v,a;
        double dp;
        node(int aa=0,int b=0,int c=0,int d=0,double e=0){
            x=aa;
            r=b;
            v=c;
            a=d;
            dp=e;
        }
};
int x[500005];
int r[500005];
int v[500005];
int a[500005];
node temp[500005];
double dp[500005];
int qu[2000005];
int tail;
double X(int i){
    // return 1.0/(2*std::sqrt(r[i]));
    return std::sqrt(r[i])/r[i]/2.0;
}
double Y(int i){
    return -dp[i]+((double)(x[i]))/(2*std::sqrt(r[i]));
}
double K(int i){
    return x[i];
}
double B(int i){
    return v[i];
}
bool cmp(int i,int j,int i2,int j2){
    // return (Y(i)-Y(j))/(X(i)-X(j))<(Y(i2)-Y(j2))/(X(i2)-X(j2));
    return (Y(i)-Y(j))*(X(i2)-X(j2))>(Y(i2)-Y(j2))*(X(i)-X(j));
}
void cdq(int l,int r){
    if(l>=r){
        return ;
    }
    int mid=(l+r)/2;
    cdq(l,mid);
    tail=0;
    for(int i=l;i<=mid;i++){
        while(tail>1&&cmp(i,qu[tail],qu[tail],qu[tail-1])){
            tail--;
        }
        qu[++tail]=i;
    }
    for(int i=mid+1;i<=r;i++){
        while(tail>1&&x[i]*(X(qu[tail])-X(qu[tail-1]))>(Y(qu[tail])-Y(qu[tail-1]))){
            tail--;
        }
        int j=qu[tail];
        dp[i]=std::min(dp[i],(dp[j]+v[i]+((double)(x[i]-x[j])/(2.0*std::sqrt(::r[j])))));
    }
    cdq(mid+1,r);
    int l1=l,l2=mid+1;
    int now=l-1;
    while(l1<=mid&&l2<=r){
        if(a[l1]>=a[l2]){
            temp[++now].a=a[l1];
            temp[now].r=::r[l1];
            temp[now].v=v[l1];
            temp[now].x=x[l1];
            temp[now].dp=dp[l1];
            l1++;
        }else{
            temp[++now].a=a[l2];
            temp[now].r=::r[l2];
            temp[now].v=v[l2];
            temp[now].x=x[l2];
            temp[now].dp=dp[l2];
            l2++;
        }
    }
    while(l1<=mid){
        temp[++now].a=a[l1];
        temp[now].r=::r[l1];
        temp[now].v=v[l1];
        temp[now].x=x[l1];
        temp[now].dp=dp[l1];
        l1++;
    }
    while(l2<=r){
        temp[++now].a=a[l2];
        temp[now].r=::r[l2];
        temp[now].v=v[l2];
        temp[now].x=x[l2];
        temp[now].dp=dp[l2];
        l2++;
    }
    for(int i=l;i<=r;i++){
        a[i]=temp[i].a;
        x[i]=temp[i].x;
        ::r[i]=temp[i].r;
        v[i]=temp[i].v;
        dp[i]=temp[i].dp;
    }
    
}
signed main(){
	#ifdef ONLINE_JUDGE
	#else
	freopen(".in","r",stdin);
	freopen(".out","w",stdout);
	#endif
	int n=read(),m;
    m=read();
    for(int i=1;i<=n;i++){
        x[i]=read();
        r[i]=read();
        v[i]=read();
        a[i]=r[i];
        dp[i]=0x3f3f3f3f3f3f3f3f;
    }
    dp[1]=v[1];
    cdq(1,n);
    double ans=0x3f3f3f3f3f3f3f3f;
    for(int i=1;i<=n;i++){
        if(r[i]+x[i]>=m){
            ans=std::min(ans,dp[i]);
        }
    }
    printf("%.3Lf",ans);
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

#### [P4027](https://www.luogu.com.cn/problem/P4027) 货币兑换

先瞪DP式，我们设 $dp_i$ 表示第 $i$ 天所有的钱数的最大值，我们的dp式实际上非常简单，朴素转移直接枚举上一次买入的时间即可。

$$k_i=\frac {dp_iRate_i} {a_iRate_i+b_i}$$

$$b_i=\frac {dp_i} {a_iRate_i+b_i}$$

$$x_i=\frac {a_i} {b_i}$$

$$y_i=dp_i$$

~~好看吗？反正 x 还是不单调~~ 。

继续CDQ分治，如法炮制即可，但是注意除开上面那样DP,我们也可以直接从 $dp_{i-1}$ 转移到 $dp_i$ ，我们只用在CDQ递归到只有一个点的区间的时候处理即可。

如果有 $x_i=x_j$ 的情况出现，我们可认为斜率为 $\infty$ ，但是你需要特判因为你会发现有负数这个东西。

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>
#define double long double
inline int read();
double a[100005];
double b[100005];
double rate[100005];
double dp[100005];
int seq[100005];
int tp;
int sta[100005];
double X(int x){
  return (dp[x]*rate[x])/(a[x]*rate[x]+b[x]);
}
double Y(int x){
  return dp[x]/(a[x]*rate[x]+b[x]);
}
double K(int x){
  return -a[x]/b[x];
}
bool cmpK(int x,int y){
  return a[x]*b[y]>a[y]*b[x];
}
bool cmpK2(int x1,int y1,int x2,int y2){
  if(X(x1)==X(y1)){
    return 0;
  }
  if(X(x2)==X(y2)){
    return 1;
  }
  return (Y(y1)-Y(x1))*(X(y2)-X(x2))<(Y(y2)-Y(x2))*(X(y1)-X(x1));
}
bool cmp(int x,int y){
  return X(x)<X(y);
}
void cdq(int l,int r){
  if(l>=r){
    dp[l]=std::max(dp[l],dp[l-1]);
    return ;
  }
  int mid=(l+r)/2;
  cdq(l,mid);
  for(int i=l;i<=r;i++){
    seq[i]=i;
  }
  std::sort(seq+l,seq+mid+1,cmp);
  std::sort(seq+mid+1,seq+r+1,cmpK);
  tp=0;
  for(int i=l;i<=mid;i++){
    while(tp>1&&cmpK2(sta[tp-1],sta[tp],sta[tp],seq[i])){
      tp--;
    }
    sta[++tp]=seq[i];
  }
  for(int i=mid+1;i<=r;i++){
    while(tp>1&&(Y(sta[tp-1])-Y(sta[tp]))/(X(sta[tp-1])-X(sta[tp]))<K(seq[i])){      
      tp--;
    }
    int &j=sta[tp];
    dp[seq[i]]=std::max(dp[seq[i]],X(j)*a[seq[i]]+Y(j)*b[seq[i]]);
  }
  cdq(mid+1,r);
  return ;
}
int main(){
  #ifdef ONLINE_JUDGE
  #else
  freopen(".in","r",stdin);
  freopen(".out","w",stdout);
  #endif
  int n=read(),s;
  s=read();
  dp[1]=s;
  for(int i=1;i<=n;i++){
    scanf("%Lf%Lf%Lf",&a[i],&b[i],&rate[i]);
  }
  cdq(1,n);
  printf("%.3Lf\n",dp[n]);
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
