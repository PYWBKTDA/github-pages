---
title: Single-Source Shortest Paths with Negative Real Weights in $tilde{O}(mn^{8/9})$ Time
date: 2024-11-19
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

$dist^{r}_{G}(u,v)$表示$G$中$u$到$v$经过不超过$r$条负权边的最短路（默认$r=\infty$）

$dist^{r}_{G}(S,v)=\min_{u\in S}dist^{r}_{G}(u,v)$	$R^{r}_{G}(S)=\{v\in V\mid dist^{r}_{G}(S,v)<0\}$

$thru_{G}^{r}(u,x,v)=dist_{G}^{r}(u,x)+dist_{G}^{r}(x,v)$	$BW_{G}^{r}(u,v)=\{x\in V\mid thru^{r}_{G}(u,x,v)<0\}$

$G_{\phi}$表示修改$w_{\phi}(u,v)=w(u,v)+\phi(u)-\phi(v)$所得的图

> 一条路径在$G$和$G_{\phi}$中的权值变化仅取决于起点和终点，因此不影响最短路
>
> 另外，要求$\phi$满足$w(u,v)\ge 0\Longrightarrow w_{\phi}(u,v)\ge 0$

不妨假定每个点度数为$O(m/n)$且$w(u,v)<0\Longrightarrow u$出度为$1$

> 对于前者，设某个点度数为$d$，将其拆为$\frac{d}{m/n}$个点，用边权为$0$的双向链连接
>
> 对于后者，记$W_{u}=\min\limits_{(u,v)\in E}w(u,v)$，将$u$拆为两个点，用边权为$W_{u}$的有向边连接

$V^{-}=\{u\in V\mid \exists v\in V,w(u,v)<0\}$，对于$S\subseteq V^{-},G_{S}$表示仅保留$S$中负权边所得的图

##### Step 1

取$r=\Theta(k^{1/9})$，求势能函数$\phi$，使得$\forall u,v\in V,|BW_{\phi}^{r+1}(u,v)|\le n/r$

随机大小为$s=cr\ln n$的集合$T\subseteq V$，将问题转换为求$\phi$使得$\forall u,v\in V,T\cap BW_{\phi}^{r+1}(u,v)=\empty$

> $thru^{r+1}(u,x,v)$和$thru_{\phi}^{r+1}(u,x,v)$关于$x$顺序相同，因此$\mathbb{P}(|BW_{\phi}^{r+1}(u,v)|>n/r)\le \frac{{n-n/r-1\choose s}}{n\choose s}\le n^{-c}$，正确率$\ge 1-n^{2-c}$

限制即$\phi(v)\le \phi(u)+\min\limits_{x\in T}thru^{r+1}(u,x,v)$，构造图$H=(V,(V\times T)\cup (T\times V),dist^{r+1}_{G})$，取$\phi(v)=dist_{H}(V,v)$

最短长度不超过$|T|+1$，时间复杂度为$\tilde{O}(mr^{2})$

##### Step 2

求大小为$\Omega(k^{1/3})$的集合$S\subseteq V^{-}$，满足以下条件之一：

- $\forall u,v\in S,dist^{1}(u,v)\ge 0$
- $\exists x,y\in V,\forall u\in S,dist^{1}(x,u),dist^{1}(u,y)<0$

依次确定$x$和$y$，问题也即对于$1\le \rho\le k$，求集合$S\subseteq V^{-}$，满足以下条件之一：

- 大小为$\Omega(\rho)$且$\forall u,v\in S,dist^{1}(u,v)\ge 0$
- 大小为$\Omega(k/\rho)$且$\exists x\in V,\forall u\in S,dist^{1}(x,u)<0$

若$\forall x\in V^{-},|R^{1}(x)|<ck/\rho$，随机大小为$\frac{1}{4}\rho/c$的集合$T\subseteq V$，取$S=\{u\in T\mid u\not\in R^{1}(T)\}$

> $\mathbb{P}(u\in R^{1}(T))\le \frac{1}{4}\Longrightarrow \mathbb{P}(|S|\ge \frac{1}{8}\rho/c)\ge \frac{1}{2}$

随机集合$T\subseteq V$，每个点有$\rho/k$的概率在$T$中，重复上述过程$c\ln n$次，取超过$c\ln n/2$次满足$R^{1}(x)\cap T\ne \empty$的$x$

> - 若$|R^{1}(x)|<\frac{1}{4}k/\rho$，则$\mathbb{P}(R^{1}(x)\cap T\ne\empty)\le \frac{1}{4}\Longrightarrow \mathbb{P}(t\ge c\ln n/2)\le n^{-c/12}$（Chernoff Bound）
> - 若$|R^{1}(x)|\ge 2k/\rho$，则$\mathbb{P}(R^{1}(x)\cap T\ne\empty)\ge \frac{3}{4}\Longrightarrow \mathbb{P}(t<c\ln n/2)\le n^{-c/12}$
>
> 换言之，有$1-n^{1-c/12}$的概率，选出的$x$必然满足$|R^{1}(x)|\ge \frac{1}{4}k/\rho$且$|R^{1}(x)|\ge 2k/\rho$时必然被选出

##### Step 3

求势能函数$\phi$，消除$G_{S}$中的负权边（以下默认$G$为$G_{S}$）

对于前者，取$\phi(v)=dist(V,v)=\min\{dist^{1}(S,v),0\}$

对于后者，取$\phi(v)=\min\{\max\{dist^{r+1}(x,v),-dist^{r+1}(v,y)\},0\}$，则$R_{\phi}^{r}(S)\subseteq BW^{r+1}(x,y)$

> $\phi(x)=\phi(y)=0$	$\forall u\in S,\phi(u)=0$	$\forall v\not\in BW^{r+1}(x,y),\phi(v)=\min\{dist^{r+1}(x,v),0\}$
>
> $dist^{r}_{\phi}(u,v)\ge dist^{r+1}_{\phi}(x,v)-dist_{\phi}^{1}(x,u)\ge 0$

构造图$H$，将每个点拆成$r+1$层，用边权为$0$的双向链连接，并从$u_{i}$向$v_{i}$连非负权边、向$v_{i+1}$连负权边

取$\phi_{H}(v_{i})=dist^{i}(V,v)$，对于$v\not\in R^{r}(S)$，将对应的$r+1$个点缩点，并取$\phi(v)=dist(V,v)=dist_{\phi_{H}}^{\lceil k/r\rceil}(V_{0},v_{0})$

每个点度数为$O(m/n)$，时间复杂度为$\tilde{O}(mk^{1/3}/r)$

##### Step 4

重复Step 1~3，直至消除所有负权边

每轮的时间复杂度为$\tilde{O}(mr^{2}+mk^{1/3}/r)=\tilde{O}(mk^{2/9})$，并有$\Omega(1)$的概率消除$\Omega(k^{1/3})$条负权边

期望$O(k^{2/3})$轮会消除$\Omega(k)$条负权边，总时间复杂度为$\tilde{O}(mn^{8/9})$
