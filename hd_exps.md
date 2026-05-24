---
layout: page
title: Experiments on Image Datasets
subtitle: Using Inflationary Flows on Higher-Dimensional Data
mathjax: true
---

### 8.1 Experiments on Image Benchmark datasets 


For the PR-Reducing flows, the final scale ratio between preserved vs. shrunken dimensions for finite integration times is dependent on 
the quantity \\( e^{\rho(g_* -g_i)T} \\). Therefore, for fixed end integration time \\( T \\) and rate \\( \rho \\), this scaling is dictated 
by \\( g_* - g_i \\), which we call the "inflation gap" (IG, <strong>Appendix B.2</strong>). As this inflation gap increases, 
compressed dimensions are shrunken to a greater extent, and the denoising networks are required to amortize score estimation over wider 
noise scales, a harder learning problem. Therefore, for our proposed model, compression should be understood <em>both</em> in terms of the 
number of dimensions being preserved and the size of this inflation gap.


To assess how these two factors affect model performance, we performed two sets of experiments on two benchmark image 
datasets (CIFAR-10 [Krizhevsky, 2009](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf) and 
AFHQv2 [Choi et al., 2020](https://arxiv.org/abs/1912.01865); <strong>Appendix B.4.2</strong>). In the first set of experiments, 
we fixed \\( T \\), \\( \rho \\), and the inflation gap (\\( \text{IG} = 1.02 \\)) while varying only the number of preserved 
dimensions \\( d \\) between \\( d=1 \\) (compression to \\( \approx 0.03\% \\)) and \\( d=3072 \\) 
(no compression) for both datasets (see <strong>Tables 1, 2</strong> below - 
values represent mean \\( \pm 2 \sigma \\) over 3 sets of seeds, each with either 50K samples for FID
scores, or 10K samples, for round-trip MSE). 


<caption><strong><center> Table 1: FID and Round-Trip MSE for AFHQv2 at Constant Inflation Gap (IG= 1.02) </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Dimensions</center></b></td>
    <td style='width:17%'><b><center>FID</center></b></td>
    <td style='width:18%'><b><center>MSE</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center">1</td>
     <td style="text-align: center"> 12.65 <span>&#177;</span>  0.07 </td>
     <td style="text-align: center"> 1.47 <span>&#177;</span> 0.07 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">11.95<span>&#177;</span> 0.06 </td>
    <td style="text-align: center">1.55<span>&#177;</span> 0.21 </td>
  </tr>
  <tr>
    <td style="text-align: center">30</td>
    <td style="text-align: center">13.64<span>&#177;</span> 0.02 </td>
    <td style="text-align: center">3.79<span>&#177;</span> 0.13 </td>
  </tr>
  <tr>
    <td style="text-align: center">62</td>
    <td style="text-align: center">14.05<span>&#177;</span> 0.18 </td>
    <td style="text-align: center">5.32<span>&#177;</span> 0.18 </td>
  </tr>
  <tr>
    <td style="text-align: center">307</td>
    <td style="text-align: center">15.64<span>&#177;</span> 0.10 </td>
    <td style="text-align: center">3.33<span>&#177;</span> 0.13 </td>
  </tr>
  <tr>
    <td style="text-align: center">615</td>
    <td style="text-align: center">14.63<span>&#177;</span> 0.07 </td>
    <td style="text-align: center">2.42<span>&#177;</span> 0.18 </td>
  </tr>
  <tr>
    <td style="text-align: center">1536</td>
    <td style="text-align: center">13.36<span>&#177;</span> 0.12 </td>
    <td style="text-align: center">0.14<span>&#177;</span> 0.03 </td>
  </tr>
  <tr>
    <td style="text-align: center">3041</td>
    <td style="text-align: center">13.97<span>&#177;</span> 0.13 </td>
    <td style="text-align: center">0.28<span>&#177;</span> 0.06 </td>
  </tr>
  <tr>
    <td style="text-align: center">3072</td>
    <td style="text-align: center">11.90<span>&#177;</span> 0.08 </td>
    <td style="text-align: center">0.38<span>&#177;</span> 0.04 </td>
  </tr>
</table>



<p><caption><strong><center> Table 2: FID and Round-Trip MSE for CIFAR-10 at Constant Inflation Gap (IG= 1.02) </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Dimensions</center></b></td>
    <td style='width:17%'><b><center>FID</center></b></td>
    <td style='width:18%'><b><center>MSE</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center">1</td>
     <td style="text-align: center"> 20.76 <span>&#177;</span>  0.09 </td>
     <td style="text-align: center"> 1.07 <span>&#177;</span> 0.10 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">21.29<span>&#177;</span> 0.04 </td>
    <td style="text-align: center">0.81<span>&#177;</span> 0.11 </td>
  </tr>
  <tr>
    <td style="text-align: center">30</td>
    <td style="text-align: center">23.36<span>&#177;</span> 0.14 </td>
    <td style="text-align: center">2.21<span>&#177;</span> 0.08 </td>
  </tr>
  <tr>
    <td style="text-align: center">62</td>
    <td style="text-align: center">23.30<span>&#177;</span> 0.19 </td>
    <td style="text-align: center">2.27<span>&#177;</span> 0.24 </td>
  </tr>
  <tr>
    <td style="text-align: center">307</td>
    <td style="text-align: center">28.07<span>&#177;</span> 0.13 </td>
    <td style="text-align: center">0.71<span>&#177;</span> 0.02 </td>
  </tr>
  <tr>
    <td style="text-align: center">615</td>
    <td style="text-align: center">24.49<span>&#177;</span> 0.27 </td>
    <td style="text-align: center">0.29<span>&#177;</span> 0.03 </td>
  </tr>
  <tr>
    <td style="text-align: center">1536</td>
    <td style="text-align: center">17.44<span>&#177;</span> 0.16 </td>
    <td style="text-align: center">0.16<span>&#177;</span> 0.06 </td>
  </tr>
  <tr>
    <td style="text-align: center">3041</td>
    <td style="text-align: center">16.60<span>&#177;</span> 0.05 </td>
    <td style="text-align: center">0.30<span>&#177;</span> 0.02 </td>
  </tr>
  <tr>
    <td style="text-align: center">3072</td>
    <td style="text-align: center">17.01<span>&#177;</span> 0.10 </td>
    <td style="text-align: center">0.22<span>&#177;</span> 0.03 </td>
  </tr>
</table></p>


For the second set of experiments, we worked with the AFHQv2 dataset and fixed \\( T \\), \\( \rho \\), and \\( d=2 \\), 
while varying the inflation gap ( \\( \text{IG} = 1.10, 1.25, 1.35, 1.50 \\), 
see <strong>Table 3</strong> below, same set up as before). 

<caption><strong><center> Table 3: FID and Round-Trip MSE for AFHQv2 at Varying Inflation Gaps (IGs) </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Dimensions</center></b></td>
    <td style='width:14%'><b><center>IG</center></b></td>
    <td style='width:17%'><b><center>FID</center></b></td>
    <td style='width:18%'><b><center>MSE</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center"> 1.02</td>
     <td style="text-align: center"> 11.95 <span>&#177;</span>  0.06 </td>
     <td style="text-align: center"> 1.55 <span>&#177;</span> 0.21 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">1.10</td>
    <td style="text-align: center">13.98<span>&#177;</span> 0.13 </td>
    <td style="text-align: center">1.35<span>&#177;</span> 0.08 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">1.25</td>
    <td style="text-align: center">17.84<span>&#177;</span> 0.15 </td>
    <td style="text-align: center">1.65<span>&#177;</span> 0.09 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">1.35</td>
    <td style="text-align: center">34.68<span>&#177;</span> 0.37 </td>
    <td style="text-align: center">1.19<span>&#177;</span> 0.18 </td>
  </tr>
  <tr>
    <td style="text-align: center">2</td>
    <td style="text-align: center">1.50</td>
    <td style="text-align: center">107.64<span>&#177;</span> 0.43 </td>
    <td style="text-align: center">0.11<span>&#177;</span> 0.02 </td>
  </tr>
</table>


Finally, we also compared our <em>inflationary flows</em> (IFs) model generative performance on CIFAR-10 against 
three existing <em>injective flow</em> model baselines (<strong>Appendix B.5.2</strong>) --- 
M-Flows ([Brehmer & Cranmer, 2020](https://arxiv.org/pdf/2003.13913)), Rectangular Flows (RFs, [Caterini et al., 2021](https://arxiv.org/pdf/2106.01413)), 
and Canonical Manifold Flows (CMFs, [Flouris & Konukoglu, 2023](https://proceedings.neurips.cc/paper_files/paper/2023/file/572a6f16ec44f794fb3e0f8a310acbc6-Paper-Conference.pdf)) 
--- for different numbers of preserved dimensions (\\( d=30, 40, 62 \\)). 
<strong>Table 4</strong> below showcases best FID scores (out of 3 independently generated sets of images, each with 10K samples) 
for each such experiment. For these comparison experiments, we fixed \\( \text{IG}=1.02 \\) when training our networks for 
the different \\( d \\) values.


<caption><strong><center> Table 4: FID Score Comparison with Injective Flows for CIFAR-10 </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Dimensions</center></b></td>
    <td style='width:14%'><b><center>IFs (IG=1.02)</center></b></td>
    <td style='width:17%'><b><center>M-Flow</center></b></td>
    <td style='width:18%'><b><center>RFs</center></b></td>
    <td style='width:18%'><b><center>CMFs</center></b></td>
  </tr>
  <tr>
    <td style="text-align: center">30</td>
    <td style="text-align: center">23.3 </td>
     <td style="text-align: center"> 541.2 </td>
     <td style="text-align: center"> 544.0 </td>
     <td style="text-align: center"> 532.6 </td>
  </tr>
  <tr>
    <td style="text-align: center">40</td>
    <td style="text-align: center">24.3</td>
    <td style="text-align: center">535.7</td>
    <td style="text-align: center">481.3</td>
    <td style="text-align: center">444.6</td>
  </tr>
  <tr>
    <td style="text-align: center">62</td>
    <td style="text-align: center">23.2</td>
    <td style="text-align: center">280.9</td>
    <td style="text-align: center">280.8</td>
    <td style="text-align: center">287.9</td>
  </tr>
</table>


As a general trend, increasing the number of preserved dimensions at a constant inflation gap led
to improvements in generative quality (lower FID scores) and reduced MSE (<strong>Tables 1,2</strong>). However,
some schedules we assessed are not entirely consistent with this trend. We hypothesize this is at
least partially due to variance arising from different network initializations for each schedule, as well
as differences between the two datasets explored here. As expected, increasing inflation gap while
maintaining the number of preserved dimensions leads to worsened generative performance (higher
FID scores, <strong>Table 3</strong>). Finally, in terms of predictive calibration, our model provides substantial gains
when compared to existing injective flow model baselines (<strong>Table 4</strong>).


### 8.2 Animations for Image Dataset Experiments 

To see animations for sample generation (FID) and round-trip (MSE) experiments 
under select schedules, please check the links below!

#### [AFHQv2, Constant Inflation Gap (IG = 1.02) Experiments]({% link afhqv2_constant_IG_exps.md %})

#### [CIFAR-10, Constant Inflation Gap (IG = 1.02) Experiments]({% link cifar10_constant_IG_exps.md %})

#### [AFHQv2, Varying Inflation Gaps (IG = 1.10, 1.25, 1.35, 1.50) Experiments]({% link afhqv2_varying_IG_exps.md %})


<!---<p>To verify that inflationary flows successfully compress high-dimensional data, we compressed two 
benchmark datasets (CIFAR-10 [Krizhevsky et al., 2009](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf) 
AFHQv2 [Choi et al., 2020](https://arxiv.org/pdf/1912.01865)) 
to 20%, 10%, and 2% of their nominal dimensionality and examined their round-trip and pure generative 
performance. For estimating score, we used DBM networks trained using the scheme of 
[Karras et al., 2022](https://arxiv.org/pdf/2206.00364) combined with the novel scaling, noise, and preconditioning proposed above 
(see Appendices B.1, B.4.2 of our paper for details).</p>---> 

<!---<p>In the table below, we show computed Frechet Inception Distance (FID) 
scores [Heusel et al., 2017](https://proceedings.neurips.cc/paper_files/paper/2017/file/8a1d694707eb0fefe65871369074926d-Paper.pdf) 
over three different sets of 50,000 randomly generated images for each of 
these schedules and for both datasets. Note that preserving 
intrinsic dimension (PRP schedules) leads to better (smaller) FID scores, as expected. Perhaps surprisingly, 
increasing the number of preserved dimensions in the PR-Reducing regimes leads to <em>worse</em> (i.e., higher) 
FID scores. This is because retaining more dimensions in our PR-Reducing schedules leads to larger scale gaps 
between our preserved and compressed dimensions (i.e., larger \\( e^{\rho(g_* - g_i)T} \\)), thus increasing the 
required noise range over which networks must optimally estimate scores. That is, PR-Reducing schedules 
with higher numbers of preserved-dimensions pose a more challenging learning problem 
(Appendix B.2 of our paper).</p>--->

<!---<p>In the same table, we also include mean squared errors (MSEs) for round-trip integration experiments
(i.e., from data to compressed space and back) across all schedules and datasets, computed over 3 randomly 
sampled sets, each with 10K images. Trends observed here are similar to the ones seen for FID experiments. 
PR-Preserving networks produced lower MSEs overall, whereas PR-Reducing networks with higher percentage of 
preserved dimensions (i.e., 20%) yielded higher MSEs than PR-Reducing networks with smaller number of 
preserved dimensions (i.e., 2%). Together, these results suggest that dimension reduction with 
inflationary flows may necessitate trade-offs between effective compression and the difficulty of score estimation, 
as noted above.</p>--->

<!---<p><caption><strong><center> FID and Round-Trip MSE Experiment Results for Image Datasets </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Metric</center></b></td>
    <td style='width:14%'><b><center>Dataset</center></b></td>
    <td style='width:17%'><b><center>PRP</center></b></td>
    <td style='width:18%'><b><center>PRR to 2%</center></b></td>
    <td style='width:18%'><b><center>PRR to 10%</center></b></td>
    <td style='width:18%'><b><center>PRR to 20%</center></b></td>
  </tr>
  <tr>
    <td>FID</td>
    <td>CIFAR-10</td>
     <td> 17.01 <span>&#177;</span>  0.10 </td>
     <td> 22.23 <span>&#177;</span> 0.16 </td>
     <td> 23.63 <span>&#177;</span>0.13 </td>
     <td> 25.93 <span>&#177;</span> 0.40 </td>
  </tr>
  <tr>
    <td>Round-Trip MSE</td>
    <td>CIFAR-10</td>
    <td> 0.23 <span>&#177;</span> 0.03 </td>
    <td> 2.06 <span>&#177;</span> 0.04 </td>
    <td> 2.25 <span>&#177;</span> 0.01 </td>
    <td> 4.16 <span>&#177;</span> 0.39 </td>
  </tr>
  <tr>
    <td>FID</td>
    <td>AFHQv2</td>
    <td> 11.89 <span>&#177;</span> 0.08 </td>
    <td> 13.07 <span>&#177;</span> 0.07 </td>
    <td> 13.67 <span>&#177;</span> 0.09 </td>
    <td> 16.77 <span>&#177;</span> 0.14 </td>
  </tr>
  <tr>
    <td>Round-Trip MSE</td>
    <td>AFHQv2</td>
    <td> 0.38 <span>&#177;</span> 0.04 </td>
    <td> 5.57 <span>&#177;</span> 0.20 </td>
    <td> 7.95 <span>&#177;</span> 0.31 </td>
    <td> 8.17 <span>&#177;</span> 0.08 </td>
  </tr>
</table></p>--->

<!---<p>Finally, we include below animations showcasing roundtrip-integration and generation experiments for
some sample small batches of the AFHQv2 dataset ([Choi et al., 2020](https://arxiv.org/pdf/1912.01865)). 
For the round-trip experiments, reconstructions are qualitatively good across 
all conditions. Of note, networks trained with proposed dimension-reducing schedules produce inflated representations that resemble true low-rank Gaussian 
samples, consistent with a reduced-dimensional latent space. </p>--->

<!---<p>### 8.2 Animations</p>--->

<!---<p>### 8.2.1 PR-Preserving</p>--->

<!---<p>#### Round-trip  </p>--->

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/NV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRP_roundtrip.mp4" type="video/mp4">
</video></p>---> 

<!---<p>#### Generation</p>-->

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/M14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRP_gen.mp4" type="video/mp4">
</video></p>--->

<!---<p>### 8.2.2 PR-Reducing to 2%</p>--->

<!---<p>#### Round-trip</p>---> 

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/OV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto62D_roundtrip.mp4" type="video/mp4">
</video></p>--->  

<!---<p>#### Generation</p>---> 


<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/N14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto62_gen.mp4" type="video/mp4">
</video></p>---> 

<!---<p>### 8.2.3 PR-Reducing to 10%</p>--->

<!---<p>#### Round-trip</p>--->

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/PV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>-->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto307D_roundtrip.mp4" type="video/mp4">
</video></p>--->

<!---<p>#### Generation</p>--->

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/O14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto307D_gen.mp4" type="video/mp4">
</video></p>---> 

<!---<p>### 8.2.4 PR-Reducing to 20%</p>--->

<!---<p>#### Round-trip</p>--->

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/QV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto615D_roundtrip.mp4" type="video/mp4">
</video></p>---> 

<!---<p>#### Generation</p>--> 

<!---<p><center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/P14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center></p>--->

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto615D_gen.mp4" type="video/mp4">
</video></p>--->  




