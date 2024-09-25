# Chapter 0 - Preliminaries
## 0.1 Sets and Direct Product
$$
A_1 \times A_2 \times \cdots \times A_n = \{(a_1, a_2, \ldots, a_n) \mid a_i \in A_i, \forall i \}
$$

### Cardinality
- A fact: For finite direct products on finite sets. 
  $$
  |A_1 \times A_2 \times \cdots \times A_n| = \prod |A_i|
  $$
- Remark: For infinite sets $|\prod A_i| = \prod |A_i|$
- Def: Let $I$ be a set (finite or infinite). The direct product of the sets $A_i$ indexed by $I$ is defined as
  $$
    \prod_{i\in I} A_i = \{(a_i)_{i\in I} \mid a_i \in A_i\}
  $$

## 0.2 Maps (Functions)
Let $A,B$ be 2 sets. A map $f: A\rightarrow B$ satisfies $f(a)$ is `uniquely determined by` $f$ and $a$

Image
- $f(a)$ is called the `image` of $a$
  - The image of $f$ is $\operatorname{Im}(f) = \{f(x): x\in A\}$
- $a$ is called a `pre-image` of $f(a)$
  - The set of pre-image $f^{-1}(y) = \{x \in A\mid f(x) = y\}$

Injective, Surjective and Bijective
- $f$ is called `injective` if $f(a) = f(b) \Rightarrow a = b$
- $f$ is called `surjective` if $\operatorname{Im} f = B$
- $f$ is called `bijective` if $f$ is both injective and surjective


### Determining Cardinality 
Two sets $A,B$ have the same cardinality iff $\exist f: A\rightarrow B$ and $f$ is bijective

## Map Composition
Let $f: A\rightarrow B$, $g: B\rightarrow C$. The `composition` of $f, g$ is 
$$
g \circ f: \begin{array}{l}
  A \rightarrow C\\
  (g\circ f)(a) = g(f(a))
\end{array}
$$

### Lemma
Associativity: 
- Let $f: A\rightarrow B$, $g: B\rightarrow C$, $h: C\rightarrow D$. 
  Then $(h\circ g)\circ f = h\circ (g\circ f)$

Identity:
- $1_A: A\rightarrow A, f(a) = a$. 

Let $f: A\rightarrow B$
1. $f$ is injective iff $\exists g: B\rightarrow A$ s.t. $g\circ f = 1_A$
2. $f$ is surjective iff $\exists g: B\rightarrow A$ s.t. $f\circ g = 1_B$
3. $f$ is bijective iff $\exists g: B\rightarrow A$ s.t. $g\circ f = 1_A$ and $f\circ g = 1_B$
  

## 0.3 Equivalence Relations
Let $A$ be a set. A relation $R$ on $A$ is a subset of $A\times A$ satisfies
- Reflectivity: $(a,a) \in R, \forall a\in A$
- Symmetry: $(a,b) \in R \Rightarrow (b,a) \in R$
- Transitivity: $(a,b) \in R, (b,c) \in R \Rightarrow (a,c) \in R$

Remark:
- If $(a,b)\in R$, $a\sim b$
- $a\sim a$, $a\sim b \Rightarrow b \sim a$, $a\sim b, b\sim c \Rightarrow a\sim c$

### Equivalence Class
Let $\sim$ be a relation on $A$. The equivalence class of $a\in A$ is
$$
[a] = \{b\in A \mid a\sim b\}
$$

- Proposition: $\forall a,b\in A$, either $[a] = [b]$ or $[a]\cap [b] = \emptyset$

### Partition
Let $A$ be a set. A partition of $A$ is a collection of non-empty subsets $\{A_i\}_{i\in I}$ of $A$ s.t.
$$
A = \bigcup_{i\in I} A_i, \quad A_i \cap A_j = \emptyset, \forall i\neq j
$$

- $\{[a]\}_{a\in A}$ is a partition of $A$
- For any partition $\{A_i\}_{i\in I}$ of $A$, exists a relation $\sim$ on $A$ s.t. $\{A_i\} = \{[a_i]\}$

### Example
$$
\Z_m = \{0,1,\ldots, m-1\} = \Z / m\Z
$$