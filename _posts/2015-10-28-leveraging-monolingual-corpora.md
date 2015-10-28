---
layout: post
title:  Papers Leveraging Monolingual Corpora
permalink: papers-leveraging-monolingual
tags: language-models,papers
---

{% include katex_import.html %}

Here's another set of papers! These ones are all about utilizing the tremendous amount of monolingual corpora out there. One of my interests here is in applying them to aid our comprehension across languages.

1. [Correlational Neural Networks](http://arxiv.org/abs/1504.07225)[^1]

   Common Representation Learning is the task of embedding different descriptions of data into a common subspace. An example is two sentences in different languages that mean the same thing. It would be good to reliably be able to associate one with the other without explicitly telling a machine that, for example, "cats" and "dogs" are alike.

   This paper does this a deep autoencoder model with a well-considered objective function. It then applies this model to tasks like cross-language document classification where a document is represented by the tf-idf sum of its bag of words representation. Here's the objective function it's trying to minimize:

   <br>

   $$\displaystyle J(\theta) = \sum_{i=1}^N L(z_i, g(h(z_i)))+L(z_i, g(h(x_i)))+L(z_i, g(h(y_i)))-\lambda corr(h(X), h(Y))$$

   $$\displaystyle corr(h(X), h(Y))=\frac{\sum_{i=1}^N (h(x_i) - \overline{h(X)})(h(y_i) - \overline{h(Y)})}{\sqrt{\sum_{i=1}^N (h(x_i) - \overline{h(X)})^2 \sum_{i=1}^N (h(y_i) - \overline{h(Y)})^2}}$$

   <br>

   Above, ```L``` is the reconstruction loss. They use cross-entropy error if the input takes binary values and squared error loss otherwise. ```z_i``` is the paired inputs from the different descriptions (sentences), ```(x_i, y_i)```, and ```h(z) = f(Wx + Vy + b)```, where ```f``` is a non-linear activation function. ```W``` and ```V``` are the learned projection matrices for the different descriptions, and ```b``` is a learned bias vector. Then ```z' = g([W'h(z), V'h(z)] + b')``` is the reconstruction that we compare to ```z``` while learning reconstruction matrices ```W'```, ```V'```, and ```b'```. Function ```g``` is any activation function. Note that ```[a, b]``` is the concatenation of vectors ```a``` and ```b```.

   The sum is made of four parts. The first part, ```L(z_i, g(h(z_i)))```, teaches the autoencoder to reconstruct its input (both languages). The second and third terms train it to reconstruct the input from just one description. So, if we are training on French and English, it must learn to output the French language with just the English sentence. These three together imply that the model learns to embed the sentences in the same vector space. The fourth term maximizes the correlation between the vector embeddings of similar sentences across languages. This constrains the vectors in different languages to be close to each other. Pretty slick addition.

    An important note is that the inputs are sentence-level binary bags of words. This means that word order is not taken into effect and changing this to include word order would be a reasonable next thing to try.

    Overall, I really like this idea. It's simple but makes sense and the results suggest that it's working well for document classification. They are able to assemble deep models and can scale up the language representation with lots of monolingual data without compromising the model.

2. [Leveraging Monolingual Data for Crosslingual Compositional Word Representations](http://arxiv.org/abs/1412.6334)[^2]

   A couple of months before the above paper, this one achieved even better results on document classification with a different method and a slightly different objective.

   The goals are similar. Can a single method:

   - Contrain the word-level representations to be compositional?

   - Leverage both monolingual and bilingual data.

   - Scale to large vocabulary sizes without greatly impacting time.

   While very favorable towards the [Chandar et al] paper, they describe a deficiency:

   > [Chandar et al]'s method represents each sentence as a bag of words vector with the size of the whole vocabulary. This leads to computational scaling issues and necessitates a vocabulary cut-off which may hamper performance for compounding languages such as German.

   Accordingly, their neural network representing sentences as an addition composition over word vectors can be trained on seven million sentences in under six hours on a single core CPU. They did show a composition function using bigrams to somewhat consider word order, but left it as future work:

   > To increase the expressiveness of our method we plan to investigate more complex composition functions, possibly based on convolution to preserve word order information.

   I mentioned a different objective as well. Here it is:

   <br>

   $$\displaystyle L_{total} = \sum_{i=1}^{N_{bi}} L_{bi}(v_{i}^{l_1}, v_{i}^{l_2}) + \sum_{i=1}^{N_{mono_1}} L_{mono}(x_{i}^{l_1}) + \sum_{i=1}^{N_{mono_2}} L_{mono}(y_{i}^{l_2}) + \lambda||\theta||^2$$

   <br>

   The first term is the bilingual objective. It's calculated by taking aligned sentences, computing their vector representation with the composition function, and then minimizing the squared euclidean distance between them:

   <br>

   $$\displaystyle L_{bi}(v_i^{l_1},v_i^{l_2}) = ||v_{i}^{l_1} - v{i}^{l_2}||^2$$

   <br>

   The monolingual objective is very different. It's based on a notion that phrases are more similar to their sub-phrases than to randomly sampled phrases. For example, if the phrase is ```My friend went to the park after getting ready,``` then the sub-phrase ```went to the park``` is more relevant than ```took a shower.``` Given a large enough monolingual corpus, another plausible sub-phrase like ```went to the movies``` would also appear and be embedded close to the larger phrase. This would consequently embed it close to ```went to the park.``` Here is the loss function that achieves this:

   <br>

   $$\displaystyle L_{mono}(a)=[max(0, m + ||a_c^{outer} - a_c^{inner}||^2 - ||a_c^{outer} - b_c^{noise}||^2) + ||a_c^{outer} - a_c^{inner}||^2] \frac{len(a^{inner})}{len(a^{outer})}$$

   <br>

   Above, ```m``` is a margin, the ```outer a``` is a phrase sampled from a sentence, the ```inner a``` is a sub-phrase of the outer, and the ```noise b``` is a phrase extracted from a sentence uniformly sampled from the corpus. All extracted phrases have length larger than three.

3. [Skip-Thought Vectors](http://arxiv.org/pdf/1506.06726v1.pdf)[^3]

   This work was inspired by Mikolov et al's [paper](http://arxiv.org/pdf/1301.3781.pdf)[^4] in 2013. It abstracts the skip-gram model for word embeddings to the sentence-level. The result are generic sentence representations that perform robustly across a wide variety of tasks, including semantic relatedness, paraphrase detection, image-sentence ranking, and classification.

   How does it induce the vectors? Take a large corpus of contiguous sentences (e.g. books). For each consecutive sentence tuple ```(x, y, z)```, use an RNN with GRUs to encode the midde sentence, ```y```. Now use an RNN (with GRU) to decode this encoding and try to predict ```x```, the sentence before. Do the same thing with another decoder for predicting ```z```, the sentence after. These two decoders have separate weight matrices but share the weight matrix for the vocabulary.

   The objective function is the sum of the log-probabilities of the previous and next sentences conditioned on the encoder representation:

   <br>

   $$\displaystyle \sum_{t} \log (P(w_{i-1}^t | w_{i-1}^{\le t-1}, h_i)) + \sum_{t} \log (P(w_{i+1}^t | w_{i+1}^{\le t-1}, h_i))$$

   <br>

   Another nice takeaway from this paper was that they were able to expand the vocabulary for the language model from 20k words to 930k words using pre-trained word2vec representations. This was done by first developing the RNN as above and then finding a linear mapping matrix ```W``` where ```v' ~ Wv``` for ```v``` in the word2vec vocabulary and ```v'``` in the RNN vocabulary. Now any word represented in word2vec can also be mapped to the RNN.


<br><br><br>

[^1]: Chandar et al, University of Montreal
[^2]: Soyer et al, National Institute of Informatics, Tokyo, Japan
[^3]: Kiros et al, University of Toronto
[^4]: Google, Efficient Estimation of Word Representations in Vector Space

{% include katex_render.html %}
