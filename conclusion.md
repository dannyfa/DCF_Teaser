---
layout: page
title: Conclusion
subtitle: 
mathjax: true
---
<!--- 
TO DO's 
Add links to refs/bib
Cross-ref figs, tables, sections 
--->

Accurate, reproducible methods for understanding and mapping high-dimensional neural dynamics and behavior remain one of the most pressing needs in systems neuroscience. Here, using a flow-matching approach, we combine the strengths of both "mapping" approaches to dimensionality reduction, which assume i.i.d. data, and "dynamical" approaches that focus on the relationships between successive time points. By learning a pair of vector fields, one for compressing data, one for dynamics, we reduce dimensionality while preserving temporal structure. This is made possible by two key innovations: First, rather than specify a simple source distribution like an isotropic Gaussian, we implicitly fit this distribution by learning an encoder (data coupling). Second, we train using Nested Dropout, which allows us to produce an ensemble of true low-dimensional latent spaces. Just as importantly, we have constructed a model that is identifiable up to signs, with the result that our learned latent space is reproducible across training runs.


In experiments, we applied this method to a challenging toy data set, along with population neural data, behavioral video, and audio data. We found that while some comparison models were able to identify latent spaces with known structure in simple cases (neural data), comparison models failed to identify structure in the more challenging behavioral video data, and many failed at even very simple toy examples (balls data). Moreover, the flow-based approach produced both higher-accuracy data reconstructions (<strong>Table XX</strong>) than other generative models and higher latent space decoding accuracy (<strong>Table XX</strong>) than all but LFADS.

#### Limitations

Without smoothing, the modeling framework presented here is not directly applicable to discrete data (e.g., spike counts), which are ubiquitous in neuroscience. However, this limitation can be easily addressed by leveraging recent developments in <i>discrete</i> flow-matching techniques \cite{gat2024discrete, campbell_etal_2024}, though we leave this for future work. Another limitation lies in the fact that our model does not directly estimate data intrinsic dimensionality but instead requires practitioners to choose the hyperparameter \\( K\_{\text{eff}} \\) (cf. Section XX), which dictates effective latent space dimensionality. For our experiments, we proposed a simple definition of \\( K\_{\text{eff}} \\), which allows the model to train stably and provides a conservative estimate of the true data dimensionality. Future work might further extend our existing approach to allow for latent dimension pruning and adaptive learning of \\( K\_{\text{eff}} \\) based on some measure of information preservation in latent space.