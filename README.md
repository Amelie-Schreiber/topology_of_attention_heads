# topology_of_attention_heads
Persistent topological representation of attention heads within a transformer model. 


To determine if the sum of two distance metrics is also a distance metric, let's review the properties that a function must satisfy in order to be considered a distance metric. A function $d(x, y)$ is a distance metric if it satisfies the following four conditions for all points $x$, $y$, and $z$:

1. Non-negativity: $d(x, y) \geq 0$
2. Identity of indiscernibles: $d(x, y) = 0$ if and only if $x = y$
3. Symmetry: $d(x, y) = d(y, x)$
4. Triangle inequality: $d(x, y) + d(y, z) \geq d(x, z)$

Now, let's assume we have two distance metrics $d_1(x, y)$ and $d_2(x, y)$. We'll create a new function $d(x, y) = d_1(x, y) + d_2(x, y)$ and check if it satisfies the properties of a distance metric.

1. Non-negativity: Since $d_1(x, y)$ and $d_2(x, y)$ are both non-negative, their sum will also be non-negative.

2. Identity of indiscernibles:
    If $x = y$, both $d_1(x, y)$ and $d_2(x, y)$ will be $0$, so their sum will be $0$.
    If $d(x, y) = 0$, that means $d_1(x, y) + d_2(x, y) = 0$. Since both $d_1$ and $d_2$ are distance metrics, they must be non-negative, so the only way for their sum to be $0$ is if both $d_1(x, y)$ and $d_2(x, y)$ are $0$. This implies $x = y$.

3. Symmetry:
    $d(x, y) = d_1(x, y) + d_2(x, y) = d_1(y, x) + d_2(y, x) = d(y, x)$

4. Triangle inequality:
    $d(x, y) + d(y, z) = (d_1(x, y) + d_2(x, y)) + (d_1(y, z) + d_2(y, z))$
    $\phantom{=} = (d_1(x, y) + d_1(y, z)) + (d_2(x, y) + d_2(y, z))$

Since $d_1$ and $d_2$ are distance metrics, they both satisfy the triangle inequality. So,
$d_1(x, y) + d_1(y, z) \geq d_1(x, z)$ and $d_2(x, y) + d_2(y, z) \geq d_2(x, z)$.
Adding these inequalities, we have:
$(d_1(x, y) + d_1(y, z)) + (d_2(x, y) + d_2(y, z)) \geq d_1(x, z) + d_2(x, z)$
which simplifies to:
$d(x, y) + d(y, z) \geq d(x, z)$

Since $d(x, y)$ satisfies all four properties of a distance metric, we can conclude that the sum of two distance metrics is also a distance metric.
