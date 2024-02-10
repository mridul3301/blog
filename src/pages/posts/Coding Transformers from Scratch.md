---
description: 07-01-2024
public: true
layout: ../../layouts/BlogPost.astro
title: Coding Transformers from scratch 
createdAt: 16894161365
updatedAt: 16894171365
tags:
  - NLP

heroImage: /posts/attention_mathematics/bg.png
slug: pro-display-xdr
---

<script>
  function toggleZoom(imageElement) {
    if (imageElement.style.transform === 'scale(1.5)') {
      imageElement.style.transform = 'scale(1)';
      imageElement.style.cursor = 'zoom-in';
    } else {
      imageElement.style.transform = 'scale(1.5)';
      imageElement.style.cursor = 'zoom-out';
    }
  }

  document.addEventListener('DOMContentLoaded', function() {
    const imageLinks = document.querySelectorAll('a img');

    imageLinks.forEach((link) => {
      link.addEventListener('click', function(event) {
        event.preventDefault();
        toggleZoom(this);
      });

      link.addEventListener('mouseenter', function() {
        if (this.style.transform === 'scale(1.5)') {
          this.style.cursor = 'zoom-out';
        } else {
          this.style.cursor = 'zoom-in';
        }
      });

      link.addEventListener('mouseleave', function() {
        this.style.cursor = 'default';
      });
    });
  });
</script>


**<span style="text-decoration:underline; font-size: 24px">Introduction</span>**<br>

In this blog post, we will discuss how to implement vanilla Transformers from scratch paper using ***pytorch***.
If you have not seen my detailed blog post on Transformers, you can go and check <a href="https://blogs.mridulsharma.com.np/posts/Attention" target="_blank" style="text-decoration: none; color:skyblue">here</a><br>

Let's start by importing necessary modules.<br>

``` python

  # Import libraries
  import torch
  import torch.nn as nn
  import math
  
  ```
<br>

**<span style="text-decoration:underline;font-size: 24px">Input Embeddings</span>**<br>

Embeddings are just vector representation of words. So, Input Embedding just means the vector representation of inputs. i.e mapping between words and vectors.<br>

According to paper *Attention is all you need*, the embedding dimension is ***512***.<br>
Meaning that each word is represented using a vector of dimension ***(1, 512)***.<br>
Let's gooo:

First of all, we need two things :<br>

- ***d_model*** : Dimension of the model (Embedding Dimension), which is basically 512 in our case.
- ***vocab_size*** : Size of our dictionary.
<br>
<br>
Now, torch already provides us with a class for generating word embeddings. i.e `nn.Embedding`.
<br>
<br>

[![MLE](/posts/attention_mathematics/Embedding.png)](javascript:void(0);)

<br>
Also, in the paper, we can see that embeddings are multiplied by the square root of `d_model`.<br>
<br>

[![MLE](/posts/attention_mathematics/paper_exp_emb.png)](javascript:void(0);)

<br>
Let's code it :
<br>
<br>

``` python

  class InputEmbeddings(nn.Module):

    def __init__(self, d_model:int, vocab_size:int):
        super().__init__()
        self.d_model = d_model
        self.vocab_size = vocab_size
        self.embedding = nn.Embedding(vocab_size, d_model)

    def forward(self, x):
        return self.embedding(x) * math.sqrt(self.d_model)
  
  ```
<br>
The layer to generate input embedding is ready. Let's move on to Positional Encoding.
<br>
<br>

**<span style="text-decoration:underline;font-size: 24px">Positional Encoding</span>**<br>

Since we’re dealing with natural language, the positions and order of the words are extremely important as sentences
follow grammatical rules and different order of the same words can give different meanings. In transformer models,
each word in a sentence flows through the encoder/decoder stack simultaneously and the model itself doesn’t have
any sense of position/order for each word. Therefore, there’s a need to incorporate the order of the words into the model.
<br>

For this purpose, we use positional encodings.

For Positional Encodings, we need following :

- ***d_model*** : Dimension of the model (Embedding Dimension), which is basically 512 in our case.
- ***seq_len*** : Sequence length of input, as we need to create a vector for input of each position.
- ***dropout*** : It's just hyperparameter to prevent overfitting.
<br>
<br>
For dropout also, we have got built in class in torch.
<br>
<br>

[![MLE](/posts/attention_mathematics/dropout.png)](javascript:void(0);)

<br>

We also need to build a matrix of ***seq_len x d_model*** because we need vectors of size ***d_model*** for our input of ***seq_len*** input length.

Then, we build our positional encodings using the specified equations in the paper.
<br>

[![positional-encoding](/posts/attention_images/positional-encoding.png)](javascript:void(0);)

<br>