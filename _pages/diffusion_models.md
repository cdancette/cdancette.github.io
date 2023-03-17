---
layout: page
permalink: /diffusion-models/
title: Augmenting diffusion models with semantic spatial layout conditioning
description: 
nav: false
---

Diffusion models have been very successful at generating image conditioned on textual inputs. Recently quite a few methods proposed to augment them with the ability to take semantic maps as conditioning, like SpaText, Paint with Words (in the ediff-I paper), ControlNet, and T2I-Adapter. 

In this post, we evaluate these different methods on the COCO dataset, and check how these novel methods perform in comparison with traditional train-based methods like OASIS and SDM.

We consider three evaluation scenarios: The first setting is the classical semantic image synthesis framework, where we give as input a complete segmentation map (which might contain *unlabelled* pixels, generally discarded), and the task is to generate a real image whose segmentation mask will be identical. The metrics are: 
- mIoU between input mask and detected mask (to detect the mask, we use a state-of-the-art ViT-Adapter segmentation network)
- FID using the entire COCO validation set as reference set, at resolution 512;
- CLIP, the average CLIP similarity between the generated image and conditioning text prompt.

Here are the results:

| Method      | mIoU  | FID     | CLIPScore  |
| :---        |    :----: |  ---: |    ---: |
| Stable Diffusion   |   11.7    | 23.6      |  31.8 |
| SpaText       |   16.8    | 19.8      |  30.0 |
| T2I-Adapter       |   33.3    | 17.2      |  31.5 |
| Paint with Words  |    21.2   | 36.2      |  29.4 |
| OASIS       |   52.1    | 15.0      |  N/A |
| SDM         |    49.3   | 17.2      | N/A |

Stable Diffusion refers to classical text-to image generation, without conditioning on masks.
We note that the zero-shot method Paint-with words has a much better mIoU score than without conditioning (21.2 vs 11.7), but still very far from classical train-based methods that reach ~50 mIoU.

We also evaluate these methods similarly to SpaText: we remove masks covering less than 5% of the image, and consider $1 \leq K \leq 3$ conditioning masks for each generated image.

| Method      | mIoU  | FID     | CLIPScore  |
| :---        |    :----: |  ---: |    ---: |
| Stable Diffusion   |   11.7    | 23.6      |  31.8 |
| Spatext       |   23.8    | 16.2      |  30.2 |
| T2I-Adapter       |   31.6    | 19.2      |  30.6 |
| Paint with Words  |    23.8   | 25.8      |  29.6 |
| OASIS       |   41.4    | 46.8      |  N/A |
| SDM         |    29.3   | 65.3      | N/A |

In this setting, we observe that train-based methods OASIS and SDM break, because they have not been trained to take only a few masks as conditioning information. This is why zero-shot methods like Paint-with words are more interesting. Spatext has a very good FID since it is the setup on which it has been trained.

Finally, we consider the intermediate setup where we consider as conditioning masks all masks covering more than 5% of the generated image.


| Method      | mIoU  | FID     | CLIPScore  |
| :---        |    :----: |  ---: |    ---: |
| Stable Diffusion   |   11.7    | 23.6      |  31.8 |
| Spatext       |   19.2    | 18.9      |  30.1 |
| T2I-Adapter       |   35.1    |   17.8   |  31.3 |
| Paint with Words  |    23.5   | 35.0      |  29.5 |
| OASIS       |   53.7    | 18.2      |  N/A |
| SDM         |    41.7   | 28.6      | N/A |

OASIS and SDM work much better, because the conditioning information covers most of the image.


Overall, on the three different settings, the best method seems to be T2I-Adapter, which adapts diffusion models to take spatial layout masks as conditioning information by fine-tuning adaptation layers on the COCO dataset. However, for free-form text conditioning, Paint with words offers a more flexible interface.