# Diagnosing and mitigating bias in word embeddings

Based on the tutorial https://learn.responsibly.ai/word-embedding
and its [notebook](https://colab.research.google.com/github/ResponsiblyAI/word-embedding/blob/main/tutorial-bias-word-embedding.ipynb).

Powered by [`responsibly`](https://docs.responsibly.ai/) - Toolkit for auditing and mitigating bias of machine learning systems.

This is a shortened version of the original tutorial. For more extensive context, references and explanations, refer to https://learn.responsibly.ai/.


## Disclaimer

Aiming for simplicity in the explanation, the concept of gender is oversimplified, treated as binary. Nevertheless, gender is a complex social construct. This should kept in mind when we go back from a learning context to the real-world.


## Overview

In this 45 minute tutorial, we will explore bias in **word embeddings** - a widespread building block of many machine learning models that work with human languages. 

A word embedding is an n-dimensional space where words are represented as vectors. In such space, words with similar behavior in text are close to each other. Following the **distributional hypothesis** (Harris 1954), behavior in text is often used as a proxy for the meaning of words. 

The dimensions in this particular space have been found  by a neural network, processing huge amounts of raw text data, such as news, wikipedia articles, reddit posts, tweets, etc. However, the resulting space is not intepretable at human level, since the resulting dimensions display no meaning that can be understood using common knowledge or common sense.

Nevertheles, word embeddings have an easy-to-explain geometrical representation that allows an intuitive understanding of this building block and its potential biases. 

Bias can affect any aspect of human judgement. To illustrate the phenomenon and how to address it in word embeddings, we focus on **gender** bias. We use an oversimplified representation of gender, considering only binary gender. This simplification is aimed to improve understanding of the techniques, and needs to be specifically worked out for actual deployment in applications. 

## What we will be doing in this tutorial: 

1. **Define:** We have decided that we wish to work on gender bias, but gender bias is a very complex phenomena. First of all, we will **narrow down** our definition to be able to apply the available techniques. 
We will be working specifically on gender bias with respect to *occupations*.  

2. **Diagnose:** Determine whether our embedding model has observable bias according to our definition based on occupations.

3. **Mitigate:** Once we've observed some bias in our embedding, we will apply some techniques to reduce the effect of bias in the embedding.

In order to do that, we use the techniques developed by [Bolukbasi et all](https://arxiv.org/pdf/1607.06520.pdf) (2016). They consist in: 

1. **Identifying the gender sub-space within the embedding space**: we will be characterizing a direction, or in a more general sense a sub-space, in which we can distinguish the components of the gender bias. 

2. In order to mitigate the bias but also be able to mantain the desirable and useful characteristics of the embedding, there are 2 techniques that the authors presents and are available in ```responsibly```:

  2.1. **Neutralize**: given a *set of words that should be unbiased*, in our case professions that should be gender neutral, this technique ensures that these words are in the middle of the previously identified subspace.

  2.2 **Equalize**: this technique works on *words* that should be neutral regarding to sets of words that represent a concept in different gender directions, for example {grandmother, grandfather}. After the equalization, the words that we consider should be neutral regarding gender (or the particular bias) will be equidistant to all the words of the set.

In this short version we will only be working on neutralization.

In all these techniques we refer to *sets of words* that are relevant to our bias analysis. These sets of words are a **key component** in all of the techniques we will be working on this workshop, and should be carefully elaborated for each particular use case.

We encourage you to work beyond the gender example and try to model forms of bias that are relevant for your community. Try to formalize them as binary concepts and find words that should be gender neutral!
