---
layout: post
title: Three Machine Translation Papers
permalink: three-machine-translation-papers
tags: machine-translation,papers
---

I've been thinking recently about a novel way to do Machine Translation. Here are a few of the papers I've explored to see if it's been done before.

1. [Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation](http://arxiv.org/pdf/1406.1078v3.pdf)[^1]

   This paper develops a complete RNN encoder - decoder for translating from English to French. It uses this system to rescore hypotheses produced by a phrase-based system.

   While the author has gone on to develop more advanced systems (see below), there are two major takeaways from this paper. The first is how well the RNN encoder-decoder generates a continuous-space representation of phrases. The 1000-dimensional encodings it produces capture both semantic and syntactic structures helpful in translating between languages.

   The second major takeaway is the introduction of the Gated Recurrent Unit (GRU). This is similar to the Long Short Term Memory (LSTM) unit but much simpler and seemingly comparable[^2]. It has a reset gate and an update gate. After non-linearities are applied, the hidden state is a linear combination of the those two gates and the input.

2. [Sequence to Sequence Learning with Neural Networks](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf)[^3]

   This famous paper references the above work by Cho as well as the work below by Bahdanau. I might be mistaken, but think that this is the first [successful] result for MT using a deep RNN encoder-decoder. They obtained a really good BLEU score (34.8) using an ensemble of five deep models utilizing 384M parameters and an 8,000 dimensional state.

   The main takeaway that I had from this paper was the idea of reversing the source string. They found that this was extremely valuable. It makes intuitive sense because if the source string is ```The dog is here``` and the target string is ```Le chien est ici```, then it's highly likely that we would want to associate ```The``` with ```Le```. By encoding that ```The``` last, it has the highest contribution to the encoding. The decoder is then more likely to associate the two correctly.

   Their hypothesis for this phenomenon is on page 4:

   >... Normally, when we concatenate a source sentence with a target sentence, each word in the source sentence is far from its corresponding word in the target sentence. As a result, the problem has a large "minimal time lag"[^4]. By reversing the words in the source sentence, the average distance between corresponding words in the source and target language is unchanged. However, the first few words in the source language are now very close to the first few words in the target language, so the problem’s minimal time lag is greatly reduced. Thus, backpropagation has an easier time "establishing communication" between the source sentence and the target sentence...

   A couple of other notes were that deep LSTM models worked much better than shallow ones. This isn't surprising and theirs used four layers. The training time, however, was surprising. They had to parallelize the algorithm over an 8-GPU architecture, with the four LSTM layers each having their own GPU. The remaining four were used to run softmax calculations. This resulted in a speed of 6,300 words per second for a training time of ten days.

3. [Neural Machine Translation by Jointly Learning to Align and Translate](http://arxiv.org/abs/1409.0473)[^5]

   This paper was birthed from the work above by Cho (and he's a co-author on this one). They conjectured that a bottleneck in the system is that the encoding is a fixed length vector. The input, a sentence, is of variable length. So is the output (another sentence). To get from one to the other, most systems encode the variable-length sequence of words into a fixed length vector and then decode them into the variable-length output. They hypothesized that this caused earlier systems to underperform on longer sentences[^6].

   Their solution is an *attention* model. This has tremendous success jointly learning to translate and align the words correctly.

   It works by having a bi-directional RNN for the encoder. The forward direction computes a sentence vector as usual (using GRUs). There is another backwards direction that does the same but in reverse. However, there isn't just one of each - there is a pair for every word in the sentence. For an `N`-length sentence, we'll have `N` pairs of these vectors. The `jth` forward direction vector is the result of the running the forward RNN up to and including the `jth` word. Similarly, the corresponding `jth` backward direction vector is the result of running the backward RNN up to and including the `jth` word. Each of these pairs are concatenated together and called an Annotation. Think of them as being a summary of the preceding and following words, focused on the nearby words.

   Now the decoder takes these Annotations as input. It then needs to decide which Annotation to *pay attention to*. It does this by computing a probability for each Annotation where the input is influenced by an alignment model. That model scores how well the inputs around each position match with the output at the specified position. It's a feed-forward neural network that is jointly trained with the whole system.

<br><br><br>

[^1]: Cho et al, Universite de Montreal
[^2]: A later [paper](http://arxiv.org/abs/1412.3555) by Chung et al suggests this.
[^3]: Sutskever et al, Google
[^4]: S. Hochreiter and J. Schmidhuber: [LSTM can solve hard long time lag problems](http://papers.nips.cc/paper/1215-lstm-can-solve-hard-long-time-lag-problems.pdf)
[^5]: Bahdanau et al, Jacobs University Bremen
[^6]: Cho et al, [On the Properties of Neural Machine Translation: Encoder-Decoder Approaches](http://arxiv.org/abs/1409.1259)
