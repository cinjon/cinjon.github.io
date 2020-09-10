---
layout: post
title: Space of Motion
permalink: space-of-motion
blurb: "Hypothesis: How can we learn to differentiate an amateur handstand from an expert handstand with the insight being that all expert handstands look similar but all amateur handstands are dissimilar due to a small number of categorical errors?"
tags: hypothesis,video
---

## Can we learn to differentiate an amateur handstand from an expert handstand?

# Background

- An expert can look at an amateur doing a handstand and immediately understand what's wrong. Examples include "not pushing through the shoulders", "not having a hollow body", "not having enough of a hollow body", "balancing with your legs", etc.
- An amateur can look a handstand and say that something is wrong, but not necessarily know what is wrong.
- Both can do this with just a [profile] video and the observer doesn't need to be present.
- This suggests that it's relatively easy to classify a handstand as being bad but much more difficult to assess why it's bad.
- Note also that there is lots and lots of data online for both categories, concentrated heavily on Instagram and YouTube. This extends to other domains as well, ranging from golf swings to shooting a basketball to doing deadlifts and squats, etc.
- Finally, there are pretty good pose estimation models now that we can leverage. See references below.

# Idea

- Gather a dataset of videos. Label these with (binary expert / amateur, categorical motion). Example motions are "handstand" and "squat".
- We would like embeddings of the space of the movements. With a well formed space, we could then take a new video and tell how close it is to an expert's motion. We could also tell what is wrong with it by the company it keeps.
- We make an assumption that the expert videos are all identical. This is reasonable because for the facets that we care about they should be. 
- We further make an assumption that the amateur videos are clustered by what they are doing wrong ("hips too low", etc).
- One variation says we don’t know those clusters a priori. In that scenario, one approach is to assume there are $$K$$ categorical errors and try to cluster them accordingly.
- In another variation, we label them beforehand. We do this by first deciding what are the possible errors and then having experts label each video with which error occurred where in the video.
- In both, we want to align embeddings with these error categories s.t each video is projected to an embedding and that space is well-formed.

# Hypotheses

- The hypothesis is that this approach will yield, through semi-supervised means, a way to discriminate aspects of human motion. In particular, it should highlight movement that is less than optimal for a given motion. With tweaks to data (golf swings instead of squats), we could shift to discovering other patterns of motion.
- A further hypothesis is that this approach will yield a space of motion that lets us do embedding arithmetic to morph from one motion into another.

# Variations

- There are two variations in the above (we know the clusters a priori or we don't), but both are defined by clustering. 
- Can we do the same clustering but without labeling the data by expert vs amateur? We should still expect there to be the same clusters, but we would need to do something else to get over the fact that we'd no longer be feeding it the inductive bias associated with our declaring that experts don't have errors and amateurs do. A fear is that it instead learns to find attributes that we don’t care about, like body composition.
- Perhaps we can build in a clever loss involving the video motion and pose estimators.
- Another variation is to utilize a 3D model of the target along with the submitted video. This will be difficult to do at training time as most videos out there do not have an associated model. However if this worked, then future versions could take advantage of this to make a better approach.

# References
- Densepose: [1](https://github.com/facebookresearch/Densepose), [2](https://arxiv.org/abs/1802.00434), [3](http://densepose.org/)
- [DeepLabCut](https://github.com/AlexEMG/DeepLabCut)
- DeeperCut: [1](http://pose.mpi-inf.mpg.de/), [2](https://arxiv.org/abs/1605.03170)
- Yaser Sheikh’s group: [1](https://arxiv.org/pdf/1611.08050.pdf), [2](https://arxiv.org/pdf/1801.01615.pdf), [3](http://www.cs.cmu.edu/~yaser/)

