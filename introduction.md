---
layout: page
title: Introduction 
subtitle: 
mathjax: true
---

<!--- 
TO DO's
Add links to all cites/refs in here!
Related work will not be included in website -- too much text!! 
 --->

<div style="text-align: center;">
<figure>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2026/05/DCF_concept.jpg" alt="schematic"> 
    <figcaption style="text-align: justify;"><font size="+1"><strong>Figure 1: Dynamic Compression Flow Schematic.</strong> Dynamical data \( \mathbf{x}_t \) at \( \tau=1 \) with dynamics defined by \( \mathbf{v}_{\boldsymbol{\theta}} \) are mapped to a lower-dimensional compressed representation (\( \tau=0 \)) via a compressive/generative flow \( \mathbf{u}_{\boldsymbol{\phi}} \). Both \( \mathbf{u}_{\boldsymbol{\phi}} \) and \( \mathbf{v}_{\boldsymbol{\theta}} \) are trained via flow matching defined by an encoder/coupling \( \boldsymbol{\mu}_{\boldsymbol{\psi}} \).</font></figcaption> 
</figure>
</div>

The emergence of large-scale neural recording technologies has drastically changed our understanding of neural function, shifting systems neuroscience from a single unit perspective to a focus on neural populations and their collective dynamics. Fortunately,  several lines of empirical evidence have shown that such seemingly complex and high-dimensional data can actually be described in terms of a much smaller number of "latent" variables \cite{gao2017theory, trautmann2019accurate, vyas2020computation, ebitz2021}. Not surprisingly, this observation has led to a proliferation of dimensionality reduction algorithms for neuroscience data. 

These algorithms broadly fall into two major types: In the first, the primary goal is to construct a lower-dimensional representation of the original high-dimensional data while maximizing some measure of information retained. This "mapping" from data to latent space can be accomplished using classic linear \citep[e.g., PCA, ICA, NMF,][]{mairal2009online, charles2011learning, draelos2021bubblewrap}, as well as non-linear approaches \citep[e.g., Isomap, LLE,][]{tenenbaum2000global, roweis2000nonlinear},  visualization-based approaches \citep[e.g., t-SNE, UMAP, PHATE,][]{van2008visualizing, mcinnes2018umap, moon2019visualizing}, and even more modern approaches such as VAEs \cite{Kingma_Welling_2014, Rezende_Mohamed_Wierstra_2014, goffinet2021low,martinez2026qlvm}, self-supervised learning \cite{azabou2024relax, schneider2023learnable}, and the information bottleneck \cite{tishby2000information}. Regardless of the specific method, such models often ignore temporal structure, treating data as independent. Consequently, learned representations will often severely distort and scramble the original dynamics. 

In the second class of models, nonlinear dynamical systems approaches attempt to capture temporal evolution directly, fitting flow fields to data \cite{charles2011sparsity, kutz2016dynamic, gao2016linear, rajan2016recurrent, linderman2017bayesian, pandarinath2018inferring, kerg2019non, zhao2020variational, wiltschko2020revealing, nair2023approximate, busch2023multi, driscoll2024flexible, weinreb2024keypoint}.  Despite their considerable success in identifying repeating dynamical motifs in data \cite{mante2013context,markowitz2023spontaneous,nair2023approximate,liu2024encoding,vinograd2024causal}, such models still struggle to identify useful structure under (1) non-repeatability and (2) noise-dominant regimes, which are ubiquitous in neuroscience data. That is, when modeling data without the benefit of smoothing and trial-averaging, these methods can struggle to identify structure, since they are tailored to identify repeating dynamical motifs \cite{williams2021statistical}. Likewise, most models assume that noise is small and/or follows simple forms, so that system evolution is governed by a well-defined velocity field. However, these assumptions can fail catastrophically when variance in the data is mostly due to unmeasured variables \cite{musall2019single} or has heavy-tailed structure, resulting in latent dynamics that appear random rather than lawful \cite{draelos2021bubblewrap}.

Recent breakthroughs in "simulation-free" flow training and the development of flow matching \cite{lipman_etal_2024_tutorial, Lipman_Chen_Ben-Hamu_Nickel_Le_2023, Albergo_Vanden-Eijnden_2023, Albergo_Boffi_Vanden-Eijnden_2023, Pooladian_2023, tong_2024} from earlier diffusion-based models \citep[DBMs,][]{Song_2021a, Karras_Aila_Aittala_Laine} have led to an explosion of work using latent flows \cite{Polyak_etal_2024, Dao_lantentflowmatching_2023, Hu_LatentFMEditing_2024, Schusterbauer_FMBoost_2024} and diffusions \cite{Vahdat_Kreis_Kautz, Blattmann_Rombach_Ling_Dockhorn_Kim_Fidler_Kreis_2023, Preechakul_Chatthee_Wizadwongsa_Suwajanakorn_2021, Hudson_Zoran_Malinowski_Lampinen_Jaegle_McClelland_Matthey_Hill_Lerchner}. Yet despite their huge success as generative models, most of these approaches do not directly allow for dimensionality reduction, relying instead on a front-end encoder network to infer latent representations from data. Unfortunately, this approach can re-introduce the same identifiability issue mentioned above. 
Even though recent diffusion-based models for neural latent dynamics and spiking data do address low-dimensional latent structure \cite{wang2023extraction,kapoor2024latent}, our focus here is instead on using flow matching to construct dynamics-preserving representations of data while also directly learning the velocity fields.
Additionally, in standard diffusion formulations, the source distribution is typically fixed to be Gaussian, while flow matching can define transport between arbitrary source and target distributions, which may be helpful in understanding non-Gaussian neural activity distributions across time. 

Here, we propose <i>Dynamic Compression Flows</i> (DCFs) as a means of inferring low-dimensional latent structure in a way that respects temporal dynamics in the data (<strong>Figure 1 </strong>). <strong> Our contributions are as follows: </strong>
* We develop a <i>dual</i> flow-matching approach, learning one generative/compressive flow field that maps the data to a low-dimensional latent space and another that captures temporal dynamics at each level of compression. Critically, the latent representations inferred by our model remain <i>identifiable</i> (up to a sign), making latent spaces reproducible across runs and thereby addressing a major limitation of previous approaches. 
* We achieve low-dimensional support for our latent distribution within the embedding space by training using nested dropout \cite{rippel_2014_nesteddropout}, which ensures that our latent dimensions are ordered by construction while also allowing for <i>controllable</i> and <i>soft</i> dimensionality reduction. 
* We apply our proposed model extensively to both synthetic and benchmark neural and behavioral data and compare it against a variety of competing approaches, demonstrating both its effectiveness and superior performance in challenging, noise-dominated regimes. 


#### Go to next section [Model]({% link model.md %})
