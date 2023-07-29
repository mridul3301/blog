---
description: 9th July, 2023
public: true
layout: ../../layouts/BlogPost.astro
title: Pre-Processing for NLP
createdAt: 1
updatedAt: 1
tags:
  - Deep Learning
  - Paper Explainer
  - Neural Translation
  - Transformers
heroImage: /posts/attention_images/tnattention.png
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


**<span style="text-decoration:underline; font-size: 24px"> Introduction</span>**

This blog attemps to provide the information about the operation that happens inside Vanilla Transformers as mentioned in "Attention is all you need" paper. **i.e.** computations inside the transformer.

This paper introduced the concept of Transformer model architecture, which has become a foundational model in NLP tasks. The most fundamental concept in transformer architecture is the**_ self-attention _**mechanism. On surface level, **_self-attention_** is just another sequence-to-sequence operation i.e. It takes sequence as input and return sequence as output. But it is really powerful because of its ability to perform parallel computation and preserve long-term dependencies.



[![transformer_architecture](https://raw.githubusercontent.com/mridul3301/blog/main/public/posts/attention_images/real-arechitecture.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Transformer Architecture</b></i>
</div>

<br>

The above image shows transformer architecture with encoder and decoder. The original paper was introduced for neural machine translation task.

Now, Let’s try to understand the Transformer.

**<span style="text-decoration:underline;font-size: 24px">Word Encoding/Representation</span>**

At first, each input word is represented/encoded into some vectors. All the encoded vectors are of the same shape. And since the transformer model takes a fixed length sequence as input, the smaller length sequence is increased to that fixed length by padding empty strings just like in the diagram below.



[![word_representation](https://raw.githubusercontent.com/mridul3301/blog/main/public/posts/attention_images/word_representation.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Word Representation</b></i>
</div>
<br>

**<span style="text-decoration:underline;font-size: 24px">Word Embedding</span>**

Computers are unable to understand words. So, we need to represent the words as dense, low–dimensional vectors in a continuous vector space. The main idea of word embeddings is to capture the semantic and syntactic relationship between words. The embeddings are learned from large amounts of text data using unsupervised machine learning techniques. For Example : We take a paragraph, mask some portion of that paragraph, force the model to predict the masked part and repeat it multiple times. As a side effect of this, we are able to capture meaning and relationship between words by representing the word as an embedding vector. 

**<span style="text-decoration:underline;font-size: 24px">Input Embedding</span>**

Now, we pass our word representations to the word embeddings in a feed forward layer and obtain the embeddings for our input. The dimension of embeddings is described to be 512 in the original paper.






[![word_embedding](https://raw.githubusercontent.com/mridul3301/blog/main/public/posts/attention_images/word_embedding.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Input Embedding</b></i>
</div>
<br>


Now, we have generated input embeddings. Next step is to add the input embeddings with positional encodings.



[![pos-enc](/posts/attention_images/pos-enc.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Adding positional encodings on input embeddings</b></i>
</div>
<br>


**<span style="text-decoration:underline;font-size: 24px">Positional Encodings</span>**

Since we're dealing with natural language, the positions and order of the words are extremely important as sentences follow grammatical rules and different order of the same words can give different meanings. In transformer models, each word in a sentence flows through the encoder/decoder stack simultaneously and the model itself doesn't have any sense of position/order for each word. Therefore, there's a need to incorporate the order of the words into the model.

But in the vanilla Transformers, we use positional encodings.

Even if both the positional embeddings and positional encodings are for the same purpose, we need to understand that positional encodings are derived using some equations while positional embeddings are learned.

Positional embeddings are used as a query for masked prediction, while positional encodings are added before the first MHSA block model.

Equations to derive positional encodings for vanilla transformers are:



[![positional-encoding](https://raw.githubusercontent.com/mridul3301/blog/main/public/posts/attention_images/positional-encoding.png)](javascript:void(0);)



After we’ve calculated the positional encoding, it’s time to add it with the input embeddings to preserve the position of words.

[![we-pe](https://raw.githubusercontent.com/mridul3301/blog/main/public/posts/attention_images/we_pe.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Positional Encoding + Word Embedding</b></i>
</div>
<br>

Now, we've processed the input sequence and the next step is to pass he input to encoder. But at first, Let's try to understand what encoder block really is?<br>
<br>


**<span style="text-decoration:underline;font-size: 24px">Encoder Block</span>**

[![Encoder Block](../../../posts/attention_images/encoder_block.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Encoder Block</b></i>
</div>
<br>

Firstly, we see our input is passed with 3 arrows to the Multi-Head Attention Block. The ***Multi-Head*** in the block means the parallel computation **i.e.** Inputs are processed and computation occurs parallely which increases the speed. Another reason for using Multi-Head attention is to allow the network to model all the different relations in single attention operation, and multi-head basically means attention operation applied in parallel.<br><br>
But what is attention?<br>
Attention operation is a method that includes the computation where each word is assigned attention score that tells the model about what to focus and what not to focus in order to understand the meaning of sequence. In the vanilla transformers, we use self-attention mechanism. So, Let's learn more about it.<br>

**<span style="text-decoration:underline;font-size: 18px">Self-Attention</span>**

Let's assume a sequence "In Bayesian Inference, We update the prior probability of model using the new data." We can easily understand the sentence because we know the meanings and the rules of grammar. But computers have absolutely no idea about what is this and what it means. The self attention mechanism will process each word, observes all the position of tokens in the sequence and create a vector trying to make sense of the sequence. Basically, it generates a vector based on dependency of words and understanding the context.<br><br>

The self-attention operation starts with the inputs. Each input is representation of a single word in a sequence. For each input, we generate three different representations known as ***key, query & value***. In order to generate these three entities, we multiply our input with some radomly initialized matrix weights for all of them. Randomly initialized weights are different for all key, query & value.
<br><br>
[![key-query-value](../../../posts/attention_images/kqv.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Calculating Key, Query & Value for inputs</b></i>
</div>
<br>

In self-attention mechanism, The dimension of key and query matrix are same **i.e.** 64 (mentioned in the paper)
For attention operation, we multiply query and key are multiplied to obtain a single number which is again multiplied with value vector. The figure below shows how attention operation is performed.<br><br>
[![Attention for First input](../../../posts/attention_images/attention_operation.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Attention Operation for First Input</b></i>
</div>
<br>

The figure above shows the attention score for first input. Now, Let's see how it is done for second input.

[![Attention for Second input](../../../posts/attention_images/attention_for_second_input.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Attention Operation for Second Input</b></i>
</div>
<br>

This is similar for rest of the inputs. This is the way self-attention is calculated. The attention operation we performed is called ***Scaled Dot-Product Attention***. But, in the paper, ***Multi-Head Attention*** is used. We can see this figure from the original paper to understand the difference betwene the two.

[![Scaled dot-product attention vs Multi Head Attention](../../../posts/attention_images/sdp_vs_mha.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Scaled Dot-Product Attention vs Multi-Head Attention</b></i>
</div>
<br>

**<span style="text-decoration:underline;font-size: 18px">Residual Connection and Layer Normalization</span>**

After the attention operation, residual connection is added (i.e. Input to the Mult-Head Attention block and it's output are added.) and then we perform layer normalization.

[![Residual Addition and Layer Normalization](../../../posts/attention_images/res_nor.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Residual connection and layer normalization</b></i>
</div>
<br>

The purpose of the residual connection in the multi-head self-attention mechanism of a Transformer is to facilitate gradient flow during training and improve the overall learning capability of the model. In the context of multi-head self-attention, the residual connection is applied to the output of the self-attention module before it is passed through subsequent layers, such as feed-forward neural networks or additional self-attention layers. The residual connection allows the model to retain the original information from the input and combine it with the learned representation from the self-attention module. And the purpose of this normalization step is to ensure that the inputs to subsequent layers are consistent and within a similar range, which can help improve the overall performance and convergence of the model.


**<span style="text-decoration:underline;font-size: 18px">Feed Forward and Normalization</span>**

Now, the normalized output is passed through a feed forward layer (Multi-Layer perceptron) with another residual connection followed by layer normalization.

[![Residual Addition and Layer Normalization](../../../posts/attention_images/mlp_nor.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: MLP and layer normalization</b></i>
</div>
<br>

We have completed the encoder block. So, let's move on to decoder block. Decoder block is quiet similar to Encoder except it has one extra layer called ***Masked Multi-Head Attention***. But rest of decoder is exactly the same.

**<span style="text-decoration:underline;font-size: 24px">Decoder Block</span>**

[![Residual Addition and Layer Normalization](../../../posts/attention_images/decoder_block.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Decoder Block</b></i>
</div>
<br>

In the block diagram of decoder block above, we can see everything is same as Encoder except masked Multi-Head Self-Attention. Let's understand it all.<br>
<br>
Similar to the encoder block, decoder block starts with Output Embedding and Positional Encodings. The process of generating output embedding and positional encoding is exactly same as generating input embedding and positional encoding for the Encoder Block. After generating, output embedding and positional encodings, we add them together and pass the result to masked multi-head self-attention. Before passing, we generate three different representations ***i.e. key, query & value***. The process of generating these vectors is to multiply the input to the masked MHSA block by some randomly initialized matrix weights for all of them. We studied Multi-Head Self-Attention mechanism, But, what is Masked Multi-Head Self-Attention, why is masking done??<br><br>
Basically, masking is done to achieve parallelism while training. It is used to prevent the attention mechanism from looking at future tokens during the encoding process. For instance, when predicting the third word in a sentence, we should not allow the model to see the fourth or subsequent words, as that would violate the sequential nature of language. Therefore, the model masks out (ignores) the future positions during the attention calculation.<br><br>

[![Decoding operation](../../../posts/attention_images/masking.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Masked Decoding process</b></i>
</div>
<br>

This is where masking becomes necessary. The training algorithm knows the entire expected output but it hides/masks some portion of output.
<li>When it executes first operation - it hides (masks) the entire output.</li>
<li>When it executes second operation - it hides 2nd,3rd & 4th outputs.</li>
<li>When it executes third operation - it hides 3rd & 4th output.</li>
<li>When it executes fourth operation - it hides 4th output.</li>

<br>

After the masked multi-head self-attention, we make residual connections and perform layer normalization. The output of this layer is used as **value** for next MHSA block. The output from the encoder block is used to generate two representations **key & query**. Now, we have all key, query and value for the MHSA block. So, we perform self-attention operation again followed by residual connection and layer normalization.<br><br>
The output is further passed to a feed forward layer again followed by residual connection and layer normalization. These are all the operation inside decoder block.<br><br>
The output from decoder block is further passed to a linear layer and softmax layer and the word with highest probability is selected.<br><br>
[![Decoding operation](../../../posts/attention_images/final.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Final Operation</b></i>
</div>
<br>

This is just simple overview of how things work inside transformers.

### I'm still working on it and will soon publish complete version