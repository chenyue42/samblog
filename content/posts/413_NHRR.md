---
title: "413_None Homogeneous Recurrence Relation"
date: 2023-12-11T14:59:52-06:00
draft: false
math: true
tags: ["Combinatorics"]
---


<span style="color:#04c2b2">Example</span> sums of cubes

Solve $h_n=h_{n-1}+n^3,(n \geq 1)$ and $h_0=0$.

**Solution**

Note that this is not a homegenous recurrence relation. 

Unrolling gives us: 

$$
\begin{align*}
h_{n-1} &= (n-1)^3 + h_{n-2}\\
\implies
h_n &= n^3 + (n-1)^3 + h_{n-2}
\end{align*}
$$

And we can keep doing this. Using this as the intuition, we guess the closed form: 
$$
h_n = n^3 + (n-1)^3 + \ldots + 2^3 + 1
$$
To formally prove this, we can use induction! Assume it holds for some $n \geq 1$, we want to show that $h_{n+1} = h_n + (n+1)^3 = 1 + 2^3 + \ldots + (n+1)^3$. 

After proving this formula, we are now trying to come up with a nice and clean formula for finding the sum of cubes. Think of adding up cubes that length of their edges are $1, 2, \ldots, n$. 

<span style="color:#599eff">Lemma</span>


$$
1^3 + 2^3 + \ldots + n^3 = (1 + 2 + \ldots + n)^2
$$

<span style="color:#eb861c">Proof</span> by induction

This is obviously true for $n = 1$. Then assume this identity is true for some $n \in \mathbb{N}$. Let's prove $n+1$ case. 

$$
\begin{align*}
1 + 2^3 + \ldots + n^3 + (n+1)^3 &\overset{I.H.}{=} (1 + \ldots + n)^2 + (n+1)^3\\
&\overset{?}{=}(1 + 2 + \ldots + n + (n+1))^2\\
\implies
(1 + 2 + \ldots + n + (n+1))^2 &= (1 + 2 + \ldots + n)^2 + (n+1)^2 + 2(1 + \ldots + n)\cdot (n+1)\\
\text{Need to check: }\\
(n+1)^3 &\overset{?}{=} (n+1)^2 + 2(1 + \ldots + n)\cdot (n+1)
\end{align*}
$$

Start from the right side: 

$$
\begin{align*}
(n+1)^2 + 2(1 + \ldots + n)\cdot (n+1) &= (n+1)(2(1+\ldots+n) + (n+1))\\
&= (n+1)\left( 2\binom{n+1}{2} + (n+1) \right)\\
&= (n+1)\left( n(n+1)+(n+1) \right)\\
&= (n+1)^3
\end{align*}
$$

$\blacksquare$

> I have learnt an interesting way of thinking the meaning of $(1+2+\ldots + n)$ from this proof.
>
> Say we have a complete graph $K_n$. We need to count the number of edges. Pick one vetex and count all the edges connected to it, which has $n$. Then we throw this vertex away, we are left with a complete graph $K_{n-1}$. Repeating this method for $n$ times, can count all the edges. We also know that the number of edges in $K_n$ is $\binom{n}{2}$, so the sum$(1 + 2 + \ldots + n) = \binom{n}{2}$.

<span style="color:#04c2b2">Exercise</span>
$$
h_n = 2h_{n-1} + 1
$$


<span style="color:#04c2b2">Exercise</span>

$\text { Solve } h_n=3 h_{n-1}-4 n,(n \geq 1) \text { and } h_0=2$.

**Solution**

Put all the terms related to $h_n$ to the left side of the equation helps.
$$
h_n - 3h_{n-1} = -4n
$$


For this one, let's first consider a homogeneous recurrence relation without the $-4n$ term.
$$
a_n = 3a_{n-1} \implies a_n - 3a_{n-1} = 0
$$
It is easy to find out that the closed form for this HRR is $a_n = 3^n$.

We then try to guess a **particular solution** to the NHRR, but we don't care about the initial condition $h_0$. Since the right side $-4n$ is a degree 1 polynomial, we may guess the particular solution is in the form:
$$
b_n = sn + r
$$
for some constant $s$ and $r$. Since we hope that $b_n$ satisfies the recurrence relation $b_n = 3b_{n-1}-4n$, we can solve for $s$ and $r$:

$$
\begin{align*}
b_n &= 3(s(n-1)+r) - 4n\\
&= 3(sn-s+r)-4n\\
&= (3s-4)n + 3(r-s) = sn+r\\
&\implies
\begin{cases}
s=2\\
r=3
\end{cases}
\end{align*}
$$


- Solution to homogeneous recurrence relation: 
$$
a_n = 3^n
$$

- Particular solution: 

$$
b_n = 2n + 3
$$

Combine them together to get: 

$$
\begin{align*}
h_n &= c \cdot a_n + b_n = 3h_{n-1} - 4n\\
h_n &= c(3^n) + (2n+3), \quad h_0 = 2
\end{align*}
$$


There is one important thing to learn here. Question: why do we want to combine $a_n$ and $b_n$ together? Why we multiply $a_n$ by a constant $c$ while we don't do this for $b_n$? The answer is that the 

Using the initial condition, we solve for $c = -1$. Therefore, the full solution to the initial NHRR is: 
$$
h_n = 2n+3-3^n
$$
$\blacksquare$




### Tips for finding general solutions of non-homogeneous recurrence relation

Essentially, to find the general solution of a **non-homogeneous** recurrence relation, we need to:

(1) Find the general solution of the associated homogeneous relation

(2) Find a particular solution of the nonhomogeneous relation.

(3) Combine the general solution and the particular solution, and determine values of the constants arising in the general solution so that the combined solution satisfies the initial conditions.



The main difficulty is finding a particular solution in step (2). For some nonhomogeneous parts, there are certain types of particular solutions to try.

Consider a nonhomogeneous recurrence relation of the form
$$
h_n=a h_{n-1}+b_n
$$
These are the guess direction for possible **particular solutions** of $h_n$:

(a) If $b_n$ is a **polynomial** of degree $k$ in $n$, then look for a particular solution $h_n$ that is also a polynomial of degree $k$ in $n$.

(b) If $b_n$ is an **exponential**, then look for a particular solution that is also an exponential.


There are some common forms that you can try to guess:

| $f(n)$ | $a_n$                 |
| ------ | --------------------- |
| $c$    | $A$                   |
| $n$    | $A_1n + A_0$          |
| $n^2$  | $A_2n^2 + A_1n + A_0$ |
| $r^n$  | $Ar^n$                |

where $c, A, r$ are constants.

