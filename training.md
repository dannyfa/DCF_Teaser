---
layout: page
title: Training
subtitle: 
mathjax: true
---

Our model comprises both the parameters \\( (\mathbf{b}, \mathbf{L}, \mathbf{D}, \boldsymbol{\psi}) \\) of the encoder (Equation 1, Section 2.2) and of the compression and dynamical flows \\( (\boldsymbol{\phi}, \boldsymbol{\psi}) \\). The goal is to minimize flow matching losses for both \\( \mathbf{u}\_{\boldsymbol{\phi}} \\) and \\( \mathbf{v}\_{\boldsymbol{\psi}} \\) while minimizing the dimensionality of the source distribution. 

One potential approach to minimizing this latent dimensionality would be to shrink the diagonal scales \\( \{d_i\} \\) using, e.g.,  standard regularizers (ridge, LASSO) ([Hoerl & Kennard, 1970](https://homepages.math.uic.edu/~lreyzin/papers/ridge.pdf); [Tibshirani, 1996](https://academic.oup.com/jrsssb/article/58/1/267/7027929)) or global-local shrinkage such as the horseshoe ([Carvalho et al., 2010](https://proceedings.mlr.press/v5/carvalho09a/carvalho09a.pdf)).
However, we found that even for very severe shrinkage, the generative power of flow matching models is able to compensate nearly down to machine precision, encoding significant information in "unused" dimensions. As a result, such "soft" minimization approaches fail to provide meaningful limits on the dimensionality of source representations. An alternative, which we describe below, is to introduce stochastic sampling over source dimensionality, averaging over the size of this bottleneck during training.

### 3.1 Nested Dropout and Encoder Training 

Nested dropout (ND) ([Rippel et al., 2014](https://proceedings.mlr.press/v32/rippel14.html)) addresses two key issues previously noted: (1) the need to break permutation invariance among the entries of \\( \mathbf{D} \\) to ensure identifiability; and (2) the requirement of a true low-dimensional source representation. ND addresses these by randomly sampling the rank of \\( \mathbf{D} \\) in a way that enforces an ordering on latent coordinates. More specifically, at each forward pass, it samples a prefix length \\( K\sim \mathrm{Geom}(p) \\), with \\( \mathbb{E}(K)=1/p \\). This makes \\( 1/p \\) an effective latent dimensionality, and marginalizing over \\( K \\) provides control over the capacity of the source representation. In practice, we further truncate \\( K \\)  to \\( [1,D] \\), then apply the prefix mask:

$$
\begin{equation}
\mathbf{m}_K(i)=\mathbf{1}\{i\le K\},\qquad 
\boldsymbol{\mu}^{(K)}_{\boldsymbol{\psi}}(\mathbf{x})=\mathbf{m}_K\odot \boldsymbol{\mu}_{\boldsymbol{\psi}}(\mathbf{x}),
\label{eqn:nd_mask}
\end{equation}
$$

with \\( \boldsymbol{\mu}^{(K)}\_{\boldsymbol{\psi}} \\) replacing \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}} \\) in Equation 1 (Section 2.2).
As a result, only coordinates \\( 1{:}K \\) receive encoder updates in that pass, yielding explicit control of effective dimension. 

Note, however, that the flexibility of the nonlinear map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}} \\) that defines the encoder still defines an infinite family of source distributions \\( p\_{\boldsymbol{\psi},\tau=0}(\mathbf{x}) \\). Of these, we choose the one that minimizes a masked alignment loss under nested dropout,

$$
\begin{equation}
\mathcal{L}_{\mathrm{align}}=\mathbb{E}_{\mathbf{x}}\mathbb{E}_K\Big[\|\mathbf{x}_t^{(1)}-\mathbf{x}_t^{(0, K)}\|_2^2\Big],
\label{eqn:L_align}    
\end{equation}
$$

where \\( \mathbf{x}\_t^{(0, K)}=\mathbf{b}+\mathbf{LD}^{1/2}\boldsymbol{\mu}^{(K)}\_{\boldsymbol{\psi}}(\mathbf{x}_t^{(1)}) \\) is the masked encoder output as in Equation \\( \eqref{eqn:nd_mask} \\).

### 3.2 Flow Matching

We train the compressive flow and dynamical flow using conditional flow matching on the linear interpolations defined in Equation 3, Section 2.3 and Equation 4, Section 2.4.


#### Compressive FLow Matching
Sample \\( \tau\sim \mathrm{Unif}[0,1] \\) and form \\( \mathbf{x}\_t^{(\tau)} \\) via Equation 3, Section 2.3. For the linear bridge, the target conditional flow is constant, 

$$
\mathbf{u}_t^\star = \partial_\tau \mathbf{x}_t^{(\tau)} = \mathbf{x}_t^{(1)} - \mathbf{x}_t^{(0)}
$$

As in [Lipman et al., 2024](https://scontent-iad3-1.xx.fbcdn.net/v/t39.2365-6/469963300_2320719918292896_7950025307614718519_n.pdf?_nc_cat=108&ccb=1-7&_nc_sid=3c67a6&_nc_ohc=_N3PbClkPhkQ7kNvwE-mo0h&_nc_oc=Adq4hrL3iK8mJ5OkzwIf7P2uhv9FXL39usAm41r4x9kv27go5e0xjYxo7T-34vFEh4E&_nc_zt=14&_nc_ht=scontent-iad3-1.xx&_nc_gid=e8ejCUnovmgUL_EoA2nVOA&_nc_ss=7b289&oh=00_Af4iq3Tgl3lu4AsyZouPvkZSww64w1C6AlXJzPSKisFlfg&oe=6A1B7582), we minimize

$$
\begin{equation}
\mathcal{L}_{\mathrm{cf}} = \mathbb{E}_{\mathbf{x}}\mathbb{E}_\tau\Big[\|\mathbf{u}_{\boldsymbol{\phi}}(\mathbf{x}_t^{(\tau)},\tau)-\mathbf{u}_t^\star\|_2^2\Big].
\label{eqn:L_cf}
\end{equation}
$$

#### Dynamical Flow Matching 
At a fixed compression level \\( \tau \\), sample \\( s\sim \mathrm{Unif}[0,1] \\) and construct
\\( \mathbf{x}\_{t}^{(\tau)} \\) using the linear within-step bridge Equation 4, Section 2.4. For this bridge, the target velocity is constant in \\( t \\), 

$$
\mathbf{v}_{k,\tau}^\star
= \partial_t \mathbf{x}_{t}^{(\tau)}
= \frac{\mathbf{x}_{(k+1)\Delta t}^{(\tau)}-\mathbf{x}_{k\Delta t}^{(\tau)}}{\Delta t}
$$

and we minimize

$$
\begin{equation}
    \mathcal{L}_{\mathrm{df}} = \mathbb{E}_{\mathbf{x}}\mathbb{E}_{\tau, s}\Big[\|\mathbf{v}_{\boldsymbol{\theta}}(\mathbf{x}_{t}^{(\tau)},\tau,s,\mathbf{x}_{\mathrm{hist}}^{(\tau)})-\mathbf{v}_{k,\tau}^\star\|_2^2\Big].
    \label{eqn:L_df}
\end{equation}
$$

#### Training Objective
Putting Equations \\( \eqref{eqn:L_align} \\), \\( \eqref{eqn:L_cf} \\), and \\( \eqref{eqn:L_df} \\) together, we obtain the combined loss function

$$
\begin{equation}
\mathcal{L}
= \alpha\,\mathcal{L}_{\mathrm{cf}}
+ \beta\,\mathcal{L}_{\mathrm{df}}
+ \eta\,\mathcal{L}_{\mathrm{align}}.
\label{eqn:L_tot}
\end{equation}
$$

Unless otherwise specified, in all experiments, we set \\( \alpha=\beta=\eta=1 \\). 

#### Go to next section [Experiments]({% link experiments.md %})

<!--- \textbf{Appendix~\ref{app:ball_loss_weight}} provides a sensitivity check by varying $(\alpha,\beta,\eta)$ on the rotating-ball simulation. We found that the results were robust to these hyperparameters. --->