---
layout: post
title: Unsupervised and Multimodal Seq2Seq
permalink: papers-multimodal-seq2seq
tags: papers
---

{% include katex_import.html %}

Three more papers! These are on multimodal / multilingual translation, as well as an approach to incorporating monolingual data that I've also been pursuing. Thanks to Cho (at NIPS) for bringing them to my attention.

1. [Semi-Supervised Sequence Learning](http://arxiv.org/pdf/1511.01432v1.pdf)[^1]

   This paper is especially cool to me because I have been playing a lot with the model they are naming a _Seq2Seq Autoencoder_. It looks like a normal encoder-decoder model, but acts as an autoencoder just trying to output its input.

   Their experiments are on document classification and sentiment analysis. They show that pretraining LSTMs with Seq2SeqA or a recurrent language model (RNN-LM) improves their performance enough that they can go past state of the art without additional data. They also show that using more unlabeled data in the pretraining can improve how the subsequent supervised model generalizes.

   For the Seq2SeqA, Their setup is similar to the Seq2Seq model from Sutskever et al[^2]. Their only difference is that the weights of the encoder and decoder are the same.

   They tried three different approaches to the experiments:
   1. At the output layer, predict the document label.
   2. Linear label gain: put the label at every timestep and linearly increase the weights of the prediction objectives from 0 to 1.
   3. Jointly train the supervised learning task with the Seq2SeqA.

   The main problem that this solves is the instability in LSTMs, which are really sensitive to hyperparameters for long documents. In contrast, the Seq2SeqA works better and is more stable. Changing the size of the hidden state or the number of backprop steps now hardly affects training.

   Training numbers include 1024-unit memory cells, 512-unit embedding layer, and 512-unit hidden layers with dropout of 50% between the last hidden state and the classifier. There's also 80% dropout on the input.

2. [Multi-Task Learning for Multiple Language Translation](http://www.aclweb.org/anthology/P15-1166)[^3]

   This paper explores translating from one source language to multiple target languages using neural machine translation. Three contributions:
   1. ML framework doing single source to multi-lingual translation.
   2. Show that multiple languages help improve each individual language translation.
   3. Show that using language pairs with large-scale parallel corpora can improve translation quality of those with little parallel data.

   They use the same model as Bahdanau et al[^4] with bidirectional attention and GRUs. Learning is done in mini-batches with a fixed language for several iterations and then onto the next language for another several. The dataset was the Europarl corpus using English as the source and Spanish, French, Portuguese, and Dutch as the target languages, with the top 30k words for each.

   Training details are:
   - Initialization uniform between -.01 and .01.
   - SGD with Adadelta
   - Minibatch size of 50, with 1000 mini batches for each language pair
   - Word reps have dimensionality 1000 and the hidden layer size is 1000 as well.

   Each of the four pairs had increased Bleu scores versus single NMT trained on the same amount of data, ranging from .48 (En-Pt) to 1.38 (En-Es).

3. [Multi-Task Sequence to Sequence Learning](http://arxiv.org/pdf/1511.06114v1.pdf)[^5]

   This paper builds on the previous two. It explores three settings:

   - One to Many Seq2Seq. Here, the encoder is shared between several tasks like machine translation or syntatic parsing, with each task having its own decoder.

   - Many to One Seq2Seq. Here, the decoder is shared but the encoders are not. Image caption generation and translation are examples.

   - Many to Many Seq2Seq. Here, the encoders and decoders are all shared. Given the above where the unitary object was the shared one, this might be a little confusing. Briefly, training on parsing or image caption improves translation accuracy and vice versa. See below for how it works.

   It also explores combining these with unsupervised objectives, trying both the seq2seq autoencoder and skip-thoughts[^6]. For this, they went beyond what Dai and Le did and jointly trained the autoencoders with the model objectives.

   To train on diverse tasks, they allocated different numbers of parameter updates for each task because they each different amounts of data. Each parameter update consists of training data from a single task. When switching, they select a random new task *i* with probability proportional to the number of training data it has. When sharing an encoder or decoder, both the connections and the embeddings are shared.

   Data is largely drawn from WMT '15 taking the 50k most common words in each language. The models are 4-layer 1000-dimension LSTMs with parameters initialized uniformly in [-.06, .06], a mini batch size of 128, dropout .2, reversed inputs, a fixed learning rate of .7 that halves every y epochs after x epochs have passed.

   Results for the one-to-many experiments showed that adding a small number of parsing minibatches (one per 100 translation mini-batches) achieved a mean gain of .6 in translation perplexity. The translation also helped with parsing. Note though that the full results were actually 8.2 +- .2 from 8.8 +- .3 on translation and 1.88 +- .16 to 1.67 +- .03 on parsing.

   For many-to-one, the model also showed improvements. Including five image caption parameters in every 100 translation updates improved translation perplexity by .7. If we change it so that for every caption parameter, there are two translation minibatches, there is a gain of 3.3 in image caption perplexity.

   Last, for many-to-many, they used monolingual corpora and found that seq2seq autoencoder decreased translation performance but skip-thought vectors increased performance.

   A few questions:

   - What happens if we use attention as well? They left that for future work.
   - What happens if you use one encoder and one decoder but let them act as any type of input/output? As far as I can tell, the encoders and decoders here are all distinct. So in the many-to-many with German encoder going to both English and German decoders, the decoders are separate and the data is pushed through in parallel. That means that the German encoder will learn from both decoders and the English decoder will learn from both encoders. However, there is no encoder that learns jointly from both inputs and no decoder that learns jointly from both outputs.
   - What about more languages as the multi-task? Here, multi-modal is considered but not multi-lingual.

<br><br><br>

[^1]: Dai et Le, Google
[^2]: Sutskever et al, [Sequence to sequence learning with neural networks](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf)
[^3]: Dong et al, Baidu
[^4]: Bahdanau et al, [Neural machine translation by jointly learning to align and translate](http://arxiv.org/abs/1409.0473)
[^5]: Luong et al, Stanford + Google
[^6]: Kiros et al, [Skip-Thought Vectors](http://arxiv.org/abs/1506.06726)