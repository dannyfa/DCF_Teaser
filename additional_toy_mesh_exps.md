---
layout: page
title: Additional Calibration Experiments 
subtitle: Using Meshes to Assess Coverage Mismatch 
mathjax: true
---

### Experiment Set Up 
To further assess numerical error incurred when integrating our proposed pfODEs, we performed additional coverage experiments using 3D meshes 
and 2D alpha-shapes ([Akkiraju et al., 1995](http://wcl.cs.rpi.edu/papers/b11.pdf); [Edelsbrunner & Mucke, 1994](https://dl.acm.org/doi/pdf/10.1145/174462.156635)) 
on select toy datasets (i.e., 2D circles and 3D S-curve, <strong>Appendix C.2.1</strong>). If our numerical integration and score estimation were perfect, 
points initially inside these sets should remain inside at the end of 
integration; failure to do so indicates miscalibration of the set's coverage. For these experiments, 
we sampled 20K test points from a Gaussian latent space with appropriate diagonal covariance. For PR-Preserving schedules, 
this is simply a standard multivariate normal with either 2 or 3 dimensions. For PR-Reducing experiments, this diagonal covariance 
matrix contains 1's for dimensions being preserved and a smaller value (\\( 10^{-2} \\) for Circles, \\( 2.5\times 10^{-3} \\) 
for S-curve) for dimensions being compressed. 

Next, we sampled uniformly from the surfaces of balls centered at zero and with linearly spaced Mahalanobis radii ranging 
from 0.5 to 3.5 (200 pts per ball, see schematic below). We then fit either a 2D alpha-shape (2D Circles) or a mesh (3D SCurve) to each one of 
these sets of points. These points thus represent ''boundaries'' that we use to assess coverage prior to and after 
integrating our pfODEs. We define the initial coverage of the boundary to be the set of points (out of 20K test points) 
that lie inside the boundary. We then integrate the pfODE backward in time (the ''generation'' direction) for each sample and 
boundary point. At the end of integration, we again calculate the mesh or 2D alpha-shape and assess the number of samples inside, 
yielding our final coverage numbers. Similarly, we take our samples and boundary points at the end of generation, 
simulate our pfODEs forwards (i.e., the ''inflation'' direction), and once again, use 2D alpha-shapes and meshes to 
assess coverages at the end of this round-trip procedure. 

<figure>
    <figcaption><strong><center><font size="+2.5">Schematic for Toy Coverage Experiments</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Toy_Mesh_Exp_Schematic.jpeg" alt="schematic">
</figure>  



### Results and Discussion 

As shown below (Toy Coverage Experiments Results, Panels (B), (C)), we are able to preserve coverage up to some small,
controllable amount of error for both schedules and datasets using this process. Lines represent means and shaded regions \\( \pm 2 \\) standard
deviations across three sets of random seeds.


<figure>
    <figcaption><strong><center><font size="+2.5">Toy Coverage Experiments Results</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Toy_Mesh_Exp_Results.jpeg" alt="results">
</figure>  



Finally, we also include below animations showcasing PR-Preserving and PR-Reducing ''inflation'' and ''generation'' for a sample 2D dataset (moons) 
and for a set of test points initially sampled to be contained inside two small spheres, centered at a random data points.  Note that integrating our pfODE 
leads to substantial geometry distortion of the original spherical boundaries and of the test points contained within them. However, despite this distortion, 
our proposed pfODE method still allows to adequately preserve original probability coverages up to a small, controllable error. 

### Moons PR-Preserving ''inflation'' and ''generation'' 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/GV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prp_melt_gen_cluster_distortion.mp4" type="video/mp4">
</video></p>--->

### Moons PR-Reducing ''inflation''

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/HV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prr_melt_cluster_distortion.mp4" type="video/mp4">
</video></p>--->

### Moons PR-reducing ''generation''

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/G14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prr_gen_cluster_distortion.mp4" type="video/mp4">
</video></p>--->



#### Go to next section [Experiments on image benchmark datasets]({% link hd_exps.md %})