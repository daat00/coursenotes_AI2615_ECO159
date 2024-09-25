> I dont think I can do this without a hint, seriously.
>
> At first this comment was for 1.b, since I stuck there for days. By DDL I found this is true for every problem in this homework.
>
> **New:** After I wrote the last problem, I found that for most problems, I cannot do it even with a hint. Seriously?
# 1. Let $n \geq 3$ be a square-free integer. Let $R = \Z[\sqrt{-n}] = \{a + b\sqrt{-n} \mid a,b \in \Z\}$.
## (a) Show that $2, \sqrt{-n}$ and $1 + \sqrt{-n}$ are irreducible in $R$.

Consider the norm $N: R \rightarrow \Z_{\geq 0}$ by $N(a + b\sqrt{-n}) = a^2 + nb^2$.

$N(2) = 4$, $N(\sqrt{-n}) = n$, $N(1 + \sqrt{-n}) = 1 + n$.

Below WLOG suppose $N(x) < N(y)$.
1. Supoose $2 = xy$, then $N(2) = N(xy) = N(x)N(y) = 4$. So $N(x) = 1,2$. Since $n \geq 3$, let $x = a + b\sqrt{-n}$, by $b^2 n < 2 < n$ we have $b = 0$, $a = \pm 1$. 
   
   So $x$ is a unit for all decomposition, $2$ is irreducible.
2. Suppose $\sqrt{-n} = xy$. Consider the coefficient for $\sqrt{-n}$, since $xy = \sqrt{n}$, the coeff for $x$ and $y$ cannot be zero at the same time. 
   
   Then by $N(x)N(y) = n$, we have $x = \pm 1$, $y = \pm \sqrt{-n}$. So $\sqrt{-n}$ have no proper divisor and thus is irreducible.
3. Suppose $1 + \sqrt{-n} = xy$, then $N(x)N(y) = 1 + n$. Same as above, the coeff of $\sqrt{-n}$ for $x$ and $y$ cannot be zero at the same time. 
   
   Thus, we have $N(y) \geq n$. And since $n \nmid n+1$, $y = \pm (1 + \sqrt{-n})$. So $x = \pm 1$, $y = \pm (1 + \sqrt{-n})$. So $1 + \sqrt{-n}$ is irreducible.

## (b) Show that $\sqrt{-n}$ and $1 + \sqrt{-n}$ cannot be prime in $R$ at the same time.
Let $I = \sqrt{-n}R = \{na + b\sqrt{-n} \mid a \in \Z\} \cong R/\sqrt{-n}$, $J = (1 + \sqrt{-n})R = \{(a + b \sqrt{-n})(1 + \sqrt{-n}) \mid a,b \in \Z\} = \{(a-nb) + (b + a)\sqrt{-n}\}$.


#### 1. $\Z[\sqrt{-n}] \cong \Z[x]/(x^2 + n)$
Consider $\varphi: \Z[x] \rightarrow \Z[\sqrt{-n}]$ by $\varphi(f(x)) = f(\sqrt{-n})$. 
- Trivially the function operator on a number is homomorphism.
- This is surjective, consider $a + bx$

Use isomorphism theorem, $\ker \varphi = \{f(x) \in \Z[x] \mid f(\sqrt{-n}) = 0\}$. Since the minimal polynomial of $\sqrt{-n}$ is $x^2 + n$, $\ker \varphi = \{(x^2 +n)g\} = (x^2 + n)$.

So $\Z[x]/(x^2 + n) \cong \Z[\sqrt{-n}]$.
#### 2. $(\sqrt{-n}) \cong (x, x^2 + n)/(x^2+n)$, $(1 + \sqrt{-n}) \cong (x + 1, x^2 + n)/(x^2+n)$

Since $(\sqrt{-n}) \trianglelefteq \Z[\sqrt{-n}]$ and $\varphi^{-1}(\sqrt{-n}) = x + \ker\varphi = x + (x^2 +n) \subset \Z[x]$. $\sqrt{-n} \cong x + (x^2 + n)$.

Thus $(\sqrt{-n}) \cong \{xf(x) +(x^2 + n) \mid f(x) \in \Z[x]\} =  (x, x^2 + n)/(x^2+n)$.

Similarly, $(1 + \sqrt{-n}) \cong (x + 1, x^2 + n)/ (x^2+n)$.

#### 3. $\Z[\sqrt{-n}]/(\sqrt{-n}) \cong \Z/n\Z$, $\Z[\sqrt{-n}]/(1 + \sqrt{-n}) \cong \Z/(1 + n)\Z$

$$\Z[\sqrt{-n}]/(\sqrt{-n}) \cong (\Z[x]/(x^2 + n))/((x, x^2 + n)/(x^2+n)) \cong \Z[x]/(x, x^2 + n)$$

Since $(x, x^2 +n) = (x, n)$, $\Z[x]/(x, x^2 + n) \cong \Z[x]/(x,n) \cong \Z_n$.

So $\Z[\sqrt{-n}]/(\sqrt{-n}) \cong \Z/n\Z$.

Similarly, 
$$\Z[\sqrt{-n}]/(\sqrt{-n}) \cong \Z[x]/(x+1, x^2 + n) \cong \Z[x]/(x+1, n + 1) \cong \Z/(1 + n)\Z$$

#### 4. They cannot be prime simultaneously
Since $\alpha$ is prime iff $(\alpha)$ is prime ideal iff $\Z[\sqrt{-n}]/(\alpha)$ is an domain. And that $\Z_n$ is a domain iff $n$ is prime. They cannot be prime at the same time because $n, 1 + n$ cannot be prime simultaneously for $n \geq 3$.




# 2. Let $\Z[i]$ be the ring of Gaussian integers. Solve the followings:
## (a) Find the greatest common divisor of $50$ and $19 + 9i$ in $\Z[i]$.
Apply the Euclidean algorithm:
$$\begin{cases} 50 = (2 -j)(19+9i) + 3+i\\ 19+9i = (7+i)(3+i) - 1-i\\3 + i = (-2 + i)(-1-i)\end{cases}$$

So $\gcd(50, 19+9i) = 1+i$.

## (b) Let $a+bi \in \Z[i]$ with $a^2+b^2$ is equal to a prime $p$. Show that $\Z[i]/(a+bi) \cong \Z_p$.

<!-- Let $\phi: \Z \to \Z[i]/(a+bi)$ by $\phi(x) = x + 0i \mod (a+bi)$
- homomorphism: trivial
- surjective: 
  
  Notice that $(p, a) = (p,b) = 1$. So $\exist s,t$ such that $sp + t(a + bi) = 1$.$\forall x + yi \in \Z[i]$, apply Euclidean algorithm, we have $x + yi \equiv  -->

By 1.b, $\Z[i] \cong \Z[x]/(x^2 + 1)$, $(a + bi) \cong (a+bx + (x^2 + 1))\cong (a+bx, x^2 + 1)/(x^2 + 1)$.

So $\Z[i]/(a+bi) \cong \Z[x]/(a+bx, x^2 + 1) \cong \Z[x]/(a+bx, (a+bx)(a-bx) + bx^2 + b^2) \cong \Z[x]/(a+bx, p)$.

Since $(b,p) = 1$, $(a+bx,p) = (x, p)$. So 
$$\Z[x]/(a+bx, p) \cong \Z[x]/(x, p) \cong \Z_p$$

# 3. Determine the prime and irreducible elements of $\Z[i]$.
Since $\Z[i]$ is ED, as well as UFD, prime and irreducible are equivalent.

A Gaussian integer $a + bi$ is a Gaussian prime if and only if either:

1. $a^2 + b^2 = p^2$, where $p$ is prime and $p \equiv 3 \mod 4$
2. $a^2 + b^2$ is a prime number

The second case is done by 2.b. Since $\Z[i]/(a+bi) \cong \Z_p$ is a field.

Consider the first case, suppose that $\alpha = \beta\gamma$. 

1. $N(\beta) = p^2$ or $N(\gamma) = p^2$. Then the other one is a unit, so $\alpha$ is irreducible.
2. $N(\beta) = N(\gamma) = p$. Let $\beta=c+di$, then $c^2 + d^2 = p$. So $c^2 + d^2 \equiv 3 \mod 4$. Since the square of any integer is either congruent to 0 or 1 modulo 4, contradiction. 

So second case is prooved.


# 4. Suppose that $R$ is a domain and $X$ is an indeterminate. Show that $R$ is a field if and only if $R[X]$ is a PID.
$\Rightarrow$:

$R$ is a field, so $R[X]$ is a Euclidean domain. Since  Euclidean domain is PID, $R[X]$ is a PID.

$\Leftarrow$:

$R[x]$ is $PID$, then every ideal is principle.

Let $a∈R∖\{0\}$, by PID, the ideal $(x,a)=(f)$ for some $f(x) \in R[x]$. So $a = fh$ for some $h \in R[x]$, and thus $0  = \deg a = \deg f + \deg h$, $\deg f(x) = 0$. So $f \in R$.

Also $x = f \cdot (bx)$ for some $b \in R$, so $bf = 1\in (f)$, $(f) = (x,a) = R[x]$.

Thus, $1 = f(x)x + g(x)a$, clearly $f = 0$, so $a$ is a unit. Since $a$ is arbitrary, $R$ is a field.


# 5. Let $R$ be a ring, $f(x) = a_0 + a_1x + \cdots + a_nx^n \in R[x]$. Show that
## （a）$f(x)$ is a unit if and only if $a_0 \in U(R)$ and $a_1, \cdots, a_n \in Nil(R)$.

The definition for nilpotent is $a \in R$ is nilpotent if and only if $\exists n \in \Z_{\geq 0}$ such that $a^n = 0$.

**Lemma.** The sum of a nilpotent and a unit is a unit.
>??? I first dont know this lemma, then I am not familiar with nil.

$\Rightarrow$:

If $a_0∈U(R)$ and $a_1,\ldots,a_n$ are nilpotent, then clearly $a_1x+\cdots+a_nx^n$ is nilpotent (just raise it to a high enough power and every term will contain $a^{n_i}=0$). 

Then $f(x)$ is the sum of a nilpotent and a unit, hence a unit.

$\Leftarrow$:

$f(x)\in U(R[x])$, then $f(x)g(x) = 1$. Suppose $g(x) = b_0 + b_1x + \cdots + b_mx^m$, then $a_0b_0 = 1$, $a_0 \in U(R)$. 

For nilpotent, consider $a_n b_m x^{n+m} = 0$, so $a_n b_m = 0$. Then $a_nb_{m-1} + a_{n-1}b_m = 0$, by multplying $a_n$ we have $a_n^2b_{m-1} = 0$. Iterating this process, we have $a_n^{m+1}b_0 = 0$. Since $b_0 \in U(R)$, $a_n^{m+1} = 0$. So $a_n$ is nilpotent.

Then, $f(x)−a_nx^n$ is the sum of a nilpotent and a unit, so it is a unit. By induction, $a_1,\ldots,a_{n−1}$ are nilpotent.


## （b）$f(x)$ is a nilpotent if and only if $a_0, a_1, \cdots, a_n \in Nil(R)$

$\Leftarrow$:

Raise $f(x)$ to a high enough power and every term will contain $a^{n_i}=0$.

$\Rightarrow$:

Induction on $n=\deg(f)$. The case $n=0$ is obvious.

For the induction step, if $g(x)=a_0+a_1x+a_2x^2+⋯+a_{n+1}x^{n+1}$ is nilpotent, then $a_{n+1}$ is nilpotent, by looking at the coefficient of the highest term. This means that $a_{n+1}x^{n+1}$ is nilpotent, so $g(x)−a_{n+1}x^{n+1}$ is nilpotent. By induction hypothesis $a_0,⋯,a_n$ are all nilpotent. So $a_0,⋯,a_{n+1}$ are all nilpotent.


## （c）$f(x)$ is a zero divisor if and only if there exists $a \neq 0 \in R$ such that $af(x) = 0$.
$\Leftarrow$:

$a\in R[x]$. So $f(x)$ is a zero divisor.

$\Rightarrow$:

Suppose not. Choose $G≠0$ of min degree with $FG=0$.

Write $F=a+⋯+f x^k+⋯+c x^m$ and $G=b+⋯+g X^n$, where $g≠0$, and $f$ is the highest $\deg$ coef of $F$ with $fG≠0$ (note that such an $f$ exists else $Fg=0$)

Then $FG=(a+⋯+f x^k) (b+⋯+g x^n)=0$.

Thus  $fg=0$,  so $\deg(fG)<n$ and $F\circ (fG)=fFG=0$, contra minimality of $G$.


# 6. If $p$ is a prime, show that $f(x) = x^{p-1} + x^{p-2} + \cdots + x + 1$ is irreducible in $\mathbb{Q}[x]$.
#### Gauss
If $f(x) ∈Z[x]$ then we can factor $f(x)$ into two polynomials of degrees $r$ and $s$ in $Z[x]$ if and only if we can factor $f(x)$ into two polynomials of the same degrees $r$ and $s$ in $\mathbb{Q}[x]$.

#### (Eisenstein’s Criteria). 
Let
$$f(x) = a_nx^n + a_{n−1}x^{n−1} + ···+ a_0$$
be a polynomial with integer coefficients. Suppose that there is a prime $p$ such that $p$ divides $a_i$, $i ≤ n −1$, $p$ does not divide $a_n$ and $p^2$ does not divide $a_0$. Then $f(x)$ is irreducible in $\mathbb{Q}[x]$.

By Gauss, it suffices to consider factorisations of $f(x)$ over $\Z$.

First note that
$$f(x) = x^{p-1} + x^{p-2} + \cdots + x + 1 = \frac{x^p - 1}{x - 1}$$
Consider the map
$$\Z[x] \to \Z[x], f(x) \mapsto f(x + 1).$$
This is an example of an evaluation homomorphism; in this case we evaluate $f(x)$ at $x + 1$. Thus we get a ring homomorphism. This map is an isomorphism, since the inverse map sends $f(x)$ to $f(x - 1)$ (evaluation at $x - 1$).

Note that
$$g(x) = f(x + 1) = (x + 1)^{p-1} + (x + 1)^{p-2} + \cdots + (x + 1) + 1 = x^{p-1} + px^{p-2} + \cdots + p.$$

Observe that $\binom{p}{i}$ is divisible by $p$, for all $1 \leq i < p$, so that we can apply Eisenstein to the polynomial $g(x)$, using the prime $p$, to conclude that $g(x)$ is irreducible.

Suppose that $f(x) = h(x)k(x)$ is a factorisation of $f(x)$ over the integers. Then
$$g(x) = f(x + 1) = h(x + 1)k(x + 1),$$
is a factorisation of $g(x)$ over the integers. Here we use the fact that the map $f(x) \mapsto f(x+ 1)$ is a ring homomorphism. As we already decided we cannot factor $g(x)$ into polynomials of lower degree, it follows that we cannot factor $f(x)$ either.

# ∗7. Let $E$ be a domain and $D \subseteq E$ be a PID. Assume $a, b \in D \setminus \{0\}$. If $d$ is the greatest common divisor of $a$ and $b$ in $D$, then $d$ is also the greatest common divisor of $a$ and $b$ in $E$.

$(a, b) = \{xa + yb \mid x, y \in D\}$

So if $D$ is a PID, there is $d \in D$ such that $(a, b)_D = dD$. Now push forward these ideals to $E$.
$$(a,b)_E =(a,b)_D E=(dD)_E =dE$$
Thus the ideal of $E$ generated by $a$ and $b$ is still principal and still generated by the same element $d$ now well-determined up to a unit of $E$

---

> Again, this problem set is far beyond my ability. I nearly did nothing this week but this homework, and still, none of the problem is done without a web search. I suppose a am learning nothing wasting such the time. :/ 