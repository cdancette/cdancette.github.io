---
layout: post
title: "An overview of biases reduction methods for Visual Question Answering"
keywords: machine learning, visual question answering, biases
excerpt_separator: "<!-- more -->"
---

Visual Question Answering task consists in answering a question about an image.
<!-- Show example here -->

Visual Question Answering has been shown to contain lot of unwanted biases, that enables models to reach high accuracy, but fail to answer for the right reasons.

<!-- Show examples of papers that say this -->

<!-- more -->

<!-- omit in toc -->
### Table of contents

- [Baseline methods to reduce biases](#baseline-methods-to-reduce-biases)
- [Adversarial methods](#adversarial-methods)
- [Structuring the output loss](#structuring-the-output-loss)
- [Ensembing-based regularization](#ensembing-based-regularization)
- [Others](#others)

### Baseline methods to reduce biases

**Reweighting / Sampling methods**

<div class="row mt-3 justify-content-center">
    <div class="col-6 mt-3 mb-3">
        <img class="img-fluid rounded" 
            src="{{ site.baseurl }}/assets/reducing-biases/reweighting.png">
    </div>
</div>


<!-- ![Jeu](/assets/reducing-biases/reweighting.png) -->

The first straightforward method that can help to reduce biases in certain cases is reweighting your dataset. 
The idea is to over-sample difficult or rare examples, and under-sample common and easy examples.
This will help for problems that suffer from an imbalance in labels / input concepts. 
For example, in Visual Question Answering, there often is an imbalance in the answer distribution. 
Reweighting can help 

If we have an example $X = (v, q, a) \in (I, Q, A)$ where $I$ is the image distribution, Q is the question distribution and A is the answer distribution, we can sample our example X with the uniform probability
$$ p() $$



### Adversarial methods


### Structuring the output loss

### Ensembing-based regularization

### Others

