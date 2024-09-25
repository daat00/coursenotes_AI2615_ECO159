# 1. Show that a finite domain is a field.
Suppose the finite domain is $D$.

$\forall x\in D\setminus \{0\}$, $x^{\operatorname{ord} x} = e$. So x is invertible, $D$ forms a Abelian group under $\cdot$. So $D$ is a field.

# 2. Let $d ≥1$ be a positive integer. Let $R = Z[\sqrt{−d}] = \{a + b\sqrt{−d}\mid a, b ∈\Z\}$be a subset of the complex field. Show that $R$ is a domain and determine its unit group $U(R)$.

### $R$ is a ring
1. $(R, +)$ is a Abelian, because $a_1 + b_1\sqrt{−d} - a_2 - b_2\sqrt{−d} = (a_1 - a_2) + (b_1 - b_2)\sqrt{−d} \in R$ and $R$ is commutative.
2. $(R, \cdot)$ is semi-group, since $(a_1 + b_1\sqrt{−d}) \cdot (a_2 + b_2\sqrt{−d}) = (a_1a_2 - b_1b_2d) + (a_1b_2 + a_2b_1)\sqrt{−d} \in R$. Associative is trivial.
3. Distributive law is trivial

### $R$ is a domain
1. Clearly $\cdot$ is commutative, and $1\in R$
2. Since $C\setminus \{0\}$ has no zero divisors(consider norm), $R$ has no zero divisors.

So $R$ is a domain.

# 3. Let $I$ be a subset of $R$, where $R$ is a ring with identity. Show that $I$ is an ideal of $R$ if and only if the following conditions hold:
## (a) $i + j ∈I\qquad ∀i, j ∈I$
## (b) $ra, ar ∈I\qquad ∀a ∈I, r ∈R$

$\Rightarrow$:
1. $I$ is a subring of $R$, so $I$ is closed under $+$.
2. By definition, $\forall r\in R, a\in I$, $ra, ar\in I$.

$\Leftarrow$:

First, $I$ is a subring of $R$ since
1. $I$ is closed under $+$.
2. $\forall a\in I, r \in I$, $ar, ra \in I$, $I$ is closed under $\cdot$.
3. $\forall a\in I$, let $r = -1 \in R$, $-a \in I$, so $I$ has additive inverse.

Second, $I$ is an ideal of $R$ since $I \leq R$ and $\forall a\in I, r\in R$, $ra, ar \in I$.

# 4. Let $f$ be a ring homomorphism from $R$ to $S$. If $R$ has a multiplicative identity and $f$ is surjective, show that $S$ has a multiplicative identity as well. Give a counterexample if $f$ is not surjective.

Claim $f(1_R)$ is the multiplicative identity of $S$

$\forall s\in S$, $\exists r\in R$ such that $f(r) = s$ since $f$ is surjective. So $s = f(r) = f(r\cdot 1_R) = f(r)\cdot f(1_R) = s\cdot f(1_R)$. So $f(1_R)$ is the multiplicative identity of $S$.

Counterexample: $f: \Z \rightarrow 2\Z$, $f(x) = 0$.

# 5. Let $R$ be a ring. The center $C(R)$ is defined to be the set $\{a \in R: ra = ar, \forall r \in R\}$. Show that $C(R)$ is a subring of $R$. Is $C(R)$ always an ideal? Justify your answer.

$C(R)$ is a subring of $R$ since
1. $\forall c_1, c_2\in C(R)$, $a(c_1 - c_2) = (c_1 -c_2) a, \forall a\in R$. So $C(R), +$ forms a subgroup.
2. $\forall c_1, c_2\in C(R)$, $ac_1c_2 = c_1c_2a, \forall a\in R$. So $C(R), \cdot$ forms a semi-group. 

$C(R)$ is not always an ideal. Consider $R = \Reals^{n\times n}$, $I \in C(R)$. 

Then suppose $C(R)$ is an ideal, $\forall A\in R, I\cdot A \in C(R)$. So $C(R) = R$, which indicates the matrix multiplication is commutative, contradiction.

In the following exercises, let $M_n(R)$ denote an $n ×n$ matrix ring over $R$.
===

# 6. Show that the complex field C is isomorphic to a subring of $M_2(\R)$. Is this subring an ideal of $M_2(\R)$

Denote a subring $S =  \{\begin{bmatrix}a & -b\\ b & a\end{bmatrix} \mid a, b \in \R\}$

Let $\phi: \Complex \rightarrow S$, $\phi(a + bi) = \begin{bmatrix}a & -b\\b & a\end{bmatrix}$

1. $\phi$ is one-to-one since 
   - $\phi(a + bi) = \phi(c + di) \Rightarrow a = c, b = d$.
   - $\forall A\in S$, $\phi(A_{11} + A_{21}i) = A$.
2. $\phi$ is isomorphic since 
$$\begin{align*}\phi((a + bi) + (c + di)) &= \phi((a + c) + (b + d)i) \\&= \begin{bmatrix}a + c & -b - d\\ b + d & a + c\end{bmatrix} \\&= \begin{bmatrix}a & -b\\b & a\end{bmatrix} + \begin{bmatrix}c & -d\\d & c\end{bmatrix} \\&= \phi(a + bi) + \phi(c + di)\end{align*}$$
and 
$$\begin{align*}\phi((a + bi)(c + di)) &= \phi((ac - bd) + (ad + bc)i) \\&= \begin{bmatrix}ac - bd & -ad-bc\\ ad+bc & ac -bd\end{bmatrix} \\&= \begin{bmatrix}a & -b\\b & a\end{bmatrix} \cdot \begin{bmatrix}c & -d\\d & c\end{bmatrix} \\&= \phi(a + bi) \cdot \phi(c + di)\end{align*}$$

So $C$ is isomorphic to a subring of $M_2(\R)$.

# 7. Show that every ideal of $M_n(R)$ has the form $M_n(I)$ for some ideal $I$ of $R$.

Suppose $J$ is an ideal of $M_n(R)$, let $I_{ij} = \{a_{ij} \mid A\in J\}$.

Then $I_{ij}$ is an ideal of $R$ since
1. $\forall x_1, x_2\in I_{ij}$, $\exists A_1, A_2\in J$ such that $x_1 = (A_1)_{ij}, x_2 = (A_2)_{ij}$. 
   
   So $x_1 - x_2 = (A_1 - A_2)_{ij}$. Since $A_1 - A_2 \in J$, $x_1 - x_2 \in I_{ij}$
2. $\forall x \in I_{ij}, r \in R$, $\exists A \in J$, $A_{ij} = x$. Then $\operatorname{diag}(r,r,\ldots, r)A\in J$. So $rx \in I_{ij}$.
  
   Similarly, $xr \in I_{ij}$.

Furthermore, suppose $1\in R$, we claim that $I_{ij} = I_{i'j'}$, for arbirary $i, j, i', j'$. 

Consider $x_{ij} \in I_{ij}$, and pick up a $A\in J$ with $x_{ij} = (A)_{ij}$. Then $(E_{i'i} AE_{jj'})_{i'j'} = x_{ij} \in I_{i'j'}$. Which means $I_{ij} \subset I_{i'j'}$ and similarly $I_{i'j'} \subset I_{ij}$.

So let $I \triangleq I_{11} = I_{12} = \ldots = I_{nn}$, $J = M_n(I)$.


# 8. Let $R = \{\begin{bmatrix}a & b\\0 & a\end{bmatrix} \mid a, b \in \R\}$ and $I = \{\begin{bmatrix}0 & b\\0 & 0\end{bmatrix} \mid b \in \R\}$. Show that $I$ is an ideal of $R$ and $R/I \cong \R$.

$I$ is an ideal of $R$ since
1. Trivially $I$ is closed on $+$
2. $\forall \begin{bmatrix}a & b\\0 & a\end{bmatrix} \in R, \begin{bmatrix}0 & c\\0 & 0\end{bmatrix} \in I$, $\begin{bmatrix}a & b\\0 & a\end{bmatrix} \cdot \begin{bmatrix}0 & c\\0 & 0\end{bmatrix} = \begin{bmatrix}0 & ac\\0 & 0\end{bmatrix} \in I$ and $\begin{bmatrix}0 & c\\0 & 0\end{bmatrix} \cdot \begin{bmatrix}a & b\\0 & a\end{bmatrix} = \begin{bmatrix}0 & ac\\0 & 0\end{bmatrix} \in I$.

Define $\phi: \R \rightarrow R/I$, $\phi(a) = \begin{bmatrix}a & 0\\0 & a\end{bmatrix} + I$.
- $\phi$ is injective since $\phi(a) = \phi(b) \Rightarrow \begin{bmatrix}a & 0\\0 & a\end{bmatrix} + I = \begin{bmatrix}b & 0\\0 & b\end{bmatrix} + I \Rightarrow \begin{bmatrix}a - b & 0\\0 & a-b\end{bmatrix} \in I \Rightarrow a -b = 0$.
- $\phi$ is surjective since $\forall A = \begin{bmatrix}a & b\\0 & a\end{bmatrix} + I \in R/I$, $\phi(a) = \begin{bmatrix}a & 0\\0 & a\end{bmatrix} + I = A$.
- $\phi$ is isomorphic since 
  $$\phi(a + b) = \begin{bmatrix}a + b & 0\\0 & a + b\end{bmatrix} + I = \begin{bmatrix}a & 0\\0 & a\end{bmatrix} + I + \begin{bmatrix}b & 0\\0 & b\end{bmatrix} + I = \phi(a) + \phi(b)$$ 
  and 
  $$\phi(ab) = \begin{bmatrix}ab & 0\\0 & ab\end{bmatrix} + I = \begin{bmatrix}a & 0\\0 & a\end{bmatrix} \cdot \begin{bmatrix}b & 0\\0 & b\end{bmatrix} + I = \phi(a) \cdot \phi(b)$$
So $R/I \cong \R$.

# 9. Assume $f: R\rightarrow S$ be the ring homomorphism. Let $I$ and $J$ be ideals of $R$ and $S$, respectively and $f(I)\subset J$.
Define $\bar{f}$ as:
$$\bar{f}: R/I \rightarrow S/J, a + I \rightarrow f(a) + J$$

### (a) Show that $\bar{f}$ is well-defined and a ring homomorphism.

$\bar{f}$ is well-defined since $\forall a_1 + I = a_2 + I$, $a_1 - a_2 \in I$, $f(a_1 - a_2) = f(a_1) - f(a_2) \in J$, so $f(a_1) + J = f(a_2) + J$.

$\bar{f}$ is a ring homomorphism since
- $\bar{f}(a + I + b + I) = \bar{f}(a + b + I) = f(a + b) + J = f(a) + f(b) + J = \bar{f}(a + I) + \bar{f}(b + I)$
- $\bar{f}((a + I)(b + I)) = \bar{f}(ab + I) = f(ab) + J = f(a)f(b) + J = \bar{f}(a + I)\bar{f}(b + I)$

### (b) Show that $\bar{f}$ is a ring isomorphism if and only if $f(R) + J = S$ and $I = f^{-1}(J)$.
$\Rightarrow$:

Since $\bar f$ is surjective, $S = \bigcup_{a \in R} f(a) +J = f(R) + J$.

Also, suppose $J\setminus f(I) \neq \emptyset$, then $\exists x\in J\setminus f(I)$. Thus $\bar f(x + I) = f(x) + J =  J$, which violates injective

So $J = f(I)$

$\Leftarrow$:

$\bar f$ is surjective since $\forall s + J \in S/J$, $s\in S$, 

$s = f(r) + j$ for some $r\in R, j\in J$. So $\bar f(r + I) = f(r) + J = s + J$.

$\bar f$ is injective because $\forall a + I, b + I \in R/I$, 

$\bar f(a + I) = \bar f(b + I) \Rightarrow f(a) - f(b) \in J \Rightarrow f(a - b) \in J \Rightarrow a - b \in f^{-1} (J) = I \Rightarrow a + I = b + I$.

So $\bar f$ is a ring isomorphism.