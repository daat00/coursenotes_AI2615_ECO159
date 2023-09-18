# 多项式环
设 $R$ 是一个含幺环, $\bar R \supset R$ 是 $R$ 的扩环, $x \in \bar R \backslash R$ 是 $\bar R$ 中的一个元素. 如果 $x$ 满足
1. $\forall r \in R, xr = rx$
2. $1x = x$
   - $1$ 是 $R$ 的单位元，$\bar R$ 不一定有单位元
3. 对 $R$ 的任意一组不全为零的元素 $a_0, a_1, \ldots, a_n$, 
   $$f(x) = a_0 + a_1x + \cdots + a_nx^n \neq 0$$
   - 没有解

则称 $x$ 为 $R$ 上的一个未定元

### 未定元的存在性
设 $R$ 是一个含幺环, 则一定存在环 $R$ 上的一个未定元 $\bar x$
1. 做集合 $\bar S = \{(a_0, a_1, \ldots, a_n, \ldots) \mid a_0, a_1, \ldots, a_n,\ldots \in R\}$
2. 定义运算 $\forall \alpha = (a_0, a_1, \ldots, a_n, \ldots), \beta = (b_0, b_1, \ldots, b_n, \ldots) \in \bar S$
   $$\alpha + \beta = (a_0 + b_0, a_1 + b_1, \ldots, a_n + b_n, \ldots)$$
   $$\alpha \cdot \beta = (c_0, c_1, \ldots, c_n, \ldots)$$
   其中 $c_k = \sum_{i+j = k} a_ib_j$ (多项式乘法)
3. $+, \cdot$ 是 $\bar S$ 的代数运算，且 $(\bar S, +, \cdot)$ 构成环。
   - $\bar 0 = (0,0,\ldots,0,\ldots)$
   - $\bar 1 = (1,0, \ldots, 0, \ldots)$
4. 做 $\bar S$ 的子环 $S = \{\bar r = (r, 0,\ldots, 0, \ldots)\mid r\in R\}$
5. 令 $\bar x  = (0,1,0,0\ldots) \in \bar S$
   - $\bar r\bar x = \bar x \bar r = (0,r,0,\ldots)$
   - $\bar 1 \bar x = \bar x \bar 1$
   - $\bar x ^n = (0,0,\cdots,0,1,0,\cdots)$
6. $\phi: R\rightarrow \bar S, r \mapsto \bar r$

## 一元多项式
设 $R$ 是一个含幺环, $x$ 是 $R$ 上的一个未定元, 称形如 $f(x)=a_0+a_1x+a_2x_2+\cdots+a_nx_n$ 的表达式为 $R$ 上 (关于 x 的) 一元多项式.

其中 
- $a_ix_i$ 称为 $f(x)$ 的 $i$ 次项, $a_i\in R$ 称为 $i$ 次项的系数, $a_0$ 也称为常数项. 
- 如果 $a_n \neq 0 \in R$, 则称 $a_n$ 为*首项系数*, 并称 $f(x)$ 的次数为 $n$, 记作 $\deg f(x) = n$. 
- 系数全为零的多项式称为零多项式, 零多项式的次数规定为 $-\infty$

推论：

多项式 $f(x)$ 是它的扩环 $\bar R$ 中的一个元素. 且 $R$ 上的多项式全体 
$$R[x] = \{a_0 + a_1x + a_2x^2 + \ldots + a_nx_n \mid n \geq 0, a_i \in R\}$$
关于 $\bar R$ 的运算 $\bar \phi$ 构成 $R$ 的一个扩环. 
- 特别地, $R$ 上的一元多项式环 $R[x]$ 同构于以下 $\bar S$ 的子环
  $$\{(a_0, a_1, \cdots , a_n, \cdots) \mid a_i\in R\}$$
  且仅有有限多个 $a_i \neq 0$ 

### 一元多项式环
设 $R$ 是一个含幺环, $x$ 是 $R$ 上的一个未定元. 称环 $R[x]\supset R$ 为 $R$ 上的以 $x$ 为未定元的*一元多项式环*
- $f(x) + g(x) = \sum_{i=0}^n(a_i+b_i)x^i$
- $f(x)g(x) = \sum_{i=0}^{m+n} c_ix^i$, $c_k = \sum_{i+j = k} a_ib_j$

### 多项式环的性质
设 $R$ 是一个含幺环, $x$ 是 $R$ 上的一个未定元.
1. $\bm 0_{R[x]} = 0_R$
2. $\bm 1_{R[x]} = 1_R$
3. $R$ 是无零因子环 $\Rightarrow$ $R[x]$ 也是无零因子环
4. $R[x]$ 的单位与 $R$ 的单位相同
   - $\sum a_ix_i \sum b_jx_j = 1 \Rightarrow a_i = b_j = 0 \mid i,j \neq 0$
5. $R$ 是交换环 $\Rightarrow$ $R[x]$ 也是交换环
6. $R$ 是整环 $\Rightarrow$ $R[x]$ 也是整环

设 $R$ 是一个含幺环, 则 $\forall f(x), g(x) \in R[x]$ 有
1. $\deg(f + g) \leq \max(\deg f, \deg g)$
2. $\deg(fg) \leq \deg f + \deg g$
3. 如果 $f$ 或 $g$ 的首项系数不是 $R$ 中零因子, 则 $\deg fg = \deg f + \deg g$

## 多项式的带余除法
### 整除
设 $R$ 是一个含幺环, $x$ 是 $R$ 上的一个未定元. $\forall f(x), g(x) \in R[x]$, $g(x) \neq 0$. 如果 $\exists q(x) \in R[x]$, 使得
$$f(x) = q(x)g(x)$$
那么就称 $g(x)$ 整除 $f(x)$, 记做 $g(x)\mid f(x)$. 
- $g(x)$ 是 $f(x)$ 的因式, $f(x)$ 是 $g(x)$ 的倍式. 
- $\forall a \in U(R)$, $a$ 和 $af(x)$ 都是 $f(x)$ 的因式, 他们是平凡因式
- 如果一个次数大于零的多项式不能写成两个非平凡因式的乘积, 则称它为 *不可约多项式* 或 *既约多项式*, 否则称为可约多项式

### 最大公因式
设 $R$ 是一个含幺环, $x$ 是 $R$ 上的一个未定元. 对任意 $f(x), g(x), h(x) \in R[x]$, 其中 $h(x)$ 是非零的*首一*多项式, 如果
1. $h(x) \mid f(x)$, $h(x) \mid g(x)$
2. $\forall d(x) \neq 0, d\in R[x]$, 若 $d(x) \mid f(x), d(x) \mid g(x)$, 则 $d(x) \mid h(x)$

则称 $h(x)$ 为 $f(x)$ 与 $g(x)$ 的最大公因式, 记为 $h(x) = \gcd(f(x), g(x))$.
- 特别地, 若 $\gcd(f(x), g(x)) = 1$, 则称 $f(x)$ 与 $g(x)$ 是互素的

### 带余除法
设 $R$ 是一个含幺交换环, $R[x]$ 是 $R$ 上的一元多项式环. 

$\forall f(x), g(x) \in R[x]$, 其中 $g(x)$ 的`首项系数是 R 中单位`, 则存在唯一一对多项式 $q(x), r(x) \in R[x]$, 使得 
$$f(x) = q(x)g(x) + r(x)$$
并且 $\deg r(x) < \deg g(x)$

证明
- 存在性： 做 $\Sigma = \{f(x) - g(x)h(x)\mid h(x) \in R[x]\}$
  
  若 $0\in \Sigma$, 显然
  
  $0\notin \Sigma$, 则 $\deg g \geq 1$. 取 $r(x)\in \Sigma$ 为 $\Sigma$ 中次数最小的多项式 $r^*(x) = f(x) - g(x)h^*(x)$。只需证 $\deg r^* < \deg g$
  
  假设 $\deg r \geq \deg g$
  - 设 $\deg g = n, \deg r = m$, 暨 
  $$g(x) = \sum_{i=0}^n a_ix^i, r(x) = \sum_{i=0}^m b_ix^i, m \geq n$$
  - 令 $h'(x) = h(x) + b_ma_n^{-1}x^{m-n}, r'(x) = b_ma_n^{-1}g(x)x^{m-n}$, 则 
  $$f(x) = g(x)h'(x) + r'(x)$$
  - $r'\in \Sigma$, $\deg r' < \deg r$，矛盾
- 唯一性：设 $q_1g + r_1 = q_2 g + r_2$
  - $(q_1-q_2)g = r_2-r_1$
  - $\deg[(q_1-q_2)g] = \deg(q_1-q_2)+ \deg g = \deg(r_2-r_1) < \max(\deg r_1, \deg r_2) < \deg g$
  - $\deg(q_1-q_2) = -\infty$

## 域上的多项式环
### 域的多项式环的理想
设 $F$ 是域, 则 $F[x]$ 中任一理想都是主理想, 即 $\forall I \triangleleft F [x], \exists g \in F[x]$, 使得 $I = \langle g \rangle$.

证明
- $I = \langle 0 \rangle$, 显然
- $I\neq \langle 0\rangle$, 取 $I$ 中次数最低的非零多项式 $g(x)$, 则 $\forall f(x)\in I$, 
  $$f = qg + r, \deg r < \deg g$$
  $$r = f - qg \in I \Rightarrow r = 0$$

### 最大公因式的基表示
设 $F$ 是域, $f(x)$ 与 $g(x)$ 是 $F$ 上不全为零的两个多项式, 则存在 $F$ 上的多项式 $s(x), t(x)$ 使得
$$s(x)f(x) + t(x)g(x) = \gcd(f(x), g(x))$$

### 多项式商域
设 $F$ 是域, $f(x) \in F[x]$ 是一个次数大于零的不可约多项式, 则 $\langle f(x)\rangle$ 是 $F[x]$ 的极大理想, 从而 $F[x]/\langle f(x)\rangle$ 是一个域.

证明
- $\forall I: I \supsetneq \langle f \rangle$, $\exists g(x) \in I, g(x) \notin \langle f \rangle$. 
- 由于 $f$ 不可约，$\gcd(f,g) = 1$. 由多项式 Bezout 定理, $\exist u,v\in F$, 使 $uf + vg = 1 \in I$, $I = F$. $\langle f \rangle$ 是 $F$ 内的极大理想
- 由极大理想的性质，$R/\langle f \rangle$ 为域

### 辗转相除法
设 $F$ 是域

$f(x), g(x), h(x)$ 是 $F$ 上任意三个不全为 $0$ 的多项式, 且 $f(x) = q(x)g(x) + h(x), q(x) \neq 0$. 则 
$$\gcd(f (x), g(x)) = gcd(g(x), h(x))$$

- $r_0 = q_1r_1 + r_2, 0\leq \deg r_2 < \deg r_1$
- $r_{i-1} = q_ir_i + r_{i+1}, 0\leq \deg r_{i+1} < \deg r_i$
- $r_{n-1} = q_nr_n + r_{n+1}, \deg r_{n+1} = -\infty$
- $\gcd (r_0, r_1) = r_n$


# 多项式的根
设 $E$ 是含幺交换环, $R < E$ 是 $E$ 是子环. 取 $R$ 上多项式 $f(x) = a_0 + a_1x + \cdots + a_nx^n \in R[x]$. 

对于每个 $\alpha \in E$, 定义 $E$ 中的运算 
$$ f (\alpha) = a_0 + a_1\alpha + \cdots + a_n\alpha^n \in E$$
称 $f(\alpha)$ 是 $f(x)$ 在 $\alpha$ 处的取值, 或是 $x = \alpha$ 代入 $f(x)$ 的值. 

如果 $f(\alpha) = 0$, 则称 $\alpha$ 是多项式 $f(x)$ 在环 $E$ 中的一个根或零点

- $R$ 可以是 $E$
- 若 $E$ 是含幺交换环, 当 $f(x) = g(x)h(x)$ 时, $f(\alpha) = g(\alpha)h(\alpha)$
  - $x$ 可交换，$\alpha$ 可能不可以交换
  - 不可交换时不一定相等

### 余数定理
设 $R$ 为含幺交换环, $f(x) \in R[x]$. 则 $\forall \alpha \in R$, 存在唯一 $q(x) \in R[x]$, 使
$$f(x) = q(x)(x − \alpha) + f (\alpha)$$
- $\alpha$ 是零点 iff $(x-\alpha) \mid f(x)$

证:
- 由带余除法， $f(x) = (x-\alpha)q(x) + r(x)$, $\deg r = 0$, $r(x) \in R$
- 由于是交换环, $f(\alpha) = r(x)$

### 根的数量
设 $D$ 和 $E$ 都是整环且 $D \subset E$, 则对于每个非零多项式 $f(x) \in D[x]$, 它在 $E$ 中至多有 $\deg(f)$ 个不同的根

证明
- $f(x) = (x-\alpha_1)f_1(x)$
  
  $f(\alpha_2)= (\alpha_2 -\alpha_1) f_1(x) = 0$. 
  
  由于 $\alpha_2 -\alpha_1 \in E$ 且 $E$ 无零因子，$f_1(\alpha_2) = 0$ 
- $f(x) = \prod_{i=1}^{N}(x-\alpha_i)f_{N}(x)$. 由于 $f\neq 0$, $f_N \neq 0$. $0\leq \deg f_N \leq \deg f - N$


### 单根和重根
设 $D$ 和 $E$ 都是整环且 $D\subset E$, $f(x) \in D[x]$. 

如果 $(x − \alpha)^m \mid f(x)$, 但是 $(x − \alpha)^{m+1} \nmid f (x)$, 则当 $m \geq 2$ 时, 称 $\alpha$ 是 $f(x)$ 的重根、重数为 $m$, 
- $m = 1$ 称为单根

### 重根的存在性
设 $D$ 和 $E$ 都是整环且 $D \subset E$, $f(x) \in D[x]$, $\forall \alpha \in E$,
1. $\alpha$ 为 $f(x)$ 的重根 iff $f(\alpha) = f'(\alpha) = 0$
2. 如果 $D$ 为域, 并且在 $D[x]$ 中 $(f, f') = 1$, 则 $f(x)$ 在 $E$ 中没有重根

证明：
- $\Leftarrow$: 假设是单根，$f = (x-a)p, f' = p + (x-a)p'\neq  0$
- $0 = s(x)f(x) + t(x)f'(x) = \gcd(f,f') = 1$


### 形式微商
$D$ 是整环，$f = \sum_i a_ix^n \in D[x]$, 形式微商定义为
$$f'(x) = \sum_i ia_ix_{i-1}$$


### 重数
设 $D$ 和 $E$ 都是整环且 $D \subset E, f(x) \in D[x]$.

$\alpha_1, \ldots, \alpha_m$ 是 $f(x)$ 在 $E$ 中 $m \geq 1$ 个两两不同的根, 并且根 $α_i$ 的重数为 $m_i$, 则 $m_1 + \ldots + m_m \leq \deg f$
- 证明类似单根情形，但不能直接套用，因为 $f_1 =0$.




# 域的扩张
## 域上的线性空间
$V$ 是一个有加法运算 $+$ 的非空集合, $F$ 是一个域. 

如果 $V$ 关于加法运算构成一个交换群, 并且对 $\forall k \in F, v \in V$ , 在 $V$ 中可唯一地确定一个元素 $kv$ (称为标量乘法). 进一步, 若 $\forall k, l \in F, u, v \in V$ 满足
- $V$是加法群
1. $(kl)v = k(lv)$
2. $(k + l)v = kv + lv$
3. $k(u + v) = ku + kv$
4. $1v = v$

称 $V$ 为域 $F$ 上的一个*向量空间*或*线性空间*

其中的元素称为向量, 域中的元素称为标量, 域 $F$ 称为向量空间的基域

eg.
$F$ 是 $E$ 的子域， $F \subset E$. 则 $E$ 是 $F$ 上的线性空间

### 子空间
设非空集合 $U \subset V$, 其中 $V$ 是 $F$ 上的线性空间。如果 $U$ 关于 $V$ 的运算也构成 $F$ 上的向量空间, 则称 $U$ 为 $V$ 的子空间

### 线性组合
设 $V$ 是域 $F$ 上的向量空间, $v_1, v_2, \ldots , v_n \in V$ (不必互不相同), 那么子集
$$\langle v_1, v_2, \ldots , v_n\rangle = \{a_1v_1 + a_2v_2 + \ldots + a_nv_n \mid \forall a_i\in F\}$$
称为 $V$ 的由 $v_1, \ldots , v_n$ 张成的子空间. 

元素 $\sum a_i v_i$ 称为 $v_1, \ldots , v_n$ 的线性组合
- 设 $B$ 是 $V$ 的任一非空子集, 如果 $V$ 中任一元素都是 $B$ 中有限多个元素的线性组合, 则称 $B$ 张成 $V$
- $B$ 张成 $V$ 且 $B$ 的任意有限子集都在 $F$ 上线性无关，则称 $B$ 是 $V$ 的基

### 线性相关
$\exist \{k_i\}$, $\sum k_iv_i = 0$, 则线性相关

### 基和维数
$B$ 张成 $V$ 且 $B$ 的任意有限子集都在 $F$ 上线性无关，则称 $B$ 是 $V$ 的基

如果 $\{u_1, u_2, \ldots , u_m\}$ 和 $\{w_1, w_2, \ldots , w_n\}$ 都是域 $F$ 上向是空间 $V$ 的基, 那么 $m = n$

如果一个向量空间 $V$ 具有一个含 $n$ 个元素的基, 则称 $V$ 的维数是 $n$, 记作 $\dim_F V = n$
- 零空间 $\{0\}$ 称为是由空集张成的, 并规定它的维数是 0 , 无限维向量空间的维数规定为无穷大 $+\infty$

## 扩域
设 $F$ 和 $E$ 是两个域. 如果 $F \subseteq E$, 并且 $F$ 中的运算就是 $E$ 的运算在 $F$ 上的限制, 则称 $E$ 为域 $F$ 的*扩域*, $F$ 为 $E$ 的*子域*

### 最小扩域
设 $E$ 是域, $F$ 是 $E$ 的子域, $S \subset E, |S| > 0$ 为 $E$ 的一个非空子集. 

定义 $F(S)$ 是 $E$ 的包含 $F$ 和 $S$ 的最小子域为：
$$F(S) = \bigcap_{L\in \Sigma} L$$
其中 $\Sigma = \{L \mid L 为 E 的子域且 L \supset F \cup S\}$, 称 $F(S)$ 为 $E$ 的添加集合 $S$ 于 $F$ 的子域
- $F(S_1)(S_2) = F(S_1 \cup S_2) = F(S_2)(S_1)$ 
  
  证明：
  - $F(S_1 \cup S_2) \subset F(S_1)(S_2)$:
  
    $F(S_1 \cup S_2)$ 最小 $\Rightarrow$ $F(S_1\cup S_2) \subset F(S_1)(S_2)$
  - $F(S_1)(S_2) \subset F(S_1 \cup S_2)$: 

    $F(S_1) \subset F(S_1 \cup S_2)$, $F(S_1)(S_2) \subset F(S_1 \cup S_2)(S_2) = F(S_1 \cup S_2)$

- 记 $F(\{a\}) = F(a)$, 称为 *单扩张* 或 *单扩域*
  $$F(a) =\{\frac{f(a)}{g(a)} \mid \forall f, g\in F[x], g(a) \neq 0\}$$
  - 封闭性 $f(a), \forall f \in F[x]$
  - 可逆 $\frac{1}{g(a)}$
- 记有限个元素 $S = \{a_1,\ldots, a_s\}$ 的最小扩域 $F(S) = F(a_1, \ldots, a_s)$

## 域的扩张
设 $E$ 是 $F$ 的扩域, $E \supset F$. 

如果 $E$ 作为 $F$ 上的向量空间是有限维的, 则称 $E$ 是 $F$ 的有限扩域或有限扩张. 

$E$ 在 $F$ 上的维数 $\dim_F E$ 称为 $E$ 关于 $F$ 的扩张次数, 记作 $[E : F]$, 即
$$[E : F] = \dim_F (E)$$

### 维数定理
设 $K$ 是域 $E$ 的有限扩域, $E$ 是域 $F$ 的有限扩域, $K \supset E \supset F$, 则 $K$ 是域 $F$ 的有限扩域, 且
$$[K : F] = [K : E]\cdot[E : F]$$

证明：
- 设 $X = \{x_1, \ldots, x_n\}\subset K$ 是 $K$ 在 $E$ 上的基； $Y= \{y_1,\ldots, y_n\}\subset E$ 是 $E$ 在 $F$ 上的基。则 $YX = \{y_ix_j\}$ 是 $K$ 在 $F$ 上的基。
- $k = \sum_i a_i x_i = \sum_i x_i  \sum_j b_{ij} y_j$
- 无关： 若 $k = 0$, 则 $\sum_j by = 0, b_{ij} = 0$


## 域的构造
### 引理
- 设 $F$ 是任一域, 则对任意正整数 $n$ 均存在 $F$ 上的 $n$ 次不可约多项式.

- 设 $F$ 是任一域, $f(x) \in F[x]$ 是一个首 $1$ 的 $n$ 次不可约多项式. 
  令 
  $$F[x]_{f(x)} = \{g(x) \in F[x] \mid \deg(g) < \deg(f) = n\}$$
  并在 $F[x]_{f (x)}$ 中定义加法运算 $g(x) \oplus h(x) = g(x) + h(x)$ 和乘法运算 $g(x) \odot h(x) = g(x)h(x) \mod f(x)$, 则 $F[x]_{f (x)}$ 是一个域.
  - $x \in F[x]$ 是 $g(x) =0$ 的根
  - 在模的意义下的运算无本质区别

### 域论基本定理
设 $F$ 是域, $p(x)$ 是 $F[x]$ 中次数大于零的不可约多项式, 那么存在 $F$ 的扩域使得 $p(x)$ 在此扩域中有根

证明
- 不可约多项式是极大理想
- 做域 $E = F[x]/\langle p(x)\rangle$
- $\phi: F \rightarrow E, a \mapsto a + \langle p(x)\rangle$
- 设 $\alpha \in \bar E$ 使得 $\bar \phi(\alpha) = x + \langle p(x)\rangle$. 由于 $\bar \phi(p(\alpha)) = p(x + \langle p(x)\rangle) = p(x) + \langle p(x)\rangle= 0 + \langle p(x)\rangle$

注：为便于表示，在语言上直接说 $\alpha + \langle p(x) \rangle$ 是根，但这是同构意义下的，实际上不是一个域

例
- $x^2+1\in Q[x]$, $E = Q[x]/\langle x^2 +1\rangle$
  - $1 + \langle x^2 + 1 \rangle$, $0 + \langle x^2 +1\rangle$

### 代数扩张
设 $E$ 为域 $F$ 的扩域, $\alpha \in E$, 如果存在 $F$ 上的非零多项式 $f(x)$, 使得 $f(\alpha) = 0$, 则称 $\alpha$ 为 $F$ 上的一个代数元. 否则，称为超越元 

如果 $E$ 中的每个元素都是 $F$ 上的代数元, 则称 $E$ 是 $F$ 的代数扩张, 否则称为超越扩张

# 分裂域
设 $E/F$, $f(x) \in F[x]$ 是非常数多项式。若 $f(x)$ 能分解成 $E[x]$ 中一次因式的乘积，则称 $f(x)$ 在 $E$ 上是分裂的。
- $E = F(\alpha_1)(\alpha_2)\ldots$
- $E[x] \neq E$

若 $f$ 在 $E$ 上分裂，但在 $E$ 的任意包含 $F$ 的真子域 $\forall S \subsetneq E$ 上都不分裂，则称 $E$ 为多项式 $f$ 在 $F$ 上的分裂域

- $x^2 +1 = (x+i)(x-i) \in \mathbb{Q}[x]$, 分裂域 $\mathbb{Q}(i)$
- 设在 $E$ 上 $f(x) = b\prod (x-a_i)$, 则 $E$ 是 $F$ 的分裂域 iff $E = F(a_1, a_2,\ldots, a_n)$

### 分裂域的存在性
一定存在 $E/F$

### 分裂域的唯一性


# 有限域
设 $E/F$，$|F| = q, |E| = q^n$, 则 $E^* = E \backslash \{0\}$ 中每个元素 $\alpha$ 都有 $\alpha^{q-1} = 1$, 并且 $$