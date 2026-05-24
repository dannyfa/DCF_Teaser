---
layout: page
title: Score Function Approximation 
subtitle: Using DBMs' score function to simulate our pfODEs
mathjax: true
---

### 6. Score Function Approximation from DBMs 

Having chosen inflation and rescaling schedules, the last component needed for our 
[general pfODE]({% link inflationary_flows_model.md %}) is the score 
function \\( \mathbf{s}(\mathbf{x}, t) \equiv \nabla_{\mathbf{x}} \log p_t(\mathbf{x}) \\). Our strategy 
will be to exploit the correspondence described previously between diffusion models' forward SDEs and their correspoinding pfODEs 
that give rise to the same marginals (i.e., same the Fokker-Planck equation - [three views of DBMs]({% link three_views_DBMs.md %})). 
That is, we will learn an approximation to \\( \mathbf{s}(\mathbf{x},t) \\) by fitting the DBM corresponding to our desired pfODE, 
since both make use of the same score function.

Briefly, in line with previous work on DBMs ([Karras et al., 2022](https://arxiv.org/pdf/2206.00364)), we train neural networks 
to estimate a de-noised version \\( \mathbf{D}(\mathbf{x}, \mathbf{C}(t)) \\) of a noise-corrupted data sample 
\\( \mathbf{x} \\)  given noise level \\( \mathbf{C}(t)\\) (cf. <strong>Appendix A.4</strong> of our paper for the correspondence between 
\\( \mathbf{C}(t) \\) and noise). That is, we model \\( \mathbf{D}_\theta(\mathbf{x}, \mathbf{C}(t)) \\) using a neural network 
and train it by minimizing a standard \\( L_2 \\) de-noising error: 

$$ \mathbb{E}_{\mathbf{y} \sim \text{data}} \mathbb{E}_{\mathbf{n} \sim \mathcal{N}(\mathbf{0}, \mathbf{C}(t))} \lVert \mathbf{D}(\mathbf{y+n}; \mathbf{C}(t)) - \mathbf{y}  \rVert^2_2 $$

De-noised outputs can then be used to compute the desired score term 
using \\( \nabla_{\mathbf{x}} \log p(\mathbf{x}, \mathbf{C}(t)) = \mathbf{C}^{-1}(t) \cdot \left( \mathbf{D}(\mathbf{x}; \mathbf{C}(t)) - \mathbf{x}\right) \\) ([Song et al., 2021](https://arxiv.org/pdf/2011.13456); [Karras et al., 2022](https://arxiv.org/pdf/2206.00364)). 
Moreover, as in [Karras et al., 2022](https://arxiv.org/pdf/2206.00364) , we also adopt a series of preconditioning factors 
aimed at making training with the above \\( L_2 \\)  loss and our noising scheme more amenable to gradient descent techniques (<strong>Appendix B.1</strong> of our paper).

#### Go to next section [Calibrated Uncertainty Estimates from Inflationary Flows]({% link calibrated_uncertainty_ifs.md %})