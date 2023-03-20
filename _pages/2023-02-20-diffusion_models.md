---
layout: post
permalink: /diffusion-models/
title: Augmenting diffusion models with semantic spatial layout conditioning
description: 
nav: false
date: 2023-02-20
---


<style type="text/css">
  td {
    padding:0 25px 0 0; /* Only right padding*/
  }
  th {
    padding:0 25px 0 0; /* Only right padding*/
  }
</style>

<!-- ## Augmenting diffusion models with semantic spatial layout conditioning -->

Diffusion models have been very successful at generating image conditioned on textual inputs. Recently quite a few methods proposed to augment them with the ability to take semantic maps as conditioning, like SpaText, Paint with Words (in the ediff-I paper), ControlNet, and T2I-Adapter. 

In this post, we evaluate these different methods on the COCO dataset, and check how these novel methods perform in comparison with traditional train-based methods like OASIS and SDM.

We consider three evaluation scenarios: The first setting is the classical semantic image synthesis framework, where we give as input a complete segmentation map (which might contain *unlabelled* pixels, generally discarded), and the task is to generate a real image whose segmentation mask will be identical. The metrics are: 
- mIoU between input mask and detected mask (to detect the mask, we use a state-of-the-art ViT-Adapter segmentation network)
- FID using the entire COCO validation set as reference set, at resolution 512;
- CLIP, the average CLIP similarity between the generated image and conditioning text prompt.

Here are the results:

<center>
<table>
  <thead>
    <tr>
      <th style="text-align: left">Method</th>
      <th style="text-align: center">mIoU</th>
      <th style="text-align: right">FID</th>
      <th style="text-align: center">CLIP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">Stable Diffusion</td>
      <td style="text-align: center">11.7</td>
      <td style="text-align: right">23.6</td>
      <td style="text-align: center">31.8</td>
    </tr>
    <tr>
      <td style="text-align: left">SpaText</td>
      <td style="text-align: center">16.8</td>
      <td style="text-align: right">19.8</td>
      <td style="text-align: center">30.0</td>
    </tr>
    <tr>
      <td style="text-align: left">T2I-Adapter</td>
      <td style="text-align: center">33.3</td>
      <td style="text-align: right">17.2</td>
      <td style="text-align: center">31.5</td>
    </tr>
    <tr>
      <td style="text-align: left">Paint with Words</td>
      <td style="text-align: center">21.2</td>
      <td style="text-align: right">36.2</td>
      <td style="text-align: center">29.4</td>
    </tr>
    <tr>
      <td style="text-align: left">OASIS</td>
      <td style="text-align: center">52.1</td>
      <td style="text-align: right">15.0</td>
      <td style="text-align: center">N/A</td>
    </tr>
    <tr>
      <td style="text-align: left">SDM</td>
      <td style="text-align: center">49.3</td>
      <td style="text-align: right">17.2</td>
      <td style="text-align: center">N/A</td>
    </tr>
  </tbody>
</table>
</center>
<br>

Stable Diffusion refers to classical text-to image generation, without conditioning on masks.
We note that the zero-shot method Paint-with words has a much better mIoU score than without conditioning (21.2 vs 11.7), but still very far from classical train-based methods that reach ~50 mIoU.

We also evaluate these methods similarly to SpaText: we remove masks covering less than 5% of the image, and consider $$1 \leq K \leq 3$$ conditioning masks for each generated image.

<center>
<table>
  <thead>
    <tr>
      <th style="text-align: left">Method</th>
      <th style="text-align: center">mIoU</th>
      <th style="text-align: center">FID</th>
      <th style="text-align: center">CLIP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">Stable Diffusion</td>
      <td style="text-align: center">11.7</td>
      <td style="text-align: center">23.6</td>
      <td style="text-align: center">31.8</td>
    </tr>
    <tr>
      <td style="text-align: left">Spatext</td>
      <td style="text-align: center">23.8</td>
      <td style="text-align: center">16.2</td>
      <td style="text-align: center">30.2</td>
    </tr>
    <tr>
      <td style="text-align: left">T2I-Adapter</td>
      <td style="text-align: center">31.6</td>
      <td style="text-align: center">19.2</td>
      <td style="text-align: center">30.6</td>
    </tr>
    <tr>
      <td style="text-align: left">Paint with Words</td>
      <td style="text-align: center">23.8</td>
      <td style="text-align: center">25.8</td>
      <td style="text-align: center">29.6</td>
    </tr>
    <tr>
      <td style="text-align: left">OASIS</td>
      <td style="text-align: center">41.4</td>
      <td style="text-align: center">46.8</td>
      <td style="text-align: center">N/A</td>
    </tr>
    <tr>
      <td style="text-align: left">SDM</td>
      <td style="text-align: center">29.3</td>
      <td style="text-align: center">65.3</td>
      <td style="text-align: center">N/A</td>
    </tr>
  </tbody>
</table>
</center>
<br>


In this setting, we observe that train-based methods OASIS and SDM break, because they have not been trained to take only a few masks as conditioning information. This is why zero-shot methods like Paint-with words are more interesting. Spatext has a very good FID since it is the setup on which it has been trained.

Finally, we consider the intermediate setup where we consider as conditioning masks all masks covering more than 5% of the generated image.


<center>
<table>
  <thead>
    <tr>
      <th style="text-align: left">Method</th>
      <th style="text-align: center">mIoU</th>
      <th style="text-align: center">FID</th>
      <th style="text-align: center">CLIP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: left">Stable Diffusion</td>
      <td style="text-align: center">11.7</td>
      <td style="text-align: center">23.6</td>
      <td style="text-align: center">31.8</td>
    </tr>
    <tr>
      <td style="text-align: left">Spatext</td>
      <td style="text-align: center">19.2</td>
      <td style="text-align: center">18.9</td>
      <td style="text-align: center">30.1</td>
    </tr>
    <tr>
      <td style="text-align: left">T2I-Adapter</td>
      <td style="text-align: center">35.1</td>
      <td style="text-align: center">17.8</td>
      <td style="text-align: center">31.3</td>
    </tr>
    <tr>
      <td style="text-align: left">Paint with Words</td>
      <td style="text-align: center">23.5</td>
      <td style="text-align: center">35.0</td>
      <td style="text-align: center">29.5</td>
    </tr>
    <tr>
      <td style="text-align: left">OASIS</td>
      <td style="text-align: center">53.7</td>
      <td style="text-align: center">18.2</td>
      <td style="text-align: center">N/A</td>
    </tr>
    <tr>
      <td style="text-align: left">SDM</td>
      <td style="text-align: center">41.7</td>
      <td style="text-align: center">28.6</td>
      <td style="text-align: center">N/A</td>
    </tr>
  </tbody>
</table>
</center>
<br>


OASIS and SDM work much better, because the conditioning information covers most of the image.

