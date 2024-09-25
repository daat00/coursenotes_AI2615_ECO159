# 1. If $I, J$ are ideals of $R$, show that $IJ$ is an ideal of $R$.
$$
IJ =\{\sum_k i_kj_k \mid \forall k\in \Z^+; i_k,j_k \in R\}
$$
$\forall x = \sum_{k_x} i_{k_x}j_{kx}, y = \sum_{k_y} i_{k_y}j_{ky} \in IJ$, $r \in R$,
-  Trivially, $x - y = \sum_{k_x} i_{k_x}j_{kx} - \sum_{k_y} i_{k_y}j_{ky} \in IJ$ since it is a finite sum of product.
-  $rx = r\sum_{k_x} i_{k_x}j_{kx} = \sum_{k_x} ri_{k_x}j_{kx} \in IJ$ since $ri_{k_x} \in I$. similarly, $xr \in IJ$.

So $IJ$ is an ideal of $R$.

# 2. Let $I_1 = m\Z$, $I_2 = n\Z$ be two ideals of integer ring $Z$. Show that $I_1I_2 = mn\Z$, $I_1 + I_2 = \gcd(m, n)\Z$ and $I_1 \cap I_2 = \operatorname{lcm}(m, n)\Z$
### $I_1I_2 = mn\Z$
$I_1I_2 = \{\sum_i\sum_j mk_ink_j\mid \forall k_i,k_j \in \Z\} = mn\{\sum_{ij} k_ik_j\}$

Since $\{\sum_{ij} k_ik_j\} = \Z$, $I_1I_2 = mn\Z$.

### $I_1 + I_2 = \gcd(m, n)\Z$
By Bezout's identity, $\exists x, y \in \Z$, $mx + ny = \gcd(m, n)$ and $\gcd(m, n)  = \min^+\{mx + ny \mid x, y \in \Z\}$.

So 
1. $\gcd(m, n) \in I_1 + I_2$, $\gcd(m,n)\Z = \langle \gcd(m, n) \rangle \subseteq I_1 + I_2$.
2. $\forall mx + ny \in I_1 + I_2$, $\gcd(m, n) \mid mx + ny$, so $mx + ny \in \gcd(m, n)\Z$. And that $I_1 + I_2 \subseteq \gcd(m, n)\Z$.

Thus we have equality.

### $I_1 \cap I_2 = \operatorname{lcm}(m, n)\Z$
$I_1 \cap I_2 = \{x \mid  m\mid x \wedge n \mid x\} = \{x \mid \operatorname{lcm}(m,n) \mid x\} = \operatorname{lcm}(m,n) \Z$


# 3. Let $R$ be a ring. An element $a \in R$ is called a **nilpotent** if there exists a positive integer $m$ such that $a^m = 0$.
## (a) Show that if $R$ is a commutative ring and $a, b$ are *nilpotents*, then $a + b$ is also a nilpotent.

Suppose $a^m = b^n = 0$, then
$$(a+b)^{m+n} = \sum_{k=0}^{m+n} \binom{m+n}{k} a^kb^{m+n-k} = \sum_{k=0}^{m+n} \binom{m+n}{k} 0 = 0$$
Since for $k \geq m$, $a^k = 0$ and for $k < m$, $b^{m+n-k} = b^{n +(m-k)} = 0$.

## (b) Does (a) holds if $R$ is not a commutative ring? Justify your answer.
No. consider $a = \begin{pmatrix}0 & 1 \\ 0 & 0\end{pmatrix}$, $b = \begin{pmatrix}0 & 0 \\ 1 & 0\end{pmatrix}$, then $a^2 = b^2 = 0$, but $a+b = \begin{pmatrix}0 & 1 \\ 1 & 0\end{pmatrix}$ is not nilpotent with $(a+b)^2 = I$

## (c) Show that all *nilpotents* of a commutative ring form an ideal $N$ and the quotient ring $R/N$ has no *nilpotent* except $0$.
$N$ is an ideal since
- Additive subgroup: $\forall a, b \in N$, $-b \in N$, by (a), $a - b = a + (-b) \in N$.
- $\forall a\in N$, $r \in R$, $ra \in N$ since $a^m = 0$, $(ra)^m = r^ma^m = 0$. By commutativity, $ar \in N$.
  
Suppose $\exists x + N \in R/N$, $x \notin N$, $x^m + N = 0 + N$, then $x^m \in N$. So $x^m$ is a nilpotent, and $x$ is also a nilpotent. So $x \in N$, indicating a contradiction.

# 4. An element a of a ring $R$ is called **idempotent** if $a^2 = a$. An idempotent is called **central idempotent** if it belongs to the center of $R$. Assume that $R$ contains the identity $1$ and $e$ is a central idempotent.
## (a) Show that $1 −e$ is also a central idempotent.
$(1-e)^2 = 1 - 2e + e^2 = 1 - 2e + e = 1-e$, so $1-e$ is idempotent.

$\forall r\in R$, $r(1-e) = r - re = r - er = (1-e)r$, so $1-e \in C(R)$ is central idempotent.

## (b) Show that both $eR$ and $(1 −e)R$ are ideals of $R$, and furthermore $R \cong eR \times(1 −e)R$.
$eR$ is an ideal since
1. additive subgroup: $\forall er_1, er_2 \in eR$, $er_1 - er_2 = e(r_1 - r_2) \in eR$
2. $\forall er \in eR$, $r_0 \in R$, $err_0 = e(rr_0) \in eR$, $r_0er = er_0r \in eR$ (by central)

$(1-e)R$ is an ideal since both $R$, $eR$ are ideals.

Let $\phi(r) = (er, r - er)$, then $\phi$ is a ring homomorphism since
$$
\begin{aligned}
\phi(r_1 + r_2) &= (er_1 + er_2, r_1 - er_1 + r_2 - er_2)  = \phi (r_1) + \phi(r_2)\\
\phi(r_1r_2) &= (er_1r_2, (1-e)r_1r_2) \\
&= (e^2r_1r_2, (1-e)^2r_1r_2)\\
&= (er_1, (1-e)r_1)(er_2 - (1-e)r_2) = \phi(r_1)\phi(r_2)
\end{aligned}
$$

$\phi$ is isomorphic since 
- injective: $\ker \phi = \{r \mid er = r - er = 0\} = \{0\}$
- surjective: $\forall (er_1, (1-e)r_2) \in eR \times (1-e)R$, 
$$\begin{aligned}\phi(er_1 + (1-e)r_2) &= \left(e^2r_1 + e(1-e)r_2, (1-e)er_1 + (1-e)^2 r_2\right)\\ &= (er_1 + 0,0 + (1-e)r_2)\\ &= (er_1, (1-e)r_2)\end{aligned}$$

To sum up, $R \cong eR \times (1-e)R$.

# 5. Two central idempotents $e_1, e_2$ are called orthogonal if $e_1e_2 = e_2e_1 = 0$. Let $\{R_i\}_{i=1}^n$ be a set of rings with idenities and assume that $R$ has an identity. Show that the following conditions are equivalent:
## (a) $R \cong R_1 \times \cdots \times R_n$
## (b) There exists pairwise orthogonal idempotents $e_1, \cdots , e_n$ of $R$ such that $e_1 + \cdots + e_n = 1$ and $e_i R \cong R_i$.

(a) $\Rightarrow$ (b): 

Let $\phi: R \rightarrow R_1 \times \cdots \times R_n$ be an isomorphism, then consider $\phi(1) = (1, \cdots, 1)$. 

Let $e_i = \phi^{-1}(0, \cdots, 1, \cdots, 0)$, we have
- $e_i^2 = \phi^{-1}(\phi(e_i^2)) = \phi^{-1}([\phi(e_i)]^2) = \phi^{-1}(\phi(e_i)) = e_i$
- $e_i$ is central since $\phi(e_i)$ is commutative
- $e_ie_j|_{i\neq j} = \phi^{-1}(\phi(e_i)\phi(e_j)) = \phi^{-1}(0) = 0$

So we have the pairwise an orthogonal idempotents. Clearly, $e_1 + \cdots + e_n = \phi^{-1}(\phi(1)) = 1$.

For isomorphism, consider $\varphi_i: e_iR \rightarrow R_i$ defined as $\varphi_i(e_ir) = \phi(r)_i$. $\varphi$ is well-defined since $e_i r_1 = e_i r_2 \Rightarrow \phi(r_1)_i = \phi(e_ir_1) = \phi(e_i r_2) = \phi(r_2)_i$.

Trivially, $\varphi_i$ is a ring homomorphism since $\varphi_i(e_i r_1 + e_i r_2) = \phi(e_i r_1 + e_i r_2) = \phi(r_1) + \phi(r_2)$, and that $\varphi(e_i r_1 e_i r_2) = \varphi(e_ir_1r_2) = \phi(r_1r_2) = \phi(r_1)\phi(r_2)$.

$\varphi_i$ is injective since $\ker \varphi_i = \{e_ir \mid \phi(r) = 0\} = \{0\}$. It is surjective since $\phi$ is surjective.

So $e_iR \cong R_i$.

(b) $\Rightarrow$ (a):

Denote $\varphi_i: e_iR \rightarrow R_i$ as the isomorphism for $e_iR \cong R_i$. Then consider $\phi: R \rightarrow R_1 \times \cdots \times R_n$ defined as $\phi(r) = (\varphi_1(e_1r), \cdots, \varphi_n(e_nr))$.

$\phi$ is ring homomorphism since 
- $\phi(r_1 + r_2) = (\varphi_1(e_1r_1) + \varphi_1(e_1r_2), \cdots, \varphi_n(e_nr_1) + \varphi_n(e_nr_2)) = \phi(r_1) + \phi(r_2)$
- $\phi(r_1r_2) = (\varphi_1(e_1r_1e_1r_2), \cdots, \varphi_n(e_nr_1e_nr_2)) = \phi(r_1)\phi(r_2)$.

$\phi$ is injective since $\ker \phi = \{r \mid \varphi_i(e_ir) = 0, \forall i\} = \{r\mid r= \sum e_i r = \sum 0  = 0\} = \{0\}$. $\phi$ is surjective since $\forall (\varphi_1(e_1r_1), \cdots, \varphi_n(e_nr_n)) \in R_1 \times \cdots \times R_n$, $\phi(\sum_i e_i r_i) = (\varphi_1(e_1^2r_1 + 0), \cdots, \varphi_n(e_n^2r_n + 0)) = (\varphi_1(e_1r_1), \cdots, \varphi_n(e_nr_n))$.


# 6. Let $R$ be a commutative ring and $I$ be an ideal of $R$. Show that $\sqrt{I} = \{x \in R : \exists m \in Z_+ \text{ such that } x^m \in I\}$ is also an ideal of R.

- additive subgroup: $\forall x, y \in \sqrt{I}$, $\exists m, n \in \Z_+$, $x^m, y^n \in I$, then $(x- y)^{m+n} = \sum_{i} C_{m+n}^i x^i y^{m+n - i} \in I$ since for each term, either $x^i$ or $y^{m+n - i}\in I$. So $x-y \in \sqrt{I}$.
- $\forall x \in \sqrt{I}$, $r \in R$, $\exists m \in \Z_+$, $x^m \in I$, then $(rx)^m = r^mx^m \in I$. So $rx \in \sqrt{I}$. By commutativity, $xr \in \sqrt{I}$.

# 7. Let $I$ be an ideal of $R$. Show that the quotient ring $R/I$ is commutative if and only if $(rs −sr) ∈I$ for every $r, s ∈R$.

$\Rightarrow$:

Since $R/I$ is commutative, $\forall r,s \in R$, $(r+I)(s+I) = (s+I)(r+I)$, so $rs - sr + I = I, rs - sr \in I$.

$\Leftarrow$:

$\forall r, s \in R$, $(r+I)(s+I) = rs + I = (sr + rs - sr) + I= sr + I = (s+I)(r+I)$.

QED

# ∗8. If $p_1, \cdots, p_n$ are distinct odd primes, show that there are exactly $2^n$ solutions of $x^2 \equiv x \mod p_1 \cdots p_n$, where $0 ≤x < p_1 \cdots p_n$.

Observatoin: 

The solutions for $x^2 \equiv x \mod p_1 \cdots p_n$ is the same as those for the equation system 
$$
\begin{cases}
x^2 \equiv x \mod p_1\\
x^2 \equiv x \mod p_2\\
\cdots\\
x^2 \equiv x \mod p_n
\end{cases}
$$
Proof: 
- $x^2 \equiv x \mod p_1 \cdots p_n \Rightarrow x^2 \equiv x \mod p_i, \forall i$ since $p_i \mid p_1 \cdots p_n$. 
- $x^2 \equiv x \mod p_i, \forall i \Rightarrow p_i \mid x^2 -x \Rightarrow p_1 \cdots p_n \mid x^2 - x \Rightarrow x^2 \equiv x \mod p_1 \cdots p_n$ since $p_i$ are distinct primes.

Note that for any single equation $x^2 \equiv x \mod p_i$, it is equivalent to  $x \equiv 0, 1 \mod p_i$ because $p_i \mid = x(x-1) \Rightarrow p_i \mid x, p_i \mid x-1$

So the solution for $x^2 \equiv x \mod p_1 \cdots p_n$ is the same as the solution for the equation system
$$
\begin{cases}
x \equiv 0, 1 \mod p_1\\
x \equiv 0, 1 \mod p_2\\
\cdots\\
x \equiv 0, 1 \mod p_n
\end{cases}
$$

There are $2^n$ combinations of $0, 1$ for the equation system, by CRT, each combination have a solution mod $p_1\cdots p_n$. And trivially none of them are the same.

So there are $2^n$ solutions for $x^2 \equiv x \mod p_1 \cdots p_n$.

# ∗9. Quadratic residue.
## Let $p, q$ be two distinct prime integers and $N = pq$. $y ∈\Z_N$ is called quadratic residue if there exists $x ∈\Z_N$ such that $x^2 \equiv y \mod N$. Furthermore, x is called a square root of y. Try to given all square roots of $y$ by Chinese Remainder Theorem.

Start with $x^2 \equiv 1 \mod pq$. By the observation in problem 8, this easy question has 4 solutions, which is the combination of 

$$\begin{cases}x \equiv \pm 1 \mod p\\x \equiv \pm 1 \mod q\end{cases}$$

Since $(p,q) =1$, $\exists m,n \in \Z$, $pm + qn = 1$. Now we claim $\pm pm \pm qn$ are the 4 solutions for $x^2 \equiv 1 \mod pq$. 
> This can be verified by CRT, by plugging $\pm pm \pm qn$ into the above equation system. For example, $+ pm - qn \equiv pm - (pm + qn) \equiv 0 - 1 \mod p$.
 
Then consider $x^2 \equiv y \mod pq$. Since $y$ is a quadratic residue, $\exists x_0$, $x_0^2 \equiv y \mod pq$. So we have
$$(x_0( \pm pm \pm qn))^2 \equiv y \mod pq$$
where $pm + qn = 1$.