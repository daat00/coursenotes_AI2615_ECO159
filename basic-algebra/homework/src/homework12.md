# 1. Show that $\bigcup_{i=1}^\infty F_{p^i}$ is algebraically closed.
Denote $E = \bigcup_{i=1}^\infty F_{p^i}$. We are going to prove that $E = \overline{F_p}$.

1. $E/F_p$ is algebraic since $\forall e\in E$, $\exist j$ s.t. $e\in F_{p^j}$, and $F_{p^j}$ is algebraic over $F_p$.
2. $\forall f(x) \in F_p[x]$, let $\alpha$ be a root of $f(x)$. Then the minimal polynomial of $\alpha$ is a factor of $f(x)$, let its degree be $m$. So we have $F_p(\alpha) = F_{p^m} \subset E$. So $\alpha \in E$.

Thus $E = \overline{F_p}$, and $E$ is algebraically closed.

# 2. Show that no finite field is algebraically closed.

Suppose $F_{p^n}$ is algebraically closed for some $p,n$. Then $\overline{F_p} = F_{p^n}$. However, consider a irreducible polynomial $f(x) \in F_p[x]$ of degree $n+1$, let $\alpha$ be a root of $f(x)$. Then $f(\alpha) = F_{p^{n+1}}$. So $\alpha \notin F_{p^n}$,  indicating $F_{p^n} \neq \overline{F_p}$, which is a contradiction.

# *3. Assume $f(x) \in F_p[x]$ is a monic irreducible polynomial of degree $n$. Let $u$ be a root of $f(x)$ and $F = F_p(u)$ be a finite field. Show that:
### (a) $f(x)$ has $n$ distinct roots $u, u^p, \ldots, u^{p^{n-1}}$.
$$
\sum a_i u^i = 0

\sum a_i u^{pi} = \sum a_i^p u^{pi} = (\sum a_i u^i)^p = 0
$$

**I TRIED MY BEST, AND STILL DONT KNOW**

### (b) If $u$ is the generator of the multiplicative cyclic group $F^*$, then every root of $f(x)$ is also the generator of $F^*$. In this case, we call $f(x)$ the primitive polynomial of $F_p[x]$ of degree $n$.


Note $|F^*| = p^n - 1$, and $\gcd(p^k, p^n - 1) = 1$, $\forall k \leq n-1$. 

So $rp^k + s(p^n - 1) = 1$, $(u^{p^k})^r = u^{rp^k} = u^{1 - s(p^n - 1)} = u$. So $u^{p^k}$ is a generator of $F^*$.

### (c) There are $\phi(p^n - 1)/n$ primitive polynomial of degree $n$, where $\phi$ is the Euler function.

Let the number of primitive polynomial of degree $n$ be $L$. Then the number of generators of $F^*$ is $L \times n$, since roots are distinct. Note that $F^*$ is cyclic and thus has $\phi(p^n - 1)$ generators. So $L n = \phi(p^n - 1)$, $L = \phi(p^n - 1)/n$.
# 4. Let $f(x) = x^4 + x^3 + x^2 + x + 1 \in F_2[x]$ be a polynomial.
### (a) Show that $f(x)$ is irreducible over $F_2[x]$ but not primitive.

> **primitive element** is a generator of the multiplicative group, **primitive polynomial** is the minimal polynomial of the primitive element.

If $f(x)$ is reducible, then it must have a factor with degree $1,2$
- For linear factor, $f(0) = f(1) = 1$, impossible.
- For square factor, $f(x) = (x^2 + ax + b)(x^2 + cx + d)$. Compare the coefficient, $bd = 1$, $f(x) = (x^2 + ax + 1)(x^2 + cx + 1)$, then $a+c = 1$ for cubic term, and $1 + ac + 1 = 1$, so $ac = 1$, impossible.

So $f(x)$ is irreducible, and because $f(1) = 1$, $f(x)$ is not primitive.

### (b) Let $u$ be a root of $f(x)$, then we have $F_{16} = F_2(u)$. Try to find the generator of multiplicative group $F^*_{16}$.

$F_{16} = \operatorname{span}_{F_2}(1, u, u^2, u^3)$. $|F_{16}^*| = 15$. 

$\operatorname{ord} u = 5$, since $u^5 = 1$, and $u^4 = u^3 + u^2 + u + 1$. 

$\operatorname{ord} u^2 = 15$, since $(u^2)^3 = u^6 = u$.

So generator is $u^2$.

# 5. Let $F$ be a field with characteristic $\operatorname{char}(F) = p>0$. Assume that $f(x)$ is an irreducible polynomial in $F[x]$. Show that all roots of $f(x)$ have the same multiplicity $p^n$ for some $n\geq 0$.

WLOG suppose $f(x)$ is monic.

Since $f(x)$ is irreducible and $\operatorname{char} F = p$, $f(x)$ either has no multiple roots or $f(x) = g(x^p)$ for some $g(x) \in F[x]$.

If $f(x)$ has no multiple roots, then we are done with a $n = 0$ case.

Otherwise, $f(x)$ has multiple roots and $f(x) = f_1(x^p)$ for some $f_1(x)\in F[x]$. Since $f(x)$ is irreducible, $f_1(x)$ is irreducible and monic. Repeat the process, we have $f(x) = f_n(x^{p^n})$ for some $n$, and $f_n(x) = \prod_{i = 1}^t (x - \alpha_i)$, $\alpha_i \in \bar F$ are distinct.

Furthermore, we know that $F$ is perfect, so $\alpha_i = \beta_i^p = \cdots = \gamma_i^{p^n}$, $f_n(x^{p^n}) = \prod_{i=1}^t (x^{p^n} - \gamma_i^{p^n}) = \prod_{i=1}^t (x - \gamma_i)^{p^n}$. So $\gamma_i, \forall i$ are distinct roots of $f(x)$ with multiplicity $p^n$.

# 6. Assume that $\alpha$ is separable over $F$, $\beta$ is separable over $F(\alpha)$. Show that $\beta$ is separable over $F$.

> $\alpha$ is separable over $F$ if it is algebraic over $F$, and if its minimal polynomial $f(x)$ is separable, which means $f(x)$ has no multiple roots in $\overline{F}$.

#### Proof for algebraic
Since $\beta$ is algebraic over $F(\alpha)$, $[F(\alpha, \beta): F(\alpha)] < \infty$, so we have$[F(\alpha, \beta): F] = [F(\alpha, \beta): F(\alpha)][F(\alpha): F] < \infty$. 

So $[F(\beta): F] \le [F(\alpha, \beta): F] < \infty$ and $\beta$ is algebraic over $F$.

#### Proof for separable

Let $h(x)\in F[x]$ be the minimal polynomial of $\beta$ over $F$, $g(x) \in F(\alpha)[x]$ be the minimal polynomial of $\beta$ over $F(\alpha)$. Since $F \subset F(\alpha)$, $g(x) \mid h(x)$.

Suppose $\beta$ is inseparable over $F$, then $\operatorname{char} F = p< \infty$ and $h(x) = k(x^p)$ for some $k(x) \in F[x]$. Since $g(x) \mid h(x)$, $g(x) = k(x^p)$ for some $k(x) \in F(\alpha)[x]$. So $\beta$ is inseparable over $F(\alpha)$, which is a contradiction.

<!-- Since $\beta$ is separable over $F(\alpha)$, let its minimal polynomial be $g(x)$. Then $g(x) = \prod_{i=1}^n (x - b_i), b_i \in \overline{F(\alpha)}$. Since $\alpha$ is separable over $F$, $\alpha$ is separable over $F(\alpha)$, so $\alpha$ is separable over $\overline{F(\alpha)}$. So $\alpha$ is separable over $\overline{F(\alpha)}$. -->

# 7. Assume that $E/F$ is a separable extension. Let $M$ be an intermediate field such that $F\subset M \subset E$. Show that $E/M$ and $M/F$ are both separable.

If $E/F$ is separable then every element of $E$ is separable over $F$, so every element of $M \subset E$ is separable over $F$. Thus $M/F$ is separable.

Now we prove that $\forall a\in E$, $a$ is separable over $M$. Since $a$ is separable over $F$, let its minimal polynomial be $f(x) \in F[x]$ and $g(x) \in M[x]$, then factorize $f(x) = g(x)h(x)$ in $M[x]$, 
1. by separable over $F$, we have $\gcd(f(x), f'(x)) = 1$, so $uf + vf' = 1$
2. $uf + vf' = ugh + vg'h + vgh' = (uh + vh')g + vhg' = 1$, so $g(x)$ is separable over $M$.

So $a$ is separable over $M$, and $E/M$ is separable.