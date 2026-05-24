---
layout: page
title: PR-Preserving pfODEs
subtitle: How to set up a pfODE that preserves PR ? 
mathjax: true
---

### 4.1. Dimension-preserving pfODEs

To construct flows that preserve Participation Ratio (PR) dimensionality, we write total variance as 
\\( \boldsymbol \Sigma(t) = \text{diag}(\boldsymbol\sigma^2(t)) = \mathbf{C}(t) + \boldsymbol\Sigma_0 \\), 
where \\(\boldsymbol \Sigma_0 \\) is the original data covariance and \\( \mathbf{C}(t) \\) is our time-dependent smoothing kernel. 
Moreover, we will choose \\( \mathbf{C}(t) \\) to be diagonal in the eigenbasis of \\( \boldsymbol \Sigma_0 \\) and work in that basis, 
in which case \\( \boldsymbol \Sigma(t) = diag(\boldsymbol\sigma^2(t)) \\) and we have (<strong>Appendix A.6</strong> of our paper):

$$ dPR = 0 \iff \left( \mathbf{1} - PR(\boldsymbol \sigma^2)\frac{\boldsymbol \sigma^2}{\sum_k \sigma_k^2} \right) \cdot d\boldsymbol\sigma^2 = 0 $$

The simplest solution to this constraint is a proportional inflation, \\( \frac{d}{dt}(\boldsymbol \sigma^2) = \rho \boldsymbol \sigma^2 \\), 
along with a rescaling along each principal axis:

$$ C_{jj}(t) = \sigma^2_{j}(t) - \sigma^2_{0j} = \sigma_{0j}^2 (e^{\rho t} - 1) \qquad A_{jj}(t) = \frac{A_{0j}}{\sigma_j(t)} = \frac{A_{0j}}{\sigma_{0j}} e^{-\rho t/2} $$ 

As with other flow models based on physical processes like diffusion ([Sohl-Dickstein et al., 2015](https://arxiv.org/pdf/1503.03585)) or 
electrostatics ([Xu et al., 2022](https://arxiv.org/pdf/2209.11178); [Xu et al., 2023](https://proceedings.mlr.press/v202/xu23m/xu23m.pdf)), 
our use of the term inflationary flows for these choices is inspired by cosmology, where a similar process of rapid expansion exponentially 
suppresses local fluctuations in background radiation density ([Guth, 1981](https://journals.aps.org/prd/pdf/10.1103/PhysRevD.23.347)).
However, as a result of our coordinate rescaling, the effective covariance \\( \boldsymbol{\tilde{\Sigma}} = \mathbf{A} \boldsymbol\Sigma \mathbf{A^\top} = diag(A^2_{0j}) \\) 
remains constant (so \\( d\boldsymbol{\tilde{\sigma}^2} = \mathbf{0}\\) trivially), and the additional conditions 
of <strong>Appendix A.2</strong> (our paper) are satisfied, such that \\( \mathcal{N}(\mathbf{0}, \boldsymbol{\tilde{\Sigma}}) \\) 
is a stationary solution of the relevant rescaled Fokker-Planck equation. 


### 4.2. Dimension-preserving pfODE experiments on toy datasets

Animations below show results of numerically integrating the dimension-preserving pfODE proposed above for 
5 toy datasets (2D circles, sine, and moons; 3D s-curve and swirl) both forwards (i.e., inflation) and backwards (i.e., generation)
in time. Simulations were conducted utilizing score approximations obtained from neural networks trained on each respective toy 
dataset. Note that utilizing the proposed forms for \\( \mathbf{C}(t) \\) and \\( \mathbf{A}(t) \\) allows us to construct a pfODE 
that maps nonlinear manifolds to Gaussians and can be integrated in reverse to generate samples of the original data.

### 4.2.1 2D Circles 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/B14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Circles_PRP_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 4.2.2 2D Moons

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/F14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Moons_PRP_Joint_MeltGen.mp4" type="video/mp4">
</video></p>-->

### 4.2.3 2D Sine 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/IV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Sine_PRP_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 4.2.4 3D S-Curve


<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/LV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/SCurve_PRP_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 4.2.5 3D Swirl 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/KV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Swirl_PRP_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->  

#### Go to next section [Dimension-reducing pfODEs]({% link prr_pfODE.md %})
