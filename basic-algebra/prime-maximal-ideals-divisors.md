
# Prime Ideals and Maximal Ideals
Let $R$ be a commutative ring. Ideal $I$ is called `principal` if $\exists a \in R$ such that $I = (a) = aR = \{ar \mid r \in R\}$.

Denote $aR = (a)$.

If every ideal of $R$ is principal, then $R$ is called a `principal ideal ring`. Furthermore, if $R$ is a domain, then $R$ is called a `principal ideal domain`.

## Prime Ideals
A ideal $P$ is called a `prime ideal` if 
- $P \neq R$
- For any ideals $I,J$, $IJ \subseteq P \Rightarrow I \subseteq P$ or $J \subseteq P$.

Let $R$ be a commutative ring with $1$, then the following are equivalent:
1. $P$ is a prime ideal
2. For any $a, b \in R$ such that $ab \in P$, then $a \in P$ or $b \in P$.
   > $(a)(b) = (ab)$
3. $R/P$ is a domain.
   > Commutative and identity are trivial. Just need to show that $R/P$ has no zero divisors.
   > $$ab + P = P \Leftrightarrow a + P = P \text{ or } b + P = P$$
   > 
   > Conversely, if $R/P$ is a domain. $\forall I,J \triangleleft R$, suppose $IJ \subseteq P$, and that $I \not\subset P, J \not\subset P$. Then $\exists a \in I\setminus P, b \in J \setminus P$ with $ab \in P$.
   So $a + P \neq P \neq b+ P$, while $ab+ P = P$, indicating $\bar a, \bar b$ are zero divisors, contradiction.


> Examples:
>
> $\Z/n\Z \cong \Z_n$ is a domain iff $n$ is prime.

## Maximal Ideals
An ideal $M$ is called a `maximal ideal` if
1. $M \neq R$
2. If $I$ is an ideal satisfying $M \subseteq I \subsetneq R$, then $I = M$.

Let $R$ be a commutative ring with $1$, then the following are equivalent:
1. $M$ is a maximal ideal
2. $R/M$ is a field.
   > Commutative and identity are trivial. Just need to show that $R/M$ is invertable. For $a + M \neq M$, 
   > $$M \subsetneq (a) + M \subseteq R \Rightarrow (a) + M = R$$
   > $$\exists b \in R, c \in M, \text{ s.t. } ab + c = 1$$
   > $$1 - ab = c\in M$$
   > $$1 + M = ab + M$$
   > 
   > Conversely, suppose $M$ is not maximal, then $\exists I \triangleleft R$ such that $M \subsetneq I \subsetneq R$. Choose $a \in  I\setminus M$, $\exist b \in R$ such that $ab + M = 1 + M$, $1 -ab \in M \subset I$, $1\in I$ 

### Corollary
Let $R$ be a commutative ring with $1$, any maximal ideal is prime.

# Factorization in Domain
Let $D$ be a domain, if $a = bc$, $b,c \neq 0$, then $b$ `divides` $a$, denoted as $b \mid a$.

If $a \mid b$ and $b \mid a$, then $a$ and $b$ are `associate`, denoted as $a \sim b$.

1. Two non-zero elements $a, b$ are associate iff exists a `unit` $u$ such that $a = ub$.
2. $\sim$ is a equivalence relation.


Let $a = bc$, $b,c \neq 0$, then $b,c$ are called `proper divisors` of $a$ if $b,c$ are not units, or say, neither $a\sim b$ nor $a \sim c$.

A non-zero element $a$ of $R$ is called `irreducible` if $a$ has no proper divisors.

A non-zero element $p$ of $R$ is called `prime` if $p \neq 0$ and $p \mid ab \Rightarrow p \mid a$ or $p \mid b$.

> Example: 
> $$ D = \Z[\sqrt{-5}]$$
> Define the norm $N: D \rightarrow \Z_{\geq 0}$ by $N(a + b\sqrt{-5}) = a^2 + 5b^2$. $N(x) = 1 \Leftrightarrow x \in U(D)$.
> - $2$ is irreducible, since $N(2) = 4$.
> - $1 + \sqrt{-5}$ is irreducible, since $N(1 + \sqrt{-5}) = 6$

### Properties
Let $D$ be a domain, let the set of principal ideals be $S = \{(a) \mid a \in D\setminus \{0\}, a\notin U(D)\}$.

Then 
1. $p$ is a prime element iff $(p)$ is a prime ideal.
   > $p$ is prime iff $p \mid ab \Rightarrow p \mid a$ or $p \mid b$ iff $ab \in (p) \Rightarrow a \in (p)$ or $b \in (p)$ iff $(p)$ is a prime ideal.
2. $c$ is irreducible iff $(c)$ is a `maximal element` in $S$.
   > Definition for `maximal element`: $(c)$ is maximal element if 
   >
   > $\exists (a) \in S$ such that $(c) \subseteq (a)$, then $(c) = (a)$.

   > Proof:
   > 
   > $c$ is irr iff $r \mid c$ and $r \notin U(D)$ implies $r \sim c$. iff $(c) \subseteq (r)$ and $(r) \subsetneq S$ implies $(c) = (r)$ iff $(c)$ is maximal element in $S$. 
3. Any prime element in $D$ is irreducible.
   > Assume  $r \mid p$, then $p = rs$, $s\in R$. By prime, $p\mid r$ or $p\mid s$. So $p\sim r$ or $p\sim s$. So $p$ has no proper divisors.
4. If $D$ is a PID, an element is prime iff it is irreducible.
   > Only need to show that irr is prime. Let $c$ be an irreducible element, $(c)$ is a maximal ideal of $D$. By 1., $c$ is prime.

## Greatest Common Divisor
Let $D$ be a domain, $a,b \in D\setminus\{0\}$, $d \in D$ is called a `greatest common divisor` of $a,b$ if
1. $d \mid a$ and $d \mid b$
2. If $c \mid a$ and $c \mid b$, then $c \mid d$.

Denote by $d = (a,b)$

#### Remark
1. GCD is not unique, but any two GCDs are associate.
2. GCD exists iff ??

> Counter-example: Consider $6\in \Z[\sqrt{-5}]$, $2(1 + \sqrt{-5})$. $2, 1+\sqrt{-5}$ are common divisors, but no GCD


### Lemma
Let $D$ be a domain, $a,b \in D\setminus\{0\}$, then
1. $\forall u\in U(D)$, $(u,a) \sim u$
2. $((a,b), c) \sim (a,(b,c))$
3. $c(a,b) \sim (ac,bc)$
4. If $(a,b) \sim 1$, $(a,c) \sim 1$, then $(a,bc) \sim 1$.

## Unique factorization domain
A domain $D$ is called a `unique factorization domain` if
1. Every element $a \in D\setminus\{0\}$ can be written as a product of finite irreducible elements. 
   
   $a = up_1\cdots p_n$, where $u\in U(D)$ and $p_i$ are irreducible.
2. The factorization is unique up to order and associates. 
   
   If $a = u_1p_1\cdots p_n = u_2q_1\cdots q_m$, where $u_1, u_2 \in U(D)$ and $p_i, q_j$ are irreducible, then $n = m$ and $\exists \sigma \in S_n$ such that $p_i \sim q_{\sigma(i)}$.

### Properties
If $D$ is a UFD, then
1. (Noetherian) Any ascending chain of principal ideals of $D$ is stable. $(a_1) \subseteq (a_2) \subseteq \cdots \subseteq (a_n) \subseteq \cdots$, then $\exists k\geq 0$, s.t. $\forall n \geq k$, $(a_n) = (a_k)$.
   > $(a_i) \subset (a_{i+1}) \Rightarrow a_i = a_{i+1}x$. So chain is not stable $\Rightarrow a_1 = a_{n+1}x_nx_{n-1} \cdots x_1$, where $x_i \notin U(D)$. 
2. Every irreducible element is prime.
   > $c \mid ab \Rightarrow c \mid a$ or $c\mid b$. Write $a = u_1 \prod p_i, b = u_2\prod p_j$. 
3. For any $a,b \in D\setminus\{0\}$, the GCD $(a,b)$ exists.
   > Write $a,b$ in the form of factorization, then take the common irreducible factors.

In fact, for a domain $D$, 
- if 1. and 2. hold, then $D$ is UFD.
- if 1. and 3. hold, then $D$ is UFD
- The condition set 1. & 2. an 1. & 3. are equivalent.
> 1\. & 3. $\Rightarrow$ 1. & 2. is trivial. Use 1. 3. to determine UFD.
>
> For each $a$, recursively divide $a$ with prime. Then we get a chain.

## PID is UFD
Every PID is a UFD.

> Equivalent to show Noetherian. 
>
> For any ascending chain of ideals $(a_1) \subseteq (a_2) \subseteq \cdots \subseteq (a_n) \subseteq \cdots$, the union $I = \cup_{i=1}^\infty (a_i)$ is an ideal. Suppose $I = (a)$. Then $a \in (a_k)$, $(a) \subset (a_k) \subset \cdots \subset (a)$


## Bezout Ring
If a UFD $D$ satisfies
$$\forall a,b \in D\setminus\{0\}, \exists x,y \in D, ax + by = (a,b)$$
then $D$ is called a `Bezout ring`.
> Examples : $\Z, \mathbb{F}[x]$
> 
> Actually they are PID, because long division exists.

## Eucilidean Domain
> We want to explore the long division

A domain $D$ is called a `Eucilidean domain` if there exists a function $N: D\setminus\{0\} \to \Z_{\geq 0}$ such that
1. $N(a) = 0 \Longleftrightarrow a = 0$
2. $\forall a,b \in D, b \neq 0$, $\exists q,r \in D$ such that $a = bq + r$, where $r = 0$ or $N(r) < N(b)$.

> **Gaussian Ring**
> 
> Consider $\Z[i] = \{a + bi \mid a, b \in \Z\}$. Define $N(a + bi) = a^2 + b^2$.
>
> Analysis: $\forall a = u + vi, b = c + di$. Use $ab^{-1}$ to approx $q$. Let $ab^{-1} = s + ti$, where $s,t \in \mathbb{Q}$, and we have, $\exists y,z \in \Z$ such that $|s - y| \leq \frac{1}{2}, |t - z| \leq \frac{1}{2}$. So $r = b[(s - y) + (t - z)i]$. $N(r)/N(b) = (s - y)^2 + (t - z)^2 \leq \frac{1}{4} + \frac{1}{4} = \frac{1}{2} < 1$.
>
> So let $ab^{-1} = s + ti = y + zi + (s - y) + (t - z)i$. $a = (y + zi)b + ((s - y) + (t - z)i)b$. Claim $r \in \Z$, and $N(r) < N(b)$.
>
> Similar for $\Z[\sqrt{-2}]$. Maybe wrong for $\Z[\sqrt{-3}]$.

### ED is PID
Let $R$ be a ED with $\phi$, then $R$ is a PID.

Let $I$ be an ideal of $R$.
1. $I = \{0\}$, then $I = (0)$.
2. $I \neq \{0\}$. Consider the set $\{\phi(a): a\in I\} \subset \Z_{\geq 0}$. Let $m = \min \{\phi(a): a\in I\}\setminus \{0\}$. $\exists b \in I$ such that $\phi(b) = m$. Claim $I = (b)$.
   > $\forall a \in I$, $a = qb + r$, where $r = 0$ or $\phi(r) < \phi(b)$. So $r = 0$, $a = qb \in (b)$.


$ED \Rightarrow PID \Rightarrow UFD$

# Polynomial Ring
> Question: $UFD \Rightarrow PID \Rightarrow ED$?

Let $R$ be a ring. The `polynomial ring` of one variable over $R$
is defined as the set
$$R[x] = \{a_0 + a_1x + \cdots + a_nx^n \mid a_i \in R, n \in \Z_{\geq 0}\}$$

Further more, the degree of a polynomial is defined as the largest $n$ such that $a_n \neq 0$. Denote by $deg(f)$. A special case is $\deg(0) = -\infty$.

### Properties
1. $R[x]$ is a commutative ring with $1$.
2. $U(R[x]) = U(R)$.
3. $\deg(f + g) \leq \max\{\deg(f), \deg(g)\}$.
4. If $R$ is a domain, $\deg(fg) = \deg(f) + \deg(g)$.

### Lemma
Let $f, g\in R[x]$ with $g \neq 0$, and the leading coefficient of $g$ is a unit. Then $\exists q, r \in R[x]$ such that $f = qg + r$, where $r = 0$ or $\deg(r) < \deg(g)$.
> Use $g$ is a unit, to construct an inverse

---

Furthermore, let $F$ be a field, then $F[x]$ is a Euclidean domain since each poly is a unit.

The map $\phi: F[x] \to \Z_{\geq 0}$ defined by $\phi(f) = z^{\deg(f)}$ is a Euclidean function.

#### Gauss Lemma
If $R$ is a UFD, then $R[x]$ is also a UFD.

### Theorem
$UFD \not \Rightarrow PID \not \Rightarrow ED$

$\Z$ is UFD, $\Z[x]$ is UFD but not PID.

To find an ideal that is not a principal ideal.

Consider $(x) + (2)$, suppose $(x) + (2) = (f)$, then $f \mid 2$, so $f = \pm 1, \pm 2$. But $f \notin (x) + (2)$.

$R = \{a + b \frac{ 1 + \sqrt{19}}{2}: a,b \in \Z\}$. $R$ is a PID, but not ED.

## Multi-variable Polynomial Ring
Let $R$ be a ring, the polynomial ring of $m$ variables over $R$ is defined as
$$R[x_1, \cdots, x_m] = \left\{f = \sum_{i_1, \cdots, i_m} a_{i_1, \cdots, i_m}x_1^{i_1}\cdots x_m^{i_m} \mid a_{i_1, \cdots, i_m} \in R\right\}$$