---
description: 18-02-2023
public: true
layout: ../../layouts/BlogPost.astro
title: Eigenvalues and Eigenvectors
createdAt: 0
updatedAt: 0
tags:
  - Mathematics
heroImage: /posts/eigen/cover.png
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


**<span style="text-decoration:underline; font-size: 24px">Introduction to Vector, Basis & Span</span>**

First concept we need to understand is vectors. Vectors are a simple way to represent data. 
How do they represent data? Let’s see with an example below:

<br>

[![ev](/posts/eigen/vector_1.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Example -Vectors</b></i>
</div>

<br>

Now, Let’s look at how they are represented in coordinate axes.
<br>

[![ev](/posts/eigen/vector_2.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Vectors in coordinate space</b></i>
</div>

<br>
Looking at this, we can easily interpret what the vector A means. Generally, it's 150 units on the x-axis and 52 units on the y-axis.<br><br>
But we can try to look at it in a different manner. Let’s look at it in terms of basis vectors. So, basis vectors are the vectors that can be used in scaled combination with scalars in order to represent the entire coordinate axes. For xy coordinate space, we have î (called i hat) and ĵ (called j hat).  î is the unit vector is x direction whereas ĵ is the unit vector in y direction. They are the standard basis vectors for the xy coordinate system.

<br>

Let’s look at how basis vectors are represented in coordinate axes.
<br>

[![ev](/posts/eigen/vector_3.png)](javascript:void(0);)

<div style="text-align: center;">
  <i><b>Fig: Basis Vectors i & j</b></i>
</div>

<br>
Now, Let’s consider a simple vector to understand it better.
<br>

[![ev](/posts/eigen/vector_4.png)](javascript:void(0);)

<br>
We can see that the vector at point (2, 3) can be represented using 2 basis components of x vector and 3 basis components of y vector.

<br>
<br>

[![ev](/posts/eigen/vector_5.png)](javascript:void(0);)

<br>
Now, we slide one of the basis components at the end of another to reach the point (2, 3).<br>
We can move this way:
<br>
<br>

[![ev](/posts/eigen/vector_6.png)](javascript:void(0);)

<br>
or we can move this way:
<br>
<br>

[![ev](/posts/eigen/vector_7.png)](javascript:void(0);)

Both way we can reach the point (2, 3) with the help of basis vector.<br><br>


Observing the above example, it can be said that x-component of the vector is scaled extension of î and y_component of the vector is scaled component of  ĵ. Thus, we can say that vector “a” is a scaled sum of two basis vectors. Also, We can represent any point in the coordinate axes using these scaled  combinations. Fixing a basis vector and scaling another will give us a line as shown in the figure below.

<br>

[![ev](/posts/eigen/vector_8.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig: Scaling î while keeping ĵ constant.</b></i>
</div>
<br>

<br>

[![ev](/posts/eigen/vector_9.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig : Scaling ĵ while keeping î constant.</b></i>
</div>
<br>

This way of scaled combination is called *Linear Combination*.
Actually, we could have used other vectors as our basis vector within the plane and the result would be the same i.e. can be scaled in combination to represent any points in the coordinate space. But for some basis vectors i.e. the ones lying in the same line, total representation space is just a line.
<br><br>
And the space that can be represented with the help of two basis vectors is known as span. So, for a basis vector to have a span of the entire coordinate space, they should be linearly independent.Below are some examples of basis vectors whose span is the entire coordinate space.
<br>

[![ev](/posts/eigen/vector_10.png)](javascript:void(0);)
<br>
Below is a linearly dependent vector. It does not matter how much we try to scale it, it won’t represent anything other than a single line on which it lies.
<br>

[![ev](/posts/eigen/vector_11.png)](javascript:void(0);)
<br>

**<span style="text-decoration:underline; font-size: 24px">Linear Transformation</span>**

Linear Transformation is a mapping between two vector spaces that respects vector addition and scalar multiplication. They are ways to move around space such that the grids between each unit remain parallel and evenly spaced among both axes.
<br>

[![ev](/posts/eigen/vector_12.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig : Example - Linear Transformation</b></i>
</div>
<br>

That is a straightforward thing where one vector is transformed into another. But what does it really mean?? Again, it can be viewed in terms of basis as shown in the figure below.
<br>

[![ev](/posts/eigen/vector_13.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig : Linear Transformation in terms of basis vectors</b></i>
</div>
<br>

So, matrix is just a way to package the basis transformation. Let’s say we have some transformation to do on a vector. We can transform the basis of that vector and calculate the transformation. Let’s see another example.
<br>

[![ev](/posts/eigen/vector_14.png)](javascript:void(0);)
<div style="text-align: center;">
  <i><b>Fig : Example - Linear Transformation in terms of basis vectors</b></i>
</div>
<br>

After transforming the basis, we are now able to transform the original vector. We now know about transformation and can transform vectors easily but what about the scale of transformation? By what factor was the area of the original vector increased or decreased? Here comes the concept of determinant.