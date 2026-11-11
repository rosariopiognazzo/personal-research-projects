# MACLICO

**Hypergraph Construction via Maximal Clique based Set Covering**

Rosario Pio Gnazzo, Department of Computer Science, University of Salerno
([rosariopiognazzo@gmail.com](mailto:rosariopiognazzo@gmail.com))

*Italiano: vedi [README.it.md](README.it.md)*

---

## Overview

MACLICO is an integer linear programming (ILP) framework that takes a weighted graph $G = (V, E_g)$ as input and returns its generalization as a hypergraph $H = (V, E_h)$. The idea is that a hypergraph built from a graph should capture the topological relationships among nodes, covering all vertices with the smallest possible number of hyperedges. To make the construction robust, the framework prevents a node from appearing in too many hyperedges and favors hyperedges with strong internal correlation.

## How it works

MACLICO is made of three sequential steps.

1. **Maximal clique enumeration.** All maximal cliques of the graph are extracted with the Bron–Kerbosch algorithm, in two variants: with pivoting and with vertex (degeneracy) ordering. This yields a collection of subsets $\{C_j\}_{j=1}^{k}$.

2. **Clique weighting.** Each clique $C_J$ receives an internal cohesion score computed from the weights of its internal edges, then normalized so that the scores sum to one:

   $$z_J = \frac{\sum_{(v_i, v_j) \in E_J} w_{ij}}{\binom{|V_J|}{2}}, \qquad \lambda_J = \frac{z_J}{\sum_{j=1}^{k} z_j}$$

3. **Clique-based set cover.** A variant of the Set Cover problem selects the cliques that cover every vertex:

   $$\min_{y,\,R} \;\; \alpha R + \beta \sum_{j=1}^{k} y_j - \gamma \sum_{j=1}^{k} y_j \lambda_j$$

   subject to

   $$\sum_{j} a_{ij} y_j \ge 1 \;\; \forall v_i \in V, \qquad \sum_{j} a_{ij} y_j \le R \;\; \forall v_i \in V, \qquad y_j \in \{0,1\}, \;\; R \ge 1$$

   where $y_j = 1$ if clique $C_j$ is selected, $a_{ij} = 1$ if $v_i \in C_j$, and $R$ is the maximum redundancy, i.e. the maximum number of selected cliques containing the same vertex.

The selected cliques are the hyperedges of the resulting hypergraph.

## Tuning the objectives

The parameters $\alpha, \beta, \gamma \ge 0$ control the trade-off between maximum redundancy, number of hyperedges and total internal cohesion. The paper derives sufficient conditions that enforce a **lexicographic priority** among the three objectives, together with explicit parameter choices (under the normalization $\sum_j \lambda_j = 1$ and the bound $R_{\max} \le m$):

| Priority order | Parameters |
|---|---|
| redundancy, then number of hyperedges, then cohesion | $\alpha = 2m,\; \beta = 2,\; \gamma = 1$ |
| number of hyperedges, then redundancy, then cohesion | $\alpha = 2,\; \beta = 2m + 2,\; \gamma = 1$ |

## Paper

The full description of the framework is in [`MACLICO.pdf`](MACLICO.pdf).

## Contact

For questions or feedback: [rosariopiognazzo@gmail.com](mailto:rosariopiognazzo@gmail.com)
