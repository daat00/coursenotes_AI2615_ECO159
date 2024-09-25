# 1. Let G = $GL_n(R)$. Show that the center of G is $C(G) = \{aI_n : a ∈R^∗\}$.

Solution

The center of $G$ is the set $C_G(G)$.
$$C_G(G) = \{g\in G: ga = ag, \forall a\in G\}$$

$\forall g\in C_G(G)$, consider $a\in \{E_{ij} + I\}$, then 
$$
g(E_{ij} + I) =g + ((g_1, \ldots, g_n) E_{ij}) = (0,\ldots, g_i, \ldots, 0)
$$
$$
(E_{ij} + I) g = g + E_{ij}\begin{pmatrix}
g_1\\
\vdots\\
g_n
\end{pmatrix} = g + \begin{pmatrix}
0\\ 
\vdots\\
g_j\\
\vdots\\
0
\end{pmatrix}
$$

we should have $(0,\ldots, g_i, \ldots, 0) =  \begin{pmatrix}
0\\
\vdots\\
g_j\\
\vdots\\
0
\end{pmatrix}$, which implies $g_{ij} = 0$ for all $i\neq j$, which implies $g$ is diagonal. WLOG suppose $g = \operatorname{diag}(\lambda_1, \ldots, \lambda_n)$

Let $P_{ij}$ be the permutation matrix that exchanges the basis vector $e_i$ with $e_j$, note that $P^{−1}_{ij}=P_{ji}$, and therefore $P_{ij}\in GL_n(R)$. We have
$$(Pg)e_j=λ_jPe_j=λjei$$
$$APe_j=Ae_i=λ_ie_i$$
So to commute with $Pij$, $λi=λj$ for all i, j, and hence $g=λI$

Note that $\lambda I\in GL_n(R)$ iff $\lambda \neq 0$, $g = aI_n, a\in R^*$. 


# 2. Let $M, N$ be normal subgroups of group $G$. Show that if M∩N = {e}, then ∀m ∈M, n ∈N, mn = nm.

Solution

$\forall m\in M, n\in N$, we have 
- $mn \in mN = Nm$, $mn = n_1m$
- $mn \in Mn = nM$, $mn = nm_1$

So $n_1m = nm_1$, which implies $n^{-1}n_1 = m_1m^{-1}$. Since $LHS \in N$, $RHS \in M$, we have $n^{-1}n_1 = m_1m^{-1} = e$, which implies $n_1 = n, m_1 = m$.

So $mn = nm$.

# 3. If $H$ is any subgroup of $G$ and $N = \bigcap_{a\in G} a^{-1}Ha$, show that $N \triangleleft G$.

Solution

1. $\forall a\in G$, $a^{-1}h_1 a \cdot (a^{-1}h_2 a)^{-1} = a^{-1}h_1h_2^{-1} a \in aHa^{-1}$. So $a^{-1}Ha \leq G$, and $N \leq G$
2. $\forall g \in G$, $g^{-1}Ng = g^{-1}(\bigcap_{a\in G} a^{-1}Ha)g = \bigcap_{a\in G} (g^{-1}a^{-1}Ha g)  = \bigcap_{b\in G} b^{-1}Hb = N$. So $N \triangleleft G$.

# 4. Let G be a group and $A = G ×G$. Let $T = \{(g, g)|g ∈G\}$. Show that $T \triangleleft A$ if and only if G is abelian.

Solution

$\Leftarrow$:

$\forall (a,b) \in A$, $(a,b)T(a^{-1},b^{-1}) = \{(a,b)(g,g)(a^{-1}, b^{-1}) | g\in G\} = \{(aa^{-1}g, bb^{-1}g) | g\in G\} = \{(g,g) |g\in G\} = T$.

So $T \triangleleft A$.

$\Rightarrow$:

$\forall (a,e) \in A$, since $(a,e)T = T(a,e)$, we have $\forall g \in G$,

$$(a,e)(g,g) =(ag,g) =  (g',g')(a,e)$$

so $g' = g$, which implies $ag = ga$.



# 5. Let G be a group and $N_1, \cdots, N_k$ be normal subgroups of G such that:
**(a) $G = N_1\cdots N_k$**

**(b) For each i, $N_i ∩(N_1 ···N_{i−1}N_{i+1} ···N_k) = \{e\}$.**
## Show that G is the internal direct product of $N_1, ···, N_k$.

Solution

Only need to show that under such conditions we haave *If $h_1h2\cdots h_n = h_1'h_2'\cdots h_n'$, then $h_i = h_i'$ for all $i$.*

Suppose $h_1h_2\cdots h_n = h_1'h_2'\cdots h_n'$, then 
$$h_2\cdots h_n \cdot (h_2'\cdots h_n')^{-1} = h_1^{-1} \cdot h_1'$$

Since $LHS \in N_1$, $RHS \in N_2 \cdots N_k$, we have $h_1^{-1}h_1 = e$, $h_2\cdots h_n \cdot (h_2'\cdots h_n')^{-1} = e$.

So $h_1 = h_1'$, and recursively we have $h_i = h_i'$ for all $i$.


# 6. Let C(G) be the center of G. If G/C(G) is cyclic, show that G is abelian.

Solution

$$C_G(G) = \{g\in G: gag^{-1} = a, \forall a\in G\}$$

Since $G/ C_G$ is cyclic, WLOG suppose $G/C_G = \langle b C_G \rangle$

Then $\forall a \in G$, $a = b^m g$, where $m\in Z^+, g\in C_G$.

So $\forall x,y \in G$, $xy = b^{m_x}g_1 b^{m_y} g_2 = b^{m_y} g_2 b^{m_x}g_1 = yx$. So $G$ is abelian.


# 7. If H is a cyclic subgroup of G and H is normal in G. Show that every subgroup of H is normal in G.

Solution

Since $H$ = $\langle h \rangle \triangleleft G$, 
$$\forall g \in G, ghg^{-1} = h^n \in H$$

So $\forall \langle h^j \rangle \leq H$, where $j \mid \operatorname{ord} h$,

$$\forall gh^{\lambda j}g^{-1} \in g \langle h^j \rangle g^{-1}, \ gh^{\lambda j}g^{-1}= (ghg^{-1})^{\lambda j} = h^{jn\lambda}  = (h^j)^{\lambda n} \in \langle h^j \rangle$$

So $g \langle h^j \rangle g^{-1} \subset \langle h^j \rangle$, $\langle h^j \rangle \triangleleft G$


# ∗8. Let G be a finite group of order n. For any integer 1 ≤d ≤n, G has at most one subgroup of size d. Prove that G is cyclic.

Solution

Let $a_d$ denote the number of elements of order $d$ and $u_d$ the number of cyclic subgroup of order $d$. Then we have 
$$
a_d = u_d \phi(d)
$$

So 
$$
n = \sum_{d\mid n} a_d = \sum_{d\mid n} u_d \phi(d) \leq \sum_{d\mid n} \phi(d) = n
$$

where the inequality holds iff $u_d = 1$ for all $d\mid n$.

So $u_n = 1$, which implies $G$ is cyclic.

---

MESSAGE: 

*请问作业有标答吗*

---

Below are some message to `姚逸洲`, about hw2. Just in case he is the one who is reading hw3. **IF NOT PLEASE IGNORE.**

> 这是学长在 hw2 的 canvas 留言，我没搞明白哪错了
>
> ```
> 3.a how do you obtain the claim?
> 
> 7.<= wrong
> ```

> 这是上次的解答
>
> **(a). Prove that for any positive $m$ with $m≤n$, $S_m≤S_n$.**
> 
> $\forall f, g\in S_m$, $f\circ g$ is still a permutation of $m$. So $f \circ g\in S_m$. $S_m \leq S_n$
>
> 有限子集封闭应该能说明子群？
>
> ---
> 
> 7. Prove that a group $G$ is cyclic if and only if any subgroup of $G$ has the form $G_m:= \{g^m|g∈G\}$, where $m$ is a non-negative integer.
> 
> $\Leftarrow$:
> 
> Suppose $G$ is not cyclic, then $\exists \langle g_0 \rangle \leq G, \langle g_1 \rangle \leq G$, where $\langle g_0 \rangle \cap \langle g_1 \rangle = \{e\}$.
> 
> By the proposition, $\exists m^*, G_{m^*} = \langle g_0 \rangle$
> 
> But notice that $\forall m, g_1^m \notin \langle g_0\rangle/\{e\}$. So $G_{m^*} = \langle g_0^{m^*}\rangle$. $m^* = 1$
> 
> So $G$ is cyclic.
> 
> 
> 这个是哪一步寄了啊，没看出来