---
description: 07-01-2024
public: true
layout: ../../layouts/BlogPost.astro
title: Why Attention mechanism works? 
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


**<span style="text-decoration:underline; font-size: 24px">Introduction</span>**
