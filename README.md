# From Black-Box to Bottleneck: Interpretable Road Damage Detection via Contrastive Concept Alignment

A concept-bottleneck approach to road damage classification on RDD2022 that closes half the accuracy gap to a black-box ResNet-18 baseline while preserving full interpretability through a human-readable bottleneck.

## Authors
- Ammara Haroon
- Fizza Naqvi

## Overview

This project addresses interpretable road damage classification on the RDD2022 dataset. Black-box classifiers like ResNet-18 perform well but cannot explain their predictions, which is a critical limitation in safety-sensitive infrastructure inspection contexts. Concept Bottleneck Models (CBMs) address this by routing predictions through a layer of human-readable concepts, but require dense concept annotations. Label-Free CBMs (LF-CBMs) remove the annotation burden using CLIP, but suffer in fine-grained visual domains where CLIP's text-image alignment is weak.

We introduce **Contrastive Concept Alignment (CCA)**, a training objective that augments LF-CBM with supervised contrastive learning over bounding-box damage crops. By using the bbox annotations already in RDD2022, CCA injects class-discriminative structure directly into the concept space without requiring per-image concept labels.

## Key Results

| Model | Accuracy | Macro F1 | Interpretable |
|---|---|---|---|
| ResNet-18 (black-box) | 72.34% | 0.6959 | No |
| Vanilla LF-CBM | 66.26% | 0.6185 | Yes |
| **LF-CBM + CCA (ours)** | **69.30%** | **0.6551** | **Yes** |

LF-CBM+CCA closes 50% of the accuracy gap between vanilla LF-CBM and the unconstrained ResNet-18 baseline, with consistent improvements at every regularization setting tested.

## Method Highlights

**Contrastive Concept Alignment (CCA).** We extract bounding-box damage crops from RDD2022 and apply a supervised contrastive loss on the concept score vectors produced by the projection layer:

$$\mathcal{L}_{\text{total}} = \lambda_{\text{CLIP}} \cdot \mathcal{L}_{\text{CLIP}} + \lambda_{\text{contra}} \cdot \mathcal{L}_{\text{contra}}$$

with $\lambda_{\text{contra}} = 2.0$ selected via validation F1 sweep. The contrastive term pulls same-class crops together and pushes different-class crops apart in the concept space.

**Negative Prompting.** We additionally introduce a contrastive shift in CLIP text-embedding space, moving each concept embedding away from the prototypes of confuser classes. This raises the intra/inter cosine ratio of the concept space from 1.03 to 1.22 (+18%).

**Image-Grounded Concept Generation.** Unlike the original LF-CBM which uses GPT-3 with text-only prompts, we use Claude (Anthropic) with multimodal prompting that grounds concept generation in actual damage imagery, producing more visually faithful concept descriptions.

We used ResNet18 as out baselime model and then had LFCBM-CCA as our second improvement. In our notebook,LFCBM-CCA we used negative prompting to help improve our results.
