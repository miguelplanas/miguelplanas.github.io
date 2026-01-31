---
title: "Semantic Soil Segmentation with Deep Learning"
tags: ["polimi", "python", "computer-vision"]
date: 2026-01-25T12:00:00+01:00
draft: false
description: "Development of UNet and Atrous spatial pyramid pooling enhanced CNN (ASPP) architectures for multiclass terrain segmentation"
cover:
    image: "/projects/mars_soil/dup_aliens.png"
    alt: "Soil type segmentation"
    relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/anndl/tree/main/project2" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-anndl-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/anndl/tree/main/project2" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Project Summary

This project focuses on the development of a **semantic segmentation** system for the automatic identification of different soil types and geological structures in images. Using state-of-the-art architectures such as **UNet** and **ASPP** blocks, the system is capable of classifying each pixel of the image into categories such as rock, sand, gravel, or background, facilitating topographical analysis and autonomous navigation in explored environments.

The workflow ranges from rigorous original dataset cleaning to the implementation of custom loss functions to combat class imbalance.

---

## 1. Exploratory Data Analysis

The quality of segmentation masks is critical. In the initial phase, a deep **Exploratory Data Analysis (EDA)** was performed, revealing the presence of "anomalies" in the training dataset:

- **Duplicate Cleaning:** Using Perceptual Hashing, redundant images that would have biased learning were identified and removed.
- **Dataset Curiosities:** As in other projects, humorous entries infiltrated into technical data were detected, such as this "alien face" deliberately repeated in the training set.

![Duplicate and anomaly detection](/projects/mars_soil/dup_aliens.png)
*Figure 1: Duplicate detection using hashing, revealing humorous intrusions in the dataset.*

---

## 2. Implemented Architectures

Two main approaches were explored to maximize edge precision and context capture at different scales:

- **UNet:** Symmetric encoder-decoder architecture with *skip connections*, ideal for recovering spatial resolution lost during convolution.
- **ASPP (Atrous Spatial Pyramid Pooling):** Implementation of *atrous convolutions* at different rates to capture multiscale information without drastically increasing the number of parameters.

---

## 3. Training Strategies

To achieve stable convergence and a competitive MeanIoU (Intersection over Union), advanced techniques were applied:

- **Focal Loss:** Replacement of traditional cross-entropy with Focal Loss to penalize errors in minority classes more heavily (e.g., small rocks versus sand extensions).
- **Dynamic Callbacks:** Use of `ReduceLROnPlateau` to adjust the learning rate upon detecting stagnation and `ModelCheckpoint` to preserve the state with the best validation.
- **Data Augmentation:** Implementation of geometric and color transformations (CutMix) to improve model robustness.

---

## 4. Monitoring and Evaluation

The entire training process was monitored in real-time through **Weights & Biases (W&B)**, allowing for visual comparison of metric evolution and prediction quality on the validation set.

- **Final Evaluation:** The **MeanIoU** (Intersection over Union) was calculated per class, this being the standard metric to measure the overlap between the prediction and the ground truth.

![Final results](/projects/mars_soil/portada.png)
*Figure 2: Final model results.*

---

## Tech Stack

*   **Deep Learning:** Keras & TensorFlow.
*   **Experiment Management:** Weights & Biases (wandb).
*   **Processing:** NumPy, SciPy & OpenCV.
*   **Metrics and Analysis:** Scikit-learn & Pandas.

---

## Project Report

{{< pdf "/projects/mars_soil/AN2DL_PROJECT2.pdf" >}}

---
**Link to code:** [anndl on GitHub](https://github.com/miguelplanas/anndl/tree/main/project2)
