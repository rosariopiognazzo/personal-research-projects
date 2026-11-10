# On the Reliability of Neural Networks

A reliability-theory approach to analyzing neural networks: instead of judging a model only by performance metrics, we quantify how *reliably* each layer behaves when the network produces a correct output.

> **Status:** preprint (not peer-reviewed). Author: Rosario Pio Gnazzo, Department of Computer Science, University of Salerno.

## Motivation

Deep learning models achieve excellent performance, but evaluation is usually limited to performance-based metrics (e.g. accuracy). There are no formal metrics to quantify a model's *safety margin* or predictive robustness in safety-critical settings. Classical reliability theory, long used in engineering to assess multi-component systems, offers the right toolkit.

## Idea

Each neuron is treated as a component of a system, and each layer as a stochastic **k-out-of-n system**, focusing on **neuronal activations** rather than weights.

- A neuron $i$ in layer $k$ is *active* if its post-activation value exceeds a layer-dependent threshold $\tau^{(l)}$.
- Its reliability is the probability of being active **given that the network outputs a correct result**: $R^{(k)}_i = \mathbb{P}\{A^{(k)}_i = 1 \mid B\}$.
- Since neurons in a layer are not identically distributed (some always fire, others never), a Binomial model does not fit. The number of active neurons in a layer is instead modeled with a **Beta-Binomial** distribution, which captures over-dispersion and neuron heterogeneity.

## Method

1. **Layer reliability.** The reliability of layer $k$ is the probability that at least $s_{(k)}$ neurons are active, under $L_{(k)} \sim \text{BetaBin}(n_{(k)}, \alpha_{(k)}, \beta_{(k)})$.
2. **Parameter estimation.** $\alpha_{(k)}$ and $\beta_{(k)}$ are estimated with the **Method of Moments**, using the number of active neurons observed during training on correctly classified samples.
3. **Minimum activation threshold $s_{(k)}$.** The smallest number of active neurons guaranteeing a prescribed reliability $1-q$. For large layers, the Beta-Binomial is approximated by a Normal, giving a closed form: $s_{(k)} \approx \hat\mu_{(k)} + \Phi^{-1}_q \hat\sigma_{(k)}$.
4. **Operational metric $\rho_{(k)} = s_{(k)} / n_{(k)}$.** The minimum fraction of active neurons a layer needs to be reliable. A low $\rho_{(k)}$ suggests redundancy (robustness); a high one suggests information spread across many neurons (a hypothesis still to be validated empirically).
5. **Confidence intervals (Delta Method).** Analytical intervals are derived for $\hat\alpha_{(k)}, \hat\beta_{(k)}$ (via the Jacobian of the moment estimators and the covariance of sample mean/variance) and for the layer reliability $\mathcal{R}_{(k)}$ (via its sensitivity vector with respect to $\alpha, \beta$).

## What the intervals give you

- **Lower Confidence Bound:** a worst-case guarantee, e.g. *"layer $k$, activating at least $s_{(k)}$ neurons, has reliability at least X% with 95% confidence."*
- **Training robustness:** interval width reflects the stability of a layer's internal behavior (narrow = stable convergence, wide = volatile).

## Scope and next steps

This work presents the theoretical formulation. Planned directions include studying the asymptotic behavior of the intervals, and building a framework to automate the statistical inference, monitor layer reliability at runtime, and validate the method empirically.

## Repository contents

- `main.tex`: paper source (compiled with the TMLR style, `preprint` option; requires `tmlr.sty` and `math_commands.tex`).

## Citation

```bibtex
@misc{gnazzo2026reliability,
  author = {Gnazzo, Rosario Pio},
  title  = {On the Reliability of Neural Networks},
  year   = {2026},
  note   = {Preprint}
}
```
