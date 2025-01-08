#
## Programing
Constructor initialization list: construct in the order of **declaration**
> Initialization shall proceed in the following order:
> * First, and only for the constructor of the most derived class as described below, virtual base classes shall be initialized in the order they appear on a depth-first left-to-right traversal of the directed acyclic graph of base classes, where “left-to-right” is the order of appearance of the base class names in the derived class base-specifier-list.
> 
> * Then, direct base classes shall be initialized in declaration order as they appear in the base-specifier-list (regardless of the order of the mem-initializers).
> 
> * Then, nonstatic data members shall be initialized in the order they were declared in the class definition (again regardless of the order of the mem-initializers).
> 
> * Finally, the body of the constructor is executed.


#
## Divide and Conquer
### HW 1
> In: 两组多维点 $A$, $B$ <br>
集合内部互相作为source.<br>

?: 缩小规模 <br>
合起来，沿某一维分两半，
计算 左、右、左右之间。
### Lec 0
Slides 01 

分治乘法 Karatsuba
$$(a10^{n/2} +b)(c10^{n/2}+d)= ac 10^n +bd + 
\left((a+b)(c+d)-ac-bd\right)10^{n/2}$$
$$T(n)=3T(n/2) +O(n)\Longrightarrow O(n^{\log_2 3})$$


### Lec 1
Slides 02

分治排序：
合并利用有序性  $O(n)$，共 $\quad O(n\log n)$  

逆序对：$(i,j)$ s.t. $x_i> x_j$ <br>
  分治 -(base on 排序)$\quad O(n\log n)$

#### Master's Theorem : 
$T(n)=aT(n/b)+n^d$
$$
T(n)=\left\{
   \begin{array}{lc}
       O(n^d) & a<b^d\\
       O(n^{\log_b a}) & a>b^d\\
       O(n^d \log n) & a=b^d
   \end{array}
\right.
$$

Slides 03

k-th 小：类似快排，选 pivot(sentinel)，分<,=,>；
* 随机选pivot;
* $T(n) = O(n)+ \max\{T(|L|),T(|R|)\}$ -> $O(n^2)$
* Median of Medians $T(n/5)+O(n)$
* 分 $n/C$ 组，每组$C$个数。求各组中位数，再求中位数的中位数。
* $C=5$时，至少比3/10 大, 3/10小。
* $T(n)= T(0.2n)+T(0.7n)+O(n)$ -> $O(n)$

Slides 04 

1st 近邻：
* x_sort, Divide Conquer $\delta_l,\delta_r$
* $\delta = \min(\delta_l,\delta_r)$
* 不够： 在 $mid \pm \delta$ 可以纵向排布无数个。
* Imporve: Fix y_sort, $\delta$->**7**  
* $O(n\log^2 n)$ 

证明复杂度：

记得把 $O(n)$ 设成 $Cn$, 否则 $C$ 随n变化看不出来


#
## Episode 1
### Lec 2

Proof Comparison Sort - $O(n\log n)$
* Non-random algorithm. Possibility states $n!$, functions form a TREE -height $\log n!$
* Random algorithm. <=Certain algorithm+ random sequence

Slides 05

### FFT
$$
p(x)=\sum_{i=0}^{d-1} a_ix^i\qquad q(x)=\sum_{i=0}^{d-1} b_ix_i
$$
求
$$
r(x)=\sum_{i=0}^{2d-2} c_i x_i
$$

### 一. Interpolation 插值
$$ r(x)=p(x)q(x)$$
给定 0 到 2d-2 共 2d-1 个点 $x_0,\ldots,x_{2d-2}$，

$$ p(x_k)=\sum_i a_i x^i=
\begin{array}{c}
a_0 +a_2 x^2+\ldots+a_{2d-2} x^{2d-2}\\
x(a_1+ a_3 x^2+\ldots+ a_{2d-1} x^{2d-2})
\end{array}
$$

$$
= p_e(x_k^2)+xp_o(x_k^2)
$$ 

故若 
$x_i^2$=$x_j^2=\chi^2$, 
$\begin{array}{l} p(x_i)=p_e(\chi^2)+x_i p_o(\chi^2)\\ 
p(x_j)=p_e(\chi^2)+x_jp_o(\chi^2)\end{array}$.

只需计算$p_e,p_o(\chi^2).$  成功将 $T(N)$拆成两个$T(N/2)$+ $O(N)$

为保证$\chi^2,\chi^4$能一直做下去，引入复数
$e^{j\theta}=-e^{j(\theta+\pi)}$ .
此时，需注意平方后的两个复数仍然需要 $\pi$ 的相位差，不然还是做不下去。

是故，取 $D=2^\eta\geq 2d-1\geq 2d$ ；取 $\omega=\frac{2\pi}{D}$, 令
$x_0=e^{j\ 0\omega }, x_1=e^{j\ D/2 \omega}=-e^{j\omega}$, 
$x_n=e^{j\ n\omega},x_{n+1}= e^{j (n+1)\omega}$. 且递归后的行为
$\chi_0=x_0^2=x_1^2=e^{j\ 0\omega},\\$
$\chi_1=x_{0+D/4}^2=x_{1+D/4}^2=e^{j\ D/2\omega}=-e^{j 0\omega}$.

可以先递归把 $p_e(\omega^2),p_o(\omega^2)$ 的插值结果按从小到大的顺序
求出来，注意到 $p_e(0\omega^2)$ 对应 $0\omega,(0+D/2)\omega$。则可以通过

      p[i]= pe[i] + w * po[i];
      p[i+D/2]=pe[i] - w* po[i];
也许也能用 i/2 搞定。
$$ T(n)= 2T(n/2)+O(n)$$
$$ O(n\log n) $$

### 二、计算乘积
$$
r(x_i)=p(x_i)q(x_i) 
$$
$$ O(n)$$

### 三、反求 $r(x)$
目前已知 $D$ 个插值的函数值 $r_0,\ldots,r_{D-1}$。欲反求 $r(x)$ 各项系数。
$$ \begin{bmatrix}
r_1\\
\vdots\\
r_n
\end{bmatrix} =
\begin{bmatrix}
   1 & 1 &  & 1 & 1\\
   1 & \omega & \ldots & \omega^{n-1} & \omega^n\\
   & \vdots & &\vdots \\[6pt]
   1 & \omega^{n-1} & \ldots & \omega^{(n-1)(n-1)} & {\omega^{(n-1)n}}\\
   1 & \omega^n & & \omega^{n(n-1)} & \omega^{nn} 
\end{bmatrix}
\begin{bmatrix}
c_0\\
\vdots\\
c_n
\end{bmatrix}
$$
巧的是这是正交阵。对复矩阵求逆，
$A^{-1}=\bar A\frac{1}{|A|}=\bar A\frac{1}{D}$


#
## Graph
### Lec 3

Sildes 06

DFS：DFS Tree,  $O(|V|+|E|)$ 
* start time / finish time:
  finish time: 退出调用$\neq$ 最后遇到
* 无向图
  * tree edge
  * back edge
* 有向图
  * tree edge
  * forward edge
  * back edge
  * cross edge

拓扑排序：A -> B A是B的先修课
* iff DAG $\leftrightarrow$ iff $\exists$ tail
* 找tail，作为排序后的末项 $O(|V|^2)$  *what missed?*
* dfs tree: no back edge
* DFS，finish time 最早的是tail
*  $O(|V|+|E|)$

强连通分量 Strong Connected Component

* 最晚结束的节点一定在 Head SCC中
* 构造 $G$ 的反图 $G^R$
* If $v$ can reach $u$ in $G^R$, and they are from different SCCs, 
then we have $finish[v]>finish[u]$ .
* Algorithm
  * DFS, 按 finish time $sort$ $G^R$
  * 从大到小 DFS
  * 每次 expore 得到 SCC
  $$O(|V|+|E|)$$

### Lec 4

Slides 07

#### Shortest path
无权图：
* For dist =1 , dist = 2, dist = 3,.....
* BFS $O(|V|+|E|)$

Dijkstra : 从最优性出发，每次确定一个无法被更新的点
$$O(|V|^2+|E|) \rightarrow O((|V|+|E|)\log |V|) \rightarrow O(|V|\log |V|+|E|) $$
* Shortest Path Tree $T$
* 初始 $T=\{s\}$，并更新所有联通点的最短值
* 更新：|V| turns
  * 从$T^C$下一步能到达的点里，挑**求和后**
    $dist(s,v_T)+dist(v_T,v)$ 最小的加入 $T$.  $O(|V|)$ -$O(\log |V|)$
  * 每次向 $T$ 加入点时更新所有联通点的最短距离 <BR>
    总共 $O(|E|)$ -$O(|E|\log |V|)$-$O(|E|)$
  * 正确性：假设绕一下的才是最短，那么其一部分一定更短。

Fabnocci 堆<br>
* 藤上挂子树, MIN: pointer 
* Property:
  * 初始，每个根(父节点)有不同度数 Degree (worst 0,1,2 ... )
  * 非根节点最多 lose 一个child.
* Operation: POP,UPDATE
  * Cut when UPDATE, merge when POP
  * Cut: （往小更新时）把更新的挂到藤上，标记父节点
  * Cascading Cut: 若父节点已标记，把父节点挂到藤上，自己单独挂藤上
  * Merge: 合并根节点有相同度数的树<br>
  &emsp;&emsp; why Merge ：不 Merge 时，pop 也要$O(n_{root}+d_{max})$. 
  <br>&emsp;&emsp;即遍历过程中merge
  * Pop: $O(D)+O(t^-+D-t^+)+O(t^+)=O(2D+t^-)$
  * Amortized: $\Phi=t+2m$, Update $O(1)$, POP $O(\log |V|)$


### Lec 5
Slides 08

Bellman-Ford: 朴素的遍历，认为所有点都有机会被更新
* $dist$ 初始化所有点$\infty$，起点 0
* 记录到每个点（目前）的最短路
* 每次遍历所有的边，看能不能更好
* kth iteration returns k-step-shortest path
* |V|-edge-path is a circle. 

负环：Bellman-Ford 跑 $|V|$ 轮没结束


#
## Greedy

通过某些性质始终走在OPT上。

Slides 09

More or less, algorithm that uses some property (not brute force)
have the idea of greedy.

生成树: Minimum Spanning Tree SPT
* 连通图
* **Prim**: 找SPT相邻各点最小的 
  * Time Complexity: |V| PopMin, |E| Update -- Fab Heap $O(|E|+|V|\log |V|)$
* **Cruskal**:直接找全局最小的边，as long as不成环 
  * Time Complexity: sort + Unoin Find Set check group (|E| round find) $O(|E|\log |V)$

Unoin Find Set
* Unoin $O(1)$ ： 把矮的合到高的, rank
* Find $O(\log n)$。路径压缩： 顺便把 parent 挂上去 
  * $m\hat C=m\log^* n$
  * $k_n=2^{k_{n-1}}$


### Lec 6
Slides 10

Induction: **always within** Optimal Tunnel
* Assume not in OPT, we find nothing stop it to be within OPT.

Greedy vs. Divide & Conquer
* Both reduces problem size
* D & C: Tree-like 
* Greedy: Local, Immediate dicision, size--

Huffman: <br>
Intuitively, each time after a merge, some points' h ++.<br>
The parents' weight indicates how much weight is added by 1<br>
Cost: 
* 对每个子树，其内部配过一次 $\sum_{subtree} w_i l_i$
* 子树根被merge时，按根的weight, $cost+=lev(subroot)*w_{subroot}$
* proof：某一步已有sub OPT，是一个forest。挑两个组成同一level。设最小的两个不在，交换更小
$$sort \ O(n\log n),\ O(1)$$

Makespan: m Machines, n assignment
* Greedy: Assign remaining longest to the finish earliest
* Not correct. Local modification affects future
* $NP$ problem
* Approx algorithm:
  *  1.3 OPT
  *  Consider the last finish job (WLOG, it's the last scheduled one)
  *  pn is the shortest
  *  $RES= t+ p_n.\qquad t\leq OPT, 3p_n\leq OPT$
  *  t满负荷
  *  反证 Suppose OPT< 3pn, swap still OPT



# 
## 动态规划 Dynamic Programing
树状结构，把大问题拆成小问题，但大问题要（递归地）通过所有他的（所有）子问题解决。DAG

### Lec 7

Slides 11

记忆化搜索：自上到下<br>
DP: 自下到上，沿着 Topological Order 解 （大多数问题顺序可以一眼看出来）<br>

先想递归，再开有没有可以记忆的，研究依赖关系看小问题是否组成DAG

最短路 Remake:<br>
$dist[t]=dist[v]+w(v,t)$<br>

1. 找 Topological Order. 
2. dist[s]=0. 
3. **只能DAG**


最长上升子序列 Longest Increasing Sequence<br>
* LIS[k]: End at k
* $O(n^2)$

Edit Distance
* Insert, delete, replace
* 插空格，对齐，rewrite
* 先对齐最后一位，有3种情况，x-y, x- , -y
* ED[i,j]=min{ED[i,j]+ x!=y,ED[i,j-1],ED[i-1,j]}

背包问题 backpack problem
> n objects, each value $v_i$, weight $w_i$. Total capacity $w$.

$dp[i,w]=\max\{dp[i-1,w-w_i]+ v_i,dp[i-1,w]\}$

$O(nW)$ NP: N=# bits input. 

`小问题解不出来 -> 小问题分的不够细！`

Slides 12

Largest Number in $k$ Consecutive Numbers
* Potential Largest `决策单调性`
* Heap: Too powerful. Treat each as potential largest.
  * eg: k=3, [5, 5, 13, 9]
* **Potential Largest List** ( PLL )
  * PLL[i]: from $i$ go back $k$, all potential
  * len(PLL) < k, roughly decreasing
  * Update: kick if $l<i-k+1$ && cascading comp with $a_{i+1}$
$$charging, \qquad O(n) $$
  
Potential Prefix in `LIS`<br>
* 分出子问题时，每个长度只记录最考前的结尾
* 定义数组 min_require[length]
* 二分
$$O(n\log n)$$

Minimize Square Sum 

    In: A sequence a1,a2,...,an
    cost(l,r) = C + sum(a,l,r)^2
    Out: min cost
* 状态：~~dp[l][r]~~ -> dp[i]: cost[ 0:i ]
* dp[i]=min(for j: dp[j]+ cost(j:i))
* 优化：考察 dp[x]+C+cost(x:i)>dp[x]+C+cost(y:i), 用前缀和表示
* $$g(x,y)=\frac{(dp[y]+s[y]^2)-(dp[x]+s[x]^2)}{2s[y]-2s[x]}<s[i]$$
* Kick out "山峰"
* Charging, $O(n)$

Descartes Product
* $cost(i,j)=m_im_{i+1}\cdots m_j +\min_{i\leq k\leq j}\{c(i,k)+c(k+1,j)\}$
* $O(n^3)$ -> $O(n^2)$    
> DP Formula <br>
> $c(i,j)=w(i,j)+\min\{c(i,k)+c(k+1,j)\}$<br>
> i.e. $w(i,j)=m_1 m_2...m_j$
> <br>
>  Quadrangle Inequality (QI)<br>
>  $i\leq i'\leq j \leq j',\quad w(i,j) + w(i',j') \leq w(i',j) +w(i,j')$
> <br>
>  Monotonicity<br>
>  $i\leq i'\leq j\leq j',\quad w(i',j) <= w(i,j')$
>  <br><br>
>  $w$ satisfy QI -> $c(i,j)$ satisfy QI

* $c(i,k_1)+c(k_1+1,j) < c(i,k_2)+c(k_2+1,j)$
* Fix i,j respectly. $c(k_1+1,j)-c(k_2+1,j)$ `non-decreasing` on $j$ 
* 对角线：内循环时间加起来可控

### Lec 8 
Slides 13
#### DP on Graph 

BellmanFord as DP<br>
i.e. dp[v][k]=min {dp[v][k-1], dp[u][k-1]+d(u,v)}

多元最短路 All Pair Shortest Path
* Naive Idea: |V| round Bellman.
* Naive DP: dp[k][u][v] 
    
      uv没有用到别的起点
      所有起点基本是独立的，只换了暴力的顺序

Floyd Warshall
* dist[k,u,v]: shortest dist from u to v only acrossing inter-vertices in {v1,...,vk}
* dist[`|V|`,u,v] & implicitly numbered each elem
* No negcircle -> No vertice appear twice!
* For dist[k,u,v] either $v_k\in path$ : (dist[k-1,u,k]+dist[k-1,k,v]) or not (dist[k-1,u,v]).


Traveling Salesman Problem (TSP)
* Complete Graph == Undirected 
* Brute Force $O(n!)$
* Difference between SP : Require traversing all vertices.
* dist[S,u,v]: Exactly $S\subset path$, from u to v.
$$O(n^2 2^n)$$

Maximum Independent Set
> Independent Set: $S$<br>
> $\forall u,v\in S, (u,v)\notin E$
  
    In: G(V,E): undirected Tree
    Out: Max size independent Set
DP: begin with some recursion<br>
Tree: try the root!

Root: In or Notin<br>
$f[v]=max\{\sum_{children} f[u], \sum_{grandchildren} f[u]+1\}$

Complexity: $O(n)$


DP: '暴力'枚举所有子问题<br>
Greedy：研究出来怎么转移，直接过去


Tree Decomposition
<br>
Treewidth : How a graph looks like a tree<br>
Idea: Combine connected nodes, the minimum number in a Super Node is $k$
Treewidth= k-1


# 
## Network Flow
> Directed Graph, $\forall e\in E, c(e)$
> <br> Output: Max flow from $s$ to $t$

DP? Not DAG<br>
Greedy: Allow Refund

### Algorithm
Ford-Fulkerson: Do not limit how to choose edge

*Residual Network*<br>
Given $G=(V,E)$, $c$, and a flow $f$.
<br>
$G^f=(V,E^f)$ and $(u,v)\in E^f$ if
1. $(u,v)\in E$ and $f(u,v)<c(u,v)$. $c^f(u,v)=c(u,v)-f(u,v)$
2. $(v,u)\in E$ and $f(u,v)>0$. $c^f(u,v)=f(v,u)$

A small bug: circle in original graph: add a vertice

Algorithm: Construct $G^f$, find a path. Update $flow$. Repeat. $O(|E|*f)$

### Correctness
Correctness: 
Max-FLow Min-Cut Theorem  
1. Cut is bigger than any flow
2. FF stop -> no path -> cut -> reach equality
   
### Application
* Matching
* Prove via MFMC: find a max flow
* Champion problem: matches, teams. Arrange each team not to overwhelm
  target: max flow < remaining matches
* LIS: how many simultaneously
* Hall's Merriage Theorem:<br>
二分图$A,B$. Matching exists iff $|S|\leq N(S)$ for all $S \subset A$

### Implement
`Edmonds Karp`: BFS to find path
* The path is minimum path for $|E|$
* Weak monotonicity: $G\rightarrow G^f$, dist(u) is non-decreasing. 
* Each time at least one edge delete. Stop when path break
* Key: when capacity is full, each loop $(u,v)$ -> $(v,u)$ -> $(u,v)$ will 
  cause $dist(u)+2$. <br>
  Reason: $dist(v)=dist(u)+1, dist(u)= dist(v)+1$
* Each vertice can only critical $O(|V|)$
* $O(|V||E|)_{turns\ per\ edge} * O(|E|)_{bfs} =  O(|V|*|E|^2)$ 
* Do NOT rely on rational

  
`Dinic`: To make $dist(t)$ strictly increase
* Cut off *all* shortest path in each turn
* Level graph: level by distance, preserve ascending edges.
* Construct *blocking flow*: elimate all shortest path
* Frontward dfs: (Do not trace back)
    1. Reach $t$. 
    2. End up a dead-end $e$. $e$ never reach $t$
* $O(|V|)_{turn} * O(|E|_{del\ 1\ edge}*|V|_{dfs})$


#
## Linear Program
$$
  \max \bm c^T x\\
  s.t.\quad \begin{array}{l}
    \bm A x\leq \bm b\\
    x\geq 0
  \end{array}
$$
### Algorithm:
Simplex(axis rotation), Gradient

### Application:
max flow:
$$
\begin{gather*}
  \max \sum_{e\in E} f_{e}\\
  s.t. \begin{array}{l}
    0\leq f_e\leq c_e\\
  \sum_{in} f = \sum_{out} f
  \end{array}
\end{gather*}
$$
* FF is similar to simplex
* Max flow - min cut : dual problem's polyhedron's vertice is $0,1$

Integer Program (IP): additional require $\bm x\in \mathbf{Z}^+$

LP relaxation: `孤立点不能放松条件`Vertex Cover ALG <= 2OPT

Recover: 1/2

### Dual: Upper Bound
选取系数， 使 $y^T AX$系数与$\max c^Tx$相同，再尽可能最小另一侧
* Weak Duality Theorem: Leq
* Strong Duality Theorem: Eq at convex

### Totally Unimodular:
Def: every square submatrix has determinant 0, 1or −1 
<br>
Property: $A$ is unimodular, $b\in \mathbf{Z}^n$. Solution $x$ for $Ax\leq b$ has $x\in Z^n$ (Crammer)

To prove $A$ is unimodular:<br>
Induction on $k\times k$ matrix. Try to find a all $0$ or only one $\pm 1$ case. This cases are trivial. For two $\pm 1$ case, all row must be 1,-1. Add to 0th coloum, done.

$(A,I)$


# 
## 均摊分析 Amortized Analysis
### **Def**: Consider the total cost of $k$ arbitrary operations. 
(Do not mean random)

Total cost 
$$ C(P)=C(p_1)+C(p_2)+\cdots+C(p_k) $$
Assume two type $P_1,P_2$. $\hat C(P)$ is the amortized cost if 
$$C(p_1)+C(p_2)+\cdot+C(p_k)\leq k_1 \hat C(P_1)+k_2 \hat C(P_2)$$
Note: $\hat C(P)$ does **not** mean a real $P$ operation costs $\hat C$.


### **potential function** $\phi$
$\phi$ : A function evaluate the current state. 
$$\hat C=C+A\Delta\phi$$
example: Stack -- $\phi$ = # stack content



#
## NP Hardness
* SAT(3SAT): Boolean Satisfiability (-> true)
* k-Vertex Cover: Use k nodes to cover edges
* k-Independent Set: k vertex with no common edge
* Subset Sum: Is the sum any subset of the input arr equal to $s$
* Dominating Set: All vertex are the neighbor of dominating set
* k-Clique: k-complete subgraph.

### NP: polynomial verifier (searching)
$P\subset NP$

### Karp Reduction: Convert Input
$f$ Karp reduce to $g$ if exists a polynomial convertor
$A$ such that
* convert $f$'s input to $g$'s input
* Two problem output the same
* Using $g$ solve $f$
* Denote as $f\leq_k  g$

### NP-hard: Harder than any NP problem (But **may not** NP)
### NP-complete: A `NP` that is NP-hard

### SAT is NP-complete
The verifier is polynomial, convert to a truth table.<br>
Note both $x,y$ are given, its poly.

### Reduction: Examples
* SAT $\leq$ 3SAT<br>
  add $y,\neg y$.
* 3SAT $\leq$ Independent Set<br>
  * Global Construction Method:Triangle. Each variable derive 2 vertices: $x, \neg x$.
  * Proof: SAT true -> independent #()
  * Independent true -> SAT true
  * NOT 一一对应

___
* Independent Set $\leq$ Vertex Cover
  * Observation: If $S$ is an independent set,<br> 
  $S^c_V$ of independent set is a cover
  * Reduction: $G,k$ -> $G,n-k$
  * 一一对应
* Independent Set $\leq$ Clique:
  * *Clique*: Every two nodes have an edge
  * Reduction: 都有边 vs 都没边
  * $\bar G =E^C$
  * 一一对应
* Vertex Cover $\leq$ Donimating Set
  * Cover 边 vs Cover 点
  * 把边换成点 
  * 问题：用dominating set解vertex cover只需要cover新点，不需要cover旧点，但dominating set 本身需要 cover 旧点
  * 解：把原来的点接成clique
  
  
___
* Vertex Cover $\leq$ Subset Sum 
  * 2 step: Use a median as bridge
  * Bridge: Vector Subset Sum
  1. Vertex Cover $\leq$ Vector Subset Sum<br>
    $$\begin{gather*} 
    a_1 = (v,e,e,e,e)\\

    k = (k,1,1,1,1)
    \end{gather*}$$
  2. Vector Subset Sum $\leq$ Subset Sum: Big INT
  * Subset Sum+: 限制输入是正的
* SubsetSum $\leq$ Subset Sum
  * Partition: 把$S$分成 $A,B$，使 sum(A)=sum(B)
  * Partition+
* 3SAT $\leq$ Hamiltonian Path
  * Bridge: Directed .ver Hamiltonian Path (Fix s,t)
  1. 3 SAT $\leq$ D .ver
    <br>
    `Gadget`
  2. Directed .ver $\leq$ Undirected .ver <br>
    把 f 的一个点拆成三个点。入边-v-v-v-出边
* Subgraph: Clique

### How to Reduction
* Boolean -> SAT
* Graph(Independent): Independent Set; Vertex Cover; Dominating Set
* Number: Subset Sum; Partition
* Traversing graph: Hamiltonian Path
    
### NP Intermediate
Proved not in $P$, but haven't prove complete
* Graph isomorphism 图同构
* Factoring  
