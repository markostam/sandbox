# GAN's - Ian Goodfellow - NIPS 2016

## Generative Modeling

+ Generate a density estimation (fitting a gaussian to a distribution of points)
+ Based on training samples from a distribution, generate more samples from that same distribution.
  + GAN's generally fall in this category.
  
## Roadmap

1. Why study generative modeling.
2. How do generative models work? How do GANs compare to others.
3. How do GANs work?
4. Tips and tricks.
5. Research frontiers.
6. Combining GANs with other methods.

## Why study generative modeling?

+ Hi dimensional probability distributions are a problem across many engineeinrg spaces.
+ Useful for RL: train your agent in an environment totally simulated by a GAN.
  + easily parallelized
  + mistakes not as costly as if trained in the physical world
+ Simulate possible future for planning or simulated RL
+ Able to handle missing data better:
  + fill in missing inputs when labels are missing.
  + semi-supervised leaning, create larger training sets
+ Multi-modal outputs
+ Realistic generation taskss.


## Real World Generation tasks

### Next video frame prediction (lotter et al 2016)

+ GAN gives much better performance than MSE.
<img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2017.32.45.png" width="500">
+ successfully predicts the ear and gives the eyes sharp edges

### Single Image Super-Resolution (Ledig et al 2016)

+ bicubic interpolation (hand designed mathematical formula) vs. SRResNet vs SRGAN
+ RestNet and GAN both generate distribution that mimics that of the original and come out with something that is more visually pleasing and less blurry than the bicubic model.
<img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2017.51.38.png?raw=true" width="500">

### iGAN

+ assists human in creating artwork.
+ draw a black line and green field and it will draw photo quality photo

### Photo-realistic image editing

+ introspective adverserial network.
+ neural face-editing.

### Image to Image translation
+ mapping from extracted edges to the real image (possible to create huge space of training data)
<img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2018.02.51.png?raw=true" width="500">


## How do generative models work?

### Maximum Liklihood

+ <img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2018.05.46.png?raw=true" width="400">
+ Easy to compare models using maximum likelihood
+ We write a density function that the model describes: *p(x|θ)*
  + *p(x|θ)* is a function that takes parameters theta and input x that describes exactly where the data is spread thinly and where it concentrates.
  + max liklihood consists of taking the log probability that this function assigns to all training data and adjusting the parameters theta to inrease that probability.
  + the way models go about doing this is how generative models differ.
  
### Taxonomy of generative models:

<img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2018.25.20.png?raw=true" width="500">

+ main split is over *explicit* or *implicit* density functions
+ Explicit: 
   + It can be challenging to design a paramaetric function that is TRACTABLE:
    + Fully visible belief nets (NADE, MADE, PixelRNN)
    + Change of variables (nonlinear ICA).
  + or Approximate density:
    + Variational (Variational Autoencoder)
    + Markov Chain (boltzmann machine)
+ Implicit:
  + design a procedure than can draw samples from the probability distribution, even if we don't know it explicitly.
    + Markov Chain (general stochastic network)
    + Draw samples directly (GANs, deep moment matching networks), but don't necessarily represent a density function.

## Fully Visible Belief Nets

+ 
