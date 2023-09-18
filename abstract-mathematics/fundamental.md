# 数论
## 整除
设 $a,b\in \Z$, $b\neq 0$。若 $\exist q \in \Z$ 使 $a = bq$, 则 $b$ 整除 $a$. 记作 $b | a$.否则 $b \nmid a$

* $a|b \Rightarrow a| tb$
* $a|b$, $b|a \Rightarrow a = \pm b$
* $a,b$ 都是 $m$ 的倍数，则 $sa\pm tb$ 也是 $m$ 的倍数
* $a|b$, $b | c \Rightarrow a|c$

## 素数
如果整数 $p>1$, 并且 $p$ 只能被 $\pm 1$ 和 $\pm p$ 整除，则 $p$ 是素数

## 带余除法
若 $a,b\in \Z$, $b>0$, 则存在 $q,r\in \Z$，使 $a = bq+r, 0\leq r< b$ 成立，且 $q,r$ 唯一。
其中 $q$ 叫不完全商，$r$ 是余数

## (最大) 公因子
设 $a_1,\ldots,a_n$ 是 $n (n \geq 2)$ 个整数. 若整数 $d$ 是它们之中每一个的因子, 那么 $d$ 就叫作 $a_1,\ldots, a_n$ 的一个公因子。公因子中最大的一个叫作最大公因子 (greatest common divisor), 记作 $\gcd(a_1,\ldots,a_n)$. 

特别地, 若 $\gcd(a_1,\ldots ,a_n) = 1$, 我们称 $a_1,\ldots ,a_n$ 互素（*不等于两两互素*)

若 $a_1, \ldots,a_n$ 中任意两个不同的整数都互素, 则它们两两互素。

* 对于 $d \neq 0$, $\pm d$ 都是因子
* 对任意 $a,b \in\Z$, $\gcd(a,b) = \gcd(|a|,|b|)> 0$
* $0$ 可被任意非零整数整除，故 $\forall b\neq 0, \gcd(0,b) = |b|$

## 辗转相除法
设 $a,b,c$ 是三个不全为 $0$ 的整数，且 $a = qb+c$ 其中 $q\neq 0$.
则 $a,b$ 与 $b,c$ 有相同的公因子，因而 $\gcd(a,b) = \gcd(b,c)$
* 一般讨论的时候把 $0$ 单拎出来
* $\gcd(a,m) = \gcd(a+tm,m)$
* 证:<br>
$\forall d \ s.t.\  d |a, d|b$, we have $d|(a-qb)$<br>
$\forall d \ s.t.\ d|b, d|c$, we have $d|(qb+c)$

## 最大公因子的性质
1. 最大公因子是两数的线性组合 $gcd(a,b) = \alpha a+ \beta b$. (不唯一，$\alpha a+ \beta b +ab-ab$)
   - 证：
   $$\begin{gather*}
    r_n = (0,r_n) = (r_{n-1}, r_n) = (r_{n-2}, r_{n-1}) =\ldots = (b,r_1)\\
    \gcd(a,b) = r_n = r_{n-2}-c_{n}r_{n-1} = (r_{n-4}- c_{n-2}r_{n-3}) - c_n(r_{n-3} - c_{n-1}r_{n-2}) = ma+nb
    \end{gather*}$$
   - Bezout Thereom：给定 $a,b$, 设 $\gcd(a,b) =d$. 则对于方程 $ax+by = d$, $\min\limits_{x,y}^+ d =g$.<br>
   Also $\gcd(a_1,\ldots,a_n) = \min^+(\sum a_i x_i)$<br>
   即最小的正线性组合整除各 $a_i$ 及其线性组合（**整数的性质**）<br>

2. 设 $a,b$ 都不为 $0$，则 $a,b$ 的公因子都是 $\gcd(a,b)$ 的因子
   - 证： $d|a, d|b \rightarrow d|(ma+nb)$

3. 设 $a,b \neq 0$, 则 $d = \gcd(a,b)$ iff
   * $d|a, d|b$
   * $\forall c | a, c|b$, we have $c|d$

4. 若 $a,b$ 不全为 $0$，则 $\gcd(a,b) =1$ iff $\exist m,n \ s.t.\ ma+nb =1$
   - If: 设 $d|a, d|b\rightarrow d| ma+nb \rightarrow d|1$.

5. Bezout Theorom: $a,b$ 不全为 $0$. 令 $S = \{xa + yb >0 |x,y\in \Z\}$. 则 $\gcd(a,b) = \min S$
    - 证：令 $m = \min^+(\sum a_i x_i)$, 再考察任意线性组合 $A = \sum a_i x_i$.由带余除法 $A = \lambda m + r, r<m$. 又由于 $r = A - \lambda m$ 是线性组合, $r =0$。
    - 另证：由于 $g=\gcd(a,b)\in S$, $\min S \leq g$. 又 $g | \min S$, $g\leq \min S$

6. 若 $a,b,c\in \Z$, $\gcd(a,c)=1$, 则 $ab,c$ 与 $b,c$ 有相同的公因子。特别地，若 $b,c$ 不全为零，则 $(ab,c) = (b,c)$。
   - 证：$am+cn = 1 \rightarrow abm + cbn = b$
7. 若 $a,b,c\in \Z$
   - $(a,c) = (b,c) =1 \Rightarrow  (ab,c) = 1$
   - $(a,c) = 1, c|ab \Rightarrow c|b$
   - $c$ is prime, $c|ab \Rightarrow c|a$ or $c|b$

## 最小公倍数
设 $a_1,\ldots, a_n$ 是 $n (n >2)$ 个整数. 这 $n$ 个数的*公倍数*是它们的倍数.

$n$ 个数的公倍数中的最小正数叫作最小公倍数(least common multiple), 记作 $\operatorname{lcm} (a_1,\ldots ,a_n).$

由于 $0$ 没有正倍数，讨论 lcm 时默认非零

## 最小公倍数的性质
1. $\forall a,b\in \Z^+$, $a,b$ 的所有公倍数就是 $\operatorname{lcm}(a,b)$ 的所有倍数
2. $\forall a,b \in \Z^+$, $\operatorname{lcm}(a,b) = \frac{ab}{\gcd(a,b)}$

### 证明
1. $[a,b]$ 是 $a,b$ 能同时整除的最小正数，对任意公倍数 $N$ 做带余除法 $N = \lambda [a,b] +\beta$, $\beta$ 能被同时整除，因而 $\beta =0$。
2. 显然 $\frac{ab}{(a,b)}$ 是公倍数。假设存在更小公倍数 $\alpha = \frac{ab}{m(a,b)}$, 则 $a | \alpha, b| \alpha \rightarrow m(a,b)|a,b$. 说明 $\gcd(a,b) = m(a,b)$.矛盾
* gcd 包括了所有相同因数，lcm 包括了最少数量的不同因数
* Let $d=(a,b)$, $a = a_0d, b= b_0d, (a_0,b_0)=1$.$[a,b] = a_0b_0d$

## 算数基本定理
$\forall a> 1$, $a$ 可表示为素数的乘积，即 
$$a=p_1p_2\cdots p_n,\quad p_1\leq \cdots \leq p_n$$
并且若 
$$a = q_1q_2\cdots q_m, \quad q_1\leq\cdots\leq q_m$$
, 则 
$$m=n, q_i = p_i, \forall i$$

### 证
* 归纳证明存在性：$n$ 的每个因数都小于 $n$, 由归纳假设得证
* 唯一性：$p_1|q_1q_2\cdots q_m$, 则 $\exist j, p_1|q_j, p_1 = q_j$, 故 $m\geq n$. 同理 $q_1 = p_i$, $m \leq n$

### 推论
$\forall a \in \Z, a>1$, $a$ 能够惟一地写成 
$$a = p_1^{\alpha_1}p_2^{\alpha_2}\cdots p_k^{\alpha_k}, α_i > 0,i = 1,\ldots ,k$$
其中 $p_i$ 为素数且对任意 $1 \leq i < j \leq k$ 都有 $p_i < p_j$.

该式称作 $a$ 的*标准分解式*, 并且 $a$ 的正因子 $d$ 可表示成如下形式: $d = p_1^{\beta_1}p_2^{\beta_2}\cdots p_k^{\beta_k}, \alpha_i\geq \beta_i \geq 0,i = 1,2,··· ,k$.


## 同余
给定 $m$, 若 $a\%m = b\%m$, 则 $a \equiv b \pmod m$。

充要条件： $m| a-b$

### 同余的性质
若 $a\equiv b \pmod m$, $u \equiv v \pmod m$, 则
1. 加法：$ax+uy\equiv bx+vy \pmod m$
2. 乘法：$au\equiv bv \pmod m$
3. $f(a)\equiv f(b) \pmod m$, $f$ 是整系数多项式

做差整除立即可证

<br>

若 $a\equiv b \pmod m$

1. 对于任意公因子 $d$ 且 $(d,m)= 1$, 有$a_1 \equiv b_1 \pmod m$, where $a = a_1d$.
   - 即互质时有除法
   - 对质数， $a \% m = a/p \%m$
2. $a\equiv b \pmod \beta$, where $\beta|m$ and $\beta >0$
3. $(a,m) = (b, m)$

<br>

若 $a\equiv b \pmod {m_i},\ 1\leq i\leq k$, 则 $a \equiv b \pmod {[m_1,\ldots, m_k]}$
- $a \equiv b \pmod {p_1p_2} \Leftrightarrow \left\{\begin{array}{l} a\equiv b \pmod {p_1}\\ a\equiv b \pmod {p_2}\end{array}\right.$. For prime $p_1, p_2$

## 剩余类
给定 $m\in \Z^+$, 则全部整数可以按余数分成  $m$ 个集合，记作 $C_0,\ldots, C_{m-1}$. 这 $m$ 个集合叫做模 $m$ 的*剩余类*。

- $C_r$ 中任两数差 $m$ 的倍数
- 最大公因数 $(r,m) = (r+ tm, m)$
- *剩余类集* $\Z_m = \{\bar 0, \bar 1, \ldots, \overline{m-1} \}$

<!-- ### 剩余类的性质 -->

## 完全剩余系
每一类各取一个，作为剩余类的*代表元*

**def**: 给定 $m$，设 $a_0,\ldots, a_{m-1}$ 分别来自 $m$ 个剩余类，则 $a_0,\ldots, a_{m-1}$ 这 $m$ 个数是模 $m$ 的一个*完全剩余系*

**iff**:  $a_0,\ldots, a_{m-1}$ 模 $m$ 两两不同余

* 给定 $m$, 设 $a,b \in \Z^+$ 且 $(a,m) =1$, 则若 $a_0,\ldots, a_{m-1}$ 是完全剩余系，$aa_0+b, aa_1+b, \ldots aa_{m-1} +b$ 也是*完全剩余系*
* 设 $m,n$ 互素，$x_0,\ldots, x_{m-1}$ 是模 $m$ 的一个完全剩余系，$y_0,\ldots, y_{n-1}$  是模 $n$ 的一个完全剩余系，则 $nx_0+my_0, nx_0+my_1,\ldots,nx_0+ my_{n-1},nx_1+my_0,\ldots, nx_{m-1}+my_{n-1}$ 是模 $mn$ 的完全剩余系

证：反证做差整除

## 简化剩余系
如果一个模 $m$ 剩余类里面的数与 $m$ 互素，就把他叫做一个*与模 $m$ 互素的剩余类*

在与模 $m$ 互素的全部剩余类中，从每一类中选一个数组成集合，这个集合叫做模 $m$ 的一个*简化剩余系*，记作 $x_1,\ldots, x_{\varphi(m)}$
1. 只要 $C_r$ 里有一个数与 $m$ 互素，则 $C_r$ 与 $m$ 互素
2. 设 $x_1, \ldots, x_{\varphi(m)}$ 是模 $m$ 的一个简化剩余系，则 $ax_1,\ldots,ax_{\varphi(m)}$ 也是模 $m$ 的一个简化剩余系。其中 $\gcd(a,m)=1$

设 $m,n$ 互素，$x_1,\ldots, x_{\varphi(m)}$ 是模 $m$ 的一个简化剩余系，$y_1,\ldots, y_{\varphi(n)}$  是模 $n$ 的一个简化剩余系，则 $nx_1+my_1, nx_1+my_2,\ldots,nx_1+ my_{\varphi(n)},nx_2+my_1,\ldots, nx_{\varphi(m)}+my_{\varphi(n)}$ 是模 $mn$ 的简化剩余系

### 证
1. 证这 $\varphi(m)\varphi(n)$ 个数与 $mn$ 互素.<br>
假设存在 $nx_j + m y_i$ 与 $mn$ 不互素，则必存在素数 $p$ 使 $p| my_i+ nx_j$, $p|mn$. 
<br>
由于 $p|mn$, $p|m$ 或 $p|n$. 设 $p|m$, 则 $p| my_i+nx_j \Rightarrow p|nx_j$. 又因 $(m,x_j) = 1, p\nmid x_j$. 故 $p|n$，$(m,n) \neq 1$.
<br>
对称地， $p\nmid m, p\nmid n$, 与 $p|mn$ 矛盾。这说明 $(my_i + nx_j, mn) = 1$

2. 知 $my+ ny$ 组成完全剩余系，拥有不同余数。于是证 $\forall x,y\in \Z\ s.t.\ (my+nx,mn) =1$ 都有 $(x,m)= (y,n) =1$。
   $$(my+nx,mn) =1 \Rightarrow (my+nx,m) = (my+nx,n) = 1 \Rightarrow (nx,m) = (my,n) = 1$$

### 推论
若 $m,n$ 互素，$\varphi(mn)=\varphi(m)\varphi(n)$


## 欧拉函数
$\varphi : \Z^+ \rightarrow \Z^+$. $\varphi(n)$ 返回 $0,1,2,\ldots,n-1$ 中与 $n$ 互素的数的个数

* If $n$ is prime, $\varphi(n) =n-1$
* If $(m,n) =1$, $\varphi(mn) = \varphi(m)\varphi(n)$
* For prime $p$, $\varphi(p^t) = p^t - p^{t-1}$
* For $n = p_1^{t_1}p_2^{t_2}\cdots p_s^{t_s}$, $\varphi(n) = n(1-\frac{1}{p_1})(1-\frac{1}{p_2})\cdots(1-\frac{1}{p_s})$

## 欧拉定理
设 $n\in \Z^+/\{1\}$, 若 $(a,n) =1$, 则 $a^{\varphi(n)}\equiv 1\pmod n$

### 证
设 $x_1,x_2,\ldots, x_{\varphi(n)}$ 是模 $n$ 的一个简化剩余系，那么因为 $a$, $n$ 互素，$ax_1,\ldots, ax_{\varphi(n)}$ 也是模 $n$ 的一个简化剩余系。由于简化剩余系遍历了所有互素余数，有
$$ax_1\cdot ax_2 \cdots ax_{\varphi(n)} \equiv x_1 \cdots x_{\varphi(n)} \pmod n$$
$$a^n x_1\cdots x_{\varphi(n)} \equiv x_1 \cdots x_{\varphi(n)} \pmod n$$
又因为 $(x_1x_2\ldots x_{\varphi(n)},n) = (x_i,n) = 1$, 所以
$$a^n \equiv 1 \pmod n$$

## 费马小定理
若 $p$ 是素数，则 $a^p \equiv a \pmod p$
### 证
若 $(a,p) = 1$, 则 $a^{p-1} \equiv 1 \Rightarrow a^p \equiv a$

若 $(a,p)\neq 1$, 则 $p|a \Rightarrow p| a(a^{p-1}-1)$

## 同余式
对 $f(x) = \sum_0^n a_i x^i$, $n>0$，对于给定的 $m>0$, 方程 $f(x) \equiv 0 \pmod m$ 是模 $m$ 的*同余式*。

若 $a_n\not\equiv 0 \pmod m$, 则 $n$ 叫做同余式的次数。若 $\lambda$ 是使 $f(\lambda) \equiv 0 \pmod m$ 成立的一个整数，则 $x\equiv \lambda \pmod m$ 叫做同余式的一个解，并且把满足 $f(x) \equiv 0 \pmod m$ 且对 $m$ 相互同余的一切数算作同余式的一个解。

eg. 一次同余式 
$$x \equiv b_1 \pmod {m_1}, \quad x \equiv b_2 \pmod {m_2}, \quad \cdots$$

## 中国剩余定理
### 引理
~~设 $p$ 是素数，~~ 

$p>1$, $a$ 是任意整数。若 $(a,p) = 1$, 则存在唯一整数 $0\leq m<p$ 使 $ma \equiv 1\pmod p$

存在性：$ma+np = 1 \Rightarrow (m-tp)a + (n+ta)p = 1 \Rightarrow m_1 a \equiv 1 \pmod p$

唯一性：做差整除

iff

### 定理
设 $m_1,m_2\ldots, m_k$ 是 $k$ 个两两互素的正整数，令 $m = \prod_1^k m_i$ 并记 $m = m_i M_i$. 则同余式组
$$x \equiv b_1\pmod {m_1},\quad x\equiv b_2 \pmod {m_2}, \cdots, x\equiv b_k \pmod {m_k}$$ 
的解是
$$x \equiv \lambda_1M_1b_1 + \lambda_2M_2b_2 + \cdots + \lambda_kM_kb_k \pmod m$$
其中 $\lambda_iM_i \equiv 1 \pmod {m_i}$, 由引理可知 $\lambda_i$ 唯一

### 证明
1. 验证：$\lambda_iM_ib_i \% m_j = \delta_{ij}b_i$
2. 唯一：设 $x_1, x_2$ 是同余式的解，$x_1 \equiv x_2 \equiv b_i \pmod {m_i}$. 由于 $(m_i,m_j) =1$, $x_1 \equiv x_2 \pmod {[m_1,m_2,\ldots] = m}$


## RSA
### I. 生成密钥
- 随机选两个不同大素数 $p,q$, 计算 $n=pq$, $\varphi(n) = (p-1)(q-1)$.
- 在区间 $[1,\varphi(n)]$ 内任选一个大整数 $e$, 使 $(\varphi(n),e) =1$
- 计算 $d$，使 $de\equiv 1\pmod {\varphi(n)}$
- $\{e,n\}$ 作为公钥，$\{d,n\}$ 作为私钥

### II. 加密运算
对明文 $m< n$ 进行加密
$$c = Encode(m) \equiv m^e \pmod n$$
### III. 解密运算
对 $c$ 解密
$$m = D(c) \equiv c^d \pmod n$$

## 二次剩余 
对于方程 $x^2 \equiv a \pmod n$，若 $x$ 有解，则称 $a$ 是模 $n$ 的二次剩余；若 $x$ 无解，则称 $a$ 是 $n$ 的二次非剩余。

通常 $a \not \equiv 0 \pmod n$

### 性质
- 二次剩余是对称的 $x^2 \equiv (n-x)^2 \pmod n$
- 对于奇素数 $p$，方程 $x^2 \equiv a \pmod p$ 中 $p$ 的二次剩余的个数为 $\frac{p-1}{2}$, 二次非剩余的个数也为 $\frac{p-1}{2}$
  - 证明： 等价于证 $1^2,\ldots, \left(\frac{p-1}{2}\right)^2$ 互不相同
- 若 $x^2 \equiv \lambda^2 \pmod n$, 则 $x \equiv \pm \lambda \pmod n$
- 对于奇素数 $p$, 
  - $a$ 是模 $p$ 的二次剩余的充要条件是 $a^{\frac{p-1}{2}} \equiv 1 \pmod p$
  - $a$ 是模 $p$ 的二次非剩余的充要条件是 $a^{\frac{p-1}{2}} \equiv -1 \pmod p$
  * 证：$a^{p-1} \equiv 1 \Rightarrow a^{\frac{p-1}{2}}\equiv \pm 1$.

## Rabin
随机选取两个大素数 $p,q$, 并且 $p\equiv q \equiv 3 \pmod 4$. 令 $n=pq$, 以 $\{n\}$ 为公钥，$\{p,q\}$ 为私钥，加密时将明文 $m$ 加密为 $c \equiv m^2\pmod n$，解密时解二次剩余 $x^2 \equiv c$, 得到 $4$ 个解

# 集合论
将一些不同的对象放在一起, 即为*集合*. 其中的对象称为集合的*元素*.
$$S = \{1,2,3,4\}, S = \{a\in \Z| a\%2 =1\}$$

## 集合族
设 $I$ 为一集合且任意 $i \in I$ 都对应一个集合 $A_i$, 则由这些 $A_i$ 的全体构成的集合称为*集合族*, 通常记为 $\{A_i\}_{i\in I}$, 其中 $I$ 称为该集合族的下标集合或指标集合

## 子集
设 $A$ 为一个集合, 如果 $a$ 是 $A$ 中的元素, 则称 $a$ 属于 $A$, 记为 $a\in A$, 否则记为 $a \notin A$

若 $\forall a\in A, a\in B$, 则称 $A$ 是 $B$ 的子集, 记为 $A \subseteq B$.

- $A \subseteq B, A \supseteq B \Rightarrow A = B$
- $A \subseteq B, A \neq B$, 则 $A$ 是 $B$ 的*真子集* (proper subset), 记为 $A \subsetneq B$.
- 不含任何元素的集合称为*空集* (empty set), 记为 $\emptyset$. $\emptyset$ 是任何集合的子集, 且是任何非空集合的真子集.

集合 $\Omega$ 的所有子集的集合称为 $\Omega$ 的幂集 (power set), 记为 $P(\Omega)$

## 集合的阶
如果集合 $A$ 的元素个数有限, 称 $A$ 为*有限集*, 其元素个数称为集合的*阶*或*基数*或*势* (cardinality / order of ﬁnite set), 记为 $|A|$ 或 $\#A$. 

元素个数无限的集合, 即*无限集*. 它的阶定义为 $\infty$.

如果存在单射 $f:A\rightarrow B$, 则 $|A|\leq |B|$. 如果有双射 $f:A\rightarrow B$, 则 $|A| = |B|$

### 可列集
存在*双射* $f: N\rightarrow D$

## 集合的运算
$$A \cap B, \qquad\bigcap_{i\in I} A_i$$
$$A \cup B, \qquad\bigcup_{i\in I} A_i$$
不交并
$$\bigsqcup_{i\in I} A_i := \bigcup A_i, A_i \cap A_j = \empty$$
$A$ 关于 $\Omega$ 补集
$$\Omega -A = \Omega \backslash A =\bar A$$
直积（笛卡尔积）
$$A\times B, \qquad \prod_{i\in I} A_i = \{(a_i)_{i\in I} | a_i \in A_i\}$$

## 映射
$$A\xrightarrow{f} B; \quad f:A \rightarrow B,\   a\mapsto f(a)$$
任意一个原像 $a\in A$ 都要有对应的像 $b\in B$
- 单射 (injective) $f(a_1)= f(a_2) \Rightarrow a_1 = a_2$ 
- 满射 (surjective) $\forall b\in B, \exist a\in A, f(a) = b$
- 一一映射 (bijective)
- $f = g \Leftrightarrow \forall a\in A, f(a) = g(a)$

### 变换
一个集合 $A$ 向其自身的映射 $f: A\rightarrow A$ 被称作集合 $A$ 上的*变换*

每个元素到自己的一一映射被称作恒等映射: $1_A$

### 复合
设 $f: A\rightarrow B$, $g: B\rightarrow C$. 则可通过连续作用，得到*复合映射* $h: A\rightarrow C$. 记作 $h = g\circ f, (g\circ f)(a) = g(f(a))$

### 逆映射
对 $A\xrightarrow{f} B$, 若 $\exist B\xrightarrow{g} A$, 且 $f\circ g = 1_B, g\circ f = 1_A$, 则 $f$ 是*可逆映射*, $g$ 是 $f$ 的逆映射
* 逆映射唯一，$f^{-1}$

### 结合律
$(h \circ g) \circ f = h \circ (g \circ f)$.
* $(h\circ g)$ 先对整个 $B$ 建立映射，在缩小定义域到 $f$ 的像集。 

### 逆映射存在性
1. 映射 $f: A \rightarrow B$ 是一一映射的充分必要条件是 $f$ 是可逆映射.
2. 设 $f: A \rightarrow B$ 和 $g: B \rightarrow C$ 都是一一映射, 则 $g \circ f : A \rightarrow C$ 也是一一映射, 并且 $(g \circ f)^{−1} = f^{−1} ◦ g^{−1}$.

## 关系
设 $S$ 是一个非空集合, $R$ 是关于 $S$ 的元素的一个条件. 如果对 $S$ 中任意一个有序元素对 $(a,b)$, 我们总能*确定* $a$ 与 $b$ *是否*满足条件 $R$, 就称 $R$ 是 $S$ 的一个*关系* (relation). 如果 $a$ 与 $b$ 满足条件 $R$, 则称 $a$ 与 $b$ 有关系 $R$, 记作 $aRb$; 否则称 $a$ 与 $b$ 无关系 $R$.

因为 true, false 确定，关系 $R$ 也称为二元关系 （true, false 而非 a,b）

## 等价关系
设 $R$ 是非空集合 $S$ 的一个关系, 如果 $R$ 满足
1. 反身性, 即对任意的 $a \in S$, 有 $aRa$;
2. 对称性, 即若 $aRb$, 则 $bRa$;
3. 传递性, 即若 $aRb$, $bRc$, 则 $aRc$

则称 $R$ 是 $S$ 的一个*等价关系*. 称 $aRb$ 为 $a$ 等价于 $b$, 记作 $a \sim b$.

## 等价类
如果 $\sim$ 是集合 $S$ 的一个等价关系, 对任意 $a \in S$, 令 $[a] = \{x \in S | x \sim a\}$. 称子集 $[a]$ 为 $S$ 的一个*等价类*. 

$S$ 的全体等价类的集合称为集合 $S$ 在等价关系下的*商集*, 记 $S/ ∼$.

- 剩余类集 $\Z_m$ 是商集，每一个剩余类是等价类
- 集合与集合等价：$A \sim B \Leftrightarrow \exist f: A\rightarrow B$ 且 $f$ 是一一映射。
  - 等价的两个集合等势 $|A|=|B|$

* 给定 $\sim$ 是一个 $S$ 上的等价关系，则 $S$ 中的每个元素一定在一个等价类中，且各等价类不相交

## 分类
如果非空集合 $S$ 是它的某些两两不相交的非空子集的并, 则称这些子集为集合 $S$ 的一种*分类* (partition), 其中每个子集称为 $S$ 一个*类* (class). 如果 $S$ 的子集族 $\{S_i\}_{i\in I}$ 构成 $S$ 的一种分类, 则记作 $P = \{S_i\}_{i\in I}$.

- 集合 $S$ 的任何一个等价关系都确定了 $S$ 的一种分类, 且其中每一个类都是集合 $S$ 的一个等价类. 
  
  反之, 集合 $S$ 的任何一种分类也都给出了集合 $S$ 的一个等价关系, 该关系可定义为 $a,b$ 在同一个 $S_i$ 中
