# 1. Show that $\Z[\sqrt{-2}]$ is an Euclidean domain. Furthermore, it is UFD.
Denote $\sqrt{-2}$ as $\lambda$ for short. Let $N(a + b\lambda) = a^2 + 2b^2$. $N(a + b\lambda) = 0$ iff $a = b = 0$

$\forall a,b \in \Z[\sqrt{-2}]$, let $a = u + v\lambda, b = x + y\lambda$. 

Then do Euclidean algorithm to abtain $a = qb + r$ as following:
1. Approx $q$ with $ab^{-1}$: Let $ab^{-1} = s + t\lambda$, where $s,t \in \mathbb{Q}$. Then $\exists m,n \in \Z$ such that $|s - m| \leq \frac{1}{2}, |t - n| \leq \frac{1}{2}$. Let $q = m + n \lambda$, then $r = b(ab^{-1} - q) = b[(s - m) + (t - n)\lambda]$. 
2. $N(r) =N(b)[ (s - m)^2 + 2(t - n)^2 ]\leq N(b)(\frac{1}{4} + \frac{2}{4}) = N(b)\frac{3}{4} <  N(b)$.
   
So $\Z[\sqrt{-2}]$ is an Euclidian Domain with $a = (m+ n\lambda) b + b[(s-m) + (t-n)\lambda]$, and thus PID, UFD
# *2. Show that $\Z[\frac{1+\sqrt{-3}}{2}]$ is an Euclidean domain. Furthermore, it is UFD (Hint: a possible solution is trying to use $\Z[\frac{1}{2}]$).
Denote $\frac{1+\sqrt{-3}}{2}$ as $\lambda$ for short. Let $N(a + b\lambda) = (a + b/2)^2 + 3/4 b^2 = a^2 + ab + b^2$. $N(a + b\lambda) = 0$ iff $a = b = 0$

Do Euclidean algorithm to abtain $a = qb + r$ as following:
1. Approx $q$ with $ab^{-1}$: Let $ab^{-1} = s + t\lambda$, where $s,t \in \mathbb{Q}$. Then $\exists m,n \in \Z$ such that $|t - n| \leq \frac{1}{2}, |m - [s + \frac{1}{2}(t - n)]| \leq \frac{1}{2}$. Let $q = m + n \lambda$, then $r = b(ab^{-1} - q) = b[(s - m) + (t - n)\lambda]$.
2. $N(r) =N(b)[(s-m + \frac{1}{2}(t -n))^2 + \frac{3}{4}(t-n)^2]\leq N(b)(\frac{1}{4} + \frac{3}{4}\frac{1}{4}) = N(b)\frac{7}{16} <  N(b)$.

So $\Z[\frac{1+\sqrt{-3}}{2}]$ is an Euclidian Domain. The main difference is that $u,v$ are not independent, so simply go for $|m - s|<1/2$ will loosen the bound to $3/4$.
# 3. Show that polynomial ring $F[x]$ over a field $F$ is an Euclidean ring. This is a stronger version of Question 4, Assignment 9. (Hint: Define a map $\phi: F[x] \to \N_{\geq 0}$, which maps $f(x)$ to $2^{\deg(f)}$. By convention, we define $\deg(0) = -\infty$ and $2^{-\infty} = 0$)
Let $\phi: F[x] \to \N_{\geq 0}$, $f(x) = 2^{\deg(f)}$. By convention, we define $\deg(0) = -\infty$ and $2^{-\infty} = 0$

$\forall f(x), g(x) \in F[x]$, we lookup for $f(x) = q(x)g(x) + r(x)$ as following:
1. If $\deg f < \deg g$, then $q(x) = 0, r(x) = f(x)$.
2. If $\deg f \geq \deg g$, apply long division we can iteratively cancel the highest term, resulting an $r(x)$ with $\deg r < \deg g$. Namely, $r(x) = f(x) - q(x)g(x)$ and $\phi(r) = 2^{\deg r} < 2^{\deg g} = \phi(g)$.

# 4. Let $R$ be a domain. Show that $U(R[x]) = U(R)$.
Clearly $U(R) \subseteq U(R[x])$.

Conversely, $\forall f\in R[x]$, if $f$ is a unit, then $\exists g \in R[x]$ such that $fg = 1$. Since $R$ is a domain, $\deg fg =\deg f + \deg g = 0$, so $\deg f = \deg g = 0$, and thus $f,g \in R$. So $f \in U(R)$.

# 5. Let $E$ be a field. Determine the fractional field of the integral ring $E[x]$ and then the extension degree $[F: E]$.
The fractional field $F$ of $E[x]$ is 
$$F = \{ \frac{f(x)}{g(x)} \mid f(x), g(x) \in E[x], g(x) \neq 0 \}$$
Seems cannot get a simpler form.

$[F: E] = \infty$ since $[E[x]: E] = \infty$ already.


# 6. Suppose $u \in \Complex$ is a root of polynomial $f(x) = x^4 + x^3 + x^2 + x+ 1$.
## (a) Compute $[\mathbb{Q}(u) : \mathbb{Q}]$.
$u$ is algebraic since $f(u) = 0$.

$Q(u) = \operatorname{span}(1, u, u^2, u^3)$. Only prove for multiplication inverse. 

$\forall v = \sum_{i=0}^3 a_iu^i \in Q(u)$, consider the poly $v(x) = \sum_{i=0}^3 a_ix^i \in \mathbb{Q}[x]$. Since $f(x)$ is irreducible, $\gcd(f(x), v(x)) = 1$. So $\exists p(x), q(x)$ such that $p(x)f(x) + q(x)v(x) = 1$. So $0 + q(u)v(u) = 1$, and $v(u)^{-1} = q(u)$. 

So basis are $1, u, u^2, u^3$, and $[\mathbb{Q}(u) : \mathbb{Q}] = 4$.
## (b) Try to express $u^5,(u+ 2)^{-1}$ as $\mathbb{Q}$-linear combination of $1,u,u^2,u^3$.
$$
\begin{cases}
u^5 = 1 + (u - 1)f(u) = 1\\
(u+2)^{-1} = (u^3 - u^2 + 3u - 5)/{-11}
\end{cases}
$$
# 7. Assume $f(x) = \sum_{i=0}^n a_ix^i$ is a monic polynomial (i.e. $a_n = 1$) and $p$ is a prime number. Denote the image of $a \in \Z$ under the canonical homomorphism $\Z \to \Z_p$ by $\bar{a}$. Let $\bar{f}(x) = \sum_{i=0}^n \bar{a}_ix^i$.
## (a) If there exists some prime $p$ such that $\bar{f}(x)$ is irreducible over $\Z_p[x]$, show that $f(x)$ is irreducible over $\Z[x]$.
Suppose $f(x) = g(x)h(x)= \sum_{i=0}^n a_ix^i$, then $\bar g(x) \bar h (x) = \overline{\sum_{i=0}^n a_ix^i} = \bar f(x)$. Since $\bar f(x)$ is irreducible, WLOG $\bar g(x)$ is a unit, so $\deg \bar g(x) = 0$, let $g(x) =\lambda + \sum_{i = 1}^n pk_ix^i$.

Since $a_n = 1$, $k_i = 0$, otherwise $pk_i * h_m > 1$. Similarly, $\lambda = U(1) = \pm 1$, so $g(x)$ is a unit and  $f(x)$ is irreducible.
## (b) If $f(x)$ is not monic, is the result (a) still correct?
No, non-monic leading coeff cannot restrict $k_i, \lambda$. Consider $f(x) = 2x^2 - x - 1$, then $\bar f(x) = x + 1$ is irreducible over $\Z_2[x]$, but $f(x) = (2x + 1)(x-1)$ is reducible over $\Z[x]$.