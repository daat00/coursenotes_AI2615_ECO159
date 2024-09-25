# 1. Let $R$ be a finite commutative ring with identity. Show that an ideal is prime if and only if it is maximal.

The $\Leftarrow$ direction is trivial, since any maximal in commutative-with-1 ring is prime. 

For the $\Rightarrow$ direction, since $R$ is finite, $R/P$ is finite, so every element of $R/P$ have an inverse. So $R/P$ is a field, and $P$ is maximal.

# 2. Let $f:R\to S$ be a surjective ring homomorphism and let $K=\ker f$.
## (a) If $P$ is a prime ideal of $R$ and $K\subseteq P$, show that $f(P)$ is a prime ideal of $S$.
By third isomorphism theorem, $(R/K)/(P/K) \cong R/P$. Since $R/K \cong S$, $P/K \cong f(P)$ we can conclude $S/f(P) \cong R/P$ and that
1. $f(P) \neq S$, because $R\neq P$
2. $f(P) \triangleleft S$, since $P/K \triangleleft R/K$

Now, for prime, equivalent to prove that $P/K$ is prime in $R/K$. 
By fourth isomorphism theorem, for any two ideals of $R/K$, they can be written as $I/K$ and $J/K$, where $I, J \triangleleft R$. Further, the product $I/K \cdot J/K = IJ/K$.

Then $I/K\cdot J/K \subseteq P/K \Rightarrow IJ/K \subseteq P/K \Rightarrow IJ \subset P \Rightarrow I \subset P \lor J \subset P \Rightarrow I \triangleleft P \lor J \triangleleft P \Rightarrow I/K \subset P/K \lor J/K \subset P/K$

## (b) If $Q$ is a prime ideal of $S$, show that $f^{-1}(Q)$ is a prime ideal of $R$.

Suppose $I,J \triangleleft R$ and $IJ\subset f^{-1}(Q)$. Then $f(IJ) = f(I)f(J)\subset Q$. Also, by homormophism, $f(I) \triangleleft S, f(J) \triangleleft S$. So by prime, $f(I) \subset Q \lor f(J) \subset Q$. Then $I \subset f^{-1}(Q) \lor J \subset f^{-1}(Q)$.

## (c) Show that there is a one-to-one correspondence between prime ideals of $R$ containing $K$ and prime ideals of $S$.

Let $\phi: \phi(P) = f(P)$, then $\phi$ is well-defined by (a) and surjective by (b). For injection, if $\phi(P_1) = \phi(P_2) = Q$, then $P_1/K = P_2/K = Q$. By third homormophisim, $P_1 = P_2$.

So $\phi$ is a bijection.
# 3. For a positive integer $m\geq 2$, find all prime and maximal ideals of $\mathbb{Z}_m$.
Sub-ring of $\Z_m$ have the form $k\Z_m$ for some $k\mid m$. Trivially $k\Z_m \triangleleft \Z_m$, and $k_1\Z_m \cdot k_2\Z_m = k_1k_2\Z_m = (k_1k_2, m)\Z_m$.
#### Prime Ideals
$k\Z_m$ is prime iff $k \mid (k_1k_2,m) \Rightarrow k \mid k_1\lor k\mid k_2$ iff *what*? I can't go on in this way.

$k\Z_m$ is prime iff $\Z_m/k\Z_m = Z_k$ is a domain iff $k$ is prime

#### Maximal 
$k\Z_m$ is maximal iff $\Z_m/k\Z_m = Z_k$ is a field iff $k$ is prime. Otherwise multiplication inverse is not well-defined.

So the prime and maximal ideals are the same, and they are $k\Z_m$ where $k$ is prime factor of $m$

# 4. An ideal $M\neq R$ in a commutative ring $R$ with identity $1$ is maximal if and only if for every $r\in R\setminus M$, there exists $x\in R$ such that $1-rx\in M$.

$\Rightarrow$

Suppose $M$ is maximal, then $R/M$ is a field. 

For any $r\in R\setminus M$, $r+M \neq 0+M$, so $r+M$ is invertible. Let $x +M = (r+M)^{-1}$, then $rx + M = 1 + M$. So $(1-rx) +M = 0 + M = M$, $1 -rx \in M$.

$\Leftarrow$

Consider $R/M$. $\forall r+M \in R/M, r\notin M$, by hypothesis, $\exists x\in R$ such that $1-rx \in M$. So $(r+M)(x+M) = rx + M = 1 + M$, indicating $r+M$ is invertible. So $R/M$ is commutative with 1 and invertable, which is a field. 

So $M$ is maximal.

# 5. Let $R$ be a commutative ring with identity. Suppose that for each $a\in R$ there exists a positive integer $n$ such that $a^n=a$. Show that every prime ideal of $R$ is maximal.

For any prime ideal $P \triangleleft R$, $R/P$ is a domain.

For any $a+P \in R/P, a\notin P$, by hypothesis, $\exists n\in \Z_{>0}$ such that $a^n = a$. Consider $(a^{n-1} -1 + P)(a + P) = 0 + P\in R/P$, since $R/P$ is a domain, $a^{n-1} -1 + P = 0 + P$.

Thus, $(a + P)(a^{n-2} +P) = a^{n-1} + P = 1 + P$. So $a+P$ is invertible, and $R/P$ is a field. So $P$ is maximal.

*?: n=1 is always true*

# 6. Let $R$ be an integral domain with quotient field $F$. If $T$ is an integral domain such that $R\subset T\subset F$, show that $F$ is isomorphic to the quotient field of $T$.

$F = \{a/b \mid a,b \in R, b\neq 0\}$. Let the quotient field for $T$ be $Y = \{a/b \mid a,b \in T, b\neq 0\}$.

Define $\phi: F \rightarrow Y$ by $\phi(a/b) = a/b$. 

$\phi$ is well-defined since $a/b = c/d \Leftrightarrow (ad)|_R = (bc)_R \Leftrightarrow (ad)|_T = (bc)|_T \Leftrightarrow a/b, c/d \in Y$.

$\phi$ is a homomorphism since 
- $\phi((a/b)(c/d)) = \phi(ac/bd) = (ac)_R/(bd)_R = (ac)_T/(bc)_T = (a/b)_Y \cdot (c/d)_Y = \phi(a/b)\phi(c/d)$. 
- $\phi((a/b) + (c/d)) = \phi((ad + bc)/bd) = (ad + bc)_R/(bd)_R = (ad + bc)_T/(bd)_T = (a/b)_Y + (c/d)_Y = \phi(a/b) + \phi(c/d)$.

$\phi$ is injective since $\ker \phi = \{(a/b)_F \mid (a/b)_Y = 0\} = \{0\}$.

$\phi$ is surjective since $\forall t_1/t_2 \in Y$, $t_1 = a_1/b_1\in F$, $t_2 = a_2/b_2\in F$, $a_2 \neq 0$. So  $t_1/t_2 = a_1b_2/ a_2b_1 = \phi(a_1b_2/a_2b_1)$.

# 7. Let $I=\{a+b\sqrt{10}:13|2a-b\}$ be the subset of the ring $R=\mathbb{Z}[\sqrt{10}]$. Determine if $I$ is an ideal of $R$ and if so, determine if it is principal, prime or maximal.

$I$ is an ideal of $R$.
1. additive subgroup: $0 \in I$. $a + b\sqrt{10} - (c + d\sqrt{10}) = (a-c) + (b-d)\sqrt{10}$. Since $13\mid 2a -b, 13\mid 2c - d$, $13\mid 2(a-c) - (b-d)$, so $a-c + (b-d)\sqrt{10} \in I$.
2. $\forall a + b\sqrt{10} \in I, c + d\sqrt{10} \in R$, $(a + b\sqrt{10})(c + d\sqrt{10}) = (ac + 10bd) + (ad + bc)\sqrt{10}$. Since $2a \equiv b \mod 13$, 
   $$2ac + 20bd - ad - bc = c(2a - b) + d(20b -a) \equiv d(20b -a)\equiv 39ad\equiv 0$$
   So $(a+b\sqrt{10})(c+d\sqrt{10}) \in I$. By commutative, $I$ is an ideal.
---

$I$ is not a principal. Consider the norm $N(a + b\sqrt{10}) = a^2 - 10b^2$. 

> I really dont know how to do this properly. I am giving a tricky solution, I suppose.

Since $I = \{a + b \sqrt{10} \mid a\in \Z, b = 2a + 13k, k\in\Z\}$, $I = \{a(1 + 2\sqrt{10}) + 13b\sqrt{10}\mid a,b\in\Z\}$. So suppose $I$ = $(x)$, then $x\mid 1+2\sqrt{10}$, $N(x) \mid 39$.
So $|N(x)| = |a^2 -10b^2|= 1, 3, 13, 39$. But since $|N(x)|\equiv a^2\not\equiv 3 \mod 10$, $|N(x)| = 1 \lor 39$. 

Meanwhile, $|N(x)| \mid |N(13\sqrt{10})| = 1690$, so $|N(x)| = 1$. If $N(x) = 1$, then $x \in U(R)$, $x\bar x =1 \in I$, contradiction. If $N(x) = -1$, then $-x\bar x = +1 \in I$, contradiction.

So $I$ is not principal.

---

For prime and maximal, consider $R/I$. 
$$
R/I = \{a + b\sqrt{10} + I\} = \{a + b\sqrt{10} + I \mid 2a - b \in \Z_{13}\} = \{x\sqrt{10} + I \mid x\in \Z_{13}\} \cong \Z_{13}
$$

So $R/I$ is a field, $I$ is both prime and maximal.



# *8. Let $R$ be a commutative ring with identity and suppose that the ideal $A$ of $R$ is contained in a finite union of prime ideals $P_1\cup\cdots\cup P_n$. Show that $A\subseteq P_i$ for some $i$.


Prove the inverse direction by induction on $n$. That is, if $A \not\subset P_i, \forall i$, then $A \not\subset P_1 \cup \cdots \cup P_n$.

Trivial for $n = 1$.

For $n \geq 2$, we have the hypothesis $\forall m < n$, if $A\not\subset P_i, \forall i<=m$, then $A\not\subset P_1 \cup \cdots \cup P_m$. 

Consider $D_i = A - \cup_{j\neq i} P_j$. By hypothesis, $D_i \neq \emptyset$. Further, if $D_i \cap P_i = \emptyset$, then $A \cap P_i = \emptyset$, reducing to the case $n-1$ and done. WLOG, assume $D_i \cap P_i \neq \emptyset$, then $\exists z_i \in (A - \cup_{j\neq i} P_j)\cap P_i, \forall i$.

Let $z = z_1z_2 \cdots z_{n-1} + z_n$. Trivially $z \in A$. By prime, if $z \in P_i$, where $i<n$, then $z_n = z - z_1\cdots z_{n-1} \in P_i$. Contradiction. If $z\in P_n$, $z_1z_2\cdots z_{n-1} \in P_n$. By prime, $\exist i^*$ such that $z_{i^*} \in P_n$. Contradiction. 

So $z \notin P_1 \cup \cdots \cup P_n$, and $A \not\subset P_1 \cup \cdots \cup P_n$.