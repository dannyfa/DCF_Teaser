---
layout: page
title: Model
subtitle: 
mathjax: true
---
<!--- 
TO DO's 
Add links to refs/bib
Cross-ref figs, tables, sections 
--->
### 2.1 Notation 

Let \\( \mathbf{x}\_t \in \mathbb{R}^{D} \\) denote the observation at time \\( t \\). We use a superscript \\( (\tau) \\) for the <i>compression coordinate</i>, where \\( \tau=1 \\) is data space and \\( \tau=0 \\) is the compressed space. Therefore, \\( \mathbf{x}\_{t}^{(\tau)} \\) denotes the state at compression level \\( \tau\in[0,1] \\) and time \\( t \\), so that \\( \mathbf{x}\_{t}^{(1)}=\mathbf{x}\_t \\). In the following, we assume that data are sampled at times \\( t = k\Delta t \\), \\( k\in \mathbb{Z} \\), though our approach can accommodate non-uniform spacing. Our goal is to learn a pair of flow fields: a compressive flow \\( \mathbf{u}\_{\boldsymbol{\phi}} \\) that transports states along \\( \tau \\) from a low-dimensional generative subspace to high-dimensional data space, and a dynamical flow \\( \mathbf{v}_{\boldsymbol{\theta}} \\) that transports states through time within each \\( \tau \\).

### 2.2 Encoder 

For training a flow-matching model, we require a mapping, called the <i>coupling</i>, for pairing points in the target (data) distribution \\( p\_{\tau=1}(\mathbf{x}) \\) with those in the source (latent) distribution \\( p\_{\tau = 0}(\mathbf{x}) \\) \cite{lipman_etal_2024_tutorial}. Here, rather than fixing the form of the source distribution, we instead choose a deterministic coupling that enforces dimension reduction:


$$ 
\begin{equation}
\mathbf{x}_t^{(0)}=\mathbf{b}+\mathbf{LD}^{1/2}\cdot \boldsymbol{\mu}_{\boldsymbol{\psi}}\left(\mathbf{x}_t^{(1)}\right),
\label{eqn:encoder}
\end{equation} \tag{1}
$$

where \\( \mathbf{b}\in\mathbb{R}^{D} \\) is a bias term,
\\( \mathbf{L}\in\mathbb{R}^{D\times D} \\) has orthonormal columns (e.g., parameterized via non-pivoted QR),
\\( \mathbf{D}=\mathrm{diag}(d_1,\ldots,d\_{D}) \\) with \\( d_i>0 \\),
and \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x})\in\mathbb{R}^{D} \\) is a parameterized nonlinear mapping. 


This parameterization separates source distribution orientation and spread: \\( \mathbf{L} \\) defines the axes of the linear subspace in ambient space, and \\( \mathbf{D} \\) controls per-coordinate scale, since sample energy \\( \|\mathbf{x}\_t^{(0)}-\mathbf{b}\|\_2^2=\boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}\_t^{(1)})^\top \mathbf{D}\,\boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}\_t^{(1)}) \\) when \\( \mathbf{L} \\) has orthonormal columns. 
Moreover, when \\( \mathbf{D} \\) is low-rank, the source points \\( \mathbf{x}\_t^{(0)} \\) lie in a low-dimensional subspace. Our goal is to learn an encoding map that effectively minimizes the rank of \\( \mathbf{D} \\) while maintaining the predictive accuracy of both compressive and dynamical flows.  

In practice, to stabilize training and remove the scale ambiguity between \\( \mathbf{D} \\) and \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\), we normalize the encoder features dimension-wise \cite{ioffe2015batchnorm,ba2016layernorm}. That is, if  \\( \boldsymbol{\mu}^{\mathrm{raw}}\_{\boldsymbol{\psi}}(\mathbf{x})\in\mathbb{R}^{D} \\) denotes the raw feature output, we set

$$
\begin{equation}
\big[\boldsymbol{\mu}_{\boldsymbol{\psi}}(\mathbf{x})\big]_i = \frac{\big[\boldsymbol{\mu}^{\mathrm{raw}}_{\boldsymbol{\psi}}(\mathbf{x})\big]_i - m_i}{\sigma_i}, 
\qquad i=1,\ldots,D, 
\label{eqn:mu_norm}
\end{equation} \tag{2}
$$

where \\( \mathbf{m}\in\mathbb{R}^{D} \\) and \\( \boldsymbol{\sigma}\in\mathbb{R}^{D} \\) are the running (EMA) estimates of the coordinate-wise mean and standard deviation of the \\( \boldsymbol{\mu}^{\mathrm{raw}}\_{\boldsymbol{\psi}}(\mathbf{x}) \\), respectively. 

Thus each coordinate of \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\) has approximately zero mean and unit variance over the data distribution.
We additionally cap the per-sample \\( \ell_2 \\) norm of the raw features to prevent rare large activations from destabilizing the running statistics and downstream flow fitting \cite{pascanu2013difficulty}.

Lastly, and critically for latent space reproducibility, the combination of our parameterization Equation \\( \eqref{eqn:encoder} \\), the deterministic non-pivoted QR parameterization for \\( \mathbf{L} \\), the normalization Equation \\( \eqref{eqn:mu_norm} \\), and nested dropout (Section 3.1), which enforces an ordered, prefix-based notion of coordinate importance, renders the encoder identifiable up to a sign for each component.

### 2.3 Compressive Flow

At any fixed time \\( t \\), the compression/generation axis connects a compressed representation to a point in the data. Flow matching learns a marginal probability path by integrating over conditional probability paths (bridges) that connect endpoints drawn from the coupling. Given a data point \\( \mathbf{x}\_t^{(1)} \\) and its image under the encoder Equation \\( \eqref{eqn:encoder} \\), \\( \mathbf{x}\_t^{(0)} \\), we define the linear \\( \tau \\)-bridge as

$$
\begin{equation}
 \mathbf{x}_t^{(\tau)} = (1-\tau)\,\mathbf{x}_t^{(0)} + \tau\,\mathbf{x}_t^{(1)},
 \label{eqn:tau_bridge}
\end{equation} \tag{3}
$$

and learn a compression velocity field

$$
\begin{equation*}
\mathbf{u}_{\boldsymbol{\phi}}:\ (\mathbf{x}_t^{(\tau)},\tau)\ \mapsto\ \partial_\tau \mathbf{x} \in \mathbb{R}^{D}.
\end{equation*}
$$

Note that while the bridge Equation \\( \eqref{eqn:tau_bridge} \\) is constructed using the encoder endpoint \\( \mathbf{x}\_t^{(0)} \\), the learned flow \\( \mathbf{u}\_{\boldsymbol{\phi}} \\) defines a <i>distinct</i> projection to \\( \tau=0 \\) via integration from \\( \mathbf{x}\_t^{(1)} \\). We denote the data at compressive level \\( \tau \\), obtaining via compressive flow integration, by \\( \mathbf{\tilde{x}}\_t^{(\tau)} \\).

The flow-based endpoint (\\( \mathbf{\tilde{x}}\_t^{(0)} \\)) can differ from the encoder endpoint (\\( \mathbf{x}\_t^{(0)} \\)), effectively rearranging the encoder geometry at \\( \tau=0 \\).
In practice, flow matching tends to produce near-straight \\( \tau \\) paths and local neighborhoods in data space are transported smoothly, while the marginal distribution at \\( \tau=0 \\) is preserved by the transport \cite{Lipman_Chen_Ben-Hamu_Nickel_Le_2023,Liu_Gong_Liu_2023}.

### 2.4 Dynamical Flow 

Unlike the compressive flow bridge Equation \\( \eqref{eqn:tau_bridge} \\), the dynamical bridge requires a coupling between <i>pairs</i> of points in data space and their source representations. Additionally, because we want the dynamical flow \\( \mathbf{v}\_{\boldsymbol{\theta}} \\) to exist at every compression level \\( \tau \\), the corresponding linear bridge must involve a <i>double interpolation</i> (<strong>Figure 2</strong>). More specifically, given a fixed compression level \\( \tau\in[0,1] \\), we first interpolate along the compression dimension for each data point as in Equation \\( \eqref{eqn:tau_bridge} \\), yielding start and end points \\( \mathbf{x}^{(\tau)}\_{k\Delta t} \\)  and \\( \mathbf{x}^{(\tau)}\_{(k+1)\Delta t} \\). We then perform a second, dynamical interpolation between these points for \\( s \in [0, 1] \\):

$$
\begin{equation}
    \mathbf{x}_{t}^{(\tau)} = (1-s)\,\mathbf{x}_{k\Delta t}^{(\tau)} + s\,\mathbf{x}_{(k+1)\Delta t}^{(\tau)}.
    \label{eqn:t_bridge}\tag{4}
\end{equation}
$$

The dynamical flow \\( \mathbf{v}\_{\boldsymbol{\theta}}\\) then models the instantaneous dynamical velocity,

$$
\begin{equation*}
    \mathbf{v}_{\boldsymbol{\theta}}:\ (\mathbf{x}_{t}^{(\tau)},\tau,s,\mathbf{x}_{\mathrm{hist}}^{(\tau)}) \ \mapsto\ \partial_t \mathbf{x} \in \mathbb{R}^{D}.
\end{equation*}
$$

Here, in addition to the bridge variables \\( \tau \\) and \\( s \\), we have allowed a potential dependence on the lag-\\( h \\) history at compression level \\( \tau \\),
\\( \mathbf{x}\_{\mathrm{hist}}^{(\tau)}=\Big(\mathbf{x}\_{(k-h)\Delta t}^{(\tau)},\ldots,\mathbf{x}\_{(k-1)\Delta t}^{(\tau)},\mathbf{x}_{k\Delta t}^{(\tau)}\Big) \\) \cite{zhang2024trajectory}.

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/interpolation.pdf" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 2: Interpolation scheme for the conditional dynamic flow Equation \( \eqref{eqn:t_bridge} \) with \( k=0 \). </strong> For \( \tau, t \in [0, 1]\), intermediate points are first interpolated in the compressive (\( \tau \)) dimension for each data sample using Equation \( \eqref{eqn:tau_bridge} \) (green lines), then along the dynamical (\( t \)) dimension (blue line). The encoder \( \boldsymbol{\mu}_{\boldsymbol{\psi}} \) (red line) maps points in the data space (\( \tau=1 \)) to points in source space (\( \tau=0 \)).</font></figcaption> 
</figure>
</div>


#### Go to next section [Training]({% link training.md %})
