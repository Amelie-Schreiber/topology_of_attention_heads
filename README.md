# topology_of_attention_heads


We focus on computing the distance between two attention heads to analyze the behavior of attention heads in deep learning models such as BERT and GPT-2. We define the distance metric between two heads $H_i$ and $H_j$ as follows:

$$
D(H_i, H_j) = \sum_{\text{token} \in \text{input data}} JS(H_i(token), H_j(token))
$$

Here, $JS$ denotes the Jensen-Shannon divergence. In order to perform a persistent homology analysis, which necessitates a true distance metric, we must ensure that our distance metric satisfies the properties of non-negativity, identity of indiscernibles, symmetry, and triangle inequality. The Jensen-Shannon divergence already satisfies the first three properties; thus, we need only address the triangle inequality. We achieve this by employing the square root of Jensen-Shannon divergence, known as Jensen-Shannon distance (JSD), as our distance metric:

$$
D(H_i, H_j) = \sum_{\text{token} \in \text{input data}} \sqrt{ JS(H_i(\text{token}), H_j(\text{token}))}.
$$

With this modification, our distance metric now satisfies all the necessary properties (which we prove below). We subsequently compute the persistent homology and plot the barcode diagram for BERT and GPT-2 individually, followed by a joint analysis, and calculate the bottleneck distance between their barcode diagrams. This approach is inspired by the analysis in §6 of the article [What Does BERT Look At? An Analysis of BERT’s Attention](https://arxiv.org/abs/1906.04341), which investigates the similarity and grouping of attention heads based on their behavior.

The findings suggest that attention heads within the same layer exhibit similar attention distributions, forming distinct clusters. This observation is intriguing considering that [Multi-Head Attention with Disagreement Regularization](https://arxiv.org/abs/1810.10183) demonstrated that encouraging diverse attention head behavior can enhance Transformer performance in machine translation. One possible explanation for this redundancy in BERT's attention heads is the use of attention dropout during training. Consequently, we can infer that persistent topological features may negatively impact model performance, prompting the need to discourage such features during training. By promoting larger Jensen-Shannon distances between specific attention heads that form clusters or higher-dimensional topological features, we enable the model to increase pairwise distances $D(H_i, H_j)$ without dictating the distribution of individual attention weights of a single head, thereby retaining flexibility in the distribution of attention weights while hopefully enhancing overall performance.

### Proving the distance metric properties

Now, that for an individual token, $\sqrt{ JS(H_i(\text{token}), H_j(\text{token}))}$ is a distance metric. If we want to prove that $D(H_i, H_j) = \sum_{\text{token} \in \text{input data}} \sqrt{ JS(H_i(\text{token}), H_j(\text{token}))}$ is a distance metric we technically need to show that the sum of two distance metrics is a distance metric since for two or more tokens we are summing two or more distance metrics. It suffices to show that the sum of two distance metrics is a distance metric. 

To determine if the sum of two distance metrics is also a distance metric, let's review the properties that a function must satisfy in order to be considered a distance metric. A function $d(x, y)$ is a distance metric if it satisfies the following four conditions for all points $x$, $y$, and $z$:

1. Non-negativity: $d(x, y) \geq 0$
2. Identity of indiscernibles: $d(x, y) = 0$ if and only if $x = y$
3. Symmetry: $d(x, y) = d(y, x)$
4. Triangle inequality: $d(x, y) + d(y, z) \geq d(x, z)$

Now, let's assume we have two distance metrics $d_1(x, y)$ and $d_2(x, y)$. We'll create a new function $d(x, y) = d_1(x, y) + d_2(x, y)$ and check if it satisfies the properties of a distance metric.

1. Non-negativity:
    Since $d_1(x, y)$ and $d_2(x, y)$ are both non-negative, their sum will also be non-negative.

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
