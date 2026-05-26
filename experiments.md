---
layout: page
title: Experiments
subtitle: 
mathjax: true
---

<!---
TO DO's

- Add musal animations (instead of extra static figures)
- Link additional balls experiment figures 

--->

Across experiments, we analyze representations at compression levels \\( \tau\in \{0,1\} \\).
At generation time, we integrate both learned flows (dynamic and compressive) as neural ODEs using an adaptive Dormand--Prince (DOPRI5) solver.

We obtain the flowed data representation \\( \{\tilde{\mathbf{x}}^{(\tau)}\_t\} \\) by two methods: <i>flowed</i>, where each observed frame \\( \mathbf{x}^{(1)}\_t \\) is compressed pointwise to level \\( \tau \\) by integrating the compressive flow \\( \mathbf{u}\_{\phi} \\) from \\( \tau=1 \\); and <i>simulated</i>, where we first compress an initial frame to \\( \tilde{\mathbf{x}}^{(\tau)}\_0 \\) and then integrate the dynamical flow \\( \mathbf{v}\_{\theta} \\) forward in time at the same \\( \tau \\).

At \\( \tau=0 \\), we map compressed states to encoder-normalized latent coordinates via \\( \boldsymbol{\tilde{\mu}}\_t=(\mathbf{L}\mathbf{D}^{1/2})^{-1}\left(\mathbf{\tilde{x}}^{(0)}\_t-\mathbf{b}\right) \\) and visualize the first three coordinates, which are the most important under nested-dropout ordering. We also project velocities by using \\( \boldsymbol{\dot{\tilde{\mu}}}\_t=(\mathbf{L}\mathbf{D}^{1/2})^{-1}\mathbf{\dot{\tilde{x}}}^{(0)}\_t \\), and plotting dynamical velocities at the start of each step (\\( s=0 \\) in Equation 4, Section 2.4). 

Experiment train times and parameter choices as well as trajectory roll-out time estimates can be found in the Appendix (Sections B,C respectively) of our manuscript. Links to manuscript, repo, and bird (audio) data are listed under "Resources".

### 4.1 Simulated Data

To test the ability of DCF to extract low-dimensional dynamics from high-dimensional data, we simulate \\( 10 \\) short videos of a ball moving counterclockwise (<strong>Figure 3</strong>). Each video comprises \\( 50 \\) time steps, with each frame being a \\( 28\times 28 \\) grayscale image.
Both the compressive flow \\( \mathbf{u}\_{\phi} \\) and the dynamical flow \\( \mathbf{v}\_{\theta} \\) are parameterized by 4-level convolutional encoder-decoders (U-Net style) with channel widths \\( \{32,64,128,256\} \\) and a 256-dimensional bottleneck embedding.
The encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\) is a convolutional VAE with the same multiscale channel schedule.
We use no dynamical history. That is, \\( h=0 \\) for \\( \mathbf{x}\_{\mathrm{hist}}^{(\tau)} \\) in Section 2.4.

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/ball_main_website-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 3: Rotating-ball simulation.</strong> <strong>Left:</strong> We simulated 10 videos of a ball moving counterclockwise (50 frames, \( 28\times 28 \)) and trained DCF with nested dropout (\( p=\frac{1}{50} \)) and no history (\( h=0 \)).
    (<strong>A</strong>) Ground-truth frames (top) and rollouts decoded in data space (middle, \( \tau=1 \)), with the corresponding simulated states in compressed space (bottom, \( \tau=0 \)), shown at \( t\in\{0,5,10,15,20\} \).
    (<strong>B</strong>) Simulated trajectories visualized in the first three coordinates of the latent space. Colored points correspond to frames in (<strong>A</strong>).</font></figcaption> 
</figure>
</div>

We set nested dropout to \\( K\sim\mathrm{Geom}(p) \\) with \\( p=1/50 \\), so that \\( K\_{\mathrm{target}}=\mathbb{E}(K)=50 \\) can provide a generous latent budget.
With no additional penalty on \\( \mathbf{D} \\), the fitted scales \\( \{d\_i\} \\) decay rapidly (first column of <strong>Figure S1</strong>, Appendix A of manuscript).

We define the <i>effective dimension</i> as the smallest \\( K \\) such that \\( \sum\_{i=1}^{K} d\_i \geq 0.95\sum\_{i=1}^{D} d\_i \\), which yields \\( K\_{\mathrm{eff}}=30 \\) in this run.
As expected (<strong>Figure 3A</strong>), simulated frames in data space (\\( \tau=1 \\)) track the ground-truth ball location over time, while the corresponding rollouts in compressed space ( \\( \tau=0 \\), embedded in image space) provide a nearly identical image.

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/app_ball_dim3-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 4: Soft 3D latent representation via nested dropout for ball simulation. </strong> <strong>Left:</strong> We repeat the rotating-ball experiment with no history (\( h=0 \)) and a tighter nested-dropout budget \( p=\frac{1}{3} \) so \( K_{\mathrm{target}}=\mathbb{E}(K)=3 \)).
    (<strong>A</strong>) Absolute per-pixel deviation between the original frame and its compression to \( \tau=0 \), comparing the encoder endpoint versus the learned compressive flow (rMAE: root mean absolute error per pixel).
    (<strong>B</strong>) Projected dynamical velocities from <i>simulated</i> trajectories in the first three latent coordinates.
    (<strong>C</strong>) Simulated trajectories in the same 3D latent space, with five representative time points from trial 1 highlighted.
    (<strong>D</strong>) Corresponding ground-truth frames (top), rollouts in data space (middle, \( \tau=1 \)), and rollouts in compressed space (bottom, \( \tau=0 \)).</font></figcaption> 
</figure>
</div>

Moreover, the simulated latent trajectory in the first three coordinates of \\( \boldsymbol{\tilde{\mu}} \\) forms a smooth closed loop (<strong>Figure 3B</strong>), consistent with the underlying periodic motion. In further experiments imposing additional shrinkage on \\( \mathbf{D} \\) (<strong>Figure 4</strong>; <strong>Figure S1</strong>, Appendix A of manuscript), we found that inferred dynamics and latent spaces were <i>reproducible</i> across runs. This is expected given that our model produces <i>identifiable</i> representations by construction, with additional cross-run stability checks in <strong>Figure 8</strong>.

For these and subsequent experiments (cf. Sections 4.2-4.4) we compare DCF against several other competing approaches, namely VAEs ([Kingma & Welling, 2014](https://pure.uva.nl/ws/files/2511146/162970_1312.6114v10.pd.pdf); [Rezende et al., 2014](https://proceedings.mlr.press/v32/rezende14.html)), MARBLE ([Gosztolai et al., 2025](https://www.nature.com/articles/s41592-024-02582-2)), LFADS ([Pandarinath et al., 2018](https://www.nature.com/articles/s41592-018-0109-9)), DMD ([Kutz et al., 2016](https://epubs.siam.org/doi/book/10.1137/1.9781611974508)), T-PHATE ([Busch et al., 2023](https://www.nature.com/articles/s43588-023-00419-0)), and CEBRA ([Schneider et al., 2023](https://www.nature.com/articles/s41586-023-06031-6)). For larger datasets, we could only train T-PHATE, which needs to load everything into memory, on a subset of data. Results for these models on the balls dataset are shown in <strong>Figure 5</strong>. Surprisingly, several of these models struggled to identify a simple, low-dimensional manifold underlying the data.

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/ball_comparisons.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 5: 3D latent representations of comparison models on the rotating ball dataset. </strong> Nearly all comparison models fail to capture the cyclical latent structure of the ball dataset; those that do display more variable latent trajectories than DCF.</font></figcaption> 
</figure>
</div>


### 4.2 Neural Data

We evaluated DCF on a dataset comprising population neural activity from a center-out reach task performed by nonhuman primates (592 training trials, 197 held-out test trials; [Pei et al., 2021](https://arxiv.org/abs/2109.04463)).
Each trial's data consisted of smoothed spike counts from 137 neurons over 100 time steps.
All methods in <strong>Table 1</strong> used the same train/test split and neural time window, with cursor velocity used only afterward for downstream linear decoding.

<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Method</center></b></td>
    <td style='width:17%'><b><center>Median \( R^2 \) (25<sup>th</sup> percentile, 75<sup>th</sup> percentile)</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center"><strong>DCF (ours)</strong></td>
     <td style="text-align: center"> <strong>0.304 (0.020, 0.505)</strong></td>
  </tr>
  <tr>
    <td style="text-align: center">VAE*</td>
     <td style="text-align: center">0.242 (0.041, 0.427)</td>
  </tr>
  <tr>
    <td style="text-align: center">MARBLE*</td>
     <td style="text-align: center">0.195 (-0.083, 0.336)</td>
  </tr> 
  <tr>
    <td style="text-align: center">LFADS</td>
     <td style="text-align: center">0.311 (0.093, 0.416)</td>
  </tr>
  <tr>
    <td style="text-align: center">DMD*</td>
     <td style="text-align: center">0.014 (-0.157, 0.128)</td>
  </tr> 
  <tr>
    <td style="text-align: center">T_PHATE*</td>
     <td style="text-align: center">-0.012 (-0.152, 0.010)</td>
  </tr>
  <tr>
    <td style="text-align: center">CEBRA*</td>
     <td style="text-align: center">0.202 (0.010, 0.294)</td>
  </tr>
</table>
<caption><strong>Table 1: Model comparisons for neural data (cursor velocity prediction). </strong> Decoding (linear predictions) of cursor velocity from 3D latent representations of neural activity. Reported values are \( R^2 \) quartiles across held-out trials. Asterisks (*) indicate models performing significantly worse than ours (p-value \( <0.05 \), one-sided Wilcoxon signed-rank test with Bonferroni correction for multiple comparisons) </caption>

For this experiment, the encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\), the compressive flow \\( \mathbf{u}\_{\boldsymbol{\phi}} \\), and the dynamical flow \\( \mathbf{v}\_{\boldsymbol{\theta}} \\) were all parameterized by multilayer perceptrons (MLPs) with depth 4 and width 256.
We use a lag-10 dynamical history (\\( h=10 \\)) and nested dropout \\( p=1/50 \\) for all reported results. 

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/maze_comparison_appendix-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 6: 3D latent representations of neural data. </strong> <strong>Left:</strong> Flowed mean trajectories (compressing data pointwise via compressive flow \( \mathbf{u}_{\boldsymbol{\phi}} \)) in the first three latent coordinates. Color indicates monkey reach direction; lines are averaged across reach direction. <strong>Right:</strong> 3D latent trajectories of comparison models. The latent representation of CEBRA (without supervision), LFADS, VAE, MARBLE and DMD are averaged across monkey reach location. T-PHATE latent representation is unstructued and therefore not averaged. </font></figcaption> 
</figure>
</div>

As expected, most latent models, including DCF, learned a well-organized latent space in which neural dynamics corresponding to distinct reach targets organized topographically (<strong>Figure 6</strong>). Quantification on a velocity prediction task using these latent spaces (<strong>Table 1</strong>) shows that DCF outperforms other models, despite falling well short of larger, prediction-focused approaches (e.g.,[Azabou et al., 2023](https://arxiv.org/abs/2310.16046)). Additionally, our model achieves higher reconstruction quality of neural activity (i.e., firing rates) on held-out data (<strong>Table 2</strong>). Consistent with latent identifiability up to sign, DCF also recovers stable target-organized 3D latent geometry across five random seeds, whereas LFADS and CEBRA show lower seed-to-seed consistency (<strong>Figure 8</strong>).

<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Method</center></b></td>
    <td style='width:17%'><b><center>Median \( R^2 \) (25<sup>th</sup> percentile, 75<sup>th</sup> percentile)</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center"><strong>DCF (ours)</strong></td>
     <td style="text-align: center"> <strong>0.999 (0.998, 0.999)</strong></td>
  </tr>
  <tr>
    <td style="text-align: center">VAE</td>
     <td style="text-align: center">0.618 (0.559, 0.678)</td>
  </tr>
  <tr>
    <td style="text-align: center">LFADS</td>
     <td style="text-align: center">0.595 (0.527, 0.661)</td>
  </tr>
</table>
<caption><strong>Table 2: Model comparisons for neural data (reconstruction). </strong> Reconstruction of neural activity (firing rates) on held out test-data. Reported values are \( R^2 \) quartiles across held-out trials.</caption>

### 4.3 Video Data 

We next applied DCF to a well-studied behavioral video dataset ([Musall et al., 2019](https://www.nature.com/articles/s41593-019-0502-4)), which consists of a single long video with \\( 71{,}942 \\) frames of size \\( 64\times 64 \\). These are challenging data for most dimension reduction methods, since most parts of the frame are highly static, with only intermittent bursts of activity.
Here, since we focus on inference, we did not use a train/test split and do not report long-horizon trajectory rollouts.
Both flows were parameterized by 4-level convolutional encoder-decoders (U-Net style) with channel widths \\( \{32,64,128,256\} \\) and a 256-dimensional bottleneck embedding.
We fit the model with nested dropout \\( p=1/50 \\) and no history (\\( h=0 \\)), yielding an effective dimension \\( K\_{\mathrm{eff}}=30 \\).
Since the behavior is highly repetitive, we visualized only the first 1,438 frames (about \\( 2\% \\) of the video), which is sufficient to capture the latent structure.

The latent structure forms four prominent bands in 3D latent space (<strong>Figure 7A</strong>), separated primarily along \\( (\mu\_1,\mu\_2) \\) while sharing a common within-band axis \\( \mu\_3 \\).

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/musal_main_comparison-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 7: Latent structure in behavioral video.  </strong> <strong>Left:</strong>  (<strong>A</strong>) Flowed latent structure forms four prominent bands (colored points), with outliers marked by maximal latent distance (brown \( \times \)) and maximal velocity magnitude (cyan \( \times \)).
    (<strong>B</strong>) Representative snapshots along each band (top/mid/low in \( \mu_3 \)). Lowering \( \mu_3 \) corresponds to stronger mouth movement, paw lift, or both. 
    (<strong>C</strong>) Outlier frames selected by maximal latent distance (left) or maximal velocity magnitude (right), correspond to transient paw and controller movements.
    (<strong>D</strong>) 3D Latent representations from comparison models. Most either collapse the four bands or fail to capture outliers. See <strong>our supplemental videos </strong> for additional details on structure identified by our model and comparisons against competing approaches.</font></figcaption> 
</figure>
</div>

Within each band, lower values of \\( \mu\_3 \\) correspond to stronger mouth movement, while higher \\( \mu\_3 \\) is more quiescent (<strong>Figure 7A,B</strong>).
Across bands, the mean appearance is broadly similar, but each band captures a slightly different baseline visual state, reflected by systematic mean shifts and variability patterns (<strong>Figure 7B</strong>).
Moreover, outliers selected by maximal latent distance or maximal velocity magnitude (<strong>Figure 7C</strong>) correspond to transient paw and controller movements. Importantly, this latent structure was not well captured by comparison models (<strong>Figure 7D</strong>), and the same four-band structure was recovered across five random seeds (<strong>Figure 8</strong>).

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/cross_run-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 8: Cross-run stability of learned latent representations.  </strong> (<strong>A</strong>) Musall mouse-video experiment. Top: sign-aligned 3D DCF latent representations from five random seeds. Bottom: pairwise mean cosine similarity between seeds.
  (<strong>B</strong>) Monkey center-out reach experiment. Top: sign-aligned, target-averaged 3D DCF latent trajectories across five random seeds. Bottom: pairwise mean cosine similarity between seeds.
  (<strong>C</strong>) Pairwise mean cosine similarity for LFADS and CEBRA on the monkey reach experiment. All heatmaps use the same color scale.</font></figcaption> 
</figure>
</div>


### 4.4 Audio Data

Lastly, we evaluated DCF on a birdsong dataset (262 training trials, 66 test trials) converted to 26 \\( 64\times 64 \\) sequential spectrograms.
We used the same convolutional architectures as in the video experiment, and the encoder feature map \\( \boldsymbol{\mu}\_{\boldsymbol{\psi}}(\mathbf{x}) \\) was a convolutional VAE with the same multiscale schedule. We fit the model with nested dropout \\( p=1/50 \\) and no history (\\( h=0 \\)), evaluating simulated rollouts.

On training trials, DCF rollouts preserved the syllable-level structure of the motif and reproduced the major time-frequency energy patterns across successive syllables (<strong>Figure 9</strong>).

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/bird_main_website-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 9: DCF captures birdsong latent dynamics. </strong> (<strong>A</strong>) Example motif with syllables A--D, shown as ground-truth spectrogram frames (top), <i>simulated</i> rollouts decoded in data space (middle, \( \tau=1 \)), and compressed space (bottom, \( \tau=0 \)).
    (<strong>B</strong>) Projected latent velocity field from simulated rollouts in the first three latent coordinates, with arrows normalized to unit length and color indicating velocity magnitude.
    (<strong>C</strong>) Simulated latent trajectories in the same 3D space, with one representative rollout highlighted and colored by time.</font></figcaption> 
</figure>
</div>

In the learned 3D latent coordinates, simulated trajectories concentrate on a low-dimensional curved manifold, and transitions between syllables align with regions of larger latent velocity magnitude (<strong>Figure 9</strong>). 

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/bird_app-scaled.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 10: Birdsong rollouts on the test set. </strong> Left: simulated latent trajectories in the first three coordinates of \( \tilde{\boldsymbol{\mu}} \) for held-out trials.
    Right: ground-truth spectrogram frames (top) and corresponding simulated rollouts decoded in data space (middle, \( \tau=1 \)) and compressed space (bottom, \( \tau=0 \)), shown at matched time points.</font></figcaption> 
</figure>
</div>

On held-out trials, the same latent geometry is retained and the decoded rollouts remain qualitatively consistent with the ground-truth spectrograms, indicating that the learned dynamics generalize beyond the training set (<strong>Figure 10</strong>). This is in contrast to typical VAE-based approaches ([Goffinet et al., 2021](https://elifesciences.org/articles/67855); [Sainburg et al., 2020](https://pubmed.ncbi.nlm.nih.gov/33057332/)), which preserve structure at the syllable level while failing to smoothly capture dynamics (<strong>Figure 11</strong>).

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/bird_app_vae_v3.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+0.5"><strong>Figure 11: Comparison embeddings using a 3d VAE. </strong> Left: simulated trajectories from a DCF model (cp. <strong>Figure 9C</strong>). Middle: embeddings of a VAE trained using 100 ms long spectrogram windows. Right: embeddings of a VAE trained using 20ms long spectrogram windows. In general, VAEs with short data windows produce latent spaces with disorganized temporal structure, while longer data windows exhibit more obvious structure but more variability than DCF.</font></figcaption> 
</figure>
</div>


#### Go to next section [Conclusion]({% link conclusion.md %})




