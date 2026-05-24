---
layout: page
title: Inflationary Flows 
subtitle: Introducing our pfODE
mathjax: true
---

### 2. Inflationary Flows pfODE 

As argued on [three views of DBMs]({% link three_views_DBMs.md %}), the probability flow ODE offers a means of deterministically 
transforming an arbitrary data distribution into a simpler form via a score function learnable through DBM training. 
Here, we introduce a specialized class of pfODEs, <em> Inflationary Flows, </em> that follow from an intuitive picture 
of local dynamics and asymptotically give rise to stationary Gaussian solutions of the Fokker-Plack equation. 

We begin by considering a sequence of marginal transformations in which points in the original data distribution are convolved with 
Gaussians of increasingly larger covariance \\( \mathbf{C}(t) \\):

$$ p_t(\mathbf{x}) = p_0(\mathbf{x}) * \mathcal{N}(\mathbf{x}; \mathbf{0}, \mathbf{C}(t)) $$ 

It is straightforward to show (<strong>Appendix A1</strong> of our paper) that this class of time-varying densities satisfies the Fokker-Planck equation
when \\(\mathbf{f} = \mathbf{0}\\) and \\(\mathbf{GG^\top} = \mathbf{\dot{C}}\\).
This can be viewed as a process of deterministically "inflating" each point in the data set, or equivalently as smoothing 
the underlying data distribution on ever coarser scales, similar to denoising approaches to DBMs ([Raphan & Simoncelli, 2011](https://www.cns.nyu.edu/pub/lcv/raphan10.pdf); [Kadkhodaie & Simoncelli, 2021](https://proceedings.neurips.cc/paper_files/paper/2021/file/6e28943943dbed3c7f82fc05f269947a-Paper.pdf)) 

Eventually, if the smoothing kernel grows much larger than \\( \boldsymbol \Sigma_0 \\), the covariance in the original data, 
total covariance \\( \boldsymbol \Sigma(t) \equiv \boldsymbol\Sigma_0 + \mathbf{C}(t) \rightarrow \mathbf{C}(t)\\), 
\\(p_t(\mathbf{x}) \approx \mathcal{N}(\mathbf{0}, \mathbf{C}(t))\\), and all information has been removed from the original distribution. 
However, because it is numerically inconvenient for the variance of the asymptotic distribution \\(p_\infty(\mathbf{x})\\) to grow much larger 
than that of the data, we follow previous work ([Song et al., 2021](https://arxiv.org/pdf/2011.13456); [Karras et al., 2022](https://arxiv.org/pdf/2206.00364)) 
in adding a time-dependent coordinate rescaling \\(\mathbf{\tilde{x}}(t) = \mathbf{A}(t) \cdot \mathbf{x}(t)\\), 
resulting in an asymptotic solution \\(p_\infty(\mathbf{x}) = \mathcal{N}(\mathbf{0}, \mathbf{A} \boldsymbol\Sigma \mathbf{A^\top})\\) 
of the corresponding Fokker-Planck equation when \\( \boldsymbol{\dot \Sigma } = \mathbf{\dot{C}} \\) and 
\\(\mathbf{\dot{A}}\boldsymbol\Sigma\mathbf{A}^\top + \mathbf{A}\boldsymbol\Sigma\mathbf{\dot{A}}^\top = \mathbf{0}\\) (<strong>Appendix A.2</strong> of our paper). 

Together, these assumptions give rise to the general pfODE (<strong>Appendix A.3</strong> of our paper):

$$ \frac{\mathrm{d}\mathbf{\tilde{x}}}{\mathrm{d}t} = \mathbf{A}(t) \cdot \left( -\frac{1}{2} \mathbf{\dot{C}}(t) \cdot \nabla_{\mathbf{x}} \log p_t(\mathbf{x}) \right)+ \left( \mathbf{\dot{A}}(t) \cdot \mathbf{A^{-1}}(t) \right) \cdot \mathbf{\tilde{x}}$$

where the score function is evaluated at \\(\mathbf{x} = \mathbf{A^{-1}\cdot \mathbf{\tilde{x}}} \\).
Notably, the above general pfODE is equivalent to the general pfODE form given in [Karras et al., 2022](https://arxiv.org/pdf/2206.00364)
in the case both \\(\mathbf{C}(t)\\) and \\(\mathbf{A}(t)\\) are isotropic (<strong>Appendix A.4</strong> of our paper),
with \\(\mathbf{C}(t)\\) playing the role of injected noise and \\(\mathbf{A}(t)\\) the role of the scale schedule. 

In the following sections, we will show how to choose both of these in ways that either preserve or reduce intrinsic data dimensionality.

#### Go to next section [Participation Ratio (PR)]({% link participation_ratio.md %})



