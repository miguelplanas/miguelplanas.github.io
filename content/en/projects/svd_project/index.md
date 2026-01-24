---
title: "Analysis and Applications of Singular Value Decomposition"
tags: ["uni"]
date: 2026-01-23T12:19:52+01:00
draft: false
description: "Study on dimensionality reduction, signal compression and extraction of dynamic components using SVD."
cover:
    image: "portada.png"
    alt: "Visualization of SVD results"
    relative: true
---

<p align="center">
  <a href="https://github.com/miguelplanas/svd_project"><img src="https://img.shields.io/badge/github-svd_project-black?style=for-the-badge&logo=github" /></a>
  <a href="https://github.com/miguelplanas/svd_project"><img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" /></a>
</p>

## Project Summary

This repository documents a practical study on the **Singular Value Decomposition (SVD)** and its versatility in data processing. The central objective is to demonstrate how a fundamental operation of linear algebra can solve complex engineering problems, from storage optimization to forensic video analysis.

---

## Technical Foundations

SVD allows factoring a matrix $A$ of dimensions $m \times n$ into the product $U \Sigma V^T$. In the context of this project, three main directions have been explored:

### 1. Dimensionality Reduction (PCA)
By computing the eigenvectors of the covariance matrix (derived from the SVD), Principal Component Analysis was implemented to project the MNIST dataset. This makes it possible to identify the latent structure of the data in low-dimensional spaces.

![PCA on MNIST](pca_mnist.png)
*Figure 1: Projection of MNIST digits onto the first two principal components.*


### 2. Low-Rank Approximation and Compression
We analyze the energy retention of the original image as a function of the number of singular values preserved. The technique allows compressing visual information by discarding components with lower statistical weight without compromising the interpretability of the image.

![SVD Compression](compresion.png)
*Figure 2: Image reconstruction varying the number of singular values (k).*


### 3. Background Segmentation in Video
An algorithm was implemented that treats a video sequence as a large matrix. By applying SVD, it is possible to separate low-rank information (the static background) from sparse perturbations (moving objects), a technique fundamental to surveillance systems.

---

## Repository Structure

* `0_implementacion_svd.ipynb`: Development of the decomposition from scratch and computation of the Moore–Penrose pseudoinverse.
* `1_pca_usando_svd.ipynb`: Complete pipeline for dimensionality reduction.
* `2_compresion_imagen.ipynb`: Image reconstruction algorithms.
* `3_eliminacion_fondo.ipynb`: Video sequence processing.

## Tools Used

The implementation was carried out entirely in **Python**, relying on scientific computing and computer vision libraries:
* **Numerical Computing:** NumPy and SciPy for high-performance matrix manipulation.
* **Computer Vision:** OpenCV for handling video streams and reading images.
* **Visualization:** Matplotlib for generating technical plots and analyzing results.

---
**Code link:** [svd_project on GitHub](https://github.com/miguelplanas/svd_project)