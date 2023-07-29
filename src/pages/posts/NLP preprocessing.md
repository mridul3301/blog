---
description: 8th June, 2023
public: true
layout: ../../layouts/BlogPost.astro
title: Pre-processing for NLP
createdAt: 1
updatedAt: 1
tags:
  - DL
  - NLP
heroImage: /posts/nlp_prep/cover.png
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

NLP project, just like any other ML project has a simple pipeline which starts with data collection followed by cleaning of data, pre-processing, model building and production.

In this blog post, we will look at the pre-processing step, which holds utmost significance in NLP tasks as it involves transforming raw data into a format that is suitable for modeling, marking the beginning-to-end journey of data preparation.



[![NLP_Pipeline](/posts/nlp_prep/nlp-pipeline.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Simple NLP pipeline</b></i>
</div>

<br>

Pre-processing the natural language might look very simple on first glance but there are many levels to it & research are being done continuously on large scale to find the better way to preprocess.

Let’s see how pre-processing is done for NLP tasks.

To begin our NLP project, we undertake the crucial step of preprocessing the vast amount of unstructured data, which we refer to as the **Corpus**. Let's say our data primarily consists of text and requires careful preparation before being used in our NLP tasks. And one of the most basic preprocessing method to start is **Tokenization**.

**<span style="text-decoration:underline;font-size: 24px">Tokenization</span>**

Tokenization is the process of breaking down the large text data into smaller pieces known as *tokens*. Assume that we have a text with 1000 words, we first divide the text  into sentences and then sentences to tokens(words in this case). 

[![Tokenization](/posts/nlp_prep/tokenization.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Tokenization</b></i>
</div>
<br>

Well, this sounds simple, but why do we even tokenize the corpus?<br>
Tokenization can be really helpful for tasks such as text segmentation, vocabulary creation, feature extraction, text normalization & language understanding. How tokenization assist the mentioned works might sound unclear right now but as we will be clear by the end of this blog.<br><br>
In English language, tokens are basically word but punctuations, numbers are also considered as tokens. Let's try tokenizing english text:<br><br>

``` python

    # Import spacy library (!pip install -U spacy if not installed)
    import spacy
    
    # Download statistical model for English
    !python -m spacy download en_core_web_sm

    # Load the downloaded model
    nlp = spacy.load('en_core_web_sm')

    # Sample text
    sample_text = "A quick brown fox forgets to jump."

    # Generating tokens with the help of en_core_web_sm model
    tokens = nlp(s)

    # Iterate over tokens
    print([t.text for t in tokens])


```
<br>
Output of the code above will be :<br><br>

[![Tokenization Output](/posts/nlp_prep/first_output.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Tokenization Output</b></i>
</div>
<br>
Still, this looks very simple but let's think about it. There are hundereds of language around the world, each with their own rule, punctuation styles, acronyms, abberviated forms etc. Again, we're dividing text to generate tokens on the basis of words & the words are meant to be the single smallest fundamental part of a language that conveys some information/carries some meaning or not. But, how is word defined? <br><br>
For this let's understand few concepts:<br>

**Morphemes** are unit of text that can have meaning but wont exist independently. For Example: pre-, post-, a-, un- ......etc.<br> 
**Graphemes** are basically even fundamental entity known as letters.<br>
We can have NLP models operating on both words or characters. What to use for our model depends of the problem we have and we can always experiment. Up to now, we are probably clear that tokenization should be done right and it plays important roles in NLP.<br>

[![Case Folding](/posts/nlp_prep/case_folding_two.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Problem with tokenization based on words</b></i>
</div>
<br>
Here, we see that "central", "processing", "unit" & "cpu" are 4 different tokens but they are just same thing used twice.

The next pre-processing step is **Case Folding**.

**<span style="text-decoration:underline;font-size: 24px">Case Folding</span>**

Case Folding is just representing the text we have either in all lowercase or uppercase. But what is the reason behind case folding?<br><br>
Just looking at it doesn't seem a big things but case folding does have serious advantage. **i.e.** The number of words in our vocabulary will decrease, same words written in different cases will be recognized as same word & this will help in faster processing.<br>
But it has it's own problems because the information we are able to retrieve decreases & can hamper the performance and efficiency. It also has problems with acronyms and abbreviated forms.<br>
In order to solve this problem, multiple search engines use rules behind the scenes. For example : Skip the case folding for aronyms.

[![Case Folding](/posts/nlp_prep/case_folding.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: No Case Folding vs Case Folding</b></i>
</div>
<br>
The vocabulary with no case folding have 31 tokens while the one with case folding have 27 and in larger data, the difference can be significant.
<br>
<br>

[![Case Folding](/posts/nlp_prep/case_folding_three.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Problem with case folding</b></i>
</div>
<br>
In this example, we can see that "Ram" & "RAM" are given same token and this can result in misleading models. 
<br>
<br>

**<span style="text-decoration:underline;font-size: 24px">Stop Words Removal</span>**

Stop words are those words that are considered to have little or no significant meaning or impact on the overall understanding of the text. Removing stop words helps reduce the dimensionality of the data and can improve the efficiency of NLP algorithms and models. Examples of stop words in English include: *"a", "an", "the", "but", "I", "we", "from"* etc. Consider the sentence: "The quick brown fox jumps over the lazy dog." If we remove the stop words, the sentence would become: "quick brown fox jumps lazy dog."


``` python

    # Import spacy library (!pip install -U spacy if not installed)
    import spacy
    
    # Download statistical model for English
    !python -m spacy download en_core_web_sm

    # Load the downloaded model
    nlp = spacy.load('en_core_web_sm')

    # Print the stop words in this model
    print(f" Stop words are : {nlp.Defaults.stop_words}")

    # Total stop words
    print(f" Total stop words : {len(nlp.Defaults.stop_words)}")

    # Sample text
    sample_text = "A quick brown fox forgets to jump."

    # Generating tokens with the help of en_core_web_sm model
    tokens = nlp(s)

    # Iterate over tokens
    print([t for t in tokens if not t.is_stop])


```
<br>
Output:<br><br>

[![Case Folding](/posts/nlp_prep/stop_words.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Tokens after removing stop words</b></i>
</div>
<br>

But we should not use stop words in every project. Let's understand it using example:<br>

[![Case Folding](/posts/nlp_prep/stop_words_two.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Problem with removing stop words</b></i>
</div>
<br>

In the example above, we can see that removing stop words worked great for first text but does a terrible job and misinterprets second text. So, removing stop words depends on the problem we're solving.

**<span style="text-decoration:underline;font-size: 24px">Stemming</span>**

Stemming is a natural language processing technique used to reduce words to their base or root form, known as the "stem." The goal of stemming is to simplify word variations and improve text processing efficiency and information retrieval in applications like search engines, text mining, and information retrieval systems.

Stemming helps the words with same stem but different forms to act as same and can help dealing with words that are not in vocabulary.<br><br>
For Example:<br>


[![stemming](/posts/nlp_prep/stemming.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Stemming Example</b></i>
</div>
<br>

Stemming can also lead to overstemming & understemming which affects the precision and performance of model.

**<span style="text-decoration:underline;font-size: 24px">Lemmatization</span>**

Lemmatization is a more sophisticated alternative to stemming and it is a technique to reduce words to their lemma/dictionary form. It is more accurate than stemmers because lemmatizers takes into account whether a word is noun, verb, adjective etc. before performing stemming. Because of this, it is preferred over stemmers.<br>

[![lemmatization](/posts/nlp_prep/lemmatization.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Difference between stemming and lemmatization</b></i>
</div>
<br>

In the figure above, we can see that lemmatizer identified the name *walker* and kept it as it is while stemmers changed it into walk.<br>
<br>

### I'm still working on it and will soon publish complete version