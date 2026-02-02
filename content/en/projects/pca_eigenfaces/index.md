---
title: "Principal Component Analysis and Eigenfaces"
tags:
  - math
  - polimi
  - python
  - computer-vision
date: 2024-12-20T10:00:00+01:00
draft: false
math: true
description: "Theoretical and practical project on Principal Component Analysis (PCA) applied to facial recognition through Eigenfaces for the Numerical Analysis for Machine Learning course at Politecnico di Milano."
cover:
  image: /projects/pca_eigenfaces/eigenfaces_publication.png
  alt: Eigenfaces visualization
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/naml" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-naml-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/naml" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Project Overview

This project presents a comprehensive study of **Principal Component Analysis (PCA)** applied to facial recognition through the computation of **Eigenfaces**, developed for the *Numerical Analysis for Machine Learning* course at Politecnico di Milano during the 2024-25 academic year. The work covers the mathematical foundations of PCA, Singular Value Decomposition (SVD), and practical applications in dimensionality reduction and image reconstruction.

---

## Introduction to PCA and Eigenfaces

Principal Component Analysis is one of the most fundamental dimensionality reduction techniques in machine learning and data science. When applied to facial images, PCA produces what are known as **Eigenfaces** — a set of basis images that capture the most significant variations in a dataset of faces.

![PCA Eigenfaces Overview](/projects/pca_eigenfaces/eigenfaces_publication.png)
*Figure 1: Overview of PCA applied to face images showing sample faces, mean face, eigenfaces, explained variance, and reconstruction quality.*

### Key Concepts

- **Dimensionality Reduction**: Reducing the feature space while preserving maximum variance
- **Principal Components**: Orthogonal directions of maximum variance in the data
- **Eigenfaces**: Principal components of facial image datasets
- **Reconstruction**: Approximating original data using reduced representations

---

## Mathematical Foundations

### Data Normalization

Before performing PCA, the data must be centered by subtracting the mean face from each image:

$$\tilde{X} = X - \bar{X}$$

where $\bar{X}$ is the mean face vector computed as the average of all face images in the dataset.

### Singular Value Decomposition

PCA is computed via **Singular Value Decomposition (SVD)** on the normalized dataset:

$$\tilde{X} = U \Sigma V^T$$

Where:
- $U$ is an $m \times m$ orthogonal matrix (left singular vectors)
- $\Sigma$ is an $m \times n$ diagonal matrix of singular values
- $V^T$ is an $n \times n$ orthogonal matrix (right singular vectors)

The rows of $V^T$ correspond to the **principal components** (eigenfaces), ordered by their importance (explained variance).

![Mean Face and Normalization](/projects/pca_eigenfaces/mean_face.png)
*Figure 2: Data normalization process showing original face, mean face, and normalized face.*

---

## Eigenfaces Visualization

The first few principal components capture the most significant patterns of variation across faces:

| Component | Captures |
|-----------|----------|
| **PC 1** | Overall face shape and lighting |
| **PC 2-4** | Major structural variations |
| **PC 5-10** | Specific facial features |
| **PC 11+** | Fine details and noise |

![First 25 Eigenfaces](/projects/pca_eigenfaces/eigenfaces_grid.png)
*Figure 3: The first 25 eigenfaces (principal components) showing progressive capture of facial features.*

### Interpretation of Eigenfaces

Each eigenface represents a "building block" of facial variation:
- **Low-index eigenfaces**: Capture global features (face shape, lighting direction)
- **Mid-index eigenfaces**: Capture specific features (eyes, nose position)
- **High-index eigenfaces**: Capture fine details (wrinkles, skin texture)

---

## Explained Variance Analysis

The explained variance ratio indicates how much of the total variance is captured by each principal component:

$$\text{Explained Variance Ratio}_i = \frac{\sigma_i^2}{\sum_{j=1}^{n} \sigma_j^2}$$

The **cumulative explained variance** helps determine how many components are needed to retain a desired percentage of information:

![Explained Variance](/projects/pca_eigenfaces/variance.png)
*Figure 4: Cumulative explained variance showing the number of components needed to retain 95% of variance.*

### Key Finding

For the faces dataset used in this project:
- **129 components** are required to retain 95% of the variance
- Original dimensionality: 1024 pixels (32×32 images)
- Compression ratio: approximately **10:1** while preserving 95% of information

---

## Dimensionality Reduction

### Projection to Lower Dimensions

Data is projected onto the first $k$ principal components:

$$X_{reduced} = \tilde{X} \cdot V_k^T$$

where $V_k$ contains the first $k$ rows of $V^T$.

### Reconstruction from Reduced Representation

The original data can be approximated by projecting back:

$$X_{reconstructed} = X_{reduced} \cdot V_k + \bar{X}$$

![Reconstruction Comparison](/projects/pca_eigenfaces/reconstruction_comparison.png)
*Figure 5: Comparison of reconstruction quality with different numbers of components (k=5, 25, 50, 100).*

---

## Reconstruction Error Analysis

The **Mean Squared Error (MSE)** quantifies reconstruction quality:

$$MSE = \frac{1}{N} \sum_{i=1}^{N} \|x_i - \hat{x}_i\|^2$$

| Components (k) | MSE | Compression Ratio |
|----------------|-----|-------------------|
| 5 | ~700 | 204.8x |
| 10 | ~540 | 102.4x |
| 25 | ~350 | 40.96x |
| 50 | ~210 | 20.48x |
| 100 | ~110 | 10.24x |
| 200 | ~50 | 5.12x |
| 400 | ~15 | 2.56x |

![Reconstruction Error](/projects/pca_eigenfaces/reconstruction_error.png)
*Figure 6: Reconstruction error decreases as the number of principal components increases.*

---

## Practical Applications

### Face Recognition

Eigenfaces can be used as features for face recognition systems:
1. Project a new face onto the eigenface basis
2. Compare the reduced representation with known faces
3. Classify based on minimum distance in the reduced space

### Image Compression

PCA provides efficient storage and transmission of facial images:
- Store only the mean face and eigenfaces (shared across all images)
- For each face, store only the $k$ projection coefficients
- Reconstruct faces on demand

### Data Preprocessing

Dimensionality reduction before applying other ML algorithms:
- Reduces computational complexity
- Removes noise captured in low-variance components
- Prevents overfitting in high-dimensional spaces

---

## Implementation Details

### Dataset

- **Source**: `faces.mat` - Standard face dataset
- **Number of images**: 400 face images
- **Image dimensions**: 32 × 32 pixels (1024 features per image)
- **Data type**: Grayscale intensity values

### Algorithm Steps

1. Load and reshape face images into vectors
2. Compute the mean face and center the data
3. Perform SVD on the centered data matrix
4. Extract eigenfaces from the right singular vectors
5. Project data onto reduced basis
6. Reconstruct and analyze error

---

## Summary and Conclusions

### Key Results

| Metric | Value |
|--------|-------|
| Original Dimensions | 1024 |
| Reduced Dimensions (k=100) | 100 |
| Compression Ratio | 10.24x |
| Variance Retained | ~92% |
| Components for 95% Variance | 129 |

### Main Takeaways

1. **PCA effectively captures facial structure**: The first few eigenfaces represent the most important variations in face images
2. **Significant compression is achievable**: Reducing from 1024 to 100 dimensions preserves most visual quality
3. **Trade-off exists**: More components = better reconstruction but less compression
4. **The mean face is crucial**: It captures the average facial structure and must be preserved for reconstruction

---

## Tools and Technologies

The project was developed using **Python** with the following libraries:

- **NumPy & SciPy**: High-performance numerical computing and SVD
- **Matplotlib**: Visualization of eigenfaces and results
- **JAX**: Automatic differentiation (for related optimization tasks)

All figures presented in this document were generated by the author using these Python libraries.

---

**Author**: Miguel Planas Díaz (Erasmus+ Student)  
**Course**: Numerical Analysis for Machine Learning  
**Institution**: Politecnico di Milano  
**Academic Year**: 2024-25
