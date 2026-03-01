---
title: "Key Switching Techniques"
date: 2026-03-01T17:11:47-06:00
draft: false
math: true
tags: ["cryptography"]
---


### Basics

#### RLWE ciphertext

$$
\begin{align*}
(a, b) &= (a, -as + \Delta m + e)\\
R_q &= \mathbb{Z}_q[x]/(x^d + 1)\\
\Delta &= \lfloor q/p\rfloor
\end{align*}
$$

#### Key-switching

$$
\begin{align*}
\mathsf{KS.setup}(s,s')&=[w, -s' \cdot w + s \cdot g_z + e] \to K\\
\mathsf{KS.switch}(K, (a, b)) &= (0, b) + g_z^{-1}(a) \cdot k\\
&= (0, b) + g_z^{-1}(a) \cdot [w, -s' \cdot w + s \cdot g_z + e]\\
&= (g_z^{-1}(a) \cdot w, -g_z^{-1}(a) \cdot w \cdot s'+ g_z^{-1}(a) \cdot s \cdot g_z+g_z^{-1}(a) \cdot e)\\
&=(a', -s'a' + s \cdot g_z^{-1}(a)g_z + e')\\
&=(a', -s'a' + sa + e') + (0, -as + m + e)\\
&=(a', -s'a' + m + e'')
\end{align*}
$$

$K\in R_q^{\ell \times 2}$ is two columns of polys. This is pretty large. Say $q$ is stored in `uint64_t`, $d = 2048$, then each polynomial in $R_q$ is `16KB`. We can use pseudorandom $w$ to save some time, but still quite large.

<span style="color:#9650af">Idea.</span> Apply $\tau_g$ to all these polys in $K$, we get a ”shifted” key-switching key.

> Reference: [Appendix 3 of InsPIRe](https://eprint.iacr.org/2025/1352.pdf#subsection.Appendix.A.3)

$$
K_g = \mathsf{KS.setup}(\tau_g(s), s)\\
\tau_g^i(K_g) \equiv \mathsf{KS.setup}(\tau_g^{i+1}(s), \tau_g^{i}(s))
$$

<span style="color:#eb861c">Proof.</span> TODO: do the math



#### About the Galois Group

> Refernce: [Preliminaries in InsPIRe](https://eprint.iacr.org/2025/1352.pdf#section.2)

For the cyclotomic ring $R_q$ that we consider in this work, the Galois group is a group of ring automorphisms, with each element:
$$
\begin{align*}
\tau_g: R &\to R\\
p(x) &\mapsto p(x^g)
\end{align*}
$$
for $g \in \set{1, 3, \ldots, 2d - 1}$.



Basically, we want to check two things:

1. The $\tau$’s are ring automorphisms.
2. The $\tau_g$’s with function composition forms a group.

<span style="color:#599eff">Claim.</span> $\tau_g$ is an automorphism in the ring $R_q$ if $g<2d$ and $\gcd(g, 2d) = 1$. 

<span style="color:#eb861c">Proof.</span> A stupid proof...

Since $R_q$ is finite, it suffices to show that the $\tau$ is injective. Suppose $\tau_g(p_a) = \tau_g(p_b)$ for $g \in \set{1, 3, \ldots, 2d-1}$, and $p_a, p_b \in R_q$: 
$$
\begin{align*}
p_a(x) = \sum_{i = 0}^{d-1}a_ix^i, \quad p_b(x) = \sum_{i = 0}^{d-1}b_ix^i\\
\end{align*}
$$
Applying the $\tau_g$ on both polynomials:
$$
\begin{align*}
\tau_g(p_a) &= \sum_{i=0}^{d-1} a_i x^{ig} \mod(x^d + 1)\\
\tau_g(p_b) &= \sum_{i=0}^{d-1} b_i x^{ig} \mod(x^d + 1)
\end{align*}
$$
It suffices to show that $x^{ig} \equiv x^{jg} \mod (x^d + 1)\implies i = j$, since in that case, the function $\tau_g$ is just effectively permuting the coefficients (no two coefficients get added together).
$$
\begin{align*}
x^{ig} &\equiv x^{jg} \mod (x^d + 1) \\
x^{k_ad + r_a} &\equiv x^{k_bd + r_b}\mod (x^d + 1)\\
(x^{d})^{k_a} \cdot x^{r_a} &\equiv (x^{d})^{k_b} \cdot x^{r_b} \mod (x^d + 1)\\
(-1)^{k_a} \cdot x^{r_a} &\equiv (-1)^{k_b} \cdot x^{r_b}  \mod (x^d + 1)
\end{align*}
$$
This equivalence means that $k_a \equiv k_b \mod 2$, and that $ r_a = r_b$. From here, we can argue $x^{ig} \equiv x^{jg} \mod (x^d + 1) \implies x^{ig \mod 2d} = x^{jg \mod 2d}$. (These two sentences may require a short lemma). $\gcd(g, 2d) = 1 \implies i \equiv b \mod 2d$. $\square$

<span style="color:#eb861c">Proof.</span> ChatGPT gave me another proof:

Pick $h$ such that $gh \equiv 1\mod 2d$. Then, for all $p \in R_q$, 
$$
(\tau_g \circ \tau_h)(p) = p(x^{gh}) = p(x)
$$
This shows every function $\tau_g$ has an inverse function $\tau_h$. $\square$

<span style="color:#599eff">Claim.</span> This galois group is isomorphic to the additive group $\mathbb{Z}_{d/2} \times \mathbb{Z}_2$ using two generators

<span style="color:#eb861c">Proof.</span> TODO

---

### Application of the Key-Switching Techniques

> Onion Ring ORAM [section 4.3](https://eprint.iacr.org/2019/736.pdf#subsection.4.3) and appendix.

#### The Substitution Function

Given a ciphertext  $c = (a, b) = (a(x), -a(x)s(x)+m(x) + e(x))$, the function $\mathsf{Subs}$ takes in the ciphertext $c$ and an odd number $g$, and outputs the 

The high-level is just two steps:

1. Apply the ring automorphism on both polynomial of the ciphertext $c$.
2. Key-switching the underlying secret key back to the original secret key.

$$
\begin{align*}
\mathsf{Subs}((a, b), g)&: \\
\text{Step 1: } & (\tau_g(a), \tau_g(b))\\
&=
(a(x^g),-a(x^g)s(x^g)+m(x^g) + e(x^g)))\\
&=(a(x^g), b(x^g))_{s(x^g)}\\
\\
\text{Step 2: } & 
\text{Let }K_g = \mathsf{KS.setup}(\tau_g(s), s)\\
&\text{Output: }\mathsf{KS.switch}(\; K_g, (\tau_g(a), \tau_g(b))\;)
\end{align*}
$$

<span style="color:#ff5e7e">Remark.</span> When $g = d+1$, TODO
$$
c= (a, b)\\
c + \mathsf{Subs}(c, d+1) = 2\cdot \text{even coefficient of }c = 2 \cdot c_{even}\\
x^{-1}(c - \mathsf{Subs}(c, d+1)) = 2\cdot c_{odd}
$$



#### Application of the $\mathsf{Subs}$

Onion Ring ORAM [section 4.3](https://eprint.iacr.org/2019/736.pdf#subsection.4.3), OnionPIRv2 [query compression](https://eprint.iacr.org/2025/1142.pdf#subsection.3.3)

---

### Fewer Key-Switching Keys

#### Expansion using a single key: 

WhisPIR [section 3.2](https://eprint.iacr.org/2024/266.pdf#subsection.3.2)

#### Two keys:

Say we are willing to send two key-switching keys, then we are able to significantly reduce the server computation.

A generator finder code : https://github.com/chenyue42/generator_finder