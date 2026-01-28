---
title: "Blood Cell Classification with Deep Learning"
tags: ["uni", "python", "computer-vision"]
date: 2026-01-25T12:00:00+01:00
draft: false
description: "Implementation of CNN architectures for cell type identification in microscopic blood smear images, integrating perceptual hashing cleaning and fine-tuning."
cover:
    image: "portada.png"
    alt: "Leukocyte classification using CNN"
    relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/anndl/tree/main/project1" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-anndl-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/anndl/tree/main/project1" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Project Summary

This project addresses the **automation of hematological diagnosis** through the development of a computer vision system capable of classifying different types of blood cells (leukocytes) from microscopic images. The implemented solution uses **Convolutional Neural Network (CNN)** architectures, pre-trained models, and a complete workflow to ensure model reliability.

---

## 1. Dataset Curation and Quality

One of the biggest challenges in medical imaging is the bias introduced by redundant data. In this project, a critical phase of **duplicate detection using Perceptual Hashing** was implemented:

- **Technique:** Use of `imagehash` (dHash) to identify nearly identical images that could contaminate the training and test sets.
- **Result:** Cleaning of corrupt or repetitive images, guaranteeing that the model's accuracy reflects its real generalization capacity.

As can be seen in the following image, there is an image repeated 1600 times and another quite funny one with Rick Astley's face repeated 200 times.

![Duplicates](portada.png)
*Figure 1: Duplicate detection using Perceptual Hashing.*

Before starting training, class imbalance was analyzed to adjust model weights and ensure equitable learning among cell types.

![Class distribution](distribucion.png)
*Figure 2: Dataset distribution after cleaning and balancing.*

---

## 2. Modeling and Training

First, a CNN architecture optimized for morphological feature extraction was designed. The process included:

1.  **Data Augmentation:** Generation of synthetic variations (rotations, zoom, brightness changes) to make the model robust against variations in sample capture.
2.  **Architecture:** Convolutional blocks with *Batch Normalization* and *Dropout* regularization to prevent overfitting.
3.  **Optimization:** Continuous metric monitoring to dynamically adjust the *Learning Rate*.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="accuracy.png" alt="Accuracy Evolution">
    <p><em>Accuracy</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="loss.png" alt="Loss Evolution">
    <p><em>Loss</em></p>
  </div>
</div>
*Figure 3: Evolution of metrics during training and validation phases.*

With the aim of improving model performance, a **Fine-Tuning** process was implemented, based on the **pre-trained MobileNetV3 model**, freezing the lower layers to protect morphological feature learning, and adjusting the upper layer weights to adapt them to the specific dataset.
---

## 3. Evaluation and Fine-Tuning

The effectiveness of this phase can be observed in the improvement of the **Confusion Matrices**, where false positives were reduced after network *fine-tuning*.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="confusion_matrix_PRE_FINETUNING.png" alt="Pre Fine-Tuning">
    <p><em>Pre Fine-Tuning</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="confusion_matrix_FINETUNING.png" alt="Post Fine-Tuning">
    <p><em>Post Fine-Tuning</em></p>
  </div>
</div>

---

## Tech Stack

*   **Deep Learning:** Keras & TensorFlow.
*   **Computer Vision:** OpenCV & PIL.
*   **Data Science:** NumPy, Pandas & Scikit-learn.
*   **Visualization:** Matplotlib & Seaborn.

---

## Project Report

{{< pdf "/es/projects/blood_cells/AN2DL_PROJECT1.pdf" >}}

---
**Link to code:** [anndl on GitHub](https://github.com/miguelplanas/anndl/tree/main/project1)
