---
layout: page
title: Three Views of Diffusion Models
subtitle: Quick intro 
mathjax: true
---

<center><iframe height="360" width="540" src="https://warpwire.duke.edu/w/C14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!--- <p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="https://warpwire.duke.edu/w/C14IAA/" type="video/mp4">
</video></p>  --->

<!--- Of note, I need to fix the margins of this video to make it have less white space --> 
<!--- <center><strong><font size="+2"><p>
  <em> Animations showcasing 3 different views of DBMS</em>
</p></strong></center></font> ---> 


### 1.1 Stochastic Differential Equation (Noising) View of DBMs

Standard Diffusion-Based models (DBMs) use a fixed, forward noising process defined 
by the stochastic differential equation (SDE) ([Sohl-Dickstein et al., 2015](https://arxiv.org/pdf/1503.03585); [Song & Ermon, 2020](https://arxiv.org/pdf/1907.05600)): 

$$ d \mathbf{x} = \mathbf{f}(\mathbf{x}, t) dt + \mathbf{G}(\mathbf{x}, t) \cdot d \mathbf{W}, $$

to transform an initial data distribution \\(p_0(\mathbf{x}) = p_{data}(\mathbf{x})\\) into an isotropic Gaussian. Here, 
typically, one assumes linear drift and monotically increasing isotropic noise  (i.e., \\(\mathbf{f} = f(t) \mathbf{x}\\) 
and \\(\mathbf{G}=\sigma(t)\mathbf{I}\\)), such that  for \\(\int_0^T \sigma(T)dt \gg \sigma_{data}, \; p_T(\mathbf{x})\\) 
becomes essentially indistinguishable from a Gaussian distribution ([Karras et al., 2022](https://arxiv.org/pdf/2206.00364)). 
DBMs work by learning an approximation to the 
corresponding reverse SDE ([Anderson, 1982](https://pdf.sciencedirectassets.com/271499/1-s2.0-S0304414900X01883/1-s2.0-0304414982900515/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLWVhc3QtMSJHMEUCIQCdPMW8lwVmUmg%2FUbOsuCa4sj3mSVpiYrQFjF%2BNBiu2owIgD8lLoOCOqcBqHZrRqRTBVVGdjlJ%2FcdhGZf9B3QbaYWIqvAUI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAFGgwwNTkwMDM1NDY4NjUiDItfFx5kNK4oUelAGiqQBaW3FLfACTS%2B06TCC8TRuCjQpat9jSrX%2FS6lIVHfsUmdpimaxhTQaUz%2B0uYGl6283rUBRe4BEB1pw78hK160z6qxC8qaZPOegZ3iWM4pC0oRdvjsfpDVrOjzYu%2F0iuvkFO09313OlJku7LZkl2IbzbuKg5FDhrwbesGUpcPAqIMnGLwjR361%2FIYHGGtI1r2%2BozGQor4e4gL7ukeoqcoF%2FJbdF9bLZjCxJ9Z8bAcOf5o4rG%2BbuGYMicL9R%2BFeITBIhQvjb3tKk8TAM4%2BrWDQxHNMqFxbGzhQSnPsNcgTnmMUsuagE%2F3PrMgIIhEpax6CTH6%2BzrPIYfmyJrtO0nvVq6To%2BNdZSZejqIHnQ7%2Fhe1yl9iw2B%2F%2B%2FBL7sLI4oVvEdIu8g10k4828caQ1A3VQcYwFb6OSkgWZxhexgyM7%2F2%2FeMmMj63f%2FU%2B36cJxL0LDtpCtm1d5j%2FHu6ilqhYK6b2KUradboSFsJkBtFDb2nSpi9id%2FV5iP35pNz8RWOrK24uVN2kURztFPxhKg934BVMV%2Bq4LfvOSOlALLBGY%2FARZpvLmaD%2B%2BelN850UrF6LFRLAwmZYiCz7L4lHtPNyAIsynD%2FwDQL5QB9VaUbpRgcCkRrioBvML35lf36jJj%2FXoi%2BHvp%2Fu2rtsuNGxLhVhZGa4qOhnMXTWOsevwTi%2BEDmoN96Wz8EaAayZAkPWbgRsOuRBWdw8Qdo0caJGrVVdun9w3ZQQl53ugtu1wknvYkYXCY4rxc0HEKJXx47iSAb4nLgmhpNMDSLHMxtN04XITZZUxCXFXqrHrxsHkYMVxuC8jzTAlQHKj%2B7pMJuUNMSxCyoDAx3xGHEc1arjGQs6zR5V9LVceAmcq0dx91kTeyeXcTsQNMJ2ujLMGOrEBdXeQyP9YylrylmcPHI0%2BNxmLfJFxnN2hWyFyxLnby8oDOnM5X7c7DeYy2t2IbFgGHQnkQntunsHQIfVFiTgRxbO%2B7HhNj1Vh5VDMKqKReTjYtRgBLg9Kdj%2BqNMSBqesm600%2B%2ByebkY35qMDUYafExM0rLn7WVtveAJLYI42Cz8oyjhww7Z5gS58NwBOaLYzmzVmabkpC%2FpX8nzHstTRPtAov8KWjlSEF%2FCLXOeSOmUyA&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240607T151658Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYRST24CN6%2F20240607%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=d4d4db0ac1fd3e103ef523ea2ae4d3fc7e5a5b87f23fdaa71043f41d0a90b7c9&hash=593b87b468bd6db6d0ac854749254fe568dced546d10680360ec88d083a52fc5&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=0304414982900515&tid=spdf-734c6496-4da1-4c0a-bf74-1e2d2a6a6d31&sid=ec9540576e8661426f0986b-b9a7980851b7gxrqa&type=client&tsoh=d3d3LnNjaWVuY2VkaXJlY3QuY29t&ua=0f15595651515602505759&rr=89019b3689840cb5&cc=us)): 

$$ d \mathbf{x} = \lbrace \mathbf{f}(\mathbf{x}, t) - \nabla \cdot [\mathbf{G}(\mathbf{x}, t)\mathbf{G}(\mathbf{x}, t)^\top] - \mathbf{G}(\mathbf{x}, t)\mathbf{G}(\mathbf{x}, t)^\top \nabla_{\mathbf{x}}\log p_t(\mathbf{x}) \rbrace \mathrm{d}t + \mathbf{G}(\mathbf{x}, t) \cdot \mathrm{d}\mathbf{\bar{W}} $$

where \\( \mathbf{\bar{W}} \\) is time-reversed Brownian motion. 
In practice, this requires approximating the score function \\(\nabla_{\mathbf{x}}\log p_t(\mathbf{x})\\) 
by incrementally adding noise according to the schedule \\(\sigma(t)\\) of the forward process and then requiring that denoising by 
the reverse SDE match the original sample. The fully trained model then generates samples from the target distribution 
by starting with \\(\mathbf{x}_T \sim \mathcal{N}(\mathbf{0}, \sigma^2(T) \mathbf{I}) \\) and integrating the reserve SDE backwards in time.
Note that this process is <em> noisy </em> and <em> mixing </em> (left panel on animation above). 


### 1.2 Distributional View of DBMs
As previously shown ([Song et al., 2021](https://arxiv.org/pdf/2011.13456); [Karras et al., 2022](https://arxiv.org/pdf/2206.00364)), this forward diffusion process gives rise to a series of marginal distributions 
\\(p_t(\mathbf{x})\\) satisfying a Fokker-Planck equation (FPE): 

$$ \partial_t p_t(\mathbf{x}) = -\sum_i \partial_i[f_i(\mathbf{x}, t) p_t(\mathbf{x})] + \frac{1}{2}\sum_{ij} \partial_i \partial_j \left[\sum_k G_{ik}(\mathbf{x}, t)G_{jk}(\mathbf{x}, t)p_t(\mathbf{x})\right]$$

where \\(\partial_i \equiv \frac{\partial}{\partial x_i}\\). In the "variance preserving" noise schedule of  [Song et al., 2021](https://arxiv.org/pdf/2011.13456), 
the above Fokker-Plack equation has as its stationary solution an isotropic Gaussian distribution. 
That is, in DBMs, the forward process is a means of gradually transforming the data into an easy-to-sample form (as with normalizing flows) 
and the reverse process as a means of data generation. 
Therefore, one can analyze DBMs through the lenz of their respective Fokker-Planck equations and the series of marginal distributions 
these induce over time - i.e., we can view these models from a <em> distributional perspective</em> (middle panel of animation). 

### 1.3 Probability Flow ODE View of DBMs

However, a different process with no noise term can generate marginal distributions satisfying the same FPE governing 
the forward noising process ([Song et al., 2021](https://arxiv.org/pdf/2011.13456)). This is the so-called <em>probability flow ODE</em> (pfODE) view of DBMs:

$$  d \mathbf{x} = \left\lbrace \mathbf{f}(\mathbf{x}, t) - \frac{1}{2}\nabla \cdot [\mathbf{G}(\mathbf{x}, t)\mathbf{G}(\mathbf{x}, t)^\top] - \frac{1}{2}\mathbf{G}(\mathbf{x}, t)\mathbf{G}(\mathbf{x}, t)^\top \nabla_{\mathbf{x}}\log p_t(\mathbf{x}) \right\rbrace dt $$ 

Unlike the noising process induced by the forward SDE, this process is instead <em>deterministic</em>, 
and data points evolve smoothly, resulting in a flow that preserves local neighborhoods (right panel of animation).
Moreover, the pfODE is uniquely defined by \\(\mathbf{f}(\mathbf{x}, t)\\), \\(\mathbf{G}(\mathbf{x}, t)\\), 
and the score function. In this work, we show how this pfODE, constructed using a score function estimated by training the corresponding DBM, 
can be used to map points from \\(p_{\mathrm{data}}(\mathbf{x})\\) to a compressed latent space in a manner that affords accurate uncertainty quantification.

### 1.4 Connections to Flow Matching 

This connection between the marginals satisfying the SDEs of diffusion processes and <em>deterministic flows</em> described by an equivalent ODE 
has also been recently explored in the context of flow matching models ([Lipman et al., 2023](https://arxiv.org/pdf/2210.02747); [Liu et al., 2022](https://arxiv.org/pdf/2209.03003); [Albergo & Vanden-Eijnden, 2023](https://arxiv.org/pdf/2209.15571)).
In a nutshell, flow matching models utilize a simple, time-differentiable, "interpolant" function to specify <em>conditional</em> families of distributions 
that continuously map between specified initial and final densities. That is, the interpolant functions define flows that map samples from a base 
distribution \\(\rho_0(\mathbf{x})\\) to samples from a target distribution \\(\rho_1(\mathbf{x})\\). Typically, these approaches rely on a simple quadratic objective 
that attempts to match the <em>conditional</em> flow field, which can be computed in closed form without needing to integrate the corresponding ODE. 


As shown in <strong>Appendix A.5</strong> of our paper, the pfODEs obtained using our proposed scaling and noising schedules are <em>equivalent</em> to the ODEs obtained by 
using the "Gaussian paths formulation" from [Lipman et al., 2023](https://arxiv.org/pdf/2210.02747) when the latter are generalized to full covariance matrices. 
As a result, our models are amenable to training using flow-matching techniques, suggesting that faster training and inference schemes may be possible through 
leveraging connections between flow matching and optimal transport 
([Tong et al., 2024](https://arxiv.org/pdf/2302.00482); [Pooladian et al., 2023](https://arxiv.org/pdf/2304.14772); [Tong et al., 2024](https://arxiv.org/pdf/2307.03672); [Albergo & Vanden-Eijnden, 2023](https://arxiv.org/pdf/2209.15571)).



#### Go to next section [Inflationary Flows pfODE]({% link inflationary_flows_model.md %})
