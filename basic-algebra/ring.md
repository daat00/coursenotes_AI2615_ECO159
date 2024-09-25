# Ring Theory
### Semi-Group
A semi-group is a set $S$ with a binary operation $*$ such that
1. closure: $\forall a,b \in S, a*b \in S$
2. associativity: $\forall a,b,c \in S, (a*b)*c = a*(b*c)$

### Monoid
A monoid is a semi-group with an identity element $1$ such that 
$$\forall a \in S, a*1 = 1*a = a$$

### Group
A group is a monoid with an inverse element $a^{-1}$ such that 
$$\forall a \in S, a*a^{-1} = a^{-1}*a = 1$$

### Abelian Group
If $ab = ba$, $G$ is `commutative` or `abelian`.


## Ring
A `ring` is a set $R$ with two binary operations $+$ and $\cdot$ such that
1. $(R, +)$ is an `abelian group`
2. $(R, \cdot)$ is a `semi-group`(associative, closure)
3. Distributive law: 
   $$a\cdot(b+c) = a\cdot b + a\cdot c$$
   $$(a+b)\cdot c = a\cdot c + b\cdot c$$

> Let $R$ be a ring, $(R^{m\times n}, +, \cdot)$ is also a ring
>
> Polynomial ring: $R[x] = \{a_0 + a_1x + \cdots + a_nx^n | a_i \in R\}$

- when $1$ exists, $1 = 0 \Longleftrightarrow |R| \leq 1$

### Zero Divisor
Let $R$ be a ring, $a \in R$ is a `zero divisor` if $\exists b \in R, b \neq 0$ such that $ab = 0$ `or` $ba = 0$.

### Unit
Let $R$ be a ring with $1$, $a \in R$ is a `unit` if $a$ is invertable, $\exists b \in R$ such that $ab = ba = 1$.

> $a$ cannot be a zero divisor and a unit `at the same time.`

$U(R) = \{a \in R | a \text{ is a unit}\}$ is a `group` under $\cdot$


# Field
A commutative ring with $1$ and without zero divisors is called an `domain`.

A `skew field` is a ring with $1$ and $U(R) = R \setminus \{0\}$.

> Hermitian Quaternion: 
> $$H = \{a + bi + cj + dk | a,b,c,d \in \mathbb{R}\}$$
> $i^2 = j^2 = k^2 = ijk = -1$; 
> $ij = k, jk = i, ki = j$;
> $ji = -k, kj = -i, ik = -j$

A `field` is a commutative ring with $1$ and $U(R) = R \setminus \{0\}$.

## Sub Ring, Sub Field
Let $R$ be a ring, $S \subseteq R$ is a `sub ring` if $S$ is a ring under the same operations.

Let $F$ be a field, $K \subseteq F$ is a `sub field` if $K$ is a field under the same operations.

Similar for skew field.


# Ring Homomorphism
Let $R, S$ be rings, $\phi: R \rightarrow S$ is a `ring homomorphism` if
1. $\phi(a+b) = \phi(a) + \phi(b)$
2. $\phi(a\cdot b) = \phi(a) \cdot \phi(b)$

Furthermore, if $\phi$ is injective, $\phi$ is a `embedding`.

### Kernel
The `kernel` of $\phi$ is $\ker(\phi) = \{a \in R \mid \phi(a) = 0\}$.

- $\ker(\phi)$ is an ideal of $R$.


# Quotient Ring
> Let $I$ be a subring of $R$, $I \triangleleft R$, then $(R/I, +)$ is a quotient group on addition $(R/I, +)$.
>
> How about multiplication?
>
> $$(a+I)(b+I) = ab + I$$
>
> This is well-defined iff $\forall r\in R$, $ri \in I$, $ir\in I$.

## Ideal
Let $R$ be a ring, a subring $I$ is an `ideal` if $\forall r \in R, i \in I$, $ri \in I$ and $ir \in I$.

The sum of two ideals is an ideal, $I + J \triangleleft R$

Let $I \cdot J = \{a_1b_1 + \cdots + a_nb_n \mid a_i \in I, b_i \in J, n\in \Z\}$, where the summation is finite, then $I \cdot J \triangleleft R$.

## Quotient Ring
Let $R$ be a ring, $I$ be an ideal of $R$, then $(R/I, +, \cdot)$ is a ring under
$$(a+I) + (b+I) = (a+b) + I$$
$$(a+I) \cdot (b+I) = ab + I$$

# Ring Isomophism
## The First Ring Isomophism Theorem
Let $\phi: R \rightarrow S$ be a ring homomorphism, then
1. $\ker(\phi) \triangleleft R$
2. $R/\ker(\phi) \cong \phi(R)$


## The Second Ring Isomophism Theorem
Let $R$ be a ring, $I \leq R, J\triangleleft R$, then
1. $I + J  = J + I\leq R$
2. $I \cap J \triangleleft I$
3. $(I+J)/J \cong I/(I\cap J)$

a.k.a. the this is all about the group structure for addition.

> $$m\Z\big/[m,n]\Z = m\Z\big/m\Z \cap n\Z \cong (m\Z + n\Z)\big/n\Z = (m,n)\big/nZ$$

## The Third Ring Isomophism Theorem
Let $R$ be a ring, $I \triangleleft R$, $J \triangleleft R$, $I \subseteq J$, then
$$
\frac{R/I}{J/I} \cong R/J
$$

## The Fourth Ring Isomophism Theorem
Let $R$ be a ring, $I \triangleleft R$, then there is a bijection between the set of subrings of $R$ containing $I$ and the set of subrings of $R/I$.
$$
\{J \leq R \mid I \subset J\} \leftrightarrow \{K: K\leq R/I\}
$$

# Direct Product
## External Direct Product
Let $R_1, \cdots, R_n$ be rings, then $R \triangleq R_1 \times \cdots \times R_n$ is a ring under component-wise addition and multiplication.
1. $G_i' = \{(0, \cdots, 0, r_i, 0, \cdots, 0) \mid r_i \in R_i\} \triangleleft R$
2. Every element in $R$ can be uniquely written as a sum of elements in $G_i'$.

## Internal Direct Product
$R$ is an `internal direct product` of ideals $R_1, \cdots, R_n$ if
1. $R = R_1 + \cdots + R_n$
2. $R_i \triangleleft R$
3. Every element in $R$ can be uniquely written as a sum of elements in $R_i$.

> The external direct product is isomorphic to the internal direct product.

# Chinese Remainder Theorem
Let $R$ be a ring with $1$, $I_1, \cdots, I_n$ be ideals of $R$ such that $I_i + I_j = R$ for $i \neq j$, then
$$
R/(I_1 \cap \cdots \cap I_n) \cong R/I_1 \times \cdots \times R/I_n
$$

> Construct a ring homomorphism $\phi: R \rightarrow R/I_1 \times \cdots \times R/I_n$ such that $\ker(\phi) = I_1 \cap \cdots \cap I_n$.
> $$\phi(r) = (r+I_1, \cdots, r+I_n)$$
> It's easy to verify that $\phi$ is a ring homomorphism, and $\ker(\phi) = I_1 \cap \cdots \cap I_n$.
> 
> $\phi$ is surjective since 
> - $R^2 = (I_1 + I_2)(I_1 + I_3) \subset I_1 + I_2I_3$
> - $R^2 = (I_1 + I_2I_3)(I_1 + I_4) \subset I_1 + I_2I_3I_4$
> - $\cdots$
> - $R = I_1 + I_2I_3\cdots I_n$
>
> In the same way, we can prove that $R = I_i + I_1I_2\cdots I_{i-1}I_{i+1}\cdots I_n$.
> 
> Since $1\in R$, $1$ can be written as a sum of 2 elements
> $$\forall a_i \in I_i,  \exists b_i \in I_1I_2\cdots I_{i-1}I_{i+1}\cdots I_n, \text{ s.t. } 1 = a_i + b_i$$
>
> Then $\forall (r_1 + I_1, \cdots, r_n + I_n) \in R/I_1 \times \cdots \times R/I_n$, let $r = r_1b_1 + \cdots + r_nb_n$, then $b_i \in \cap_{j \neq i} I_j$, and $a_i \in I_i$, so by element-wise checking, we can verify that 
> $$\phi(r) = (r_1 + I_1, \cdots, r_n + I_n)$$

### Classical Chinese Remainder Theorem
Let $m_1, \cdots, m_n$ be *pairwise coprime* integers, then for any $a_1, \cdots, a_n \in \Z$, the equation system
$$
\begin{aligned}
x &\equiv a_1 \mod m_1\\
&\cdots\\
x &\equiv a_n \mod m_n
\end{aligned}
$$
has a unique solution mod $m_1\cdots m_n$.

By general CRT,
$$
\Z/(m_1\cdots m_n)\Z \cong \Z/m_1\Z \times \cdots \times \Z/m_n\Z
$$
so $\forall a_1, \cdots, a_n \in \Z$, we have $r\in \prod m_i \Z$,
$$
(r + m_1\Z, r + m_2\Z, \cdots, r + m_n\Z) = (a_1 + m_1\Z, a_2 + m_2\Z, \cdots, a_n + m_n\Z)
$$

> $$m\Z \cdot n\Z = mn\Z$$ 
> $$m\Z + n\Z = \gcd(m, n)\Z$$
> $$m\Z \cap n\Z = \operatorname{lcm}(m, n)\Z$$

# Quotient Field
> Example: For rational numbers $\mathbb{Q}$, $\mathbb{Q}$ is a quotient field of $\Z$.

> Recap: field $\subset$ skew field $\subset$ domain $\subset$ ring
>
> A domain is a commutative ring with $1$ and without zero divisors.
>
> A skew field is a ring with $1$ and $U(R) = R \setminus \{0\}$.
>
> A field is a commutative ring with $1$ and $U(R) = R \setminus \{0\}$.

Below we denote $a/b$ as the equivalence class $\overline{(a, b)}$, where the equivalence relation is defined as $(a, b) \sim (c, d) \Leftrightarrow ad = bc$, under $D \times D^*$.

---

Consider the set $D \times D^*$, where $D$ is a domain and $D^* = D \setminus 0$, then we define the following:
$$
\bar D = \{\overline{(a, b)} \mid (a,b)\in D \times D^*\}
$$
$$
\begin{aligned}
\overline{(a, b)} + \overline{(c, d)} &= \overline{(ad + bc, bd)}\\
\overline{(a, b)} \cdot \overline{(c, d)} &= \overline{(ac, bd)}
\end{aligned}
$$

Then $(\bar D, +, \cdot)$ is a field, and $\bar D$ is called the `quotient field` of `domain` $D$.

### Theorems
1. There is an `embedding` $\phi: D \rightarrow \bar D$ (embedding is injective ring homomorphism)
2. $\bar D$ is the smallest field containing $D$.
3. If $K$ is a field containing $D$, then there is an embedding $\psi: \bar D \rightarrow K$.
   > Let $\psi(\overline{(a, b)}) = ab^{-1}$. $\psi$ is well-defined, preserves addition and multiplication, and is injective.
