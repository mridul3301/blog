---
description : 16th July, 2023
public: true
layout: ../../layouts/BlogPost.astro
title : Maximum Likelihood Estimate
createdAt: 1679637678900

updatedAt: 1679639349000
tags:
  - Mathematics
heroImage: /posts/mle_images/mle.png
slug: pro-display-xdr
---

**<span style="text-decoration:underline; font-size: 24px"> Introduction</span>**

The purpose of Maximum Likelihood Estimate is to use the data we have to make the best possible guess about something we don't know. It's like looking at the evidence and making an educated guess based on what we see. More technically, The central idea behind MLE is to select that parameters that make the observed data the most
likely.
<br><br>
Let's understand it using a simple example:<br><br>
Consider a dataset of observations that are normally distributed. For this, we would like to estimate the mean and standard deviation of this distribution. By maximizing the likelihood function based on the observed data, we can obtain the MLE estimates for the mean and standard deviation.<br><br>

Let's look at another example:<br>
Consider a normally distributed data.<br>
In this example, let's take heights of students of a class as our distribution.<br>

``` python

  ## Code to generate a normal distribution data
  import numpy as np
  import matplotlib.pyplot as plt

  # Generate normally distributed data
  np.random.seed(42)  # Set a seed for reproducibility
  mean_height = 170
  std_height = 5
  num_students = 100
  heights = np.random.normal(mean_height, std_height, num_students)

  # Plot the distribution
  plt.hist(heights, bins=20, density=True, color='skyblue', edgecolor='black')

  # Add labels and title
  plt.xlabel('Height')
  plt.ylabel('Density')
  plt.title('Height Distribution of Students')

  # Show the plot
  plt.show()


  ```
<br>

![normal distribution](../../../posts/mle_images/normal-distribution.png)

<br>
Normally distributed always means few things:

  <span>  &nbsp;&nbsp;&nbsp;&nbsp;1. Most of the measurements are expected to be close to mean.</span><br>
  <span>  &nbsp;&nbsp;&nbsp;&nbsp;2. Measurements are expected to be relatively symmetrical around the mean.</span><br><br>

  Now, Let's plot the same data with a distribution plot and showing the average line.

``` python

    import numpy as np
    import matplotlib.pyplot as plt

    # Generate normally distributed data
    np.random.seed(42)  # Set a seed for reproducibility
    mean_height = 170
    std_height = 5
    num_students = 100
    heights = np.random.normal(mean_height, std_height, num_students)

    # Calculate the average height
    average_height = np.mean(heights)

    # Plot the distribution
    plt.hist(heights, bins=20, density=True, color='skyblue', edgecolor='black')

    # Add labels and title
    plt.xlabel('Height')
    plt.ylabel('Density')
    plt.title('Height Distribution of Students')

    # Add a vertical line for the average height
    plt.axvline(x=average_height, color='red', linestyle='--',
                label='Average Height')

    # Add a normal curve
    x = np.linspace(min(heights), max(heights), 100)
    y = (1 / (std_height * np.sqrt(2 * np.pi))) * \
        np.exp(-(x - mean_height)**2 / (2 * std_height**2))
    plt.plot(x, y, color='red', linewidth=2)

    # Show the plot
    plt.show()


```
![normal distribution](../../../posts/mle_images/3.png)

### I'm still working on it and will soon publish complete version


