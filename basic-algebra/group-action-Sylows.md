# Group Actions
$G$ is a group, $\Sigma$ is a set. A map $\pi: G \times \Sigma \rightarrow \Sigma$ is called a `group action` of $G$ on $\Sigma$ if
- $\pi(e, z) = z$ for all $z\in \Sigma$
- $\pi(g_1, \pi(g_2, z)) = \pi(g_1g_2, z)$ for all $g_1, g_2 \in G$ and $z\in \Sigma$

We denote $\pi(g, z)$ as $g\cdot z$.
- $(g_1g_2)\cdot x = g_1\cdot(g_2\cdot x), \forall g_1,g_2\in G$
- $1 \cdot x =x, \forall x\in \Sigma$

> Left regular action: $G$ acts on itself by left multiplication.
> $$g\cdot x = gx$$
>
> Right regular action: $G$ acts on itself by right multiplication.
> $$g\cdot x = xg^{-1}$$
>
> Conjugation action: $G$ acts on itself by conjugation.
> $$g\cdot x = gxg^{-1}$$
>
> Let $G$ be a group and $H \leq G$. $G$ acts on the set of left cosets of $H$ in $G$ by left multiplication.
> $$g\cdot (xH) = (gx)H$$
>
> Let $G$ be a group and $H \leq G$. $G$ acts on the set of right cosets of $H$ in $G$ by right multiplication.
> $$g\cdot (Hx) = (Hx)g^{-1}$$
>
> Let $G = GL_n(\Reals)$ and $V = \Reals^n$. $G$ acts on $V$ by matrix multiplication.
> $$A\cdot v = Av$$

## Theorom
A group action of $G$ on $\Sigma$ is **equivalent** to a homomorphism $\pi: G \rightarrow S(\Sigma)$, where $S(\Sigma)$ is the group of permutations of $\Sigma$.

$$f(g) = \pi(g)(z) = g\cdot z \in S(\Sigma)$$

## Cayley's Theorem
Every group $G$ is isomorphic to a subgroup of $S_G$.

Left regular action $f(g): x \mapsto gx$, where $x\in G$.
$$g \mapsto f(g) \in S(G)$$


## Kernel
The kernel of a group action $\pi: G \rightarrow S(\Sigma)$ is the set
$$\ker \pi =\{g \in G: f(g) = I\}  =\{g\in G: g\cdot z = z, \forall z\in \Sigma\}$$

## Orbit
Let $G$ be a group, $\Sigma$ be a set, and $\pi: G \times \Sigma\rightarrow \Sigma$ be a group action. We say that two elements $x,y\in \Sigma$ are `equivalent` if $\exists g\in G$ such that $g\cdot x = y$.

The `orbit` of $x\in \Sigma$ is the equivalent class $[x]$, or say, $G \cdot x$
$$O(x) = G\cdot x = \{g\cdot x \in \Sigma: g\in G\}$$

If only one orbit, then the action is called `transitive`.

> - Both regular actions are transitive.
> - Conjugation action is NOT transitive.
>
> $G = GL_n(\Reals)$, $X = \Reals^n$, $g \cdot x = gx$ has two orbits

### Stablizer
Let $G$ be a group, $\Sigma$ be a set, and $\pi: G \rightarrow S(\Sigma)$ be a group action. The `stablizer` of $x\in \Sigma$ is
$$G_x = \{g\in G: g\cdot x = x\} \leq G$$

> Proof for subgroup
>- $(g_1g_2)x = g_1(g_2x) = x$
>- $x = g^{-1}gx = g^{-1}x$

Also
$$
\ker  = \bigcap_{x\in \Sigma} G_x
$$

### Orbit-Stablizer Theorem
Let $G$ act on $\Sigma$, and $\pi: G \rightarrow S(\Sigma)$ be a group action. Then for any $x\in \Sigma$,
$$|O(x)| = [G: G_x]$$

In particular, if $G$ is finite, then
$$|G| = |O(x)| \cdot |G_x|$$

> Define the map
> $$f: \begin{array}{ll}
> O(x)= Gx & \rightarrow G/G_x \\
> g\cdot x & \mapsto gG_x
> \end{array}$$


### Corollary
If $G$ act on $X$ is transitive, then for any $|X| = |O(x)| = [G: G_x]$

> Let $|G| < \infty$, $H,K \leq G$.

# Sylow Theorems
> All below groups are finite

Let $p$ be a prime number. A group $G$ is called a `p-group` if every element of $G$ has order a power of $p$.

Remark: If $|G| = p^n$, then $G$ is a `p-group`.

## Sylow p-subgroup
Let $G$ be a group, $p$ is a prime.

Write $|G| = p^r m$, for $r,m \geq 1, \gcd(p,m) =1$. A subgroup $H$ of $G$ is called a `p-subgroup` of $G$ if $|H| = p^r$. 

## Sylow Theorems
Let $G$ be a group, $p$ is a prime. Assume $p \mid |G|$. Then 
1. $n_p \equiv 1 \mod p$, where $n_p =$#Sylow p-subgroups of $G$.
   > Proof is omitted
2. There is at least one Sylow p-subgroup of $G$.
3. Any two Sylow p-subgroups of $G$ are conjugate.
   > Conjugate: $\forall P,Q \in \Sigma, g P g^{-1} = Q$.

   > [proof](#Appendix)

4. $n_p = [G: N_G(P)]$, where $n_p =$#Sylow p-subgroups of $G$ and $P$ is a Sylow p-subgroup of $G$
   > $N_G(P) = \{g\in G: gPg^{-1} = P\}$ is the normalizer of $P$ in $G$.
   
   > $N_G(P) = P_P$. $n_p = |\Sigma| = [G: P_P]$

### Thereom
1. If $p \mid |G|$, then $G$ has an element of order $p$.
   
   > $\operatorname{ord} x = p^t$, $\operatorname{ord} x^{p^{t-1}} = p$
2. If $G$ is a p-group, then $|G| = p^n$.
   
   > exists a prime $q \neq p$, $q \mid |G|$, $\exists x, \operatorname{ord} x = q$. 
3. A Sylow p-subgroup of $G$ is normal in $G$ if and only if it is the unique Sylow p-subgroup of $G$.
   
   > $Q  = gPg^{-1} = P$
4. If $H\leq G$ is a p-group, then $H$ is contained in a Sylow p-subgroup of $G$.

   > Let $\Sigma$ be the set of Sylow p-subgroups of $G$. $G$ acts on $\Sigma$ by conjugation. 
   > $$H \times \Sigma \rightarrow \Sigma$$
   > $$(h, P) \mapsto hPh^{-1}$$ 
   > $$n_p = \sum_{Q\in I} |B(Q)| \equiv 1 \mod p$$
   > for some $I\subset \Sigma$
   > $$ |B(Q)| \mid |H| = p^k$$
   > $\exists P \in \Sigma$, such that $|B(P)| = 1$. So the stablizer $H_p = H$
   > $$PH = HP = P$$
   > since $P$ is a Sylow group, $|PH| = \frac{|P||H|}{|P\cap H|} = p^k$, $p^r = |P| \leq |PH| \leq p^r$. $PH = P$, $H \subset P$.



# Appendix
## Proof for Sylow Theorem 3
Let $\Sigma$ be the set of all Sylow p-subgroups of $G$.

Consider the group action
$$
\begin{gather*}
   G \times \Sigma \rightarrow \Sigma\\
   (g, P) \mapsto gPg^{-1}
\end{gather*}
$$
Equivalent to proof this action has only one orbit.

Suppose there is an orbit $\Delta$ and two Sylow p-subgroup $P, P'$ such that $P \in \Delta$, $P' \notin \Delta$.

Consider $\Delta$, let 
$$
\begin{gather*}
   P \times \Delta \rightarrow \Delta\\
   (g, Q) \mapsto gQg^{-1}
\end{gather*}
$$
The stablizer of $P$ is $P_P = \{g\in P: gPg^{-1} = P\} = P$.
So the orbit for $P$ is $\{P\}$

By the way, we claim that $|O(Q)| > 1$. So $|\Delta| = \sum_{Q} |O(Q)| = 1 + \sum_{Q \neq P} |O(Q)| = 1+ \sum p^\lambda \equiv 1 \mod p$. For $P'$, $|\Delta| = \sum p^\lambda \equiv 0 \mod p$. Contradiction.