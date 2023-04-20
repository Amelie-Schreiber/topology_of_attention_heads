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

The findings suggest that attention heads within the same layer exhibit similar attention distributions, forming distinct clusters. This observation is intriguing considering that [Multi-Head Attention with Disagreement Regularization](https://arxiv.org/abs/1810.10183) demonstrated that encouraging diverse attention head behavior can enhance Transformer performance in machine translation. One possible explanation for this redundancy in BERT's attention heads is the use of attention dropout during training. Consequently, we can infer that persistent topological features may negatively impact model performance, prompting the need to discourage such features during training. By promoting larger Jensen-Shannon distances between specific attention heads that form clusters or higher-dimensional topological features, we enable the model to increase pairwise distances $D(H_i, H_j)$ without dictating the distribution of individual attention weights of a single head, thereby retaining flexibility in the distribution of attention weights while enhancing overall performance.
