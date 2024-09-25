# 1. Let $E_1/F$ and $E_2/F$ be field extensions, and $[E_1 : F ] = n$, $[E_2 : F ] = m$. Show that $[E_1E_2 : E_1] \leq m$ and $[E_1E_2 : E_2] \leq n$.

Only need to show $[E_1E_2: E_2] \leq n$, the other is symmetric.

Suppose $E_1 = \operatorname{span}_F (a_1, a_2, \ldots, a_n)$, $E_2 = \operatorname{span}_F (b_1, b_2, \ldots, b_m)$. Then 
$$E_1E_2 \subset \operatorname{span}_{E_2}(a_1, a_2, \ldots, a_n)$$
since $\operatorname{span}_{E_2}(a_1, a_2, \ldots, a_n)$ is a field containing both fields and $E_1E_2$ is smallest. (Actually I think they are equal)

Note that $a_1, a_2, \ldots, a_n$ may not linearly independent over $E_2$, $[E_1E_2: E_2] \leq \dim_{E_2}(a_1, a_2, \ldots, a_n) \leq n$. 



# 2. For any prime $p$ and integer $n \geq 1$, show that there exists an irreducible polynomial of degree $n$ over $\mathbb{Z}_p$.

Let $F = \mathbb{Z}_p$, $E = \mathbb{F}_{p^n}$. Then $E/F$ is a field extension. Since $E^* = E/\{0\}$ is cyclic, let $\alpha$ be a generator of $E^*$. Then $E = F(\alpha)$. Let $f(x)$ be the minimal polynomial of $\alpha$ over $F$. Then $f(x)$ is irreducible and $\deg f(x) = [E:F] = n$.

(To avoid circular argument, the existence of $F_{p^n}$ can alternatively be shown as the splitting field of $x^{p^n}−x\in F_p[x]$)

# 3. Find the minimal polynomial of $\sqrt{2} + \sqrt{3}$ over the field $K$, where
### (a) $K = \mathbb{Q}$
$$f(x) = (x^2 - 5)^2 -24$$

### (b) $K = \mathbb{Q}(\sqrt{3})$
$$f(x) = (x - \sqrt{3})^2 -2$$
### (c) $K = \mathbb{Q}(\sqrt{6})$
$$f(x) = (x - \sqrt{2} - \sqrt{3})$$

# 4. Assume that $K/F$ is a field extension and $f(a)$ is algebraic over $F$, where $f(x)$ is of positive degree in $F[x]$ and $a \in K$, show that $a$ is algebraic over $F$.

Let $g(x)$ be the minimal polynomial of $f(a)$ over $F$. Then $g(f(a)) = 0$. 

Let $h(x) = g(f(x))$. Then $h(x) \in F[x]$ and $h(a) = 0$. So $a$ is algebraic over $F$.

# 5. Assume that $K/F$ is an algebraic extension of field. Let $D$ be a domain and $F \subseteq D \subseteq K$. Show that $D$ is a field.

Let $E = F(D)$, that is, the smallest field containing $D$. Then equivalent to show $E = D$.

$\forall \alpha \in E - D$, $\alpha \in K$. Since $K/F$ is algebraic, $F(\alpha) = \operatorname{span}_F(1. \alpha, \ldots, \alpha^{n-1})\subset D$. So $\alpha \in D$. Contradiction. $E = D$

# 6. Construct two fields $K$ and $F$ such that $K$ is an algebraic extension of $F$ but not a finite extension of $F$.

Let $F = \mathbb{Q}$, $K = \mathbb{Q}(\sqrt{2}, \sqrt{3}, \ldots)$.

# 7. Let $K/F$ be a field extension.
## (a) If $[K : F ]$ is prime, show that $K = F (u)$, where $u$ is any element such that $u \in K, u \notin F$.

$\forall u \in K - F$, $[K: F] = [K:F(u)][F(u):F]$. Since $[K:F]$ is prime and $[F(u): F] \neq 1$, $[K:F(u)] = 1$. So $K = F(u)$ for any $u \in K - F$

## (b) If $u \in K$ satisfies $[F (u) : F ]$ is odd, show that $F (u) = F (u^2)$.

Suppose $[F(u): F] = s < \infty$. By finite, $u$ is algebraic, so $F(u) = \operatorname{span}_F(1, u, \ldots, u^{s-1})$.

Also $F(u^2) \subset F(u)$, which implies $[F(u): F(u^2)] \leq 2$. Since $[F(u): F] = [F(u): F(u^2)][F(u^2): F]$ is odd, $[F(u): F(u^2)] = 1$. So $F(u) = F(u^2)$.


# *8. If $p$ is a prime, find the minimal polynomial of $\cos \frac{2\pi}{p} + i \sin \frac{2\pi}{p}$ over $\mathbb{Q}$.

Let $\alpha = \cos \frac{2\pi}{p} + i \sin \frac{2\pi}{p} = e^{\frac{2\pi i}{p}}$. 

If $p = 2$, $\alpha = -1$. So $f(x) = x + 1$.

If $p \geq 3$, $p$ is odd, $\alpha^p = 1$. So $\alpha$ is a root of $x^p - 1$. Since $p$ is prime, $x^p - 1 = (x-1)(x^{p-1} + x^{p-2} + \ldots + x + 1)$. So $\alpha$ is a root of $x^{p-1} + x^{p-2} + \ldots + x + 1$.

The minimal polynomial of $\alpha$ is $x^{p-1} + x^{p-2} + \ldots + x + 1$, since it's irreducible