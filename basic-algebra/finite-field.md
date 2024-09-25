# Finite Field
A finite field is a field with a finite number of elements.

#### Lemma for Char
A finite field must have a prime characteristic.

> The characteristic of a field is either $0$ or a prime number. 
> 
> Let $F$ be a finite field with $q$ elements. Then $iq \in F$. So $iq = 0$ for some $i \in \mathbb{N}$. So $F$ has characteristic $p$.

### Lemma 
Let $F$ be a finite field with $\operatorname{char} F = p$. Then 
1. $(a+b)^p = a^p + b^p$ for all $a, b \in F$
2. $a^q = a, \forall a\in F$ if $|F| = q$.
3. $|F| = p^n$ for some $n \in \mathbb{N}^+$.
4. $F^* = F \setminus \{0\}$ is a cyclic group of order $p^n - 1$.

> 1. $\frac{p!}{k!(p-k)!} \equiv 0 \mod p$
> 2. By Lagrange's theorem, $a^{q-1} = 1$
> 3. Since $p = \operatorname{char} F$, $\Z_p \subset F$. So $[F: \Z_p] < \infty$. Let $n = [F: \Z_p]$ and $\alpha_1, \ldots, \alpha_n$ be a basis of $F$ over $\Z_p$. Then $F = \{\sum_{i=1}^n a_i \alpha_i \mid a_i \in \Z_p\}$. So $|F| = p^n$.


> 4. Factorize $q-1 = p_1^{n_1} \cdots p_k^{n_k}$. By Sylow's theorem, there exists a subgroup $H_i$ of order $p_i^{n_i}$.
> 
>    Consider the poly $x^{p_i^{n_i}} - 1$. $\forall \alpha \in H_i$, $\alpha^{p_i^{n_i}} = 1$. So $H_i$ is the set of roots of $x^{p_i^{n_i}} - 1$ and it has $p_i^{n_i}$ elements. Then consider $x^{p_i^{n_i -1} -1} -1$. It has at most $p^{n_i -1}$ roots. Thus $\exists g_i \in H_i$ s.t. $g_i$ is the root of $x^{p_i^{n_i}} -1$ but not $x^{p_i^{n_i -1} -1} -1$. 
> 
>    Then $\operatorname{ord} g_i \mid p^{n_i}$, $\operatorname{ord}g_i = p^r$. Since $g^{p^{n_i -1}} \neq 1$, $r = n_i$. 
> 
>    Moreover, $\operatorname{ord} (g_1 \cdot \ldots \cdot g_n) = \prod p_i^{n_i} = q-1$. So $F^*$ is cyclic.

> Note: If $\gcd(\operatorname{ord} a, \operatorname{ord} b) = 1$, and $ab = ba$, then $\operatorname{ord} (ab) = \operatorname{lcm}(\operatorname{ord} a, \operatorname{ord} b)$.


---
#### Remark
If $f(x) \in F[x]$ is the minimal polynomial of $\alpha \in F$, then $[F(\alpha):F] = \deg f(x)$.

> Proof
>
> Let $f(x) = \sum_{i=0}^{n} a_i x^i$ with $a_n = 1$. $F(\alpha) = \{\sum_{i=0}^{n-1} a_i \alpha^i \mid a_i \in F\}$. $\{1, \alpha, \ldots, \alpha^{n-1}\}$ is a basis of $F(\alpha)$ over $F$. So $[F(\alpha):F] = n = \deg f(x)$.

### Theorem
For any  prime $p$ and any integer $n \geq 1$, there is a unique finite field $q$ of order $p^n$.

> $\forall p, n\geq 1$, $\exists$ an irreducible polynomial $f$ of degree $n$ over $\Z_p$.
>
> **Existence** Take an irreducible polynomial $f$ of degree $n$ over $\Z_p$. Then $[\Z_p(\alpha): \Z_p] = \deg f$. So $|\Z_p(\alpha)| = p^n$. 
>
> **Uniqueness** Suppose $E_1, E_2$ are two fields of size $p^n$. Consider $E_1E_2$ and the poly $x^{p^n} - x$. $\forall \alpha \in E_1$, $\alpha$ is a root of $x^{p^n} - x$. Also, $\forall \beta \in E_2$, $\beta$ is a root for the polynomial. Since the poly has $p^n$ roots, $E_1 = E_2$.

Notation: A finite field of $q$ elements is denoted by $F_q$ or $GF(q)$.
> Examples：
> 
> $F_2 = \Z_2$, $F_4 \neq \Z_4$. $F_4 = F_2(\alpha)$ where $\alpha$ is a root of $x^2 + x + 1$.
>
> ---
> 
> There is a finite field of order $8 = 2^3$.
> - How to find it? consider $x^3 + x + 1\in \Z_2[x]$. It is irreducible. So $F_8 = \Z_2(\alpha)$ where $\alpha$ is a root of $x^3 + x + 1$.
> - Let $\alpha$ be a root of $x^3 + x + 1$, $F_2(\alpha) = \{a + b\alpha + c\alpha^2 \mid a, b, c \in \Z_2\}$. So $F_8 = \{0, 1, \alpha, \alpha^2, \alpha + 1, \alpha^2 + 1, \alpha^2 + \alpha, \alpha^2 + \alpha + 1\}$.
>
> Consider the polynomial $x^3 + x^2 + 1 \in F_2[x]$, it has no root in $F_2$, then let $\beta$ be a root of $x^3 + x^2 + 1$. Then $F_8 = \Z_2(\beta) = \{0, 1, \beta, \beta^2, \beta + 1, \beta^2 + 1, \beta^2 + \beta, \beta^2 + \beta + 1\}$.
>
> *Unique*: $F_2(\alpha) = F_2(\beta)$. Consider $(\beta + 1)^3 + (\beta + 1) + 1 = 0$. So $\beta + 1$ is a root of $x^3 + x + 1$. So $\beta + 1 = \alpha$. So $\beta = \alpha - 1$. ($\alpha$ is three root)

## Algebraic Closure
A field $F$ is `algebraically closed` if every non-constant($\deg \geq 1$) polynomial in $F[x]$ has at least one root in $F$.

*Equivalent to say that **every** polynomial in $F[x]$ can be factorized into product of linear polynomials*

> $f(x) = (x - \alpha) g(x)$, where $g \in F[x]$. Iteratively do this until g is a const

> 1. $\Complex$ is algebraically closed.
> 2. $\bigcup_{i=1}^\infty F_{p^i}$ is algebraically closed.

### Algebraic Closure
Every field is contained in an algebraically closed field.

*skip proof*

> $$\mathbb{Q} \subset \mathbb{R} \subset \Complex$$
> $$\mathbb{F}_p \subset \bigcup_{i=1}^\infty F_{p^i}$$

---
#### Definition
Let $F$ be a field. A field $E$ is called an `algebraic closure` of $F$ if
1. $E/F$ is an algebraic extension.
2. Every polynomial in $F[x]$ can be factorized into linear polynomials in $E[x]$.(a.k.a. has a root in $E$)

> $\mathbb{C}$ is an algebraic closure of $\mathbb{R}$. However, $\mathbb{C}$ is not an algebraic closure of $\mathbb{Q}$ since $\pi$ is **not** algebraic over $\mathbb{Q}$.

#### Property
An algebraic closure of a field $F$ is algebraically closed. ($E$ is algebraically closed).

> Show that every poly $f(x) \in E[x]$ has at least one root in $E$. (closure is derived from $F$)

> Let $\alpha$ be a root of $f(x)$, then $E(\alpha)/E$ is algebraic. ($\forall b\in E(\alpha)$, $E(b) \subset E(\alpha)$)
> <br>
> Since $E/F$ is also algebraic, $E(\alpha)/F$ is algebraic, $\alpha$ is alg. over $F$. So $\exist g(x)\in F[x]$ s.t. $g(\alpha) = 0$. So $g(x) = a\prod(x-\alpha_i)$, $\alpha_i \in E$. $\alpha \in  \{\alpha_1, \alpha_2, \ldots, \alpha_n\} \subset E$

---

Let $F$ be a field, let $\Omega$ be an algebraic closed field containing $F$. Let 
$$\bar F = \left\{\alpha \in \Omega: \alpha \text{ is algebraic over } F\right\}$$
Then $\bar F$ is an algebraic closure of $F$. Moreover, in $\Omega$, the algebraic closure of $F$ is unique

> $\bar F$ is a field
>
> 1. $\forall \alpha \in \bar F/\{0\}$, $\alpha$ is alg. over $F$. So $F(\alpha)/F$ is alg. So $\alpha^{-1}\in F(\alpha)$ and it is alg. over $F$. So $\alpha^{-1} \in \bar F$.
> 2. $\forall \alpha_1, \alpha_2 \in \bar F$, $F(\alpha_1, \alpha_2)/F$ is finite extension and alg. So $\alpha_1 + \alpha_2, \alpha_1\alpha_2, \alpha_1 - \alpha_2 \in \bar F$.
>
> $\bar F/F$ is algebraic. (by definition)
>
> $\forall f(x) \in F[x]$, since $\Omega$ is closed, $f(x) = a\prod (x - \alpha_i)$, where $\alpha_i \in \Omega$. Note $\alpha_i$ is algebraic over $F$. So $\alpha_i \in \bar F$.
>
> In conclusion, $\bar F$ is an algebraic closure of $F$. 
> 
> *Unique*: Let $E\subset \Omega$ be an algebraic closure, then by definition $E\subset \bar F$ since $E/F$ is algebraic.
> 
> $\forall \alpha \in \bar F$, by algebraic $\alpha$ is a root of $g(x) = \prod(x - \alpha_i)$. $\alpha \in \{\alpha_1, \ldots, \alpha_n\} \subset E$. So $\bar F \subset E$.

---
$\bar F$ is denoted as `algebraic closure` of $F$.

## Splitting Field
Let $f(x) \in F[x]$, let $f(x) = a\prod_{i=1}^n (x-\alpha_i)$, $\alpha_i \in \bar F$. 

The `splitting field` of $f(x)$ over $F$ is the smallest field $E$ containing $F$ and $\{\alpha_1, \ldots, \alpha_n\}$. Which is $F(\alpha_1, \ldots, \alpha_n)$

> 1. The splitting field of $x^2 + x + 1 \in F_2[x]$ over $F_2$ is $F_2(\alpha, \alpha +1) = F_2(\alpha) = F_4 = \{0,1, \alpha, \alpha + 1\}$
> 2. The splitting field of $x^3 -2\in \mathbb{Q}$ is $\mathbb{Q}(\sqrt[3]{2}, \sqrt[3]{2}\omega, \sqrt[3]{2} \omega^2) = \mathbb{Q}(\sqrt[3]{2},\omega)$, where $\omega = \exp(\frac{2\pi i}{3})$.

## Separable Polynomial
Let $f(x) \in F[x]$. Factorize $f(x)$ on $\Omega[x]: f(x) = a\prod_{i=1}^n (x-\alpha_i)^{e_i}. a\in \Omega\setminus 0, \alpha_i \in \Omega$, where $e_i \geq 1$. $\alpha_1, \ldots, \alpha_r$ are pairwise distinct.  Then $\alpha_i$ is called a simple root of $f(x)$ if $e_i = 1$. Otherwise $\alpha_i$ is called a multiple root.

#### Lemma
$f(x)\in F[x]$ has multiple roots iff $\gcd(f, f') \neq 1$.

#### Lemma
Let $f(x) \in F[x]$ be irreducible, then
1. if $\operatorname{char} F = 0$, then $f(x)$ has no multiple root.
2. if $\operatorname{char} F = p$, then $f(x)$ has multiple root iff $f(x) = g(x^p)$ for some $g(x) \in F[x]$.
> 1. Since $f' \neq 0$ and $f$ is irreducible, $\gcd(f, f') = 1$.
> 2. $\Leftarrow$: Let $g(x) = \sum_{i=0}^m g_i x^i$, then $f(x) = g(x^p) = \sum_{i=0}^m g_i x^{pi}$ So $f' = 0$, $\gcd(f, f') \neq 1$.
>   
>    $\Rightarrow$ Since $f(x)$ has multiple root, $\gcd(f,f') \neq 1$. Also, by $f$ is irreducible, $\gcd(f,f') = f$. So $f' = 0$. Then let $f(x) = f_i x^i$, $f' = 0 \Rightarrow f_i i = 0$, $f_i = 0$ or $(i = 0 \mod p)$, $f(x) = \sum_{j = 0}^t f_{jp}x^{jp} = g(x^p)$

### Definition
A poly in $F[x]$ is called `separable` if every irreducible factor of $f(x)$ has no multiple root.

> Examples
> 1. $(x - 1)^2(x - 2)\in \mathbb{F}_3[x]$ is seperable
> 2. Let $t$ be a transedental element over $\mathbb{F}_p$, then $x^p - t$ is inseparable over $\mathbb{F}_p(t)$, since $t \notin \{\alpha^p: \alpha \in \mathbb{F}_p(t)\}$.

#### Discuss on $\operatorname{char} F$
If $\operatorname{char} F = 0$, then every poly is separable.
> Trivial through definition

If $\operatorname{char} F = p > 0$, then $x^p -  a \in F[x]$ is either inseperable or a power of $(x - \alpha)^p$ for some $\alpha \in F$.
> Equivalent to show either $\exists \alpha \in F$ s.t. $a = \alpha^p$, and $x^p - a = (x - \alpha)^p$; Or $x^p - a$ is inseparable.
>
> Now assume $a \notin \{\alpha^p: \alpha \in F\}$, we want to show $x^p - a$ is irreducible. Let $\gamma \in \Omega$ s.t. $\gamma$ is root of $x^p - a$. Then $a = \gamma^p$. $x^p - a= (x- \gamma)^p$. So it has multiple roots
>
> Now only need to plus that $x^p - a$ is irreducible. Suppose it is reducible, then $x^p - a = (x - \gamma)^k(x - \gamma)^{p-k}$ for some $1 \leq k \leq p- 1$, and $(x - \gamma)^k, (x - \gamma)^{p-k} \in F[x]$. 
> 
> So we have the const term $\gamma^k \in F$. Since $\gcd(k,p) = 1$, $uk + vp = 1$. So $\gamma = \gamma^{uk + vp} = (\gamma^k)^u \cdot 
(\gamma^p)^v \in F$. So $a = \gamma^p \in \{\alpha^p\}$. Contradiction.


## Perfect
### Definition
A field $F$ is called `perfect` if every poly in $F[x]$ is separable.

#### Theorem
If $\operatorname{char} F = 0$, then $F$ is perfect.

Let $\operatorname{char} F = p > 0$, then $F$ is perfect iff $F = \{\alpha^p: \alpha \in F\}$.

> $\Leftarrow$: Suppose $F \neq \{\alpha^p: \alpha \in F\}$. Then $\exists \beta \in F, \beta \notin \{\alpha^p: \alpha \in F\}$. Let $f(x) = x^p - \beta$.  $f(x)$ is inseparable. So $F$ is not perfect.
>
> $\Rightarrow$: Suppose $F$ is not perfect, then $\exists f(x) \in F[x]$ irreducible with multiple root. Let $f(x) = g(x^p)$ for some $g$. Then $f'(x) = pg'(x^p)x^{p-1} = 0$. So $g'(x^p) = 0$. Then $f(x)$ is reducible

---

Examples

> **Any finite field is perfect.** Let $p = \operatorname{char} F$. Then $F = \{a_1, \ldots a_q\} = \{a_1^p,\ldots, a_q^p\}$ since $a_i^p - a_j^p = (a_i - a_j)^p  = 0$. So $F$ is perfect.
>
> $\bigcup F_{p_i}$ is perfect, since all irreducible polynomial is linear.
>
> Let $f$ be trancedental, $F_p(t)$ is not perfect
>
> $\overline{F_p(t)}$ is perfect since linear

#### Simple
A finite seperable extension is `simple` if $E/F$ is finite and seperable. Then $\exists r \in E$ s.t. $E = F(r)$.

> Since $E/F$ is finite, then $E = F(a_1, \ldots, a_n)$ for some algebraic $a_i$. Induct on $n$. $E = F(a_1, \ldots, a_{n-1})(a_n) = F(\beta, a_n) = F(r)$
>
> Let $E = F(\alpha, \beta)$, let $f(x), g(x)$ be 
>
> 2312220/2027