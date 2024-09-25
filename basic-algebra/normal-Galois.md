> Example for splitting field
>
> Assume $f(x)$ is an irr. poly of deg $n$ in $F[x]$, and $\gamma$ is a root of $f(x)$
>
>  $$ F_q(\gamma) = \{a_0 + a_1\gamma + \cdots + a_{n-1}\gamma^{n-1} \mid a_i \in F_q\}$$
>
> $f(\gamma^{q^m}) = f(\gamma)^{q^m} = 0$ since $f(\gamma) = 0$. So $\gamma^{q^m}$ is a root of $f(x)$.
> $$\gamma, \gamma^q, \ldots, \gamma^{q^{n-1}} \in F_q(\gamma)$$
> So $F(\gamma)$ is a splitting field of $f(x)$ over $F$.
>
> ---
>

# Normal Field Extension
Let $E/F$ be a field extension. $E$ is called a `normal extension` of $F$ if every irreducible poly $f(x) \in F[x]$, $f(x)$ has either zero or all roots in $E$.

#### F - embedding
> For field, the kernel of homomorphism is either $0$ or the whole field.

Let $F$ be a field and $\Omega$ be an algebraically closed field containing $F$. Assume $E/F$ is a field extension and $E \subset \Omega$. An embedding $E \to \Omega$ is a $F$-embedding if $\forall a \in F$, $a \mapsto a$, a.k.a., $f|_F = \operatorname{id}_F$.

> Assume $\alpha \in \Omega$, and $E = F(alpha)$. Let $f(x)$ be the min poly of $\alpha$ over $F$. Then $f(x)$ has all roots in $\Omega$. So $f(x) = \prod_{i=1}^n (x - \alpha_i)$. So $E = F(\alpha_1, \ldots, \alpha_n)$. So $E \cong F(\alpha_1, \ldots, \alpha_n)$.

####
Any F-embedding $\sigma: F(\alpha) \to \Omega$, $\alpha \mapsto \beta$ is determined by $\beta$.
$$0 = \sigma(f(\alpha)) = \sigma_{F[x]}(f)(\sigma(\alpha)) = f(\sigma(\alpha))$$
so $\sigma$ maps a root to a root

#### Conjugate
$\forall \alpha\in E$, let $f(x)$ be its mininal polynomial over $F$. All its roots in $\Omega$ are called conjugates of $\alpha$ over $F$.

####
Let $F \subset E \subset \Omega$. Let $\alpha \in E$, then $\beta \in \Omega$ is a conjugate of $\alpha$ iff there exists a $F$-embedding $\sigma: E \to \Omega$ s.t. $\sigma(\alpha) = \beta$.

Only prove this for $[E:F] <\infty$.

> $\Leftarrow$: $0 = \sigma(f(\alpha)) = \sigma(f)(\sigma(\alpha)) = f(\sigma(\alpha)) = f(\beta)$
>
> $\Rightarrow$: (by induction)
>
> Suppose $E = F(\gamma_1, \ldots, \gamma_n)$
> 
> If $n = 1$, done.
>
> For $n - 1$, by assumption we have an F-embedding $\sigma_{n-1}: E_{n-1} = F(\gamma_1, \ldots, \gamma_{n-1}) \to \Omega$.
>
> Let $g(x)$ be the minimal polynomial of $\gamma_n$ over $E_{n-1}$. Since $E_n = E_{n-1}(\gamma_n)$, try to look for a $\tilde{\sigma}: \tilde{\sigma}|_{E_{n-1}} = \sigma_{n-1}$.
>
> $$0 = \tilde{\sigma}(g(\gamma_n)) = \tilde{\sigma}(g)(\tilde{\sigma}(\gamma_n)) = \sigma_{n-1}(g)(\tilde{\sigma}(\gamma_n)) = g(\tilde{\sigma}(\gamma_n))$$
> 1. Take any root of $\sigma_{n-1}(g)$ in $\Omega$, say $\delta$.
> 2. $\tilde{\sigma} = \begin{array}{l}
    \tilde{\sigma}(\gamma_n) = \delta\\
    \tilde{\sigma}|_{E_{n-1}} = \sigma_{n-1}\end{array}$

#### Corallary 
If $E/F$ is a finite extension, then the number of F-embeddings $\sigma: E \to \Omega$ is less than or equal to $[E:F]$.

> Let $E = F(\gamma_1, \ldots, \gamma_n)$, then $\sigma$ is determined by $\sigma(\gamma_1), \ldots, \sigma(\gamma_n)$. So the number of $\sigma$ is less than or equal to the number of choices of $\sigma(\gamma_1), \ldots, \sigma(\gamma_n)$, which is $[E:F]$.
>
> The number of distinct roots of $\sigma(g(x))$ where $g(x)$ is the minimal poly. of $\gamma_n$ over $E_{n-1}$. $\leq [E_n: E_{n-1}]$
>
> $$\#\{\sigma: E_{n-1} \to \Omega\} \leq [E_{n-1}:F]$$
> $$\#\{\sigma: E_n \to \Omega\} \leq [E_n:E_{n-1}]\cdot [E_{n-1}:F]$$


## 
Let $E/F$ be a finite extension, the following are equivalent.
1. $E/F$ is normal.
2. For any F-embedding $\sigma: E \to \Omega$, $\sigma(E) = E$.
3. $\forall \alpha \in E$, all conjugates of $\alpha$ over $F$ are in $E$.

----

### Some Results without Proof
A finite extension $E/F$ is normal iff $E$ is the splitting field of some poly in $F[x]$.

1. Assume $E/F$ and $F/K$ are algebraic extension. If $E/K$ is normal, then $E/F$ is also normal.
2. IF $E_i/F$ is normal, then $\bigcap E_i/F$ is also normal.

# Galois Extension
Let $E/F$ be a finite extension. If $E/F$ is normal and separable, then $E/F$ is called a `Galois extension`. The group of automorphisms of $E$ fixing $F$ is called the `Galois group` of $E/F$.