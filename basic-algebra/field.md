# Field Theory
For a ring $R$, the `characteristic` of $R$ is the smallest positive integer $n$ such that $n \cdot 1_R = 0_R$ if such an integer exists. Otherwise, the characteristic of $R$ is $0$.

### Lemma on Char
The $\operatorname{char} F$ of a field $F$ is either $0$ or a prime number.
> $\forall\operatorname{char} F \neq 0$, let $p = \operatorname{char} F$. Then $p \cdot 1_F = 0_F$. If $p$ is not prime, then $p = ab$ for some $a, b \in \Z_{\geq 0}$. Then $0_F = p \cdot 1_F =  (a \cdot 1_F) \cdot (b \cdot 1_F)  = 0_F$, which is a contradiction.

## Prime field
The `prime field` of a field $F$ is the smallest subfield of $F$ containing $1_F$. 

#### Prime field Structure
The prime field of $F$ is isomorphic to either $\mathbb{Q}$ or $\Z_p$ for some prime $p$.

> Let $E$ be the prime field of $F$. 
>
> Case 1: $\operatorname{char} F = 0$. Then $\Z \subset E\subset F$. Since $\mathbb{Q}$ is the smallest quotient field containing $\Z$, $E \cong \mathbb{Q}$.
>
> Case 2: $\operatorname{char} F = p$. Then $1,2,\ldots, p-1\in F$. $E \cong \Z_p$.

## Extension field
If both $E, F$ are fields with the same multiplicative and addition and $F \subset E$, we say that $E$ is an `extension field` of $F$.

> Examples: $Q \subset R \subset C$.

#### Remark
If $E/F$ is a field extension, then $E$ can be viewed as a vector space over $F$ with addtion same as it in $E$ and scalar multiplication defined by $\lambda \cdot a$, $\forall \lambda \in F \subset E, a \in E$.

## Dimension
The extension degree of $E/F$ is the dimension of $E$ over $F$, denoted by $[E:F] = \dim_F E$.

If $[E:F] < \infty$, then $E$ is a `finite extension` of $F$. Otherwise, $E$ is an `infinite extension` of $F$.

### Transitivity of extension degree
Let $K/E$ and $E/F$ be field extensions. Then $K/F$ is a field extension, and $[K:F] = [K:E][E:F]$. (Also true for infinite extension)

The basis is $\{a_i b_j \mid i \in I, j \in J\}$, where $\{a_i\}$ is a basis of $E/F$ and $\{b_j\}$ is a basis of $K/E$.

--- 

### Generated subfield
Let $E/F$ be a field extension, $S \subset E$.
- The smallest ring containing $F$ and $S$ is denoted by $F[S]$.
- The smallest field containing $F$ and $S$ is denoted by $F(S)$.

In particular, if $S = \{a\}$, then $F[S] = F[a]$ and $F(S) = F(a)$.

If $S = \{a_1, \ldots, a_n\}$, then $F[S] = F[a_1, \ldots, a_n]$ and $F(S) = F(a_1, \ldots, a_n)$.

#### Usual representation for $F[S]$ and $F(S)$
Let $E/F$ be a field extension. Let $S \subset E$, then
1. $S = \{a\}$, then 
   $$F[a] = \{f(a) \mid f(x) \in F[x]\}$$
   $$F(a) = \{\frac{f(a)}{g(a)} \mid f(x), g(x) \in F[x], g(a) \neq 0\}$$
2. $S = \{a_1, \ldots, a_n\}$, then 
   $$F[a_1, \ldots, a_n] = \{f(a_1, \ldots, a_n) \mid f(x_1, \ldots, x_n) \in F[x_1, \ldots, x_n]\}$$ 
   $$F(a_1, \ldots, a_n) = \{\frac{f(a_1, \ldots, a_n)}{g(a_1, \ldots, a_n)} \mid f(x_1, \ldots, x_n), g(x_1, \ldots, x_n) \in F[x_1, \ldots, x_n], g(a_1, \ldots, a_n) \neq 0\}$$

## Algebraic and transcendental
Let $E/F$ be a field extension. Let $a \in E$. An element $a$ is `algebraic` over $F$ if there exists $f(x) \in F[x]$ such that $f(a) = 0$. Otherwise, $a$ is `transcendental` over $F$.

> $\sqrt{2}$ is algebraic over $\mathbb{Q}$, $e, \pi$ is transcendental over $\mathbb{Q}$

### Minimal Polynomial
#### monic polynomial
`Monic poly` is the poly such that the leading coeff is $1$

#### minimal polynomial
Let $\alpha$ be algebraic over $F$. 

A monic polynomial $f(x) \in F[x]$ of least degree such that $f(\alpha) = 0$ is called the `minimal polynomial` of $\alpha$ over $F$.

> 1. The minimal polynomial of $\sqrt{2}$ over $\mathbb{R}$ is $x - \sqrt{2}$.
> 2. The minimal polynomial of $\sqrt{2}$ over $\mathbb{Q}$ is $x^2 - 2$.

#### Property
The mininal polynomial is unique and irreducible. (Also prime since $F[x]$ is a UFD)

Conversonly, if $f(x) \in F[x]$ is irreducible, then $f(x)$ is the minimal polynomial of some $\alpha \in F[x]$.

> Proof the converse.
>
> Let $f$ be irreducible and $f(\alpha) = 0$, then $\forall g$, $x - \alpha \mid g(x), f(x)$. So $\deg \gcd(g,f) \neq 1$. So by irreducibility, $\gcd(g,f) = f$, $f \mid g$. 

## Structure of Generated subfield
Let $E/F$ be a field extension. Let $\alpha \in E$.
#### Case 1
If $\alpha$ is transcendental over $F$, then $F(\alpha) \cong F(x)$. Where $F(x)$ is the quotient field of the polynomial ring $F[x]$.

Define the map $\phi: F(x) \rightarrow F(\alpha)$ by $\phi(x) = \alpha$. Then $\phi$ is an isomorphism.

#### Case 2
If $\alpha$ is algebraic over $F$, let $n = \deg f(x)$ to be the degree for minimal polynomial, then $F(\alpha)$ can be written as 
$$F(\alpha) = \operatorname{span}(\alpha^0, \alpha^1, \ldots, \alpha^{n-1}) = \{\sum_{i=0}^{n-1} a_i \alpha^i\mid a_i \in F\}$$
Further, $F(\alpha) \cong F[x]/(f(x))$, where $f(x)$ is the minimal polynomial of $\alpha$ over $F$.

> Proof
>
> Let $R = \{\sum_{i=0}^{n-1} a_i \alpha^i \mid a_i \in F\}$. Then $R \subset F[\alpha] \subset F(\alpha)$. Since $F \subset  R, \alpha \in R$ and $F(\alpha)$ is the smallest field containing $F$ and $\alpha$. $F(\alpha) \subset R$ if $R$ is a field.
>
> $R$ is trivially a commutative ring with $1$. Then $\forall \beta = \sum^{n-1}_i b_i\alpha^i\in R$, the polynomial $\gcd(\beta(x), f(x)) = 1$ since $n-1 < n$. So by bezout identity, $\beta(\alpha)h(\alpha) + 0 = 1$.
>
> (Or $-a_0^{-1}(a_1 + a_2\alpha + \ldots + a_{n}\alpha^{n-1})\alpha = 1)$)


### Theorem: Determine algebraic using $[F(\alpha):F]$
Let $E/F$ be a field extension, then $\alpha \in E$ is algebraic over $F$ if and only if $[F(\alpha):F] < \infty$.

> $\Rightarrow$
>
> Let $f(x) \in F(x)$ be the min poly for $\alpha$. Then $[F(\alpha):F] = \deg f(x) < \infty$.
>
> $\Leftarrow$
>
> Assume that $[F(\alpha):F] = n < \infty$. Then $\{1, \alpha, \ldots, \alpha^n\}$ is linear dependent. Let $f(x) = \sum_{i=0}^{n-1} a_i x^i$. Then $f(\alpha) = 0$. So $\alpha$ is algebraic over $F$.

### Theroem
An extension $E/F$ is finite iff $E = F(\alpha_1, \ldots, \alpha_n)$ for some algebraic elements $\alpha_1, \ldots, \alpha_n \in E$.

> $\Rightarrow$
> 
> Assume $E/F$ let $n = [E:F]$. Let $\gamma_1, \gamma_2, \ldots, \gamma_n$ be a basis of $E$ over $F$. Then $E = F(\gamma_1, \ldots ,\gamma_n)$. Only need to prove the algebraic. Since $[F(\gamma_i): F] = \frac{[E:F]}{[E:F(\gamma_i)]}< \infty$, $\gamma_i$ is algebraic over $F$.
>
> $\Leftarrow$: 
>
> Since $\alpha_1$ is algebraic over $F$, $[F(\alpha_1):F] < \infty$. Since $\alpha_2$ is algebraic over $F$, $\alpha_2$ is algebraic over $F(\alpha_1)$. So $[F(\alpha_1, \alpha_2):F(\alpha_1)] < \infty$. So $[F(\alpha_1, \alpha_2):F] < \infty$. By induction, $[F(\alpha_1, \ldots, \alpha_n):F(\alpha_1, \ldots, \alpha_{n-1})] < \infty$. Thus $[F(\alpha_1, \ldots, \alpha_n):F] = \prod [F(\bm \alpha_i):F(\bm \alpha_{i-1})] < \infty$.

---

#### Algebraic for Extension
An extension $E/F$ is algebraic if *every* element of $E$ is algebraic over $F$.

---
### Theorem
If $K/E$ and $E/F$ are both algebraic, then $K/F$ is algebraic.

> $\forall \alpha \in K$, $\exists a_0, \ldots, a_n \in E$ such that $\sum a_i \alpha^i = 0$. So $\alpha$ is algebraic over $F(a_0, a_1, \ldots, a_n)$. Then $[F(\alpha, a_0, \ldots, a_n):F(a_0, \ldots, a_n)] < \infty$. So $[F(\alpha, a_0, \ldots, a_n):F] < \infty$. 
>
> Since $a_0, \ldots, a_n \in E$, they are algebraic over $F$. So $[F(a_0, \ldots, a_n):F] < \infty$. So $[F(\alpha, a_0, \ldots, a_n):F(\bm a)] [F(\bm a): F]< \infty$. So $\alpha$ is algebraic over $F$.


## Composite field
Let $K, E_1, E_2$ be fields and $K \supset E_1, E_2$. Then the `composition field` of $E_1$ and $E_2$ is defined as the smallest field containing $E_1$ and $E_2$.

#### Remark
Let $E_1E_2/E_1$, $E_1E_2/E_2$, $E_1/F$, $E_2/F$. Then $[E_1E_2:E_1] \leq [E_2:F]$ and $[E_1E_2:E_2] \leq [E_1:F]$.

> Example
>
> $E_1 = \mathbb{Q}(\sqrt{2})$, $E_2 = \mathbb{Q}(2^{1/3})$, then $E_1E_2 = \mathbb{Q}(2^{1/6})$