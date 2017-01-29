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


### Real World Generation tasks

+ Next video frame prediction (lotter et al 2016)
  + GAN gives much better performance than MSE.
  
<img src="https://github.com/markostam/sandbox/blob/master/photos/Screenshot%202017-01-29%2017.32.45.png" width="400">
