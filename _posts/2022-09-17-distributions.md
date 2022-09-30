---
layout: post
title: "Distributions"
description: "This post shows distributions and how to plot them."
feature-img: "assets/img/rainbow.jpg"
thumbnail: "assets/img/cradle_mountain.jpg"
tags: [Data Visualization, Python, Statistics]
---

Distributions are super important. In this post I'll talk about some common distributions, how to plot them, and what they can be used for.

<b>Table of Contents</b>
* TOC
{:toc}

Distributions can be categorized as discrete or continuous based on whether the variable is discrete or continuous.


```python
import math

import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
from scipy.stats import binom, norm, poisson
from tweedie import tweedie
```


```python
# set seed
np.random.seed(0)
```

# Continuous Distributions

## Continuious Uniform Distribution

The most basic distribution is the uniform distribution. In this case every value between the limits is equally likely to be called. These can be continuous or discrete. Continuous uniform distributions are also called rectangular distributions.

#### Plot


```python
def plot_hist(bins):
    num_bins = len(bins) - 1
    plt.xlabel('Variable')
    plt.ylabel('Count')
    plt.title(f"Continuous Uniform Distribution with {num_bins} Bins")
    plt.show()
```


```python
num_samples = 10000
```


```python
x = np.random.uniform(0, 1, num_samples)
```


```python
num_bins = 20
count, bins, ignored = plt.hist(x, bins=num_bins, color='g', alpha=0.9) 
plot_hist(bins)
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_13_0.png)
    


Note that the number of bins used to plot it can make it look more or less spiky. Here's the same data plotted with 200 bins.


```python
num_bins = 200
count, bins, ignored = plt.hist(x, bins=num_bins, color='g', alpha=0.9) 
plot_hist(bins)
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_15_0.png)
    


#### Uses

Whether things are truly continuously distributed or not can be a bit of a question of definition. If time and space are quantized, then nothing in the physical world is truly a continuous uniform distribution. But these things are close enough. Some uniform distributions are:
* Random number generators (in the ideal case)
* The location a dart lands on a dartboard
* Asteroid impacts (roughly - it would only be continuous given a steady state of the universe)

## Gaussian (or Normal) Distribution

Gaussian distributions are all around us. These are the famous bell curves. I've found that physicists are more likely to say "Gaussian" and mathematicians are more likely to say "normal". It seems that "normal" is more popular, although old habits die hard, so I still say "Gaussian".

#### Equation

$$
f(x) = \frac{e^{-\frac{x^2}{2}}}{\sqrt{2\pi} } 
$$

Or, with more parameters:

$$
f(x) = \frac{1}{\sigma \sqrt{2\pi} } e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2}
$$

Many distributions look roughly Gaussian but are not truly Gaussian. A paper on "[The unicorn, the normal curve, and other improbable creatures](https://psycnet.apa.org/record/1989-14214-001)" looked at 440 real datasets and found that none of them were perfectly normally distributed.

#### Plot


```python
fig = plt.figure()

mean = 0
standard_deviation = 1
x = np.random.normal(loc=mean, scale=standard_deviation, size=num_samples)

num_bins = 50
n, bins, patches = plt.hist(x, num_bins, density=True, color='g', alpha=0.9)

# add a line
y = norm.pdf(bins, loc=mean, scale=standard_deviation)
plt.plot(bins, y, 'r--')

plt.xlabel('Variable')
plt.ylabel('Probability')

plt.title("Gaussian Distribution".format(num_bins))
plt.show()
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_26_0.png)
    


You can also do this with `seaborn` and `displot`.


```python
mu, sigma = 0, 0.1 # mean and standard deviation
s = np.random.normal(mu, sigma, num_samples)
```


```python
ax = sns.displot(s, color='skyblue')
ax.set(xlabel='Variable', ylabel='Count');
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_29_0.png)
    


#### Uses

* Height is a famous example of a Gaussian distribution

## Tweedie Distribution


```python
num_params = 20
```


```python
exog = np.random.rand(num_samples, num_params - 1)
exog = np.hstack((np.ones((num_samples, 1)), exog))
beta = np.concatenate(([500], np.random.randint(-100, 100, num_params - 1))) / 100
eta = np.dot(exog, beta)
mu = np.exp(eta)

x = tweedie(mu=mu, p=1.5, phi=20).rvs(num_samples)
```


```python
num_bins = 50
n, bins, patches = plt.hist(x, num_bins, density=True, color='g', alpha=0.9)
plt.xlabel('Variable')
plt.ylabel('Probability')

plt.title("Tweedie Distribution".format(num_bins))
plt.show()
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_35_0.png)
    


# Discrete Distributions

But lots of distributions aren't continuous, they deal with discrete predictions. For example, you can't flip 7.5 heads after 15 tries.

## Discrete Uniform Distribution

Lots of things are discrete uniform distribution.

#### Plot


```python
x = np.random.randint(0, 10, num_samples)
```


```python
count, bins, ignored = plt.hist(x, color='g', alpha=0.9, rwidth=0.9) 
num_bins = len(bins) - 1
plt.xlabel('Variable')
plt.ylabel('Count')
plt.title(f"Discrete Uniform Distribution")
plt.show()
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_42_0.png)
    


#### Uses

* The results of rolling a die
* The card number from a card randomly drawn from a deck
* Winning lottery numbers
* Birthdays aren't perfectly uniform because they tend to bunch up for a variety of reasons, but they are close to uniform

## Binomial Distribution

Binomial distribution are based on discrete results from discrete events, like flipping a coin `n` times. From Wikipedia, a "binomial distribution with parameters n and p is the discrete probability distribution of the number of successes in a sequence of n independent experiments, each asking a yes–no question, and each with its own Boolean-valued outcome"

#### Plot

Let's say with run something with 100 trials, each of which has a 10% change of happening. To run that experiment, we can do:


```python
num_trials = 100
prob_success = 0.1
```


```python
x = np.random.binomial(n=100, p=0.1)
print(f"In this experiment, we got heads {x} times")
```

    In this experiment, we got heads 14 times
    

But we can run this whole experiment over and over again and see what we get.


```python
fig = plt.figure()
x = np.random.binomial(n=num_trials, p=prob_success, size=num_samples)
num_bins = 25

n, bins, patches = plt.hist(x, num_bins, density=True, color='g', alpha=0.9, rwidth=0.9)

plt.xlabel('Variable')
plt.ylabel('Probability')

plt.title("Normal Distribution Histogram (Bin size {})".format(num_bins))
plt.show()

```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_52_0.png)
    


You can also do the same thing with `scipy`.


```python
binon_sim = binom.rvs(n=num_trials, p=prob_success, size=num_samples)
plt.hist(binon_sim, bins=num_bins, density=True, rwidth=0.9)
plt.xlabel('Variable')
plt.ylabel('Probability')
plt.show()
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_54_0.png)
    


#### Usage

* Flipping a coin
* Shooting free throws
* Lots of things that seem to be Gaussian but are discrete

## Poisson Distribution

Many reallife things follow a Poisson distribution. Poisson distributions are like binomial distributions in that they count discrete events. But it's different in that there is no fixed number of experiments to run. Imagine a soccer game. So it's 
discrete occurances (yes/no for a goal being scored) along a continuous distribution (time).

At any moment, a goal could be scored. Thus it is continuous. There's no specific number of "goal attempts" in a Poisson distribution.

If you wanted to find the number of shots on goal and see how many of them were goals, then it would become binomial. But if you look at it over time it's Poisson. It's like the limit of a binomial distribution where you have infinite attempts and each has an infintessimal change of being positive.

Note that Poisson and binomial distributions can't be negative.

#### Equation

Mathematically, this looks like: $$ P(x = k) = e^{-\lambda}\frac{\lambda^k}{k!} $$

#### Plot


```python
data_poisson = poisson.rvs(mu=3, size=num_samples)
```


```python
ax = sns.displot(data_poisson, color='skyblue')
ax.set(xlabel='Variable', ylabel='Count');
```


    
![png](2022-09-17-distributions_files/2022-09-17-distributions_63_0.png)
    


#### Usage

* Decay events from radioactive sources
* Meteorites hitting the Earth
* Emails sent in a day

#### Example

Poisson distributions are very useful in predicting rare events. If once-in-a-lifetime floods occur once every hundred years, what is the chance that we have five within a hundred year period?

In this case $$ \lambda = 1 $$ and $$ k = 5 $$


```python
math.exp(-1)*(1**5/math.factorial(5))
```




    0.0030656620097620196


