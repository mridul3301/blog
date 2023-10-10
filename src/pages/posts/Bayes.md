---
description: 25-08-2023
public: true
layout: ../../layouts/BlogPost.astro
title: Bayesian Statistics
createdAt: 1695921769
updatedAt: 1695921769
tags:
  - Mathematics
  - Probability

heroImage: /posts/bayes_images/cover.png
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

**<span style="text-decoration:underline; font-size: 24px">Introduction</span>**

Before diving into the bayesian concept, let's first understand what does it mean when something has some probability of occurring??<br>

We can understand it in two different manner : Frequentist & Bayesian.

For example: When considering the probability of rolling a four on a fair six-sided die, we can think of it in an objective manner . If we were to roll the die an infinite number of times, take random samples, and calculate the proportion of fours observed, it would tend towards one-sixth. This means that, in the long run, the proportion of the outcome we're interested in converges to one-sixth. So, one way to explain what is the probability of an outcome in terms of frequencies is if the event were to happen infinitely many times, what would be the proportion of outcome we’re interested in. This is simple, straightforward and sounds correct because frequentist approach reflects objective reality.

Another point to consider is the subjective approach, in which people have their own reasons for believing in a particular probability. This subjective viewpoint, which differs from the seemingly more accurate but rigidly objective viewpoint, serves as the foundation for our Bayesian model. It is not subjective in the sense of arbitrary beliefs; rather, it recognizes that various people may respond differently to the same inquiry based on experiences.

While the frequentist approach works well when picturing an endless number of dice rolls, it becomes difficult to apply it to unique one-time occurrences such as F1 Race because racing does not have a finite number of outcomes. For example, calculating the likelihood of Hamilton winning the F1 at Losail circuit requires insights that the frequentist model may not readily supply. Using a frequentist model would require imagining an infinite sequence of races, each with a different outcome, sample it and calculate the proportion of desired outcome and all of that without any background knowledge. Now, we are starting to see the flaws of the frequentist approach.

Let’s look at an example on how the bayesian approach can be subjective and true:
<br>

[![bayes](/posts/bayes_images/bayes_1.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Example - Subjective Reality (but still true) </b></i>
</div>

<br>

Bayesian approach is said to be subjective in the way mentioned below. In the example above, all persons A, B & C are correct about the disease. It’s just they predicted based on the knowledge and the prior information available to them. But, A  cannot say that the probability is 90% with just the information he has. The probability only gets updated after we add more and more information. The degree of belief helps form the probabilistic base of everything.


**<span style="text-decoration:underline; font-size: 24px">Conditional Probability</span>**

In order to get complete intuition behind the bayesian statistics, we need to understand conditional probability. Let’s say that event A and event B are related to each other. Conditional Probability is the probability of a certain outcome from event B, given that event A has already happened. Conditional probability can be expressed mathematically in following way:
<br>

[![bayes](/posts/bayes_images/bayes_2.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Conditional Probability </b></i>
</div>

<br>