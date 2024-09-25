# 1. Let $G$ be a group of order $p^n$ for some prime and an integer $n≥1$. Show that  $C(G)$ contains more than one element.

Solution 

Equivalent to show that $C(G)\setminus \{e\} \neq \emptyset$

Define a group action $\phi: G \times G \rightarrow G$ by $\phi(g, x) = gxg^{-1}$.

Then $O(x) = G \cdot x = \{gxg^{-1}: g \in G\}$, and $C(G) = \{x\in G : gxg^{-1} = x, \forall g \in G\}$. So $x \in C(G)$ iff $G\cdot x = \{x\}$, and thus
$$C(G) = \bigcup_{G \cdot x: |G \cdot x| = 1} G \cdot x$$

Suppse $O_1, O_2, \ldots, O_n$ are distinct orbits and $\bigcup O_i = G$, then 

$$|G| = \sum_{i} |O_i| = |C(G)| + \sum_{i: |O_i| >1} |O_i|$$

By orbit-stablizer theorem, $|O_i| = |G| / |G_x|$, so if $|O_i| >1$, $p\mid |O_i|$
Thus 
$$0 \equiv |G| \equiv |C(G)|\mod p$$
Since $|C(G)| \geq 1$, $|C(G)| \geq p$.

# 2. Show that every group of order $p^2$ is abelian, where $p$ is prime (This is a generalization of the result that every group of order $4$ is abelian).
Equivalent to show that $C(G) = G$.

From 1., $|C(G)| \geq p$, so $|C(G)| = p$ or $p^2$. 

Suppose $|C(G)| = p$, then $|G/C(G)| = p$, so $G/C(G)$ is cyclic. Let $x \in G$ be a generator of $G/C(G)$, then $G = \{x^i y: i = 0, 1, \ldots, p-1, y \in C(G)\}$.

For any $a, b \in G$, $a = x^i y_a, b = x^j y_b$, then $ab = x^i y_a x^j y_b = x^i x^j y_a y_b = x^{i+j} y_a y_b = x^{j+i} y_b y_a = x^j y_b x^i y_a = ba$.

Contradiction. So $|C(G)| = p^2$, $C(G) = G$.

# 3. If $|G|= p^3$ and $|C(G)|≥p^2$, show that $G$ is abelian.
Again, since $|C(G)| \equiv 0 \mod p$, $|C(G)| = p^2$ or $p^3$.

So $|G/C(G)| = p$ or $1$. If $|G/C(G)| = p$, then same as 2., $G$ is abelian. If $|G/C(G)| = 1$, then $G = C(G)$, and $G$ is abelian.

# 4. Show that any non-abelian group of order 6 is isomorphic to $S_3$.
Let $G$ be a non-abelian group of order $6$. $G$ has a 2-Sylow subgroup $H$ and a 3-Sylow subgroup $K$. 

By Lagrange Theroem, $|H| = 2$, $|K| = 3$. Clearly $H \cap K = \{e\}$, and $HK = G$.

So $G = \{1, x, y, y^2, xy, xy^2 \}$. Consider $yx$, $yx \neq xy$ by non-abelian, so $yx$ can only equal to $xy^2$. So $yx = xy^2$, $G$ is isomorphic to $S_3$.


# 5. Show that: if a finite group $G$ has a subgroup $H$ of index $n$, then $H$ contains a normal subgroup of G, whose index is a divisor of n! 
(Hint: consider the action of G on G/H by left translations).

Solution

Let $\Sigma = \{gH \mid \forall g\in G\}$, $\phi: G \times \Sigma \rightarrow \Sigma$ be a group action, where $\phi(g)(xH) = (gx)H$. This is well-defined and indeed a group action.

Consider $\ker \phi = \{g \in G\mid \phi(g) = I\}$, $\ker \phi \triangleleft G$.

Meanwhile, $\phi(G) \leq S(\Sigma)$, so $[G: \ker \phi] = |\phi(G)| \mid | S(\Sigma) | = n!$. So $[G: \ker \phi] \mid n!$.

# 6. Let $p$ be the smallest prime dividing the order of a finite group G. Show that any subgroup H of index p is normal 
(This is a generalization of the result that any subgroup of index 2 is normal).

Solution

Let $H$ be a subgroup of index $p$. Let $\Sigma = \{gH \mid \forall g\in G\}$.

Define a group action $\phi: G \times \Sigma \rightarrow \Sigma$, $\phi(x)(gH) = (xg)H$

Then from 5., $|G/\ker \phi| \mid p!$. Meanwhile, $|G/\ker\phi| \mid |G|$. So because $p$ is the smallest prime dividing $|G|$, $|G/\ker\phi| = 1\ or\  p$. But $|G/\ker\phi| \neq 1$, otherwise the index for $H$ cannot be $p$.

Also, we have $\forall g\in \ker \phi$, $\phi(g)(H) = gH = H$, $g\in H$. So $\ker \phi \triangleleft H \leq G$.

Thus $[H:\ker\phi] * [G:H] = [G:\ker\phi]$, $[H:\ker\phi] = p/p = 1$. So $H = \ker \phi$, $H \triangleleft G$.


# ∗7. Show that if $P$ is Sylow subgroup, then $N(N(P)) = N(P)$
(N(P) is the normalizer of P).

Solution

Suppose $G$ is the full set.

$$N(P) = \{g\in G \mid gPg^{-1} = P\}$$
$$N(N(P)) = \{g\in G \mid gN(P)g^{-1} = N(P)\}$$
1\. $N(P) \subset N(N(P))$:

$\forall x\in N(P)$, since $N(P)$ is a group, $xN(P)x^{-1} = N(P)$. 


2\. $N(N(P)) \subset N(P)$:

$\forall x\in N(N(P))$, by definition $xN(P)x^{-1} = N(P)$. Since $P \subset N(P)$ we have 
$$xPx^{-1} \subset xN(P)x^{-1} = N(P)$$

Since $|xPx^{-1}| = |P|$,  $xPx^{-1}$ is a Sylow subgroup of $N(P)$. So by $n_p = [N(P):N(P)] =1$, $xPx^{-1} = P$, $x\in N(P)$.


# ∗8. (Burnside Lemma)
## Let $G$ act on a set $X$. For an element $g∈G$, define $X^g= {x∈X: g·x= x}$. Assume that both $G$ and $X$ are finite, show that the number of orbits is $\frac{1}{|G|} \sum_{g\in G} |X^g|$

$$ 
\sum_{g\in G} |X_g|  = \sum_{g\in G} \sum_{x\in X} 1({g\cdot x = x}) = \sum_{x\in X} \sum_{g\in G} 1(g\cdot x = x) = \sum_{x\in X} |G_x| 
$$
where $1(\cdot)$ is the indicator function, and $G_x = \{g\in G \mid g\cdot x = x\}$

By Lagrange Theorem, 
$$
\sum_{x\in X} |G_x| = \sum_{x\in X} \frac{|G|}{|O(x)|} = |G| \sum_{x\in X} \frac{1}{|O(x)|} 
$$

Note that $X$ is the disjoint union of orbits of different elements of $X$, which is, $X = \bigcup_{x\in X} G\cdot x$. We furthermore conclude that for each $G \cdot x_0$, $\sum_{x\in G\cdot x_0} \frac{1}{|O(x)|} = 1$, and that
$$\sum_{x\in X} \frac{1}{|O(x)|} = \sum_{O \in X/G}  1 = |X/G|$$

Thus
$$
|X/G| =  \sum_{x\in X} \frac{1}{|O(x)|} = \frac{1}{|G|} \sum_{g\in G} |X_g|
$$