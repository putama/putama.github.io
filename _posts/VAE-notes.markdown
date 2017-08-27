---
layout: post
title:  "Notes on Variational Autoencoder"
categories: main
---

## First notes: http://kvfrans.com/variational-autoencoders-explained/
## Background:
* Plain GAN can only generate image from random noise
* Generated image cannot be conditioned on certain variable
* No constraint on how the generated image looks like

## VAE intro:
* Consider a deconvolutional network that able to generate an image which characteristics are determined by an input vector
* Changes on the distribution of this input vector corresponds to a visual features of the output image
* We refer to this input vector as a latent variable
* Now consider a standard autoencoder that starts with convolutions and ended with deconvolutions, the input vector is now the intermediate vector representation
* VAE add some constraint to the network to encode latent vector that roughly follows unit gaussian distribution
* We do so by adding up two loss terms: the generative loss (measure the quality of the constructed images), the latent loss (KL divergence measure of the latent vector to gaussian distribution)

## VAE how:
* Applies re-parameterization trick to the encoder network: implicitly model mean and standard deviation of the latent vector
* The architecture will be roughly: input -> convolutions -> {mean vector\|std vector} -> sampled latent vector -> deconvolutions -> output image
* latent vector = z_mean + (z_stdev * sampled_normal[0,1])
* as the training goes, it gets more efficient: when the std raised until 1, less information is lost (?)

## [https://jaan.io/what-is-variational-autoencoder-vae-tutorial/](Jaan's Post)
Discussed variational autoencoder from 2 perspectives: neural network and graphical models:

### Neural network perspective: 
* The autoencoder consist of 3 main components: encoder, decoder, loss function
* The encoder finds latent representation z, namely q(z|x). input x
* The decoder finds construct the original data from the latent representation, p(x|z)
* The loss: very similar to the previous post
* The KL term in the loss makes sure that the latent representation meaningful, as in, similar numbers representations get closer together
* KL term also works as regularizer, keeping the z to be 'sufficiently diverse'. 

### Graphical Models perspective:
* approximation of p(z\|x) with q(x). Measure the quality of the approximate with KL-divergence
* Minimizing KL-Divergence equals to maximizing ELBO
* parameterized q with lambda, generator network is parameterized by phi
* variational inference algorithm to learn variational parameters lambda
* variational EM algorithm to learn phi

## [http://blog.fastforwardlabs.com/2016/08/22/under-the-hood-of-the-variational-autoencoder-in.html](FastForward Whitepaper)
* vanilla autoencoder is deterministic, while a variational autoencoder is stochastic
* vae is a mashup of a probabilistic encoder $$q_\phi (z \mid x)$$ that approximate untractable p(z\|x); and generative decoder p(x\|z)
* both are abstracted as a neural network with prior that z follow gaussian distribution 
* the encoder job is to find the mean and variance of gaussian posterior, latent z is sampled from this distribution.
* take away: Bayesian methods provide a framework for reasoning about uncertainty. Deep learning provides an efficient way to approximate arbitrarily complex functions, and ripe opportunities to probe uncertainty (over parameters, hyperparameters, data, model architectures…).
* reparameterization: introducing sampling node with probability would make the model itself stochastic. we inject randomness by introducing arbitrary random variable e. z = mu + e * sigma
* tricky derivation of the cost function <- read this more thouroughly



[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com