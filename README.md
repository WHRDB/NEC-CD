# NEC-CD: Reliability-Aware Negative-Evidence Learning and Calibration for Image-Level Weakly Supervised Remote Sensing Change Detection

## News 📢

- The NEC-CD implementation is currently **not publicly released**. For peer-review purposes, the code can be provided upon request.
- The six benchmark datasets used in the experiments, together with the corresponding experimental checkpoints, are planned for public release in a future update.

## Overview ✨

- [News 📢](#news-)
- [Introduction 📄](#introduction-)
- [Framework Overview 🧩](#framework-overview-)
- [Method Highlights 🔍](#method-highlights-)
- [Benchmarks 🗂️](#benchmarks-)
- [Experimental Setting ⚙️](#experimental-setting-)
- [Code Availability 💻](#code-availability-)
- [Datasets and Checkpoints ⬇️](#datasets-and-checkpoints-)
- [Citation 📚](#citation-)
- [License 🔑](#license-)

## Introduction 📄

Image-level weakly supervised remote sensing change detection aims to localize changes in bi-temporal images using image-level labels, without requiring pixel-level annotations. However, image-level classification provides limited spatial constraints, often allowing local background variations to produce false change responses. These unreliable activations can impair change localization and reduce the quality of pseudo-labels generated for subsequent training.

An important property of image-level supervision is its asymmetry: a changed label indicates that at least one change exists in an image pair, whereas an unchanged label verifies the absence of target changes throughout the pair. Motivated by this observation, we propose **NEC-CD**, a reliability-aware framework for negative-evidence learning and calibration in remote sensing change detection.

NEC-CD exploits verified negatives from unchanged image pairs to constrain change responses at both relative and absolute response levels. Specifically, unchanged pairs supervise normalized change activations and sampled raw change scores, helping suppress unreliable local responses. To extend negative supervision into feature space, an exponential moving average (EMA) teacher selects high-confidence, cross-view-consistent change anchors. The student is encouraged to approach the corresponding teacher features at the same locations while separating them from features extracted from unchanged pairs.

At inference, predicted unchanged-class responses within changed pairs are treated as **uncertain counter-evidence**, rather than verified negatives. Their suppressive effect is modulated by a training-free photometric consistency prior that measures local bitemporal appearance stability. The calibrated change scores are then thresholded under image-level label constraints to generate pixel-level pseudo-labels without manual pixel annotations.

Experiments on six public benchmarks—**CLCD, Hi-CNA, JLYHCD, WHU-CD, GVLM, and GZ-CD**—demonstrate the effectiveness of NEC-CD and its state-of-the-art performance in image-level weakly supervised change detection.

## Framework Overview 🧩

The overall framework of NEC-CD is provided below:

[View the overall framework diagram (PDF)](./over_framework/over_framework.pdf)

The diagram illustrates the main components of NEC-CD, including negative-evidence-guided response learning, EMA teacher-guided feature learning, and reliability-aware inference calibration.

## Method Highlights 🔍

- **Reliability-aware use of negative evidence:** NEC-CD distinguishes label-verified negatives from uncertain predicted negative evidence and uses them according to their reliability.
- **Response-level constraints:** Unchanged image pairs constrain normalized change activations and sampled raw change scores, suppressing spurious responses at relative and absolute levels.
- **EMA teacher-guided feature learning:** High-confidence, cross-view-consistent change anchors are selected by an EMA teacher. Student features are pulled toward same-location teacher targets and separated from unchanged-pair features.
- **Appearance-aware inference calibration:** Predicted unchanged-class responses in changed pairs are treated as uncertain counter-evidence. Their suppressive influence is adjusted using local bitemporal photometric consistency.
- **Pseudo-label generation:** Calibrated scores are thresholded under image-level label constraints to generate pixel-level pseudo-labels for subsequent training.

## Benchmarks 🗂️

NEC-CD is evaluated on the following six public remote sensing change detection benchmarks.

| Dataset | Task |
| --- | --- |
| CLCD | Image-level weakly supervised change detection |
| Hi-CNA | Image-level weakly supervised change detection |
| JLYHCD | Image-level weakly supervised change detection |
| WHU-CD | Image-level weakly supervised change detection |
| GVLM | Image-level weakly supervised change detection |
| GZ-CD | Image-level weakly supervised change detection |

Dataset access information and the exact data organization used in our experiments will be provided in a future update.

## Experimental Setting ⚙️

NEC-CD is implemented in **PyTorch** and trained on a single **NVIDIA RTX 4090 GPU**. An **ImageNet-pretrained ResNet-50** is used as the shared bi-temporal backbone.

### First-stage training

| Configuration | Value |
| --- | --- |
| Backbone | ImageNet-pretrained ResNet-50 |
| GPU | 1 × NVIDIA RTX 4090 |
| Training epochs | 5 |
| Batch size | 16 |
| Initial learning rate | 5 × 10⁻³ |
| Learning-rate schedule | Polynomial decay |
| Polynomial power | 0.9 |

### Second-stage training and baseline comparison

Calibrated first-stage scores are used to generate pseudo-labels for second-stage supervision. For controlled comparisons among two-stage methods, NEC-CD and the reproduced two-stage baselines use the same ResNet-50-based second-stage detector, with a batch size of **8**. Each method is trained using its own generated pseudo-labels.

End-to-end methods follow their respective training protocols and do not use pseudo-label-based second-stage training.

## Code Availability 💻

The source code is **not publicly available at the current stage**. It can be provided to reviewers for verification during the peer-review process upon request.

Executable installation, training, and inference instructions will be added if and when the corresponding code is released. No unverified commands or dependency versions are provided in this repository at present.

## Datasets and Checkpoints ⬇️

The datasets used in the experiments and the corresponding experimental checkpoints are planned to be released in a future update. Download links and usage instructions will be added to this section when they become available.

## Citation 📚

The citation information and BibTeX entry will be added when the paper's public preprint or publication details are available.

## License 🔑

A license will be specified when the source code is publicly released. Until then, this repository should be regarded as providing project and method information only; no code-use permissions are granted by this README.
