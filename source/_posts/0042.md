---
title: CSP-S SC 迷惑行为大赏
date: 2024-11-04
tags: [OI]
toc: true
mathjax: true
---

# CSP-S SC 迷惑行为大赏

## 统计

共打出了 $8313$ 个 `freopen`. 

```
$ rg freopen --stats
8313 matches
8247 matched lines
4112 files contained matches
4258 files searched
```

恭祝共有 $4258-4112=146$ 个文件没有freopen

$9096$ 个 `return`.

```
$ rg return --stats
9096 matches
9082 matched lines
4045 files contained matches
4258 files searched
```

$121$ 个 114514。

```
$ rg "114514" --stats
121 matches
97 matched lines
65 files contained matches
```

## 经典 freopen

### detect,in

```
$ rg ",in\""
SC-S01101/detect/detect.cpp
13:     freopen("detect,in","r",stdin);
```

### ,out

```
$ rg ",out\""      
SC-S01101/detect/detect.cpp
14:     freopen("detect,out","w",stdout); 

SC-S01218/detect/detect.cpp
61:     freopen("arena,out", "w", stdout);

SC-S00831/color/color.cpp
5:      freopen("color,out";w;stdout)

SC-S00831/arena/arena.cpp
5:      freopen("arena,out";w;stdout)
```

### //freopen

```
$ rg "//*freopen" 
SC-S00619/detect/detect.cpp
6:      //freopen("duel.in","r",stdin);
7:      //freopen("duel.out","w",stdout);

SC-S00619/duel/duel.cpp
7:      //freopen("duel.in","r",stdin);
8:      //freopen("duel.out","w",stdout);

SC-S00619/arena/arena.cpp
5:      //freopen("duel.in","r",stdin);
6:      //freopen("duel.out","w",stdout);

SC-S00666/detect/detect.cpp
5:      //freopen("detect.in","r",stdin);
6:      //freopen("detect.out","w",stdout);

SC-S00820/detect/detect.cpp
13:    //freopen("detect.out ","w",stdout);

SC-S01241/color/color.cpp
41:     //freopen("detect.in","r",stdin);
42:     //freopen("detect.out","w",stdout);

SC-S01241/detect/detect.cpp
41:     //freopen("detect.in","r",stdin);
42:     //freopen("detect.out","w",stdout);

SC-S01241/arena/arena.cpp
41:     //freopen("detect.in","r",stdin);
42:     //freopen("detect.out","w",stdout);

SC-S00449/color/color.cpp
4://freopen("color.in","r",stdin);
5://freopen("color.out","w",stdout);

SC-S00025/detect/detect.cpp
9:      //freopen("detect.in","r",stdin);
10:     //freopen("detect.out","w",stdout);

SC-S00125/arena/arena.cpp
13:don't : //freopen //freopen //freopen

SC-S01103/color/color.cpp
25://freopen

SC-S01103/detect/detect.cpp
25://freopen

SC-S01103/arena/arena.cpp
25://freopen

SC-S00061/color/color.cpp
5:      //freopen("duel.in","r",stdin);

SC-S00061/detect/detect.cpp
5:      //freopen("duel.in","r",stdin);

SC-S00061/arena/arena.cpp
5:      //freopen("duel.in","r",stdin);

SC-S01208/arena/arena.cpp
45:     //freopen("pyin.txt","r",stdin);

SC-S00217/color/color.cpp
36:     //freopen("test.in","r",stdin);
37:     //freopen("test.out","w",stdout);

SC-S00217/detect/detect.cpp
11:     //freopen("test.in","r",stdin);
12:     //freopen("test.out","w",stdout);

SC-S00217/duel/duel.cpp
8:      //freopen("test.in","r",stdin);
9:      //freopen("test.out","w",stdout);

SC-S00217/arena/arena.cpp
16:     //freopen("test.in","r",stdin);
17:     //freopen("test.out","w",stdout);

SC-S01179/detect/detect.cpp
25:     //freopen("detect.in", "r", stdin);
26:     //freopen("detect.out","w",stdout);

SC-S00265/arena/arena.cpp
103://freopen()

SC-S00952/arena/arena.cpp
18:     //freopen(".in", "r", stdin);
19:     //freopen(".out", "w", stdout);

SC-S00176/detect/detect.cpp
14:     //freopen("detect.in","r",stdin);
15:     //freopen("detect.out","w",stdout);

SC-S00707/arena/arena.cpp
24://freopen
25://freopen
```

### 神秘错误

```
$ rg "in\",\"w\",stdin"
SC-S01173/color/color.cpp
5:      freopen("color.in","w",stdin);

SC-S00662/color/color.cpp
2:freopen("color.in","w",stdin)

SC-S00662/detect/detect.cpp
2:freopen("detect.in","w",stdin)

SC-S00662/duel/duel.cpp
2:freopen("duel.in","w",stdin)

SC-S00662/arena/arena.cpp
2:freopen("arena.in","w",stdin)

SC-S00011/color/color.cpp
8:      freopen("color.in","w",stdin);

SC-S00011/duel/duel.cpp
9:      freopen("duel.in","w",stdin);

SC-S00330/color/color.cpp
29:     freopen("color.in","w",stdin);

```

```
$ rg "out\",\"w\",stdin"
SC-S00214/detect/detect.cpp
52:     freopen("detect.out","w",stdin);

SC-S00266/color/color.cpp
32:     freopen("color.out","w",stdin);

SC-S00266/arena/arena.cpp
8:      freopen("arena.out","w",stdin);

```

```
$ rg "out\",\"r\""      
SC-S01173/color/color.cpp
6:      freopen("color.out","r",stdout);

SC-S00784/arena/arena.cpp
8:      freopen("arena.out","r",stdout);

SC-S00123/detect/detect.cpp
162:    freopen("detect.out","r",stdin);

SC-S00662/color/color.cpp
3:freopen("color.out","r",stdout)

SC-S00662/detect/detect.cpp
3:freopen("detect.out","r",stdout)

SC-S00662/duel/duel.cpp
3:freopen("duel.out","r",stdout)

SC-S00662/arena/arena.cpp
3:freopen("arena.out","r",stdout)

SC-S00423/color/color.cpp
7:      freopen("color.out","r",stdout);

SC-S00229/color/color.cpp
27:     freopen("color.out","r",stdout);

SC-S00229/detect/detect.cpp
27:     freopen("detect.out","r",stdout);

SC-S00330/color/color.cpp
30:     freopen("color.out","r",stdout);

SC-S00011/color/color.cpp
9:      freopen("color.out","r",stdout);

SC-S00011/duel/duel.cpp
10:     freopen("duel.out","r",stdout);
```

### ".in"

```
$ rg "\(\".in\""
SC-S00140/detect/detect.cpp
36:     freopen (".in", "r", stdin);

SC-S00952/arena/arena.cpp
18:     //freopen(".in", "r", stdin);
```

### ".out"

```
$ rg "\(\".out\""
SC-S00140/detect/detect.cpp
37:     freopen (".out", "w", stdout);

SC-S00952/arena/arena.cpp
19:     //freopen(".out", "w", stdout);
```

## 贴脸开大

```
$ rg "fuck"
SC-S*****/arena/arena.cpp
4:      printf("329154437110732\n894132907628644");//fuck CCF!!!
```

已对相关考生考号模糊处理。

## 值得赞扬

```
$ rg "love ccf"
SC-S00125/arena/arena.cpp
11:i LOVE CCF i LOVE CCF i LOVE CCF love ccf love ccf

SC-S00128/detect/detect.cpp
77://                                   cout<<"i love ccf\n";
```

## money

```
$ rg "money"
```

没有 money

## AK 之梦

```
$ rg "AK" 
SC-S00106/detect/detect.cpp
14:// 祝 cdqz 的 rgw NOI Au and AK IOI!

SC-S01116/arena/arena.cpp
7:      int AKIOI_AC_AC_AC_114514_1919810=rand()%2;
8:      if(AKIOI_AC_AC_AC_114514_1919810)cout<<114514;

SC-S00200/arena/arena.cpp
128:估计 way,lsc,nzy,hzx,lph,csy 中间有一个人能做出来，祝他们 AK csp-S!!! 

SC-S00532/detect/detect.cpp
81:    printf("I AK IOI\n");

SC-S01237/arena/arena.cpp
92:                     SendDlgItemMessage(hwnd, IDC_MAIN_TEXT, WM_SETFONT,(WPAR
AM)GetStockObject(DEFAULT_GUI_FONT), MAKELPARAM(TRUE,0));

SC-S00854/detect/detect.cpp
148:首先看时间,AK是不可能的了awa

SC-S00854/duel/duel.cpp
42:嗯......其实咱有一个梦想......AK CSP!

SC-S00346/duel/duel.cpp
54://jeffstart AK IOI

SC-S00786/arena/arena.cpp
3:#include<bits/stdc++.h>//SC-S00761 (stranger12) AK CSP-S!!!!%%%%%%%%%

SC-S00265/arena/arena.cpp
102:后记：这个抽象注释使我没能AK

SC-S00073/color/color.cpp
4:cout<<"我要AK IOI";

SC-S00073/arena/arena.cpp
4:cout<<"我要AK IOI";

SC-S00299/arena/arena.cpp
128:已经高三了，没有AK还是挺遗憾的，但也不重要了。（前三题先开个香槟） 
```

## cym 续集

```
$ rg "cym"
SC-S00967/detect/detect.cpp
7:}cym[MAXN];
24:                     cym[i].l = (double)(d[i]);cym[i].r = (double)(L);
26:             else cym[i].l = cym[i].r = 0.00;return;
29:             cym[i].r = (double)(L);
30:             if(v[i]>V)cym[i].l = (double)(d[i]);
31:             else cym[i].l = (double)(d[i])+(V*V-(v[i]*v[i]))/((double)(2*a[i
]));
35:             if(v[i]<=V)cym[i].l = cym[i].r = 0;
37:                     cym[i].r = min((double)(L),(double)(d[i])+(V*V-(v[i]*v[i
]))/((double)(2*a[i])));cym[i].l = (double)(d[i]);
58:             sort(cym+1,cym+1+n,cmp);
64:                     if(cym[i].l==0)continue;
65:                     if(sum[(int)(cym[i].r)]-sum[upget(cym[i].l)-1]>0){
73:                     if(cym[i].r==0||!vis[i])continue;
74:                     if(cym[i].l<=p[now]&&now!=0)continue;
75:                     while(p[j]<cym[i].r&&j<m)j++;if(p[j]>cym[i].r)j--;
```

## 米哈游打钱系列

```
$ rg "Genshin"   
SC-S01103/color/color.cpp
27://Genshin

SC-S01103/detect/detect.cpp
27://Genshin

SC-S01103/arena/arena.cpp
27://Genshin
```

```
$ rg "草神"
SC-S01150/color/color.cpp
33://当你看见这句话时,我已经草神2+1(bushi 
35://因为你的草神已经被我拿走了(hh
38://能给草神做生贺

SC-S01150/detect/detect.cpp
33://当你看见这句话时,我已经草神2+1(bushi 
35://因为你的草神已经被我拿走了(hh
38://能给草神做生贺

SC-S01150/arena/arena.cpp
33://当你看见这句话时,我已经草神2+1(bushi 
35://因为你的草神已经被我拿走了(hh
38://能给草神做生贺
```

```
$ rg "genshin"
SC-S00636/arena/arena.cpp
186://                                                          if(xu[lson]==0)c
out<<"genshin\n";
```

## 爆 0 专场

```
$ rg "ye shi"
SC-S00677/arena/arena.cpp
22:     puts("ye shi bao ling le xiong di men");
```

## AFO

```
$ rg "AFO"
SC-S00125/arena/arena.cpp
15:i am gaoyi grade so may be i will be AFO this year or the next year

SC-S00632/arena/arena.cpp
22:and i can't get1= in 2022 & 2023. then i AFOed for
30:CTF works. if not, i will completely AFO.

SC-S00967/arena/arena.cpp
11:     I think its my time to AFO

SC-S00786/arena/arena.cpp
21:     else cout<<rand()%1145<<endl;//AFO QAQ

SC-S00285/arena/arena.cpp
10://  17:28    tmd！！！ans1错了！！！！！！！！！！！！！！反而ans2是对的（崩
了） AFO 
11://  17:30 AFO

SC-S01081/arena/arena.cpp
39:要被其他人吊打了，呜呜呜，要AFO了…… 
```

## slove

```
$ rg "slove"
SC-S00368/arena/arena.cpp
8:int slove(int l,int r,int dep){
28:     int L=slove(l,mid,dep-1);
29:     int R=slove(mid+1,r,dep-1);
71:                     ans[(int)(log2(i))]=slove(1,i,log2(i)-1);

SC-S00528/detect/detect.cpp
63:void dfs(int u,int last_check,int last_car_to_slove,int cnt){
64:    if(che(last_check,last_car_to_slove)){
65:        dfs(u+1,last_check,last_car_to_slove,cnt);
66:        dfs(u+1,,find_l(u,last_car_to_slove),cnt+1);
```

## 沉淀

```
$ rg "沉淀"

SC-S00265/arena/arena.cpp
78:首先发现当K_sp<Q时，会产生沉淀，但是这时候内环境不会产生变化，因为有神经-体液
-免疫调节网络。
```

## 膜拜大佬

```
$ rg "%%%"
SC-S00106/detect/detect.cpp
15:// orz rgw%%%

SC-S00106/duel/duel.cpp
5:// orz rgw%%%

SC-S00464/detect/detect.cpp
125://                          printf("%% * %lld %lld %lld *%%%%%%%%\n",i,awa,q
wq);

SC-S00786/arena/arena.cpp
3:#include<bits/stdc++.h>//SC-S00761 (stranger12) AK CSP-S!!!!%%%%%%%%%

SC-S01103/color/color.cpp
14://%%%%%%%%%%%%%%%%%%%%%%%%

SC-S01103/detect/detect.cpp
14://%%%%%%%%%%%%%%%%%%%%%%%%

SC-S01103/arena/arena.cpp
14://%%%%%%%%%%%%%%%%%%%%%%%%

SC-S00078/duel/duel.cpp
42://希望此题能拿100分！！%%%lyn，tjh，xxh，xxy，lqy，zjz 
```

## 励志故事

```
$ rg "hard"
SC-S00125/arena/arena.cpp
8:I'm in SC, so if i can get every pts i can and AC just a hard problem,
44:i love the problem that has easy timian but hard

SC-S00121/arena/arena.cpp
32:Though the life is hard;

SC-S01103/color/color.cpp
6://I love OI, and I have Worked hard.

SC-S01103/detect/detect.cpp
6://I love OI, and I have Worked hard.

SC-S01103/arena/arena.cpp
6://I love OI, and I have Worked hard.
```
