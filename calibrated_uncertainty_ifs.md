---
layout: page
title: Uncertainty Estimation
subtitle: Using Inflationary Flows for Calibrated Inference 
mathjax: true
---

### 7.1 Sources of error when doing inference and generation with Inflationary Flows 

Several previous lines of work ([Gneiting & Raftery, 2012](https://www.tandfonline.com/doi/abs/10.1198/016214506000001437); [Diebold & Mariano, 2012](https://www.tandfonline.com/doi/abs/10.1198/073500102753410444); [Malinin & Gales, 2018](https://proceedings.neurips.cc/paper_files/paper/2018/file/3ea2db50e62ceefceaf70a9d9a56a6f4-Paper.pdf); [Yao et al., 2019](https://arxiv.org/pdf/1906.09686); [Urteaga et al., 2021](https://proceedings.mlr.press/v149/urteaga21a.html))
have focused on assessing how well model-predicted marginals \\( p(\mathbf{x}) \\) match real data 
(i.e., the <em>predictive</em> calibration case). Though we do compare our models' predictive calibration performance 
against existing injective flow models (see [Experiments on image benchmark datasets]({% link hd_exps.md %})), here we are primarily focused on 
quantifying error in unconditional posterior inference. That is, we are interested in quantifying the mismatch between 
inferred posteriors \\( q(\mathbf{z}|\mathbf{x}) \\) and true posteriors \\( p(\mathbf{z}|\mathbf{x}) \\), especially in 
contexts where the true generative model is unknown and must be learned from data. This is by far the most common scenario in 
modern generative models like VAEs, flows, and GANs.

As with other implicit models, our inflationary flows provide a deterministic link between complex data and 
simplified distributions with tractable sampling properties. This mapping requires integrating the proposed 
[general pfODE]({% link inflationary_flows_model.md %}) for a given choice of \\( \mathbf{C}(t) \\) and 
\\( \mathbf{A}(t) \\) and an estimate of the score function of the original data. As a result, sampling-based estimates of 
uncertainty are trivial to compute: given a prior \\( \pi(\mathbf{x}) \\)  over the data (e.g., a Gaussian ball centered on 
a particular example \\( \mathbf{x}_0 \\)) , this can be transformed into an uncertainty on the dimension-reduced space by sampling 
\\( \{ \mathbf{x}_j\} \sim \pi(\mathbf{x}) \\) and integrating the  [general pfODE]({% link inflationary_flows_model.md %}) 
forward to generate samples from \\( \int p(\mathbf{x}_T|\mathbf{x}_0)\pi(\mathbf{x}_0)\,d\mathbf{x}_0 \\). As with MCMC, 
these samples can be used to construct either estimates of the posterior or credible intervals. Moreover, 
because the pfODE is unique given \\( \mathbf{C} \\), \\( \mathbf{A} \\), and the score, the model is <em>identifiable</em> 
when conditioned on these choices. 

The only potential source of error, apart from Monte Carlo error, in the above procedure
arises from the fact that the score function used in [general pfODE]({% link inflationary_flows_model.md %}) is 
only an <em>estimate</em> of the true score. 


### 7.2 Calibration Experiments on Toy Datasets 

To assess whether integrating noisy estimates of the score could produce errant 
posterior samples, we conducted a simple MCMC experiment on the 2D circles dataset (<strong>Appendix B.6</strong>). 
Briefly, we drew samples from a 3-component Gaussian Mixture Model (GMM) prior \\( \mathbf{z} \\) and integrated the generative process 
backward in time to obtain corresponding data space samples \\( \mathbf{x} \\) (see <strong>HMC Calibration Experiment Schematic</strong> below, 
components shown in orange, blue, and purple).


<figure>
    <figcaption><strong><center><font size="+2.5">HMC Calibration Experiment Schematic</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/11/MCMC_schematic-scaled.jpg" alt="schematic">
</figure>


We then utilized Hamiltonian Monte Carlo (HMC) ([Cobb & Jalaian, 2021](https://proceedings.mlr.press/v161/cobb21a.html); [Chen et al., 2014](http://proceedings.mlr.press/v32/cheni14.html); [Homan & Gelman, 2014](https://dl.acm.org/doi/10.5555/2627435.2638586)) 
to obtain samples from the posterior distribution over the weights of the GMM components. 
Below (see <strong>HMC Posterior Over GMM Weights, panels A, B</strong>), we showcase kernel density estimates (KDEs) constructed from the joint posterior 
samples over the mixture distribution weights in the dimension-preserving and dimension-reducing cases. 
Dashed vertical and horizontal lines indicate posterior means for each component. Reference ground-truth weights were 
\\( \mathbf{w} = [0.5, 0.25, 0.25] \\). Note that the resulting posterior correctly covers the original 
ground-truth values, suggesting that numerical errors in score estimates, at least in this simplified scenario, do not 
appreciably accumulate. 

<figure>
    <figcaption><strong><center><font size="+2.5">HMC Posterior Over GMM Weights</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/12/MCMC_plots_FINAL_12262024-scaled.jpg" alt="schematic">
</figure>

This is because, empirically, score estimates do not appear to be 
strongly auto-correlated in time (see plots of network residual auto-correlations (i.e., \\( R(t_1, t_2) \\)) below and <strong>Appendix C.3</strong> for details), 
suggesting that \\( \mathbf{\hat{s}}(\mathbf{x}, t) \\), 
the score estimate, is well approximated as a scaled colored noise process and the corresponding pfODE as an SDE. In such a case, 
standard theorems for SDE integration show that while errors due to noise do accumulate, these can be mitigated by a careful choice 
of integrator and ultimately controlled by reducing step size 
([Kloeden & Platen, 1992](https://link.springer.com/book/10.1007/978-3-662-12616-5); [Mou et al., 2019](https://arxiv.org/pdf/1907.11331)). 


<figure>
    <figcaption><strong><center><font size="+2.5">Network Residual Auto-Correlations</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Network_ACs.jpeg" alt="auto-correlations">
</figure>  


Finally, we verified this empirically with both additional low-dimensional experiments
([Additional Toy Calibration Experiments]({% link additional_toy_mesh_exps.md %}))
and with round-trip integration of the pfODE in high-dimensional datasets (see [Experiments on image benchmark datasets]({% link hd_exps.md %}, <strong> Appendix B.5.1 </strong>)).


#### Go to next section [Experiments on image benchmark datasets]({% link hd_exps.md %})

