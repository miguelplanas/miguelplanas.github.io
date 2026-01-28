---
title: "Analysis and Applications of Singular Value Decomposition"
tags: ["uni", "python", "computer-vision", "math"]
date: 2026-01-23T12:19:52+01:00
draft: false
description: "Study on dimensionality reduction, signal compression, and dynamic component extraction using SVD."
cover:
    image: "portada.png"
    alt: "SVD results visualization"
    relative: true
---
<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/svd_project" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-svd_project-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/svd_project" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Project Summary

This repository documents a practical study on **Singular Value Decomposition (SVD)**, a fundamental tool in Numerical Analysis. The project not only explores the mathematical theory behind the factorization $A=U\Sigma V^t$, but demonstrates how this abstract operation is the engine behind real engineering solutions, from data compression to computer vision.

The work is divided into two main blocks: a constructive implementation of the algorithm from scratch and a series of practical applications that exploit the SVD's capability to separate "signal" from "noise".

---

## 1. Algorithm Implementation

Although modern scientific libraries already include optimized solutions, this project addresses the manual implementation to understand numerical stability and internal transformations. The process has been broken down into three constructive stages, as detailed in the technical report:

1.  **Bidiagonal Reduction:** Transformation of the original matrix $A$ to a bidiagonal form $B$ using orthogonal matrices ($U_1$ and $V_1$). This simplifies the problem's complexity.
2.  **SVD of the Bidiagonal Matrix:** Calculation of singular vectors using the relationship with the eigenvectors of $B^TB$ and an orthonormalization process.
3.  **Final Composition:** Reconstruction of the complete factorization $A=U\Sigma V^T$.

Additionally, the **Moore-Penrose Pseudoinverse** ($A^+$) has been implemented, a direct application that allows solving least squares problems with high precision.

---

## 2. Practical Applications

The power of SVD lies in its ability to generate low-rank approximations. The three implemented use cases are detailed below.

### Dimensionality Reduction: PCA via SVD

Principal Component Analysis (PCA) is usually associated with the covariance matrix, but SVD offers a more direct and efficient numerical route.

In the corresponding notebook, we demonstrate how the right singular vectors ($V$) of a centered data matrix act as the principal directions. There is a direct relationship where the eigenvalues of the covariance are derived from the singular values:
$$\lambda_i = \frac{\sigma_i^2}{n-1}$$

Applying this to the **MNIST** dataset (handwritten digits of 28x28 pixels), we manage to project high-dimensional images onto a 2D plane. As seen in the figure, this reveals the latent structure of the data, grouping digits into distinguishable clusters without the need for prior labels.

![PCA on MNIST](pca_mnist.png)
*Figure 1: Projection of MNIST digits onto the first two principal components, showing the natural grouping of classes.*

### Image Compression: The Energy of Data

A digital image is, in essence, a matrix of intensities. SVD allows us to decompose this matrix and truncate the series of singular values, keeping only the first $k$. This is based on the fact that structural information is concentrated in the first modes (large singular values), while fine details and noise decay quickly.

In the experiment conducted with a high-resolution astronomical image:
* **$k=10$:** Only the general lighting is intuited.
* **$k=50$:** The structure is recognizable, but sharpness is lacking.
* **$k=100$:** Visual convergence almost perfect with respect to the original is reached.

This implies a drastic memory saving: instead of storing $m \times n$ pixels, we only need to store $k(m+n+1)$ values.

![SVD Compression](compresion.png)
*Figure 2: Evolution of image reconstruction varying the rank $k$. Note how with $k=100$ the loss of information is practically imperceptible to the human eye.*

### Video Processing: Background Separation

Perhaps the most striking application is treating video as a giant matrix $M$, where each column is a "flattened" frame.

The hypothesis is elegant: the background of a fixed scene (like a surveillance camera) is constant, which implies high temporal correlation and, therefore, a **rank 1** (or very low). Conversely, moving people or objects are sparse perturbations.

The implemented algorithm:
1.  Calculates the SVD of the video matrix.
2.  Reconstructs the background using only the **first singular value** ($\sigma_1$).
3.  Obtains the foreground (moving objects) through subtraction: $M_{foreground} = |M - M_{background}|$.

![Background segmentation in video](portada.png)
*Figure 3: Left: Original Frame. Center: Recovered Static Background (Rank 1). Right: Isolated Motion Mask.*

---

## Repository Structure

The code is organized sequentially to facilitate its study:

* `0_implementacion_svd.ipynb`: The project's "engine". Step-by-step algorithm development and property validation.
* `1_pca_usando_svd.ipynb`: Dimensionality reduction applied to the digit dataset.
* `2_compresion_imagen.ipynb`: Analysis of the singular value spectrum and visual compression.
* `3_eliminacion_fondo.ipynb`: Source separation in video sequences (Background Subtraction).

## Tools Used

The implementation has been carried out in **Python**, combining matrix calculation power with visualization tools:
* **NumPy and SciPy:** For handling complex matrix structures and linear algebra.
* **OpenCV:** For reading and preprocessing video streams and images.
* **Matplotlib:** To visualize everything from data clusters to visual comparison of frames.

---
**Link to code:** [svd_project on GitHub](https://github.com/miguelplanas/svd_project)