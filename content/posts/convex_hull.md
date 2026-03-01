---
title: "Convex Hull in 2D"
date: 2024-08-29T17:02:49-05:00
draft: false
math: true
tags: []
---

<span style="color:#28a745">Definition</span> Problem: Given $n$ points $P = \set{p_1, \ldots, p_n} \subset \mathbb{R}^2$, compute $\text{CH}(P) = $ smallest convex set contining $P$. 

Input: a list of points all the points in $P$.

Output: a list of points in $P$ that belongs to this convex set in CCW, starting from the left most point.

#### Applications

- Given a huge point cloud, convex hull helps find the "shape".

- Bounding volume. Makes ray shooting tasks faster.
- Extreme points. It gives the most extreme points in any given direction.
- Farthest pair.
- Line separation. Natural, but not fastest.



Now, let's design some algorithms!

### Math properties

$$
\begin{align*}
\text{CH}(P) &= \bigcap_{\text{convex } S \supseteq P} S = 
\bigcap_{\text{half plane } h \supseteq P} h \\
&= \set{ \text{all convex combinations of } p_1, \ldots, p_n}\\
&= \bigcup_{i, j, k} \set{\text{all convex triples $i, j, k$ (triangle)}}
\end{align*}
$$


Convex combination:

$$
\sum_{i = 1}^{n} a_i p_i, \; \sum_{i = 1}^{n} a_i = 1, a_i \geq 0. 
$$



<span style="color:#599eff">Lemma</span> $p_i$ is a vertex of $\text{CH}(P)$ iff $\exists$   line through $p_i$ such that $P$ lines on one side. (all other points on one side.)

<span style="color:#599eff">Lemma</span> $p_i p_j$ is an edge of $\text{CH}(P)$ iff $P$ lines on one side of line $p_ip_j$. 

Note: suffice to compute upper hull(UH) to solve CH. UH is divided by $\min x, \max x$. 

---

### <span style="color:#3c66b5">Brute force</span>

```pseudocode
for i = 1..n
	for j = 1..n
		flag = true
		for k = 1..n
			if p_k above p_i p_j then flag = false
		if flag then output p_i p_j
```

$O(n^3)$ time.

Some simple post processing is required to get the correct output.

#### Implement issues: 

1. How to test $p_k$ is above $p_ip_j$? 
1. Degeneracies
1. precision issues.



#### 1. Test above

Say $p_i$ is on the left of $p_j$ (smaller $x$).  

We say $p_k$ below $p_ip_j$ 

- iff $p_k$ is at the right of $\vec{p_ip_j}$ 

- iff $p_ip_jp_k$ is in CW order 

- iff $(x_k, y_k)$ below line with equation: $y - y_i = \frac{y_j - y_i}{x_j - x_i}(x - x_i)$
- iff $y_k - y_i < \frac{y_j - y_i}{x_j - x_i}(x_k - x_i)$ .
- iff $(y_k - y_i)(x_j - x_i) < (y_j - y_i)(x_k - x_i)$. This is a cross product!
- iff

$$
2 \cdot \text{signed area of }\Delta  p_ip_jp_k
\begin{vmatrix}
1 & x_i & y_i\\
1 & x_j & y_j\\
1 & x_k & y_k\\
\end{vmatrix}
 < 0
$$


The good thing about the determinant test is that it extends to higher dimensions. 

#### 2. Degeneracies

What if some points are on the edge in real world data? We don't consider it in theory :))) We consider the points in "general position".

#### 3. Precision

More messy stuff...

---

### Algorithm 1: Jaruis March(1973) / "gift wrapping"

> Looks like a selection sort.

Idea: go from one UH vertex to next.

```pseudocode
q_1 = the leftmost point. 		<--- O(n) time.
for i = 1 to n do {
	if (q_i = rightmost point) then {
		return (q_1, ..., q_i)
	}
	q_{i+1} = any init point to the right.
	for k = 1 to n do {
		if (p_k right of q_i) AND (p_k above q_iq_{i+1}) then {
			q_{i+1} = p_k
		}
	}
}

```

$O(n^2)$ running time.



### Algorithm 2: Graham Scan (1972)

> Looks like an insertion sort.

Idea: 

- add points one at a time and maintain the UH. (Incremental approach). 
- go from left to right. (Sweeping technique)
- We add the highest point on the scanning line, then update the hull by relaxing. Keep deleting suffix untill a CW appears.

I will skip the pseudocode. 

#### Analysis:

Naively $O(n^2)$. But, what if we bound the number of iterations for relaxation? Observation, the total relaxation time is bounded by $O(n)$. 

$$
O(n + \# \text{decrement}) \leq O(n + \# \text{increment}) = O(n)
$$

But we do have a sorting in $O(n \log n)$.

