# 环
设 $R$ 是一个非空集合. 如果在 $R$ 上定义了两个代数运算 "$+$" 和 "$\cdot$", 并且满足
1. $R$ 关于加法为阿贝尔群;
2. $R$ 关于乘法满足结合律, 即 $\forall a,b,c \in R$, 有 $(a \cdot b) \cdot c = a \cdot (b \cdot c)$;
3. $R$ 关于加法和乘法满足分配律, 即 $a \cdot (b + c) = a \cdot b + a \cdot c$ 和 $(b + c) \cdot a = b \cdot a + c \cdot a$,
 
则称 $(R,+,\cdot)$ 为一个*环*, 或简称 $R$ 为环

注：
- 对乘法不构成群，但封闭
- 单位元分别为 $0$, $e$. $e$ 不一定存在

+ $\Z, \mathbb{Q}, \Reals, \Complex$ 对于数的加法与乘法构成有单位元 $1$ 的交换环, 分别称为整数环、有理数域、实数域、复数域.
+ eg. 矩阵

### 性质
设 $R$ 是一个环, 则
1. $\forall a \in R$, $−(−a) = a$, $a \cdot 0 = 0 \cdot a = 0$
2. $(−a) \cdot b = a \cdot (−b) = −a \cdot b$, $(−a) \cdot (−b) = a \cdot b$
3. $\forall n \in \Z, a,b \in R$, $(na) \cdot b = a \cdot (nb) = n(a \cdot b)$
4. $\sum a_i \cdot \sum b_j = \sum\sum a_i \cdot b_j$
5. 若 $e = 0$, 则 $R = \{0\}$

### 零因子和单位
设环 $R$ 有单位元 $e$, 则
1. 若两个元素 $a,b \in R \backslash\{0\}$, 满足 $a \cdot b = 0$, 则称 $a$ 为  $R$ 的一个*左零因子*, $b$ 为 $R$ 的一个右零因子. 统称为零因子.
2. $\forall a \in R$, 如果 $\exists b \in R$ 使得 $ab = ba = e$ 则称 $a$ 是 $R$ 的一个单位, 并称 $b$ 为 $a$ 的逆元. $a$ 的逆元唯一, 记为 $a^{−1}$.
3. 环 $R$ 为无零因子环当且仅当 $\forall a,b \in R$, 若 $a \cdot b = 0$ 则必有 $a = 0$ 或 $b = 0$
4. 单位的集合构成一个群, 称为 $R$ 的单位群, 记为 $U(R)$

- 在一个无零因子的环中, 两个*消去律*成立, 即 $\forall a,b,c \in \Reals, c \neq 0$, 如果 $ac = bc$ 或 $ca = cb$, 则 $a = b$
- 如果环 $R$ 中两个消去律有一个成立, 则 $R$ 必是无零因子环, 从而另一个消去律也成立.
- 如果关于乘法每个非零元可逆，则无零因子

## 域
设 $R$ 是一个有单位元 $e \neq 0$ 的环, 则
1. 若 $R$ 是无零因子的交换环, 则 $R$ 称为整环;
2. 若 $R$ 中每个非零元都可逆, 则称 $R$ 是一个除环. 
3. 非交换的除环称为体;
4. 若 $R$ 是一交换的除环, 则称 $R$ 是一个域


| 交换  | 无零因子 | 非零元可逆 |       |
| :---: | :------: | :--------: | :---: |
|   Y   |    Y     |     -      | 整环  |
|   Y   |    -     |     Y      | 除环  |
|   X   |    -     |     Y      |  体   |
|   Y   |    -     |     Y      |  域   |

- 有限整环必定是域  
- 若环 $(F,+,\cdot)$ 为域, 则 $F \backslash \{0\}$ 关于乘法亦是一个阿贝尔群, 称为域的乘法群, 记为 $(F,\cdot)$
- 若 $|(F,+,\cdot)|<\infty$, 则称 $F$ 为有限域.

## 子环
设 $(R,+,\cdot)$ 是一个环,$S$ 是 $R$ 的一个非空子集. 如果 $S$ 关于 $R$ 的运算构成环, 则称 $S$ 为 $R$ 的一个子环, 记作 $S < R$.
- $\{0\}, R$ 是平凡子环.

### 子环的判定
设 $R$ 是一个环, $S$ 是 $R$ 的一个非空子集, 则 $S < R$ 的充分必要条件是
1. $\forall a,b \in S, a - b \in S$;
2. $\forall a,b \in S, ab \in S$.


# 理想
设 $R$ 为环，$I \subset R, |I|>0$. 若
1. $\forall r_1, r_2 \in I$, $r_1 - r_2\in I$
2. $\forall r\in I, s\in R$, $rs,sr\in I$

则称 $I$ 为环 $R$ 的一个理想，记作 $I \triangleleft R$. 若 $I \subsetneq R$, 则称为真理想

- 理想必是子环
- 若 $I\triangleleft R, e\in I$, 则 $I = R$
- 若 $R$ 为域，则 $R$ 只有平凡理想 $\{0\}, R$.

### 理想的和与交
设 $I,J \triangleleft R$, $I+J, I \cap J$ 均为 $R$ 的理想

对比: 
- 子群 $H, K$ 的交 $H \cap K$ 是子群，他们的并不一定是子群; 子群的积 $HK < G$ iff $HK = KH$
- 正规子群 $H, K \triangleleft G$, 则 $H \cap K,HK \triangleleft G$

## 主理想 
设 $R$ 是环，$\forall a\in R$, 令 $\Sigma = \{I\triangleleft R \mid a\in I\}$,
$$ \langle a \rangle = \bigcap_{I\in \Sigma} I
$$
则 $\langle a \rangle$ 为 $R$ 的由 $a$ 生成的主理想

进一步，
$$
\langle a \rangle  = \{\sum_{i=1}^n x_i a y_i + xa + ay + ma \mid x_i,y_i,x,y\in R, n\in \N,m\in \Z\}
$$
- 若 $R$ 有单位元, $\langle a \rangle = \{\sum x_i a y_i \mid x_i,y_i \in R, n \in \Z\}$
- 若 $R$ 是交换环，$\langle a \rangle = \{xa+ma \mid x\in R, m\in Z\}$
- 若 $R$ 有单位元的交换环，则 $\langle a \rangle  = aR = \{ar \mid r\in R\}$

## 生成理想
设 $R$ 为环，$a_1,\ldots,a_s \in R$. 则 $\langle a_1\rangle, \ldots,\langle a_s\rangle$ 都是 $R$ 的理想。

令 $\langle a_1,\ldots a_s \rangle = \langle a_1\rangle + \cdots +\langle a_s\rangle$, 则 $\langle a_1,\ldots, a_s\rangle$ 是 $R$ 的由 $a_1,\ldots, a_2$ 生成的最小理想


# 商群
> $$ I(H + K) = \{i(h+k) \mid \forall i,h,k\} = \{ih+ ik \mid \forall i,h,k\} \subset IH + IK $$
> $$ I(h+K) = \{ih + ik \mid \forall i,k\} \neq Ih + IK$$
> 但对于理想
> $$ \bar x\bar y = \{xy + xi_1 + yi_2 + i_1i_2 \mid \forall i_1,i_2\} = xy + I + \{xi+yi\} = \overline{xy}$$

设 $R$ 是一个环, $I$ 是环 $R$ 的一个理想, 从而 $(I,+)$ 是 $(R,+)$ 的正规子群, 于是有商群:
$$
R/I = \{\bar x = x+I \mid x \in \R \}
$$
其加法运算定义为
$$
\bar x + \bar y = \overline{x + y}
$$
在 $R/I$ 规定乘法运算为
$$ \bar x \cdot \bar y = \overline{xy}$$

- $R/I$ 构成环，称为商环
- $\bar 0$ 是 $R/I$ 的零元
- 若 $R$ 有单位元 $e\in I$，则 $\bar e$ 是单位元
- 若 $R$ 是交换环则 $R/I$ 也是交换环

# 同态
设 $R$ 和 $R'$ 为两个环, $\phi: R\rightarrow R'$. 

如果 $\forall a,b \in R$, 有
1. $\phi(a + b) = \phi(a) + \phi(b)$;
2. $\phi(ab) = \phi(a)\phi(b)$
   
则称 $\phi$ 为环 $R$ 到环 $R'$ 的一个同态映射, 简称同态.

当同态映射 $\phi$ 作为集合映射是一一映射时, 称$\phi$ 为 $R$ 到 $R'$ 的同构, 记作 $\phi : R\cong R'$
- $\phi(0) = 0'$
- $\phi(na) = n\phi(a)$
- $\phi(a^n) = \phi(a)^n$

当 $R, R'$ 都是含幺环时
1. 如果 $\phi$ 是满同态, 则 $\phi(e) = e'$
2. 如果 $R'$ 无零因子, 且 $\phi(e) \neq 0$, 则 $\phi(e) = e'$
3. 如果 $\phi(e) = e'$, 则对 $R$ 的任一单位 $u$, $\phi(u)$ 是 $R'$ 的单位, 且 $[\phi(u)]^{−1} = \phi(u^{−1})$.

### 同态的核
设同态映射 $\phi: R \rightarrow R'$. 环同态的核定义为
$$ K = \{a \in R \mid \phi(a) = 0\}$$
记作 $\ker \phi$.
- 是 $0$，不是 $e$
- $\ker \phi \triangleleft R$

## 环同态基本定理
$$R/\ker \phi \cong \phi(R)$$
其中，$\tilde{\phi}: \bar a \mapsto \phi(a)$

## 环的第二同构定理
给定环 $R$, 设子环 $S < R$, 正规子环 $I \triangleleft R$, 则 
$$S \cap I \triangleleft S,$$ 
且
$$ S/(S \cap I) \cong (S + I)/I$$

## 环的扩张定理
设 $\bar S$ 与 $R$ 是两个没有公共元素的环, $\bar \phi: \bar S \rightarrow R$ 是 $\bar S$ 到 $R$ 的单同态, 

则存在一个与 $R$ 同构的环 $S$, 及由环 $S$ 到 $R$ 的同构映射 $\phi: S\rightarrow R$, 使 $\bar S$ 为 $S$ 的子环且 $\phi|_{\bar S} = \bar\phi$.

$$ S = \bar S \cap (R \backslash \phi(S))$$
$$\phi: \begin{array}{ll}
    \bar \phi(x) & x \in \bar S\\
    x & x \notin \bar S
\end{array}$$

对任意 $x,y \in S$, 
$$\begin{gather*}
    x + y = \phi^{-1}[\phi(x) + \phi(y)] \in S\\
    xy = \phi^{-1}[\phi(x)\phi(y)] \in S
\end{gather*}$$

则 $(S, +, \cdot)$ 关于全新定义的 $+,\cdot$ 构成环。易知 $\phi$ 是同构，且 $\phi, +, \cdot$ 在 $\bar S$ 上不变

# 环的直积
### 环的外直积
设 $R_1, R_2, \ldots, R_n$ 为 $n$ 个环, 构造笛卡尔积
$$R = \prod_i^n R_i = R_1 \times \cdots \times R_n = 
\{(a_1, a_2, \ldots ,a_n) \mid a_i \in R_i, i \in [n]\}$$

规定运算 
- $(a_1, a_2, \ldots, a_n) + (b_1, b_2, \ldots ,b_n) = (a_1 + b_1, a_2 + b_2, \ldots, a_n + b_n)$
- $(a_1,a_2,\ldots ,a_n) \cdot (b_1,b_2, \ldots ,b_n) = (a_1b_1,a_2b_2,\ldots ,a_nb_n)$
  
则 $R$ 关于上述加法与乘法构成一个环

- $(0_1, 0_2, \ldots, 0_n)$, $(e_1, e_2, \ldots, e_n)$ 
- $R$ 是交换环 iff $R_i$ 都可交换 (乘法)

### 外直积的性质
设 $R = \prod_i^n R_i$ 是环的外直积, $0_1, 0_2, \ldots, 0_n$ 分别是环的零元, 则
1. $H_i \triangleq \{0_1\}\times \cdots \times \{0_{i−1}\}\times R_i \times \{0_{i+1}\}\times \cdots \times \{0_n\}$, 则 $H_i\triangleleft R$, 且 $H_i \cong R_i$
   - $r_i + R_i = R_i$
2. $R = \sum H_i$
3. 环 R 中的任意元素都能用 $H_1', H_2', \ldots, H_n'$ 的元素之和唯一表示, 即
   
   如果 $a_1 + \ldots + a_n$ = $b_1 + \ldots + b_n$, 则 $a_k = b_k, \forall k ∈ [n]$.

### 环的内直积
如果环 $R$ 的理想 $R_1, \ldots , R_n \triangleleft R$ 满足下述两个条件:
1. $R = \sum R_i$
2. $\forall r \in R$, $r$ 能被 $R_1, \ldots, R_n$ 的元素之和唯一地表示, 
   
则称 $R = \sum_{i=1}^n R_i$ 是 $R_1, \ldots , R_n$ 的*内直积*

### 内直积的判定
下述语句等价
- $R$ 是 $R_1, R_2, \ldots, R_n$ 的内直积
- $R_1 + R_2 +\ldots R_{i-1} \cap R_i = \{0\}$
- $[R_1 + R_2 +\ldots R_{i-1} + R_{i+1} + \ldots + R_n] \cap R_i = \{0\}$

### 内外直积的关系
理想 $R_1, \ldots , R_n \triangleleft R$, 则内外直积同构
$$ \sum_{i=1}^n  R_i \cong \prod_{i=1}^n R_i$$

$$\begin{aligned}
    \phi:  \quad \sum R_i &\rightarrow \prod R_i\\
    \sum r_i &\mapsto (r_1, \ldots, r_n)
\end{aligned}$$

- $\phi(\sum a_i + \sum b_i) = \phi(\sum a) + \phi(\sum b)$
- $\phi((\sum a_i)(\sum b_i)) = \phi(\sum a_ib_i) = \phi(\sum a_i)\phi(\sum b_i)$
  - 对于 $a\in R_i, b\in R_j$, 由理想性质，$ab \in R_i \cap R_j = \{0\}$

# 中国剩余定理
### 理想的互素
$I, J$ 互素 iff $I_i + I_j = R$
- 整数中，若 $(d_1,d_2) =1$, 则 $rd_1 + sd_2 = 1$. $1\in \langle d_1\rangle + \langle d_2 \rangle = \Z$
  
### 中国剩余定理
设 $R$ 是含幺环, $I_1, \ldots, I_n$ 为环 $R$ 的理想，并且对于 $\forall 1\leq i < j \leq n$, $I_j$ 和 $I_j$ 互素，则有环同构
$$R/ (I_1 \cap \cdots \cap  I_n) \cong R/I_1 \times R/I_2 \times \cdots \times R/I_n$$

- 引理
  
  $$I_i + \prod_{j\neq i} I_j  = R$$
  + $R = RR = (I_1+ I_2)(I_1 + I_3) = I_1^2 + I_1I_3 + I_2I_1 + I_2I_3 \subset I_1 + I_2I_3 \subset R$
  + $I_1 + I_2I_3 = R$, 递推 $I_1 + \prod_{j>1} I_j = R$
- 构造同态
  $$\phi: \begin{array}{l}
    R \rightarrow R/I_1 \times R/I_2 \times \cdots \times R/I_n\\
    r \mapsto (r + I_1, r + I_2, \ldots, r + I_n)
  \end{array}$$
- 同态基本定理， $\ker \phi  = \bigcap I_i$
- 满射:
  - $e\in R = I_i + \prod_{j\neq i} I_j$, $\exist a_i + b_i = e$.
  - $b_i \in e + I_i$, $b_i \in \prod_{j\neq i} I_j \subset \bigcap I_j$.
  - $\phi(b_i) = (\bar 0, \ldots, \bar e, \ldots, \bar 0)$
  - $\sum_i \phi(r_ib_i)$ 满射



# 素理想和极大理想
## 素理想
设 $R$ 是一个交换环, $P$ 是 $R$ 的*真理想*. $P\subsetneq R$

如果 $\forall a,b \in R$, $ab \in P \Longrightarrow a \in P$ 或 $b \in P$ , 则称 $P$ 为 $R$ 的一个*素理想*

例
- 设 $n\in \N$ 则 $\langle n\rangle$ 为 $\Z$ 的素理想的充分必要条件是 $n$ 为素数
- 在 $Z_2[x]$ 中, 由于 $x + 1 \notin \langle x^2 + 1\rangle$, 而
$(x + 1)^2 = x^2 + 1 \in \langle x^2 + 1\rangle$, 所以 $\langle x^2 + 1\rangle$ 不是 $Z_2[x]$ 的素理想.

设 $R$ 是有单位元 $e\neq 0$ 的交换环, $I \triangleleft R$, 则 $I$ 是 $R$ 的素理想的充分必要条件是 $R/I$ 是整环.
- $\Rightarrow$: 
  - $R/I \neq \{0\}$, $R/I$ 有单位元且可交换.
  - $\forall \bar a \bar b = \bar 0 \Rightarrow ab\in I \Rightarrow a\in I\ or\ b\in I \Rightarrow \bar a = \bar 0\ or \bar b = \bar 0$
- $\Leftarrow$:
  - $R/I\neq \{\bar 0\}$, 可交换
  - $ab\in I$, $\bar a \bar b = \bar 0$, $a \in I$

## 极大理想
设 $R$ 是一个交换环, $M$ 是 $R$ 的真理想. $M\subsetneq R$

如果对 $R$ 的任一包含 $M$ 的理想 $N$ , 必有 $N = M$ 或 $N = R$, 则称 $M$ 为 $R$ 的一个*极大理想* （不存在 $M\subsetneq N \subsetneq R$）

- 只需证 $M\subsetneq N \subseteq R \Rightarrow N = R$
  - 若存在 $e$, $e\in R$
- 极大理想的利用：<br>
  设 $R$ 是一个有单位元 $e$ 的交换环, $I$ 是其一个极大理想. 
  
  $\forall a\in R\backslash I$, $\langle a\rangle$ 是 $R$ 的主理想，从而 $\langle a\rangle + I$  是 $R$ 的一个真包含 $I$ 的理想，于是 $R = \langle a \rangle + I$

### 极大理想与域
设 $R$ 是有单位元 $e$ 的交换环, $I\triangleleft R$, 则 $I$ 是 $R$ 的极大理想的充分必要条件是 $R/I$ 是域
- $\Rightarrow$:
  - $R/I \neq \{\bar 0\}$ 为含幺交换环
  - $\forall \bar a \in R/I \mid \bar a \neq \bar 0$, $a\notin I$. 由极大理想 $R = I + \langle a \rangle$
  - $e\in R = I+ \langle a \rangle$, $\exist i,j$ s.t. $e = i + aj$
  - $\bar e = \overline{i + aj} = \bar i + \bar a\bar j = \bar a \bar j$, $\bar a$ 可逆
- $\Leftarrow$:
  - $\forall J \mid I \subsetneq J \subseteq R$, $\forall a\in J, a\notin I$, $\bar a  \neq \bar 0$
  - 由于 $R/I$ 是域，存在 $\bar b \in R/I$, $\bar a\bar b = \bar e$, $e\in ab + I\subset J$. $e\in J = R$

推论：

设 $R$ 是一个有单位元的交换环, 则 $R$ 的每个极大理想都是素理想

# 环的特征与素域
设 $R$ 为环. 如果存在最小的正整数 $n\in \Z^+$, 使得对于所有 $R$ 中元素满足
$$\forall a \in R, na = 0$$
则称 $n$ 为环 $R$ 的特征 (characteristic). 记作 $\operatorname{Char} R= n$

如果这样的正整数不存在, 则称环 $R$ 的特征为 $0$. 
- 若 $e\in R$, $\operatorname{Char} R = \operatorname{ord} e <\infty ? \operatorname{ord} e: 0$

### 定理
- 整环的特征是 $0$ 或 素数
  - $0 = ne = (pe)(qe)$, 无零因子 $\Rightarrow \operatorname{Char}R \neq n$
- 特征是素数 $p$ 的交换环 $R$ 中有 
$$(a + b)^p = a^p + b^p$$
$$(a+b)^{p^k} = a^{p^k} + b^{p^k}$$

## 域的特征
域 $F$ 的特征 $\operatorname{Char} F$ 就是将 $F$ 看作环时的特征
- 域是整环，$\operatorname{Char} F$ 只能是 $0$ 或素数

## 素域
域 $F$ 如果不含任何*真子域*, 则称 $F$ 是一个*素域*
- $\Z_p$ 是素域
  - $1\in f \subset \Z_p$

## 整环的商域
> 对一般的环, 是否也可以把它扩充为一个更大的环, 使其上每个非零元都可逆?
> 
> 有零因子的环以及非交换环都无法达到目的.

设 $D$ 是整环, $1$ 是 $D$ 的单位元, 则由 $D$ 可以构造一个包含 $D$ 的域.

1. 做集合
   $$S = \{(a,b) \mid a,b \in D, b\neq 0\}$$
2. 做等价关系
   $$(a,b) \sim (c,d) \Leftrightarrow ad = bc$$
3. 记等价类
   $$\left[\frac{a}{b}\right] = \{(c,d)\in S| (c,d)\sim (a,b)\}$$
4. 令商域
   $$F = S/\sim = \{\left[ \frac{a}{b}\right] \mid a,b \in D,b\neq 0\}$$
5. 规定
   $$\left[\frac{a}{b}\right] + \left[\frac{c}{d}\right] 
   = \left[\frac{ad+ bc}{bd}\right]$$
   $$\left[\frac{a}{b}\right] \cdot\left[\frac{c}{d}\right]
   = \left[\frac{ac}{bd}\right]$$

- 分式
- 环的扩张 $\phi: D \rightarrow F$

