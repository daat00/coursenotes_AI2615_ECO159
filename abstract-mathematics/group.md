# 群
## 代数运算
设 $S$ 为集合. 我们称映射 $f : S \times S \rightarrow S$, $(a,b) \mapsto c$ 为集合 S 上的一个*代数运算*或*二元运算* (binary operation).

- $f$ 是代数运算，代数运算是封闭的

在数学应用中, 记号 $c = f(a,b)$ 并不是一个很适宜的记号. 实际上, 我们经常使用 "$\cdot$" 和 "$*$" 等符号来表示代数运算, 即 $c = a \cdot b,a \times b,a *b,a + b,a \circ b$ 等.

### 性质
- 集合 $S$ 上的代数运算 "$\cdot$" 如果满足对任意 $a,b,c \in S$, 都有 $(a \cdot b) \cdot c = a \cdot (b \cdot c)$, 则称该代数运算满足结合律
- 如果对任意 $a,b \in S$, 都有 $ab = ba$, 则称其满足交换律

eg. 
1. 有理数加减乘，非零有理数除法
2. 剩余类集：对 $\bar a, \bar b \in \Z_m$, 规定 $+:\bar a + \bar b = \overline{a+b}$, $\cdot: \bar a\cdot \bar b = \overline{ab}$, 则 "$+,\cdot$" 都是代数运算

## 群 
设 $G$ 是一个非空集合, "$\cdot$" 是 $G$ 上的一个代数运算, 即对所有的
$a,b \in G$, 有 $a \cdot b \in G$. 如果运算 "$\cdot$" 满足下述三个条件:
1. 结合律成立, 即对所有的 $a,b,c \in G$, 有 $(a \cdot b) \cdot c = a \cdot (b \cdot c)$;
2. $G$ 中有*单位元* $e$, 即对每个 $a \in G$, 有 $e\cdot a = a \cdot e = a$;
3. $G$ 中每个元素 $a$ 均有*逆元*, 即存在元素 $b \in G$ 使得 $a \cdot b = b \cdot a = e$.

则称 $G$ 关于运算 "$\cdot$" 构成一个*群* (group), 记作 $(G,·)$.

- 如果群 $(G,·)$ 仅满足结合律, 我们称之为*半群*; 
- 如果 $(G,·)$ 满足结合律且存在单位元幺 $e$, 我们称之为*含幺半群*.
- 单位元 $e$ 和每个元素的逆元都是唯一的. $a\in G$ 的唯一的逆元通常记作 $a^{−1}$
- 约定 $a^n = a \cdot a\cdots a$, $a^0 = e$, $a^{-n} = \left(a^{-1}\right)$. 这样 $a^{m+n} = a^m \cdot a^n$, $\left(a^{m}\right)^n = a^{mn}$


### 群的阶
群 $(G,·)$ 中元素的个数称为群 $G$ 的*阶*, 记为 $|G|$. 如果 $|G|< \infty$, 则称 $G$ 为有限群, 否则称 $G$ 为无限群.

### 交换群
如果群 $G$ 上的代数运算 "$\cdot$" 还满足交换律, 即 $\forall a,b \in G$, 有 $a \cdot b = b \cdot a$, 则称 $G$ 是一个*交换群*或*阿贝尔群*

1. 通常用 "$+$" 法来表示阿贝尔群 $G$ 的代数运算, 记为 $(G,+)$. 习惯上, 只有当一个群为交换群时, 才用 "$+$" 来表示群的运算, 并称这个运算为*加法*, 运算的结果为*和*, 同时称这样的群为*加群*.
2. 阿贝尔群 $(G,+)$ 上的单位元记为 $0$, 并称 $0$ 为 $(G,+)$ 的*零元*; 记 $(G,+)$ 中 $a$ 的逆元为 $−a$, 并称 $−a$ 为 a 的负元.
3. 不是加群的群称为*乘群*. 乘群的代数运算叫做*乘法*, 运算的结果叫做*积*, 乘群的运算符号通常省略不写.
4. 约定 $na$ 表示 $n$ 个 $a$ 相加, 再约定 $0a = 0$ 以及 $(−n)a = n(−a)$. 这样 $na$ 对任意整数 $n$ 都有意义, 且对任意 $m,n \in Z$ 有 $na + ma = (n + m)a$, $m(na) = mna$, $n(a + b) = na + nb$.

eg. 整数加群 $(\Z,+)$, $(\mathbb{Q}^+, \R^+,\Complex^+, +,-,*,\div)$; 剩余类乘法群 $(U(m)= \{\bar a \in \Z_m \mid (a,m) =1\}, *)$

* 全体 $n$ 阶可逆方阵的集合 $GL_n(\reals)$ 关于矩阵的乘法运算构成群. 群 $GL_n(\Reals)$ 中的单位元是单位矩阵 $E_n$, $A$ 的逆元是 $A^{−1}$. 

## 群的性质
设 $G$ 为群, 则有
1. 群 $G$ 的单位元是唯一的;
2. 群 $G$ 的每个元素的逆元是唯一的;
3. $\forall a\in G$, $(a^{−1})^{−1} = a$;
4. $(ab)^{−1} = b^{−1}a^{−1}$;
5. 消去律: $\forall a,b,c \in G$, 如果 $ab = ac$ 或 $ba = ca$, 则 $b = c$ ($a^{-1}$ 存在)

## 群的判定
设 $G$ 是一个具有代数运算的非空集合, 则 $G$ 关于所给的运算构成群的充分必要条件是
1. $G$ 的运算满足结合律;
2. $G$ 中有左单位元 $e$, 使 $ea = a$;
3. $\forall a\in G, \exist a' \in G$ (称为 $a$ 的左逆元), 使得 $a'a = e$. $e$ 是左单位元

### 证
* $e = ee \Rightarrow a'a  = a'ae$ 因 $a'\in G$, $a'$ 有左逆，$a = ae$.
* $a' aa' = ea' = a'$, 乘左逆， $aa' = e$

设 $G$ 是一个具有乘法运算的非空有限集合, 如果 $G$ 满足结合律且左、右消去律均成立, 则 $G$ 构成群.


## 子群
设 $G$ 是一个群, $H$ 是 $G$ 的一个非空子集. 如果 $H$ 关于 $G$ 的运算也构成群, 则称 $H$ 为 $G$ 的一个*子群*, 记作 $H < G$.

- 对任意群 $G$, $G$ 本身以及只含单位元 $e$ 的子集 $H = \{e\}$ 是G 的子群;
- $H = \{e\}$ 和 $G$ 称为 $G$ 的*平凡子群* (trivial subgroup); 群 $G$ 的其它子群称为 $G$ 的*非平凡子群* (nontrivial subgroup);
- 不等于 $G$ 自身的子群称为 $G$ 的*真子群*

eg. $m\Z = \langle m \rangle = \{mz\mid z\in \Z\}$

### 单位元和逆元
设 $G$ 为群, $H$ 是 $G$ 的子群, 则
1. 群 $G$ 的单位元 $e$ 是 $H$ 的单位元;
2. $\forall a \in H$, $a$ 在 $G$ 中的逆元 $a^{−1}$ 就是 $a$ 在 $H$ 中的逆元.

### 子群的判定
1. 给定 $(G,\cdot)$, 显然结合律在任何关于 $\cdot$ 运算封闭的*非空*子集 $H$ 上都成立. 因此, 从定义出发, 如果 $H$ 满足下列条件:
   - $H \neq \empty$
   - $H$ 对 $\cdot$ 封闭;
   - $e\in H$; 
   - $\forall a \in H$, $a^{-1} \in H$
   
   则 $H < G$.

2. 设 $G$ 为群, $H$ 是群 $G$ 的*非空*子集, 则 $H$ 成为群 $G$ 的子群的充分必要条件是:
   1. $\forall a,b \in H$, $ab \in H$;
   2. $\forall a \in H$, $a^{−1} \in H$.
   
   即封闭性加上 $a,a^{-1} \in H$ -> $e\in H$

3. 设 $G$ 为群, $H$ 是群 $G$ 的*非空*子集, 则 $H$ 成为 $G$ 的子群的充分必要条件是
   - $\forall a,b \in H$, $ab^{−1} \in H$

4. (有限群) 设 $G$ 为一有限群, $H$ 是群 $G$ 的*非空*子集, 则 $H$ 成为 $G$ 的子群
的充分必要条件是对任意的 $a,b \in H$, 有 $ab \in H$.

   有限集上子群 iff 封闭子集

## 中心
中心元描述群的对称性 

设 $G$ 为群. 取 $a\in G$, 若 $\forall x \in G$, $ax = xa$, 则称 $a$ 为 $G$ 的一个*中心元*. 

记 $C(G) = \{g \in G \mid gx = xg,∀x ∈G\}$, 称 $C(G)$ 为 $G$ 的*中心*

- $C(G)$ 是 $G$ 的子群  

取 $a \in G$, 定义 $a$ 在 $G$ 中的*中心化子*为 $C(a) = \{g \in G \mid ga = ag\}$.

- $C(a)$ 是 $G$ 的子群

设 $G$ 是群, 则 $C(G) = \bigcap_{a\in G} C(a)$, 即 $G$ 的中心是所有形如 $C(a)$ 的子群的交集.

## 子群的交
群 $G$ 的任意两个子群的交集还是 $G$ 的子群.

子群的并不一定是子群。

## 生成子群
### 引理
设 $S$ 是群 $G$ 的一个非空子集, 令 $M$ 表示 $G$ 中所有包含 $S$ 的子群所组成的集合, 即 
$$M = \{H < G \mid S \subseteq H\}$$
令$K = \bigcap_{H\in M} H$, 则 $K$ 是 $G$ 的子群.

### 生成子群
设 $S$ 是群 $G$ 的一个非空子集, $M = \{H < G \mid S \subseteq H\}$, $K = \bigcap_{H\in M} H$, 称 $K$ 为群 $G$ 的由子集 $S$ 所生成的子群, 简称*生成子群*, 记作 $\langle S \rangle$. 即 $\langle S \rangle = \bigcap_{H: H<G, S\subset H} H$

子集 $S$ 称为 $\langle S \rangle$ 的*生成元集*或*生成元组*. 

如果 $S = \{a_1,a_2,\ldots,a_r\}$ 为有限集, 则记 $\langle S\rangle = \langle a_1,a_2,\ldots,a_r\rangle$.

### 生成子群的特征
设 $S$ 是群 $G$ 的非空子集, 则
1. $\langle S\rangle$ 是 $G$ 的包含 $S$ 的最小子群;
2. $$\langle S\rangle = \{a_1^{l_1}a_2^{l_2}\cdots a_k^{l_k} \mid a_i \in S,l_i = \pm 1,k \in \N\}$$
   - 即不断用封闭性加入代数组合, 类似 convex hull
   - Induction 或由封闭性 $\{a_1^{l_1}a_2^{l_2}\cdots a_k^{l_k},\ldots\}\subset \langle S\rangle$

# 群的积、陪集
## 群中集合的乘积
设 $A,B$ 是乘群 $G$ 的两个非空子集，称集合 $AB = \{ab \mid a\in A, b\in B\}$. 为群 $G$ 子集 $A$ 与 $B$ 的**积**
- $AB \neq BA$
- $AB = AC \nRightarrow B = C$

当 $G$ 是加群时，类似地有 $A+B = B+ A = \{a +b \mid a\in A, b\in B\}$.

特殊地，对单元素子集略去集合号，即 $gA = \{ga \mid a\in A\}$, $g+A = A+g$

### 集合乘积的运算律
- $A(BC) = (AB)C$
- $eA = A$
- $\exist g \in G, \ s.t.\ gA = gB$ 或 $Ag = Bg$, 则 $A = B$
  + $ga = gb \Longrightarrow a = b$
  + $A = eA = g^{-1}g A = g^{-1}gB = B$

## 子群的积
对于 $G$ 的两个子群 $H<G, K<G$, 有
- $HH = H$
  + $a=ea, H = eH \subset HH$
  + 封闭， $HH \subset H$
- $HK < G$ 当且仅当 $HK = KH$
  + $KH = HK$ 不代表 $hk = kh$
  + $\Leftarrow$: $h_1k_1k_2^{-1}h_2^{-1} \in HKH= HK$

## 子群的陪集
设 $H<G$, $\forall a \in G$, 分别称 $aH$ 与 $Ha$ 为 $H$ 在 $G$ 中的 **左陪集** 和 **右陪集** (coset)

- $H$ 的一个陪集一般不是 $G$ 的子群
- $G$ 的两个不同元素可能生成 $H$ 的同一个左陪集

### 陪集的性质
设 $H<G$, $a,b\in G$
1. $a\in aH$
2. $aH = H$ 的充分必要条件是 $a\in H$
   + $b = eb = aa^{-1}b$, $H = a(a^{-1}H)\subset aH$
3. $aH < G$ 的充分必要条件是 $a\in H$, $aH = H$
   + $a\in aH$, $e\in aH$, $a^{-1}\in H$.
4. $aH = bH$ 的充分必要条件是 $ab^{-1} \in H$
5. $aH$ 与 $bH$ **要么完全相同，要么无公共元素**
   + $aH \cap bH \neq \emptyset\Rightarrow ah_1 = bh_2$. 于是 $aH = ah_1 H  = bh_2 H = b H$
6. $\vert aH\vert = \vert bH\vert$

*群内元素再作用一次得到同一个群，暨关于 $\cdot$ 不但封闭，还是对称的*

右陪集同理

### 等价关系、群的分类
设 $H < G$. 定义 $G$ 上的关系: $\forall a,b \in G$, $a \sim b$ iff $a^{−1}b\in H$ iff $aH = bH$. 则 $\sim$ 是 $G$ 上的等价关系, 并且元素 $a$ 对 $\sim$ 的等价类 $[a] = aH$

因此群 $G$ 可表示成子群 $H$ 的一些互不相交的左陪集之并. 
从而群 $G$ 的子群 $H$ 的全体左陪集的集合组成群 $G$ 的一种分类, 即
$G = \bigcup_{g_i} g_iH$. 

特别地, 如果 $G$ 为有限群, 则 $|G|=\sum_{g_i} |g_iH|= \sum_{i=1}^t |H|= t|H|$. 其中 $t$ 为 $H$ 不同左陪集的个数.

## 左右陪集的关系
记 $G/H$ 和 $H\backslash G$ 为 $H$ 全体左陪集和右陪集组成的集合
$$
G / H = \{gH \mid g \in G\}\\
H \backslash G = \{Hg \mid g\in G\}
$$

二者满足如下一一映射：
$$
\phi : G/H \rightarrow H\backslash G,\\
aH \mapsto Ha^{−1}
$$

因此 $H$ 左右陪集的个数同阶

## 群的指数
设 $H<G$, 称 $H$ 在 $G$ 中的左陪集或右陪集的个数为 $H$ 在 $G$ 中的*指数* (index), 记作 $[G:H]$

## Lagrange 定理
设 $G$ 是一个有限群, $H<G$, 则 $|G|= |H|[G : H]$
+ 各陪集同阶

## 元素的阶
设 $G$ 是一个群, $e$ 是 $G$ 的单位元, $a \in G$. 如果存在正整数 $r$, 使 $a^r = e$, 则称 $a$ 是*有限阶元素*, 否则称 $a$ 是*无限阶元素*. 使 $a^r = e$ 的最小正整数 $r$ 称为元素 $a$ 的*阶*, 记作 $\operatorname{ord} a = r$. 对无限阶元素 $\operatorname{ord} a = \infty$

### 阶的性质
1. $\forall a\in G$, $\operatorname{ord} a = \operatorname{ord} a^{−1}$
2. 设 $\operatorname{ord} a = n$. 如果有 $m \in \Z$, $a^m = e$, 则 $n |m$
3. 设 $\operatorname{ord} a = n$, 则 $\forall m \in \Z, \operatorname{ord} a^m = \frac{n}{(n,m)}$
   + Let $r = \operatorname{ord} a^m$, then $n| rm$. So $\frac{n}{(n,m)} | \frac{m}{(n,m)}r$, $\frac{n}{(n,m)} | r$
4. 设 $\operatorname{ord} a = n$, $\operatorname{ord} b = m$. 如果 $ab = ba$, 且 $\gcd(n,m) = 1$, 则 $\operatorname{ord}(ab) = mn$
   + Let $\operatorname{ord}(ab) = r$. I claim $m|r, n|r$
   + From $a$, $a^{rm} = (ab)^{rm} = r$, so $n | rm$ and $n| r$. 
   + By $(m,n) = 1$, $\alpha mr+ \beta nr = r$. So $mn|r$ 

### 有限群元素的阶
设 $|G|= n<\infty$, 则 $\forall a \in G$, $\operatorname{ord} a <\infty$, 且 $\operatorname{ord} a \mid \vert G\vert$. 

即有限群中任一元素的阶都是群阶数的因子

证：循环子群 $H = \{a, a^2, a^3,\cdots\}$，$|H| = \operatorname{ord}a$, $|G| = \lambda |H|$
<br><br>

Fermat 小定理推广：设 $|G|= n<\infty$, 则 $\forall a \in G$, $a^n = e$
- 对于模 $p$ 乘法群，$n=p-1, e=1$.

## 正规子群
设 $H < G$, 如果 $\forall a \in G$, $aH = Ha$, 则称 $H$ 是 $G$ 的一个*正规子群* 或 *不变子群*. 记作 $H \triangleleft G$.
- $aH = Ha$ 是集合的相等，不代表 $ah = ha$
- 显然 $\{e\}\triangleleft G, G \triangleleft G$. 他们是平凡正规子群

例：
+ 交换群
+ 给 $H,K < G$, 若 $H \triangleleft G$ 且 $H \subseteq K$. 则 $H \triangleleft K$
+ 设 $H < G$, 如果 $H$ 在 $G$ 中的指数 $[G : H] = 2$, 则 $H$ 是 $G$ 的正规子群.
    - If $a \in H$, $aH = H = Ha$
    - If $a\notin H$, $aH$ 与 $H$ 完全不同. $G = H \cup aH = H \cup Ha$.

### 正规子群的判定
设 $H <G$, 下列三个条件等价
1. $H \triangleleft G$
2. $\forall a\in G, aHa^{-1} = H$
3. $\forall a\in G, aHa^{-1} \subseteq H$
   + $aha^{-1} = h'$
   + $ah \in Ha$, $ha = a(a^{-1}ha) \in aH$

Intuition: If $aHa^{-1}\subset H$, then $aH \subset Ha$ and $a^{-1} H \subset Ha^{-1}$. Also $|aH|= |Ha|$
### 正规子群的性质
设 $H\triangleleft G, K \triangleleft G$, 则 $H \cap K \triangleleft G, HK\triangleleft G$

## 陪集的运算
设 $H \triangleleft G$, 令 $G/H = \{aH \mid a \in G\}$. 

$\forall aH, bH \in G/H$, 规定 $G/H$ 中关于陪集的运算 "$\cdot$" 为 $(aH)\cdot(bH) = (ab)H$.

- 该运算 "$\cdot$" 是代数运算，即当 $aH,bH$ 出现重复时，$abH$ 与代表元选取无关
  + 因为正规，可以把 $a,b$ 换到中间
  + $\{a\} H \{b\} H = H \{ab\} H = ab HH = abH$
- $G/H$ 关于 "$\cdot$" 构成群, 单位元为 $eH = H$, 逆为 $a^{-1} H$. 若 $G$ 是加群，则 $G/H$ 也是加群

## 商群
设 $H \triangleleft G$, $G/H$ 关于陪集运算 $\cdot$ 构成的群称为群 $G$ 关于子群 H 的*商群*. 仍记作 $G/H$
- 商群的阶 $\vert G/H\vert = [G:H]$

## 柯西定理
设 $G$ 为有限交换群, $|G|= n$. 可以证明: 
- 对 $n$ 的任一素因子 $p$, $G$ 必有阶为 $p$ 的元素
- 对 $n$ 的任一因子 $m$, 存在 $m$ 阶子群

### 证明
Induction on $n$. Base $n=2$ is trivial.

For $n >2$, consider $a\in G$, $a \neq e$ and $\operatorname{ord} a = r$. If $p|r$, assume $r = kp$, then $\operatorname{ord} a^k = p$.

If $p\nmid r$, let $H = \langle a\rangle$, we can infer that $H \triangleleft G$ so it has quotient group, and its quotient group $G/H$ is permutable. Also, $|G/H| = \frac{n}{r}< n$, and to apply induction we find $p| \frac{n}{r}$ due to $p|n, p\nmid r$. 

So by hypothesis, $\exist bH \in G/H$, $\operatorname{ord}bH =p$, which means $b^p H = H$. Thus $b^p \in H$, and $b^{pr} = e$.

Meanwhile, $p = \operatorname{ord} bH \nmid r \Rightarrow (bH)^r \neq H, b^r \notin H$. So $b^r \neq e$. 

Thus by $(b^r)^p = e$, $\operatorname{ord} b^r | p$, $\operatorname{ord} b^r = p$. 

# 同态和同构
设 $(G,\cdot)$ 与 $(G',*)$ 是两个群, $\phi$ 是 $G$ 到 $G'$ 的一个映射. 

如果 $\forall a,b\in G$, 都有 $\phi(a \cdot b) = \phi(a) * \phi(b)$, 则称 $\phi$ 是群 $G$ 到群 $G'$ 的一个*同态映射*, 简称*同态*. 

- 当同态映射 $\phi$ 作为集合映射为满射时, 称 $\phi$ 为群 $G$ 到 $G'$ 的*满同态*
- 当同态映射 $\phi$ 作为集合映射是单射时, 称 $\phi$ 为群 $G$ 到 $G'$ 的*单同态*

当同态映射 $\phi$ 作为集合映射是*一一映射*时, 称 $φ$ 为群 $G$ 到 $G'$ 的*同构*. 表示成 $G \cong G'$. 
  - 群 $G$ 到它自身的同态 (同构) 映射称为群 G 的*自同态* (*自同构*)
  - 一一映射 $\neq$ 集合相等

### 同态的例子
I. 整数加群 $\Z$ & 非零实数乘群 $\Reals^+$中，如下 $\phi$ 构成同态
$$
\phi: \Z \rightarrow \Reals^*\\
n \mapsto (-1)^n
$$

II. 设 $\Reals[x]$ 为全体实系数多项式关于多项式的加法所构成的群. 令
$$ \phi: \Reals[x] \rightarrow \Reals[x]\\
f(x) \mapsto f'(x)
$$
则 $\phi$ 是一个满同态.

III. 设 $H \triangleleft G$. 则 $G$ 和商群 $G/H$
$$η : G \rightarrow G/H\\
a \mapsto aH
$$
则 $\eta$ 是满同态. 称这样的同态为自然同态

IV. 恒等同构
$$
\phi : G \rightarrow G\\
a \mapsto a
$$

V. 全体实数组成的加法群 $\Reals$ 与全体正实数组成的乘法群 $\reals^+$ 同构. 
- $\phi :y = e^x$
- $x = \ln y$ 是 $\phi$ 的逆

VI. 

$n$ 次单位根组成的集合 $U_n = \{e^{j\frac{2k\pi}{n}} \mid 0 \leq k <n \}$ 关于数的乘法构成群.

$\Z$ 的模 $n$ 剩余类可构成加群 $(Z_n,+)$. 

作映射
$$ \phi : U_n \rightarrow \Z_n\\
e^{j\frac{2k\pi}{n}}\mapsto \bar k
$$
易知 $\phi$ 是一一映射, 从而 $\phi$ 是群同构

## 同态的性质
设 $\phi$ 是群 $G$ 到群 $G'$ 的同态映射, $e$ 与 $e'$ 分别是 $G$ 与 $G'$ 的单位元, $a \in G$, 则
1. $\phi(e) = e'$;
2. $\phi(a^{−1}) = \left(\phi(a)\right)^{−1}$
3. $n\in \Z$, $\phi(a^n) = (\phi(a))^n$
4. 若 $\operatorname{ord} a <\infty$, 则 $\operatorname{ord} \phi(a) \mid \operatorname{ord} a$.
   - $\operatorname{ord} \phi(a) \leq \operatorname{ord} a$

设 $\phi$ 是群 $G$ 到 $G'$ 的同态映射, $H <G, K<G'$, 则
1. $\phi(H)$ 是 $G'$ 的子群
2. $\phi^{−1}(K)$ 是 $G$ 的子群
3. 如果 $H \triangleleft G$, 则 $\phi(H) \triangleleft \phi(G)$
4. 如果 $K \triangleleft G'$, 则 $\phi^{−1}(K) \triangleleft G$ 
   
其中，象集定义为 $\phi(H) \triangleq \{\phi(x) \mid x\in H\}$, 原象集定义为 $\phi^{-1}(K) \triangleq \{x\in H \mid \phi(x) \in K\}$. 

### 同态的核
设 $\phi$ 是群 $G$ 到 $G'$ 的同态映射, $e'$ 是 $G'$ 的单位元, 则称 $e'$ 在 $G$中的原象 $\phi^{−1} (\{e'\}) = \{a \in G \mid \phi(a) = e'\}$ 为同态映射 $\phi$ 的*核*. 记作 $\ker \phi$.
- 由于 $\{e'\} \triangleleft G'$, 则 $\ker \phi \triangleleft G$.

## 同构的性质
设 $\phi$ 是 $G \rightarrow G'$ 的同构映射，则
1. $\phi$ 是可逆映射, 且 $\phi$ 的逆映射 $\phi^{−1}$ 是群 $G'$ 到群 $G$ 的同构映射
2. $G$ 与 $G'$ 同时是交换群或都不是交换群
3. 同构是等价关系

## 群同态基本定理
设 $\phi$ 是群 $G$ 到群 $\phi(G)$ 的满同态, $K = \ker\phi$, 则 $G/K \cong \phi(G)$

### 证
$$
\varphi: G/K \rightarrow \phi(G)\\
aK \mapsto \phi(a)
$$
1. 映射合法性：与代表元 $a,b\in aK$ 无关.
2. 满射： $\forall \phi(a)\in \phi(G)$, $\varphi(aK)= \phi(a)$
3. 单射：$\phi(a) = \phi(b) \Rightarrow \phi(a^{-1}b) = e'\Rightarrow a^{-1}b\in K, aK = bK$
4. $\varphi(aK \cdot bK) = \varphi(abK) = \phi(ab) = \varphi(aK)\varphi(bK)$

# 循环群
设 $G$ 是群, 如果 $\exists a \in G$, 使 $G =\langle a \rangle$, 则称 $G$ 是一个*循环群*, 并称 $a$ 是 $G$ 的一个*生成元*. 
- $\langle a \rangle = \{\ldots,a^{-1}, a^0, a^{+1},\ldots\}$

当 $|G| = \infty$ 时, 称 $G$ 为无限循环群; 当 $|G| = n <\infty$ 时, 称 $G$ 为 $n$ 阶循环群
- 对有限循环群 $G = \langle a \rangle$, $\operatorname{ord} a = |G|$
- 无限循环群 $G = \langle a\rangle$, $\operatorname{ord} a = \infty$, 且有 $a^k = a^l \Rightarrow a^{k-l} = e \Rightarrow k=l$

### 例
- 整数加群 $\Z = \langle \pm 1\rangle$
- 剩余类加法群 $\Z_m = \langle \bar 1 \rangle$
- 模素数剩余类乘法群 $\Z_p^*$ 是 p-1 阶循环群
- $n$ 次单位根群 $U_n = \{ \omega^0, \ldots, \omega^{n-1}\}$
  - 对于满足 $(k,n) =1$ 的 $k$, $\operatorname{ord} \omega^k = n$, $U_n = \langle \omega^k \rangle$

## 循环群的生成元
设 $G = \langle a \rangle$ 为循环群, 则
1. 如果 $|G|= \infty$, 则 $a$ 与 $a^{−1}$ 是 $G$ 的两个仅有的生成元
2. 如果 $|G|= n$, 则 $G$ 恰有 $\varphi(n)$ 个生成元, 且 $\forall r,a^r$ 是 $G$ 的生成元的充分必要条件是 $(n,r) = 1$, 其中 $\varphi(n)$ 是欧拉函数
   - $a \in \langle a^r \rangle$
   - $\operatorname{ord} a^r = \frac{n}{(n,r)} = n$

## 循环群的子群
循环群的任一子群也是循环群.

### 证
设 $G = \langle a \rangle$, $H <G$.

$H=\{e\}$ 是显然的，对于 $H \neq \{e\}$, 一定存在 $l>0$, 使 $a^l\in H$. 

设 $r$ 是满足 $a^r\in H$ 的最小正数，则 $\forall a^k \in H$, $k\geq r$, 做带余除法有 $k = mr + \lambda$. 那么 $a^{\lambda} = a^{k-mr} \in H$, 则 $\lambda = 0$.

### 推论 
设 $G= \langle a\rangle$, $\operatorname{ord}a = n$. 则对于 $r: (n,r) = d$, 有 $\langle a^r\rangle= \langle a^d\rangle$
- $\langle a^r \rangle = \langle a^d\rangle$ iff $(\operatorname{ord} a, r) = (\operatorname{ord} a, d)$
- 即最大公因数相同对应同一循环群

### 证明
`对于循环群的包含关系，只需证 A 的生成元属于 B`
1. 因为 $(n,r) = d$, $un+vr = d$, $a^d = e a^{vr} = {a^r}^v\in \langle a^r\rangle$
2. $d|r$, $a^r \in \langle a^d\rangle$

### 循环群子群的通式
设 $G = \langle a\rangle$ 为循环群,
1. 如果 $|G|= \infty$, 则 $G$ 的全部子群为
   $$ \{\langle a^d\rangle \mid d =0,1,2,\ldots\}$$
2. 如果 $|G|= n$, 则 $G$ 的全部子群为
   $$ \{\langle a^d\rangle \mid \text{$d$ 为 $n$ 的正因子}\}$$

证明：
- 如果 $|G|= \infty$, 因为 $\forall r_1 > r_2 > 0$, 有 $r_1 \nmid r_2$, 所以
$a^{r_2} \notin \langle a^{r_1}\rangle$, 于是 $\langle a^{r_1}\rangle \neq \langle a^{r_2}\rangle$. 

  另一方面, 对 $\forall r > 0$, 显然 $a^r \notin \langle a^0 \rangle = \langle e \rangle$, 所以 $\langle a^r \rangle \neq \langle e \rangle$. 
  
  由此得 $G$ 的全部子群为 $\{\langle a^d\rangle \mid d = 0,1,2,\ldots\}$
-  如果 $|G|= n< \infty$, 则 $\forall r>0$, 存在 $n$ 的正因子 $d = (n,r)$, 使 $\langle a^r\rangle  = \langle a^d\rangle$. 
  
   又因为对于两个不同因子 $d_1 > d_2$, $d_1 \nmid d_2$, 于是 $a^{d_2} \notin \langle a^{d_1}\rangle$. 从而 $\langle a^{d_1} \rangle \neq \langle a^{d_2}\rangle$

   另一方面, 对 $n$ 的任一正因子 $d < n$, 显然 $ad \neq e$, 所以有 $\langle a^d\rangle \neq \langle e\rangle$. 而 $\langle e \rangle = \langle a^n\rangle$, QED

## 循环群的结构定理
设 $G$ 为循环群.
1. 如果 $G = \langle a\rangle$ 是无限循环群, 则 $G \cong (\Z,+)$
2. 如果 $G = \langle a \rangle$ 是 $n$ 阶循环群, 则 $G \cong (\Z_n,+)$

### 证
令 
$$ \phi : \Z \rightarrow G\\
k \mapsto a^k, ∀k \in \Z 
$$
(i) 显然φ 是 Z 到 G 的映射;

# 置换群
设 $A$ 为非空集合, 记 $A$ 的所有置换构成的集合为 $S(A)$, 则 $S(A)$ 在映射的复合运算下是群, 我们称 $S(A)$ 为 $A$ 的*对称群*

对称群 $S(A)$ 的任一子群称为*置换群*. 

如果 $|A| = n <\infty$, 则将 $A$ 表示为 $[n] = \{1,2,\ldots,n\}$, 并且将 $A$ 的一个置换 $\sigma \in S(A)$ 记为 $\sigma = \begin{pmatrix}
1 & 2 & \cdots & n\\
\sigma(1) & \sigma(2) & \cdots & \sigma(n)   
\end{pmatrix}$.

此时将 $S(A)$ 记为 $S_n$, 并称为 $n$ 次对称群.

- 右结合
- $n$ 次对称群 $S_n$ 的阶是 $n!$

### 置换的阶
对于 $\sigma \in S_n$, 将 $\sigma$ 分解为不相交轮换的乘积 $\sigma = \sigma_1\sigma_2 \cdots \sigma_s$, 则 $\operatorname{ord} \sigma = [r_1,r_2,\cdots ,r_s]$
### 凯莱定理
每个群都同构于一个置换群

构造置换群 $G_l = \{\phi_a \forall a\in G \mid \phi_a(x) = ax  \}$

## 轮换与对换
设 $\sigma$ 是一个 $n$ 阶置换. 如果存在 $1$ 到 $n$ 中的 $r$ 个不同的数 $i_1, i_2,\ldots, i_r$, 使 $\sigma (i_1) = i_2, \sigma(i_2) = i_3,\ldots, \sigma(i_{r−1}) = i_r, \sigma(i_r) = i_1$, 并且 $\sigma$ 保持*其余元素不变*, 则称 $\sigma$ 是一个长度为 $r$ 的*轮换*, 简称 $r$ 轮换, 记作 $\sigma = (i_1i_2\cdots i_r)$. 

其中集合 $\{i_1,i_2,\ldots ,i_r\}$ 记为 $I(\sigma)$. 特别地, $2$ 轮换称为*对换*
- $1$ 轮换省略不写，$(1)$ 表示恒等置换
- 一个置换`分解得到的`两个轮换要么不相交要么相同
- 对于轮换，$\operatorname{ord} \sigma = r$

### 轮换的交换律
任意两个不相交的轮换是可以交换的

- 设 $\sigma = (i_1i_2\cdots i_r)$, $\tau = (j_1j_2\cdots j_s)$ 是两个不相交的轮换.
- $a\in [n]$ 要么不在两个置换里，要么 $a$ 只在一个里。

### 置换的分解
每一个置换可表为一些不相交轮换的乘积.

- 设置换 $\sigma \in S_n$. $\forall i_1 \in [n]$, 找到包含 $i_1$ 的轮换 $\sigma_1 = (i_1i_2 \cdots i_r)$. 另取 $j_1 \in [n]\backslash I(\sigma_1)$, 找到轮换 $\sigma_2 = (j_1j_2\cdots j_s)$. 
- 显然 $I(\sigma_1) \cap I(\sigma_2) = \emptyset$. 由于 $n<\infty$, 以此类推可得一系列不相交轮换 $\sigma_1,\sigma_2,\cdots,\sigma_t$, 并且 $\bigcup_{k=1}^t I(\sigma_k) = [n]$. 
- 又可验证 $\sigma = \sigma_1 \circ \sigma_2 \circ \cdots \circ \sigma_t$. QED

### 置换分解的唯一性
将一个置换 $\sigma \in S_n$ 分解为对换的乘积, 所用对换个数的奇偶性是唯一的

- 按对换个数的奇偶性分为奇置换和偶置换
- 设 $G$ 是置换群. 若 $G$ 中存在奇置换, 则 $G$ 中奇置换的个数与偶置换的个数相同
- 设 $G$ 是置换群. 则 $G$ 中所有偶置换的集合 $H$ 是 $G$ 的子群
- 由 $S_n$ 的全体偶置换所构成的子群称为 $n$ 次*交错群*, 记作 $A_n$

# 直积
设 $G_1,G_2,\ldots ,G_n$ 是有限多个群. 构造集合 $G_1, G_2,\ldots ,G_n$ 的
笛卡尔积
$$ G = \{(g_1,g_2,\cdots ,g_n) \mid a_i \in G_i\}$$
并在 $G$ 中定义运算 $(g_1,g_2,\ldots,g_n)\cdot (g_1',g_2',\ldots ,g_n') = (g_1g_1',g_2g_2',\ldots, g_ng_n')$. 则 $G$ 关于上述运算构成群, 称为群 $G_1,G_2,\ldots ,G_n$ 的外直积，记作 $G = G_1\times G_2 \times \cdots \times G_n$.

- $e = (e_1,\ldots,e_n)$, $g^{-1} = (g_1^{-1}, \ldots, g_n^{-1})$
- $|G| = \prod |G_i|$
- 若都是加群，$G = G_1 \otimes G_2 \otimes \cdots \otimes G_n$
- $G$ 是交换群的充分必要条件是 $G_1,G_2,\ldots ,G_n$ 都是交换群
- $\forall \sigma \in S_n$, 有 $G_1 \times G_2 \times \cdots \times G_n \cong G_{\sigma(1)} \times G_{\sigma(2)} \times \cdots \times G_{\sigma(n)}$.

### 性质
设 $G = G_1 \times G_2 \times \cdots \times G_n$ 是群 $G_1,G_2,\cdots ,G_n$ 的外直积, $e_1,e_2,\cdots ,e_n$ 分别是群 $G_1,G_2,\cdots, G_n$ 的单位元, 则
1. $H_i \triangleq \{e_1\}\times \cdots \times \{e_{i−1}\}\times G_i \times \{e_{i+1}\}\times \cdots \times \{e_n\}$, 则 $H_i\triangleleft G$, 且 $H_i \cong G_i$
   - $gH_i = H_i$
2. $G = H_1H_2 \cdots H_n$
   - $g= (g_1,\ldots,g_n) = \prod (e_1,\ldots, g_i, \ldots, e_n)$
3. 如果 $h_1h_2 \cdots h_n = h_1'h_2' \cdots h_n′$, 其中 $h_i,h_i'\in H_i$, 则 $h_i = h_i'$.

### 阶
设 $G_1,G_2$ 是两个群, $a \in G_1, b\in G_2$, $\operatorname{ord} a <\infty, \operatorname{ord} b < \infty$. 则 $\forall (a,b) \in G_1 \times G_2$, 有 $\operatorname{ord}(a,b) = [\operatorname{ord} a,\operatorname{ord} b]$

设$G_1$ 和 $G_2$ 分别是 $m$ 阶及 $n$ 阶的循环群，则 $G_1 \times G_2$ 是循环
群的充要条件是 $(m,n) = 1$.

## 内直积
设 $H_1,\cdots ,H_n$ 是 $G$ 的子群. 如果群 $G$ 满足下述三个条件
1. $H_1,\cdots ,H_n$ 都是 G 的正规子群;
2. $G = H_1H_2 \cdots H_n$;
3. 唯一： $\forall h_1 \cdots h_n = h_1' \cdots h_n'$, 其中 $h_i,h_i' \in H_i$, 则 $h_i = h_i'$;
   - 线性空间的唯一表示
      
称 $G$ 是 $H_1,\cdots ,H_n$ 的内直积

### 不同 $H$ 之间的交换律
如果群 $G$ 是有限多个正规子群 $H_1,H_2,\cdots ,H_n$ 的内直积, 则 $\forall a\in H_i,b \in H_j, i\neq j$, 有 $ab = ba$.
- $ab = b'a \in aH_j = H_j a$, $ab = ba' \in H_i b = bH_i$. $ab' = a'b$
- Corollary: $hh' = h_1h'_1h_2h'_2 \cdots h_nh_n'$

## 内外直积的关系
如果群 $G$ 是有限个正规子群 $H_1,H_2,\cdots ,H_n$ 的内直积, 则 $G \cong H_1 \times H_2 \times \cdots\times H_n$ 
- $(h_1,\ldots, h_n)\mapsto h_1\ldots h_n$

## 判定
设 $G = H_1H_2 ···H_n$, 其中每个 $H_i$ 都是 $G$ 的正规子群, 则下述三条等价:
1. $G$ 是 $H_1,H_2,··· ,H_n$ 的内直积;
2. $\forall i,H_1 ···H_{i−1} \cap H_i = \{e\}$
3. $\forall i,H_1 ···H_{i−1}H_{i+1} ···H_n \cap H_i = \{e\}$