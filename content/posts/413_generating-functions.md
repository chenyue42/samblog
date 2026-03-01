---
title: "413_generating Functions"
date: 2023-11-01T22:48:35-05:00
draft: false
math: true
tags: ["Combinatorics"]
---
# Generating Functions

Generating functions can be regarded as algebraic objects whose formal manipulation allows us to count the number of possibilities for a problem by means of algebra. 

<span style="color:#28a745">Definition</span>

Let $h_0, h_1, h_2, h_3, \ldots$ be an infinite sequence of numbers. Its generating function is defined to be the infinite series: 
$$
g(x) = h_0 + h_1x + h_2x^2 + \cdots + h_nx^n + \cdots
$$

#### <span style="color:#04c2b2">Example 1</span>

The generating function of the infinite sequence $1, 1, 1, \ldots$ is: 
$$
g(x) = 1 + x + x^2 + x^3 + \cdots
$$
for $|x| < 1$. This generating function $g(x)$ is the sum of a geometric series with value: 
$$
g(x) = \frac{1}{1-x}
$$
**Solution**

$|x| < 1$,
`

$$
\begin{align*}
(1-x)(1 + x + x^2 + x^3 + \cdots + x^n) &= 1 - x^{n+1}\\
\Rightarrow\\
\lim_{n \to \infty}(1-x)(1 + x + x^2 + x^3 + \cdots + x^n)
&= \lim_{n \to \infty}(1 - x^{n+1}) = 1\\
\Rightarrow\\
\sum_{j=0}^{\infty}x^j = \frac{1}{1-x}
\end{align*}
$$

`
#### <span style="color:#04c2b2">Example 2</span>

Let $m$ be a positive integer. The generating function for the binomial coefficients: 
$$
\binom{m}{0}, \binom{m}{1}, \ldots, \binom{m}{m}
$$
**Solution**
$$
g(x) = \binom{m}{0}x + \binom{m}{1}x^2+ \ldots+ \binom{m}{m}x^m = (1+x)^m
$$
By the binomial theorem.



### Think about the coefficients of generating functions

#### <span style="color:#04c2b2">Example 3</span>


Let $k$ be an integer, and let the sequence $h_0, h_1, h_2, \ldots$ be defined by letting $h_n$ equal the number of nonnegative integeral solutions of  $x_1+x_2+\cdots+x_k = n.$ Show that the generating function of this sequence is equal to 
$$
g(x) = \frac{1}{(1-x)^k}
$$

**Solution**

The goal is to convince ourself that expanding the function $g(x) = \frac{1}{(1-x)^k}$ give us special coefficients. (What is that? 🤔)

Since we know that $\frac{1}{(1-x)} = 1 + x + x^2 + \ldots$, the expansion becomes: 

$$
\begin{align*}
\frac{1}{(1-x)^k} = \underbrace{(1 + x + x^2 + \ldots) \cdot (1 + x + x^2 + \ldots) \cdots (1 + x + x^2 + \ldots)}_{k}
\end{align*}
$$

Note that there are $k$ pairs of parenthesis, and from each pair of parentheis, we can choose a $x^i$, for some $i \geq 0$. Multiplying these $x_i$'s, say $x^{i_1} \cdot x^{i_2}\cdots x^{i_k}$, which is equivalent to $x^{i_1 + i_2 + \ldots + i_k} = x^n$ for some $n$. Now, given a fixed $n$, we can think about the coefficients of the term $x^n$... Well, MAGIC !!! This is same as the number of nonnegative integral solutions of $x_1 + x_2 + \ldots + x_k = n$. Just in different notation!

To make this clearer, just write out the definition of generating functions of this sequence: 
$$
g(x) = h_0 + h_1x + h_2x^2 + h_3x^3 + \ldots
$$
SEE? Each coefficients of $x^n$ corresponds to some choices of $x^i$ from the expansion of $\frac{1}{(1-x)^k}$. Thus, the sum of all the terms in generating function can be simplified / is equivalent to $\frac{1}{(1-x)^k}$ . $\blacksquare$

> The general trick for generating function problems is to find out an expression for these $h_n$'s. Sometimes, we make some guesses and multiply a few generating functions together, and observe that the coefficients of $x$'s in the multiplication is what we want. We will see more examples. (I mean a LOT 🙃)

---

#### <span style="color:#04c2b2">Example 4</span> (Read example 5 first for better understanding. Also, do compare this two.)

For what sequence is $(1+x+x^2+x^3+x^4+x^5)(1+x+x^2)(1+x+x^2+x^3+x^4)$ the generating function?

**Solution**

> For this one, we are trying to construct a case such that, when writing out the generating function for them, we get this expression. This is a "reverse designing" problem. It is hard to come up with these from no where if this is the first time you see them. But, as long as we read/learn more solutions, we can be experienced enough to make correct guesses. That's how Combinatoric works. GET USED TO IT 😢

Observe that there are three terms multiplied together. Therefore, we may have three kinds of things. Each kind has different copies... This reminds us of something called multi-set. 

Consider a multi-set: 
$$
S = \set{5 \cdot a, 2 \cdot b, 4 \cdot c}
$$
The questions is: what is the generating function for the number of $n$-combinations of $S$?
$$
g(x) = h_0 + h_1x + h_2x^2 + h_3x^3 + \ldots
$$
Where $h_n$ represents the number of different combinations using $S$. To build the connection between the initial expression and this generating function, simply try to expand it and check the coefficient of each $x^n$. It turns out that the coefficients $h_n$ give us the number of $n$-combinations using $S$. $\blacksquare$



#### <span style="color:#04c2b2">Example 5</span>

Determine the generating function for the number of $n$-combinations of apples, bananas, oranges, and pears, where in each $n$-combination, the number of apples is even, the number of bananas is odd, the number of oranges is between $0$ and $4$, and there is at least one pear.

**Solution**

Intuition is to come up to a multiplication of generating functions, where each generating function corresponds to one fruit.

Consider: 

$(1 + x^2 + x^4 + \ldots) \to $ Apples

$(x + x^3 + x^5 + \ldots) \to $ Bananas

$(1 + x + x^2 + x^3 + x^4) \to $ Oranges

$(x + x^2 + x^3 + \ldots) \to $ Pears

Note that the coeff of $x^n$ is the number of $n$-combinations of fruits.

If we expand the multiplication of these four terms, we get: 
$$
g(x) = \sum_{a_1,\ldots, a_4}x^{a_1} \cdot x^{a_2} \cdot x^{a_3} \cdot x^{a_4} = \sum_{n \geq 0}h_nx^n
$$
The coefficients $h_n$ in the expression above is what we want / is what we are asked for. We may first notice that finding the correct coefficients is the goal, then we try to construct the generating functions that satisfies the properties. 

Apples $ = \frac{1}{1-x^2}$ by replacing $x$ by $x^2$.

Bananas $=\frac{x}{1-x^2}$ by multiplying apple by $x$.

Oranges $= \frac{1-x^5}{1-x}$ since $(1-x^5) = (1-x)(1 + x + x^3 + x^4)$

Pears $= (\frac{1}{1-x}-1) = \frac{x}{1-x}$. Or replace 1 by $x$ in the expression $(1 + x + x^2 + x^3 + \ldots) = \frac{1}{1-x}$

Then we take the multiplication. $\blacksquare$



#### <span style="color:#04c2b2">Example 6</span>

Find the number $h_n$ of bags of fruit that can be made out of apples, bananas, oranges, and pears, where, in each bag, the number of apples is even, the number of bananas is a multiple of five, the number of oranges is at most 4, and the number of pears is 0 or 1.

**Solution**

Similar to Example 5: 
$$
g(x) = (1 + x^2 + x^4 + \ldots) (1 + x^5 + x^{10} + \ldots)(1 + x + x^2 + x^3 + x^4) (1+x)
$$
Simplify it to get: 
$$
g(x) = \frac{1}{(1-x)^2}
$$
To calculate $h_n$ from this, we can use Taylor expansion:

> WHY? 


⚠️ Calc review: 

$$
\begin{aligned}
& f \in \mathcal{C}^{\infty}, 0 \in \operatorname{Domain}(f) \\
& f(x)=f(0)+\frac{f^{\prime}(0)}{1 !} \cdot x+\frac{f^{\prime \prime}(0)}{2 !} \cdot x^2+\ldots \\
& f(x)=\frac{1}{(1-x)^2} \\
& f^{\prime}(x)=\frac{2}{(1-x)^3} \\
& f^{\prime \prime}(x)=\frac{2 \cdot 3}{(1-x)^4} \\
& f^{(n)}(x)=\frac{2 \cdot 3 \cdots(n+1)}{(1-x)^{n+2}}\\
& f^{(n+1)}(x)=\frac{2 \cdot 3 \cdot(n+1)(n+2)}{(1-x)^{n+3}} \\
&
\end{aligned}
$$

For $n \in \mathbb{N}:$

$$
\begin{aligned}
& f^{(n)}(0)=(n+1) ! \\
& f(x)=1+\frac{2 !}{1 !} x+\frac{3 !}{2 !} x^2+\cdots \\
& =1+\sum_{n \geqslant 1} \frac{f^{(n)}(0) x^n}{n !}=1+\sum_{n \geqslant 1}^1(n+1) x^n
\end{aligned}
$$

Then our answer is simply $h_n = n+1$. $\blacksquare$

**OR**

We can solve this in another way:

$$
\begin{align*}
\left(\frac{1}{1-x}\right)^\prime &= (1 + x + x^2 + \ldots)^\prime\\
\frac{1}{(1-x)^2} &= 0 + 1 + 2x + 3x^2 + \ldots
\end{align*}
$$

Then we find out that $h_n = n+1$. $\blacksquare$

---

### Exponential generating functions

So far, we defined the generating function for a sequence $\left(h_n\right)_{n \geq 0}$ by using the monomials $1, x, x^2, \ldots$ There were no coefficient different from 1 in front of these variables.

This is particularly suited to some sequences involving **combinations and binomial coefficients**.

However, for sequences whose terms count **permutations**, it is more useful to consider a generating function with respect to the monomials
$$
1, x, \frac{x^2}{2 !}, \frac{x^3}{3 !}, \ldots
$$
Actually, we have seen these patterns in the Taylor series: 

These monomials arise in the Taylor series
$$
e^x=\sum_{n=0}^{\infty} \frac{x^n}{n !}=1+x+\frac{x^2}{2 !}+\frac{x^3}{3 !}+\cdots
$$

The exponential generating function for the sequence $\left(h_n\right)_{n \geq 0}$ is defined to be
$$
g^{(e)}(x)=\sum_{n=0}^{\infty} h_n \frac{x^n}{n !}=h_0+h_1 x+h_2 \frac{x^2}{2 !}+h_3 \frac{x^3}{3 !}+\cdots
$$

#### <span style="color:#04c2b2">Example 1</span>

Let $n$ be a positive integer. Determine the exponential generating function for the sequence of numbers: 
$$
P(n, 0), P(n, 1), P(n, 2), \ldots, P(n, n)
$$
where $P(n, k)$ denotes the number of $k$-permutations of an $n$ element set.

**Solution**

We want to insert $P$'s into the sequence as $h$'s.
$$
\sum_{k=0}^{n} = \frac{P(n,k)x^k}{k!} = \sum_{k=0}^{n} \frac{n!}{k!(n-k)!} \cdot x! = \sum_{k=0}^{n}\binom{n}{k}x^k = (1+x)^n
$$

> We can see that the results becomes really clean.

#### <span style="color:#04c2b2">Example 2</span>

Let $a$ be any real number. Determine the exponential generating function for the sequence $a_0, a_1, a_2, \ldots$

**Solution**
$$
\sum_{k=0}^{n} = \frac{a^kx^k}{k!} = e^{ax}
$$

<span style="color:#599eff">Lemma</span>

Let $S$ be the multiset $\{n_1 \cdot a_1, n_2 \cdot a_2, \ldots ,n_k \cdot a_k\}$, where $n_1, n_2, \ldots ,n_k$ are nonnegative integers. 

Let $h_n$ be the number of $n$-permutations of $S$. Then, the exponential generating function $g^{(e)}$ for the sequence $(h_n)_{n \ge 0}$ is given by
$$
g^{(e)}(x) = f_{n_1}(x)f_{n_2}(x)\cdots f_{n_k}(x).
$$

where, for $i = 1,2, \ldots ,k$,

$$
f_{n_i}(x) = 1 + x + \dfrac{x^2}{2!}+ \cdots + \dfrac{x^{n_i}}{n_i!}.
$$


<span style="color:#eb861c">Proof</span>

An $n$-permutation of $S$ is a string of the form: 

$T = t_1 \cdots t_n$, where $t_i \in S,\forall i \in [n]$ and the number of $a_i$'s in $T \leq n_i, \forall i \in [k]$.

Denote: $P_n = $ number of $n$-permutation of $S$.  To count $P_n$:

- Choose the multiplicities of each $a_i$ in our string: $j_i$ $a_1$\'s, $j_2$ $a_2$\'s, ..., $j_k$ $a_k$\'s
- Restriction: $j_1 + \ldots + j_k = n, j_i \in [n_i], \forall i \in [k]$

The number of $n$-permutations with $j_i$ $a_1$\'s, $j_2$ $a_2$\'s, ..., $j_k$ $a_k$\'s is:
$$
\frac{n!}{j_1! \cdots j_k!}
$$


$$
\begin{align*}
\implies P_n = \sum_{(j_1, \ldots,j_k): \text{restriction}}\frac{n!}{j_1! \cdots j_k!}
\end{align*}
$$

Consider the function: 
$$
g(x) = \left( \sum_{j_1 = 0}^{n_1} \frac{x^{j_1}}{j_1} \right) \cdots 
\left( \sum_{j_k = 0}^{n_k} \frac{x^{j_k}}{j_k} \right)
$$
Goal: to show that the coef of $x^n$ in $g$ is equal to $P_n / n!$

$$
\begin{align*}
g(x) &= \sum_{0\leq j_i \leq n_i}\frac{x^{j_1}}{j_1!} \cdots\frac{x^{j_k}}{j_k!}\\
&=\sum_{n=0}^{n_1+\ldots + n_k} \left( \sum_{(j_1,\ldots,j_k):\text{restriction}} \frac{1}{j_1! \cdots j_k!}\right)\cdot x^n
\end{align*}
$$

This whole thing gives us:
$$
g(x) = \sum_{n=0}^{n_1 + \ldots + n_k} \frac{P_n}{n!}x^n
$$

#### <span style="color:#04c2b2">Example 3</span>

Let $h_n$ denote the number of $n$-digit numbers with digits 1,2, or 3 , where the number of 1 's is even, the number of 2 's is at least three, and the number of 3 's is at most four. Determine the exponential generating function $g^{(e)}(x)$ for the resulting sequence of numbers $\left(h_n\right)_{n \geq 0}$.

**Solution**

$$
\begin{align*}
g^{(e)}(x) &= \left( 1 + \frac{x^2}{2!} + \frac{x^4}{4!} + \ldots \right) && \text{choice of num of 1's}\\
&\cdot \left( \frac{x^3}{3!} + \frac{x^4}{4!} + \frac{x^5}{5!} + \ldots \right) && \text{choice of num of 2's}\\
&\cdot \left( 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \frac{x^4}{4!} \right)&& \text{choice of num of 3's}
\end{align*}
$$

The expansion becomes:
$$
g^{(e)}(x) = \sum \frac{x^{a_1}x^{a_2}x^{a_3}}{a_1!a_2!a_3!}
$$


Coefficient of $x^n$ in $g^{(e)}(x)=$
$$
\sum_{}^{} \frac{1}{a_1!a_2!a_3!} = \frac{h_n}{n!}
$$


How can we simplify this:
$$
\left( 1 + \frac{x^2}{2!} + \frac{x^4}{4!} + \ldots \right) = 
\frac{e^x + e^{-x}}{2}
$$

and

$$
\left( \frac{x^3}{3!} + \frac{x^4}{4!} + \frac{x^5}{5!} + \ldots \right) = e^x - 1 - x - \frac{x^2}{2}
$$



#### <span style="color:#04c2b2">Example 4</span>

Determine the number of ways to color the squares of a 1-by-$n$ chessboard, using the colors, red, white, and blue. Also, an even number of squares are to be colored red.

**Solution**
$$
g^{(e)}(x) = e^x \cdot e^x \cdot \left( \frac{e^x + e^{-x}}{2} \right) = \frac{e^{3x} + e^x}{2}
$$
Using a same logic.



#### <span style="color:#04c2b2">Example 5</span>

Determine the number $h_n$ of $n$-digit numbers with each digit odd, where the digits 1 and 3 occur an even number of times. 

**Solution**

Allowed digits $ = \set{1, 3, 5, 7, 9}$

Number of 1's and 3's are both even.

$$
g^{(e)}(x) = \left( 1 + x + \frac{x^2}{2!} +  \frac{x^3}{3!} + \cdots \right)^3 \cdot  \left( 1 + \frac{x^2}{2!} +  \frac{x^4}{4!} + \cdots \right)^2
$$

Where the first term correspondes to choices for number of 5's,7's, and 9's. Since they are all the same, the term is raised to power of 3. Similarly, the second term corrspondes to choices for 1's and 3's.

Simplify this we get:

$$
\begin{align*}
g^{(e)}(x) &= \left( \frac{e^{-x}+e^x}{2} \right)^2 \cdot e^{3x}\\
&= \frac{1}{2}e^{3x} + \frac{e^x + e^{5x}}{4}\\
&= \frac{1}{4} \cdot \left( 2e^{3x} + e^x + e^{5x} \right)
\end{align*}
$$

Now, want to find the coefficients of each term in the Taylor's Expansion.

$e^x =  \left( 1 + x + \frac{x^2}{2!} +  \frac{x^3}{3!} + \cdots \right)$

$e^{5x} =  \left( 1 + 5x + \frac{(5x)^2}{2!} +  \frac{(5x)^3}{3!} + \cdots \right)$

$2e^{3x} =  \left( 2 + 2\cdot(3x) + 2\cdot\frac{(3x)^2}{2!} + \cdots \right)$

$$
g^{(e)}(x) = \frac{1}{4} \cdot \sum_{n \geq 0}\left( 1+5^n+2\cdot3^n \right) \frac{x^n}{n!}
$$
The answer of the counting problem becomes: 
$$
h_n = \frac{1 + 5^n + 2 \cdot 3^n}{4}
$$

#### <span style="color:#04c2b2">Example 6</span>

Determine the number $h_n$ of ways to color the squares of a 1-by-n board with colors ...

==...==

**Solution**

TBC



#### <span style="color:#04c2b2">Example 7</span>



#### <span style="color:#04c2b2">Example 8</span>

> This example illustrates how to use generating functions to solve linear homogeneous recurrence relations with constant coefficients.

Solve the recurrence relation: 
$$
h_n = 5h_{n-1} - 6h_{n-2} \quad (n \geq 2)
$$
with initial values $h_0 = 1, h_1 = -2$

Previously, we get the expression: 
$$
g(x) = \frac{5}{1-2x} - \frac{4}{1-3x}
$$
Now, use the fact that $\frac{1}{1-x} = 1 + x + x^2 + x^3 + \ldots$, we have: 

$$
\begin{align*}
g(x) &= 5 \sum_{n \geq 0} (2x)^n - 4 \sum_{n \geq 0}(3x)^n\\
&= \sum_{n \geq 0}(5 \cdot 2^n - 4 \cdot 3^n)x^n\\
&\implies\\
h_n &= 5 \cdot 2^n - 4 \cdot 3^n
\end{align*}
$$

$\blacksquare$

#### <span style="color:#04c2b2">Towers of Hanoi puzzle revisited</span>

> Instead of using induction, what else can we do?

Let $h_n$ be the number of moves to transfer $n$ disk from one peg to a different peg and $h_n = 2h_{n-1} + 1$ for $n \geq 1, h_0 = 0$. Find a closed formula for $h_n$.

**Solution**

Note that this recurrence relation is non-homegeneous. (Definition below)

Now, back to the solution. Define the following generating functions based on the expression $h_n - 2h_{n-1}-1 = 0$.

$$
\begin{align*}
g_1(x) &= h_0 + h_1x + h_2x^2 + h_3x^3 + \ldots && \text{As usual}\\
g_2(x) &= -2h_{-1} -2h_0x - 2h_1x^2 - 2h_2x^3 + \ldots && \text{using } h_{-1} = 0\\
g_3(x) &= 0 -1x - 1x^2 - 1x^3 + \ldots && \text{using } a_0 = 0, a_i = -1,\forall i \geq 1
\end{align*}
$$



Sum them up, we get a zero function: $(g_1 + g_2 + g_3)(x) = 0, \forall x \geq 0$

Now, express $g_2, g_3$ in terms of $g_1$: 

$$
\begin{align*}
g_2(x) &= g(x) \cdot (-2x)\\
g_3(x) &= -x - x^2 - x^3 - \ldots = \frac{-x}{1-x}\\
&\implies\\
g_1(x) &\cdot (1-2x) + g_3(x) = 0\\
&\implies\\
g_1(x) &= \frac{x}{(1-x)(1-2x)}
\end{align*}
$$

We want to find Taylor series of this thing. The starting point of breaking this to parts is to construct linear terms: 

$$
\begin{align*}
g_1(x) &= \frac{x}{(1-x)(1-2x)} = \frac{c_1}{1-x} + \frac{c_2}{1-2x}
\end{align*}
$$

Solving for $c_1$ and $c_2$ we get: 
$$
g_1(x) = \frac{-1}{1-x} + \frac{1}{1-2x}
$$
As we have $\frac{1}{1-x} = 1 + x + x^2 + \ldots$, we conclude: 

$$
\begin{align*}
g_1(x) &= -\sum_{n \geq 0} x^n + \sum_{n \geq 0} (2x)^n\\
g_1(x) &= \sum_{n \geq 0}(2^n - 1)x^n\\
\implies \\
h_n &= 2^n - 1
\end{align*}
$$

Now we get THE function for Hanoi's Tower. 

---

<span style="color:#28a745">Definition</span> Homogeneous recurrence relation is a recurrence of the form:
$$
h_n = a_1 h_{n-1} + a_2h_{n-2} + \ldots + a_kh_{n-k}
$$
Where $k$ and $a_i$'s are constants. 

<span style="color:#28a745">Definition</span> The characteristic polynomial of the homogeneous linear recurrence:

$$
\begin{align*}
\frac{P(x)}{x^{n-k}} &= \frac{x^n - (a_1 x^{n-1} + a_2 x^{n-2} + \ldots + a_k x^{n-k})}{x^{n-k}}\\
r(x) &= x^k - (a_1x^{k-1} + a_2x^{k-2} + \ldots + a_kx)
\end{align*}
$$

$r(x)$ is the characteristic polynomial of the homogeneous linear recurrence.



### <span style="color:#3c66b5">Theorem</span>

Let $(h_i)_{i\ge0}$ be a sequence of numbers defined by the **homogeneous** recurrence relation

$$
h_n + a_1h_{n-1}+\cdots+a_kh_{n-k}=0, \, (n \ge k)
$$

of order $k$ and with initial values for $h_0, h_1,\ldots,h_{k-1}$.

Let $r(x) = x^k +a_1 x^{k-1}+a_2x^{k-2}+\cdots+a_k$ be the characteristic polynomial of this recurrence relation. 

Then, the generating function of $(h_i)_{i\ge0}$ is given by $g(x) = p(x)/q(x)$, where $q(x) = x^k r(1/x)$ and

$$
p(x) = h_0 + (h_1+a_1h_0)x+(h_2+a_1h_1+a_2h_0)x^2+\cdots+(h_{k-1}+a_1h_{k-2}+\cdots+a_{k-1}h_0)x^{k-1}
$$

$p(x)$ in another form:

$$
p(x) = \sum_{i = 0}^{k - 1}\left( \sum_{j = 0}^{i} a_jh_{i-j}\right)x^i
$$

where we define $a_0 = 1$.


> Note that this is slightly different from the previous definition, but this is still a homogeneous recurrence relations. 

<span style="color:#eb861c">Proof</span> is not given... I think just understanding this theorem's statement what matters.

