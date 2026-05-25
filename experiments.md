---
layout: page
title: Experiments
subtitle: 
mathjax: true
---

<!---
TO DO's
Add Figures
Add animations we have 
Add Tables 
--->

Across experiments, we analyze representations at compression levels \\( \tau\in \{0,1\} \\).
At generation time, we integrate both learned flows (dynamic and compressive) as neural ODEs using an adaptive Dormand--Prince (DOPRI5) solver.

We obtain the flowed data representation \\( \{\tilde{\mathbf{x}}^{(\tau)}\_t\} \\) by two methods: <i>flowed</i>, where each observed frame \\( \mathbf{x}^{(1)}\_t \\) is compressed pointwise to level \\( \tau \\) by integrating the compressive flow \\( \mathbf{u}\_{\phi} \\) from \\( \tau=1 \\); and <i>simulated</i>, where we first compress an initial frame to \\( \tilde{\mathbf{x}}^{(\tau)}\_0 \\) and then integrate the dynamical flow \\( \mathbf{v}\_{\theta} \\) forward in time at the same \\( \tau \\).

At \\( \tau=0 \\), we map compressed states to encoder-normalized latent coordinates via \\( \boldsymbol{\tilde{\mu}}\_t=(\mathbf{L}\mathbf{D}^{1/2})^{-1}\left(\mathbf{\tilde{x}}^{(0)}\_t-\mathbf{b}\right) \\) and visualize the first three coordinates, which are the most important under nested-dropout ordering. We also project velocities by using \\( \boldsymbol{\dot{\tilde{\mu}}}\_t=(\mathbf{L}\mathbf{D}^{1/2})^{-1}\mathbf{\dot{\tilde{x}}}^{(0)}\_t \\), and plotting dynamical velocities at the start of each step (\\( s=0 \\) in Equation 4, Section 2.4). 

Experiment train times and parameter choices as well as trajectory roll-out time estimates can be found in the Appendix (Sections B,C respectively) of our manuscript. Links to manuscript, repo, and bird data used in some of our audio experiments are listed under "Resources".

### 4.1 Simulated Data

To test the ability of DCF to extract low-dimensional dynamics from high-dimensional data, we simulate \\( 10 \\) short videos of a ball moving counterclockwise (<strong>Figure 3</strong>). Each video comprises \\( 50 \\) time steps, with each frame being a \\( 28\times 28 \\) grayscale image.
Both the compressive flow \\( \mathbf{u}\_{\phi} \\) and the dynamical flow \\( \mathbf{v}\_{\theta} \\) are parameterized by 4-level convolutional encoder-decoders (U-Net style) with channel widths \\( \{32,64,128,256\} \\) and a 256-dimensional bottleneck embedding.
The encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\) is a convolutional VAE with the same multiscale channel schedule.
We use no dynamical history. That is, \\( h=0 \\) for \\( \mathbf{x}\_{\mathrm{hist}}^{(\tau)} \\) in Section 2.4.

We set nested dropout to \\( K\sim\mathrm{Geom}(p) \\) with \\( p=1/50 \\), so that \\( K\_{\mathrm{target}}=\mathbb{E}(K)=50 \\) can provide a generous latent budget.
With no additional penalty on \\( \mathbf{D} \\), the fitted scales \\( \{d\_i\} \\) decay rapidly (first column of <strong>Figure 4A</strong>).

We define the <i>effective dimension</i> as the smallest \\( K \\) such that \\( \sum\_{i=1}^{K} d\_i \geq 0.95\sum\_{i=1}^{D} d\_i \\), which yields \\( K\_{\mathrm{eff}}=30 \\) in this run.
As expected (<strong>Figure 3A</strong>), simulated frames in data space (\\( \tau=1 \\)) track the ground-truth ball location over time, while the corresponding rollouts in compressed space ( \\( \tau=0 \\), embedded in image space) provide a nearly identical image.

Moreover, the simulated latent trajectory in the first three coordinates of \\( \boldsymbol{\tilde{\mu}} \\) forms a smooth closed loop (<strong>Figure 3B</strong>), consistent with the underlying periodic motion. In further experiments imposing additional shrinkage on \\( \mathbf{D} \\) (<strong> Figures 4, 5 </strong>), we found that inferred dynamics and latent spaces were <i>reproducible</i> across runs. This is expected given that our model produces <i>identifiable</i> representations by construction, with additional cross-run stability checks in <strong>Figure XX</strong>.

For these and subsequent experiments (cf. Sections 4.2-4.4) we compare DCF against several other competing approaches, namely VAEs \cite{Kingma_Welling_2014, Rezende_Mohamed_Wierstra_2014}, MARBLE \cite{Gosztolai_Peach_Arnaudon_Barahona_Vandergheynst_2025}, LFADS \cite{pandarinath2018inferring}, DMD \cite{kutz2016dynamic}, T-PHATE \cite{busch2023multi}, and CEBRA \cite{schneider2023learnable}. For larger datasets, we could only train T-PHATE, which needs to load everything into memory, on a subset of data. Results for these models on the balls dataset are shown in <strong>Figure XX</strong>. Surprisingly, several of these models struggled to identify a simple, low-dimensional manifold underlying the data.


### 4.2 Neural Data

We evaluated DCF on a dataset comprising population neural activity from a center-out reach task performed by nonhuman primates \citep[592 training trials, 197 held-out test trials;][]{PeiYe2021NeuralLatents}.
Each trial's data consisted of smoothed spike counts from 137 neurons over 100 time steps.
All methods in <strong>Table XX</strong> used the same train/test split and neural time window, with cursor velocity used only afterward for downstream linear decoding.
For this experiment, the encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\), the compressive flow \\( \mathbf{u}\_{\boldsymbol{\phi}} \\), and the dynamical flow \\( \mathbf{v}\_{\boldsymbol{\theta}} \\) were all parameterized by multilayer perceptrons (MLPs) with depth 4 and width 256.
We use a lag-10 dynamical history (\\( h=10 \\)) and nested dropout \\( p=1/50 \\) for all reported results. 

As expected, most latent models, including DCF, learned a well-organized latent space in which neural dynamics corresponding to distinct reach targets organized topographically (<strong>Figure XX</strong>). Quantification on a velocity prediction task using these latent spaces (<strong>Table XX</strong>) shows that DCF outperforms other models, despite falling well short of larger, prediction-focused approaches \citep[e.g.,][]{azabou2023unified}. Additionally, our model achieves higher reconstruction quality of neural activity (i.e., firing rates) on held-out data (<strong>Table XX</strong>). Consistent with latent identifiability up to sign, DCF also recovers stable target-organized 3D latent geometry across five random seeds, whereas LFADS and CEBRA show lower seed-to-seed consistency (<strong>Figure XX</strong>).

### 4.3 Video Data 

We next applied DCF to a well-studied behavioral video dataset \cite{musall2019single}, which consists of a single long video with \\( 71{,}942 \\) frames of size \\( 64\times 64 \\). These are challenging data for most dimension reduction methods, since most parts of the frame are highly static, with only intermittent bursts of activity.
Here, since we focus on inference, we did not use a train/test split and do not report long-horizon trajectory rollouts.
Both flows were parameterized by 4-level convolutional encoder-decoders (U-Net style) with channel widths \\( \{32,64,128,256\} \\) and a 256-dimensional bottleneck embedding.
We fit the model with nested dropout \\( p=1/50 \\) and no history (\\( h=0 \\)), yielding an effective dimension \\( K\_{\mathrm{eff}}=30 \\).
Since the behavior is highly repetitive, we visualized only the first 1,438 frames (about \\( 2\% \\) of the video), which is sufficient to capture the latent structure.

The latent structure forms four prominent bands in 3D latent space (<strong>Figure XX</strong>), separated primarily along \\( (\mu\_1,\mu\_2) \\) while sharing a common within-band axis \\( \mu\_3 \\).
Within each band, lower values of \\( \mu\_3 \\) correspond to stronger mouth movement, while higher \\( \mu\_3 \\) is more quiescent (<strong>Figure XX</strong>).
Across bands, the mean appearance is broadly similar, but each band captures a slightly different baseline visual state, reflected by systematic mean shifts and variability patterns (<strong>Figure XX </strong>).
Moreover, outliers selected by maximal latent distance or maximal velocity magnitude (<strong>Figure XX </strong>) correspond to transient paw and controller movements. Importantly, this latent structure was not well captured by comparison models (<strong>Figures XX, XX </strong>), and the same four-band structure was recovered across five random seeds (<strong>Figure XX </strong>).

### 4.4 Audio Data

Lastly, we evaluated DCF on a birdsong dataset (262 training trials, 66 test trials) converted to 26 \\( 64\times 64 \\) sequential spectrograms.
We used the same convolutional architectures as in the video experiment, and the encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\) was a convolutional VAE with the same multiscale schedule.

We fit the model with nested dropout \\( p=1/50 \\) and no history (\\( h=0 \\)), evaluating simulated rollouts.

On training trials, DCF rollouts preserved the syllable-level structure of the motif and reproduced the major time-frequency energy patterns across successive syllables (<strong>Figure XX </strong>).

In the learned 3D latent coordinates, simulated trajectories concentrate on a low-dimensional curved manifold, and transitions between syllables align with regions of larger latent velocity magnitude (<strong>Figure XX </strong>).

On held-out trials, the same latent geometry is retained and the decoded rollouts remain qualitatively consistent with the ground-truth spectrograms, indicating that the learned dynamics generalize beyond the training set (<strong>Figure XX </strong>). This is in contrast to typical VAE-based approaches \cite{goffinet2021low,sainburg2020finding}, which preserve structure at the syllable level while failing to smoothly capture dynamics (<strong>Figure XX </strong>).


#### Go to next section [Conclusion]({% link conclusion.md %})




