---
layout: distill
description: Reducing Unimodal Biases for Visual Question Answering.
img: https://raw.githubusercontent.com/cdancette/rubi.bootstrap.pytorch/master/assets/model_rubi.png
# redirect: https://github.com/cdancette/rubi.bootstrap.pytorch
github: https://github.com/cdancette/rubi.bootstrap.pytorch
# github_stars: 128
importance: 1
authors:
    - name: Rémi Cadène*
      url: https://cadene.com
      affiliations: 
        name: Sorbonne Université
    - name: Corentin Dancette*
      url: cdancette.fr
      affiliations: 
        name: Sorbonne Université
    - name: Matthieu Cord
      affiliations: 
        name: Sorbonne Université
    - name: Devi Parikh
      affiliations: 
        name: Facebook AI Research
published: NeurIPS 2019
---

<p>
<div class="row justify-content-center">
        <img src="https://raw.githubusercontent.com/cdancette/rubi.bootstrap.pytorch/master/assets/model_classic.png" alt="drawing" width="200"/>
        <img src="https://raw.githubusercontent.com/cdancette/rubi.bootstrap.pytorch/master/assets/model_rubi.png" alt="drawing" width="200"/>
</div>
</p>

This work was published at the NeurIPS 2019 conference. You can check the paper [here](https://papers.nips.cc/paper/8371-rubi-reducing-unimodal-biases-for-visual-question-answering), and the code [here](https://github.com/cdancette/rubi.bootstrap.pytorch).

### Goals
RUBi is a learning strategy designed to help deep learning models learn a robust behaviour.
It helps the models avoid relying on a known source of bias. For Visual Question Answering, 
the source of bias is the question modality: Models have been shown to rely too much on this
input, and too little on the image content. 


### Method

During training, we merge the prediction of the main VQA model with the prediction of a question-only branch.
The question-only branch will predict a 0-1 mask, that will influence the model's logits.
This will encourage the main model to learn less from biased examples (that are answered correctly by the question-only model), and to learn more from examples that the question-only model gets wrong.

<!-- ### Results -->


 