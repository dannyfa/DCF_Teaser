---
layout: page
title: PR-Reducing pfODEs
subtitle: How to set up a pfODE that reduces PR ? 
mathjax: true
---

### 5.1 Dimension-reducing pfODEs

In the previous section, we saw that isotropic inflation preserves intrinsic data dimensionality as measured by PR. 
Here, we generalize and consider <em>anisotropic</em> inflation at different rates along each of the eigenvectors of 
\\( \boldsymbol \Sigma \\): \\( \frac{d}{dt}(\boldsymbol \sigma^2) = \rho \mathbf{g} \odot \boldsymbol \sigma^2 \\) with 
\\( g_* \equiv \max(\mathbf{g}) \\) so that the fastest inflation rate is \\( \rho g_* \\). Then, if we take 
\\( g_i = g_* \\) for \\( i \in \{i_1, i_2, \ldots i_K \} \\) and \\( g_i < g_* \\)  for the other dimensions, 

$$ PR(\boldsymbol \Sigma(t)) =\frac{(\sum_i \sigma^2_{0i} e^{(g_i - g_*) \rho t})^2}{\sum_i (\sigma^2_{0i} e^{(g_i - g_*) \rho t})^2}  \xrightarrow[t\rightarrow\infty]{} \frac{(\sum_{k=1}^K \sigma^2_{0i_k})^2}{\sum_{j=1}^K \sigma_{0i_j}^4 } $$ 

which is the dimension that would be achieved by simply truncating the original covariance matrix in a manner set by our choice of \\( \mathbf{g} \\). Here, 
unlike in PR-preserving schedule, we do not aim for rescaling to compensate for expansion along each dimension, 
since that would undo the effect of differential inflation rates. Instead, we choose a single global rescaling factor 
\\( \alpha(t) \propto A_0\exp(-\rho g_* t/2) \\), which converges to a Gaussian distribution with covariance matching that of 
the initial data in all dimensions with \\( g_i = g_* \\). 

Two additional features of this class of flows are worth noting: 
First, the final scale ratio of preserved to shrunken dimensions for finite integration times \\(T\\) is governed by the 
quantity \\(e^{\rho (g_* - g_i)T} \\) in the equation above. For good compression, we want this number to be very large,
but as shown in <strong>Appendix A.4</strong> of our paper, this corresponds to a maximum injected noise of order 
\\( e^{\rho (g_* - g_i)T/2} \\) in the equivalent DBM. That is, the compression one can achieve with inflationary flows 
is constrained by the range of noise levels over which the score function can be accurately estimated, and this is quite limited 
in typical models. 

Second, despite the appearance given by the equation above, the corresponding flow <em>is not</em> simply 
a linear projection to the top \\(K\\) principal components: though higher PCs are selectively removed by dimension-reducing flows 
via exponential shrinkage, individual particles are repelled by <em>local</em> density as captured by the score function, 
and this term couples different dimensions even when \\( \mathbf{C} \\) and \\( \mathbf{A} \\) are diagonal. Thus, 
the final positions of particles in the retained dimensions depend on their initial positions in the full space, 
producing a <em> nonlinear map </em>. 


### 5.2 Dimension-reducing pfODE experiments on toy datasets

Animations below show results of numerically integrating the dimension-reducing pfODE proposed above for 
5 toy datasets (2D circles, sine, and moons; 3D s-curve and swirl) both forwards (i.e., inflation) and backwards (i.e., generation)
in time. Simulations were conducted utilizing score approximations obtained from neural networks trained on each respective toy 
dataset. Note that utilizing the proposed forms for \\( \mathbf{C}(t) \\) and \\( \mathbf{A}(t) \\) allows us to construct a pfODE 
that maps nonlinear manifolds to lower-rank Gaussians (reduction from \\( 2D \to 1D \\) or \\(3D \to 2D\\)) or, 
if integrating backwards intime, to generate samples from the desired distributions using  samples from lower-dimensional Gaussians. 

For 2D toy datasets, reducing from \\( 2D \to 1D \\) in eigenbasis yields a line in the original space at the end of inflation. 
For 3D toys, reducing from \\( 3D \to 2D \\) in eigenbasis yields a tilted plane in the original space at the end of inflation. 
To facilitate visualization of the 2D plane formed in latent space, we rotate the pre and post-integration states for both inflation
and generation simulations in 3D toys. Note that, for all datasets, compression is <em> not </em> lossless - there are small
differences in original shape for 2D cases and in original thickness content for 3D toys (see comments on Appendix C.2.2).


### 5.2.1 2D Circles 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/CV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Circles_PRR_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 5.2.2 2D Moons

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/H14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Moons_PRR_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 5.2.3 2D Sine 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/I14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Sine_PRR_Joint_MeltGen.mp4" type="video/mp4">
</video></p>--->

### 5.2.4 3D S-Curve

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/L14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/SCurve_PRR_Full.mp4" type="video/mp4">
</video></p>--->

### 5.2.5 3D Swirl 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/JV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/Swirl_PRR_Full.mp4" type="video/mp4">
</video></p>--->

#### Go to next section [Score Function Approximation from DBMs]({% link score_fn_approx.md %})
