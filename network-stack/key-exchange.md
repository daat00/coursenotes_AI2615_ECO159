## Diffie-Hellman(Basic Version)
#### Setup
prime $p$, generator $\alpha$ of $\Z_p^*$

#### Protocol
- $𝐴$ chooses random $𝑥$, send $𝐵$ message $𝛼^𝑥 \mod 𝑝$,
- $𝐵$ chooses random $𝑦$, send $𝐴$ message $𝛼^𝑦 \mod 𝑝$,
- $𝐵$ receives $𝛼^𝑥$ and computes the shared key as $\bm K = (𝑎^𝑥)^𝑦 \mod 𝑝$
- $𝐴$ receives $𝛼^𝑦$ and computes the shared key as $\bm K=(𝑎^𝑦)^𝑥 \mod 𝑝$
