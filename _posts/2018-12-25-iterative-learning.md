---
layout: post
title: Iterative Unsupervised Learning
permalink: iterative-unsupervised-learning
tags: hypothesis
---

## How can we build unsupervised models with axes that we care about?

# Background

- Unsupervised models are getting stronger all the time. Examples include DAEs, VAEs, and GANs from recent literature but also PCA and LDA among older works.
- Notable applications of these works are those involving manipulating a learned Z embedding to tour through the latent space. If the algorithm has done a good job separating the factors of variation, then subtle and controlled interpolations in the Z will have phenotypically desirable effects on the outcome.
- Observe that it's the totality of the Z that is predictive (oftentimes with a reconstruction objective) while what we care about afterwards are the individual factors themselves. For example, we want to be able to isolate a sunglasses factor and interpolate from one extreme to the other.
- It is because of this mismatch that we end up with a handful of isolated factors that we care about and a mishmash then of ones that we either have trouble interpreting or don't actually care for.

# Idea

- Build an unsupervised model with a Z embedding over a dataset that we care about. For purposes of example, let's say it's a VAE with ten factors on CelebA.
- Now independently explore those 20 factors. One of them may control glasses, another may control beard, and a third may correspond to gender. We decide that we want to keep these and want the model to find new ones for the other seven. This is the human-in-the-loop expressing preferences that are otherwise hard to encode.
- We train a new model that takes as input the same dataset but also the prior model. It freezes the axes that we care about (there are a number of ways to do this) and has to find new ones for the axes that we don’t want. After it’s done, we repeat the process.
- How do we get it to find new axes? It’s almost surely going to find something new because there are a lot of ways to stir the pot wrt accounting for the information not expressed in the axes kept thus far. One way to think about this process is geometrically and note that what you're seeking is a rotation of the axes s.t. the new ones have some angle separation of at least a certain threshold from the old ones. We can then incorporate this notion into the optimization function and, because this is iterative, we can keep narrowing down the space of axes that SGD can yield. This might not be 100% sound, but seems on track.

# Hypotheses

- The main hypothesis is that this iterative process will yield models that are much more controllable in ways that humans care about. It will tend to stuff the less "interesting" features into a tighter part of the space.
- The secondary hypothesis is that this process won't actually take that long to do; It is an open question as to how many iterations we'll need.

# Variations

- One variation is to do N models at a time (each starting from the prior step's output) and then pick the model that has a maximal amount of accepted axes. This might make the process go even faster as it trades off steps for diversity.
- You don't have to actually train a completely new model each time. After you've trained the VAE once, you can then train an $$f$$ and $$g$$ each time s.t. $$g(f(Z)) = Z$$. This means that you can hone in on just the axes.
