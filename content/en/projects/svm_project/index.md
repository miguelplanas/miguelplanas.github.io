---
title: "Support Vector Machines: Theory and Applications"
tags:
  - math
  - polimi
  - python
date: 2025-03-16T12:19:52+01:00
draft: false
math: true
description: "Theoretical development project on Support Vector Machines for the Numerical Analysis for Machine Learning course at Politecnico di Milano."
cover:
  image: /projects/svm_project/kernel_method.png
  alt: Kernel method visualization
  relative: true
---

## Project Overview

This project presents a comprehensive theoretical study of **Support Vector Machines (SVMs)**, developed for the *Numerical Analysis for Machine Learning* course at Politecnico di Milano during the 2024-25 academic year. The work covers the mathematical foundations of SVMs, from basic hyperplane separation to advanced kernel methods, along with practical applications in classification and regression.

---

## Introduction to Support Vector Machines

Support Vector Machines, introduced by Boser, Guyon, and Vapnik in 1992, emerged as a powerful classification method rooted in **Statistical Learning Theory**, primarily developed by Vladimir Vapnik and Alexey Chervonenkis. SVMs gained prominence in the late 1990s, becoming a leading classifier across various fields including text analysis and genomic data classification.

![Binary classification problem](/projects/svm_project/fig1.jpg)
*Figure 1: Binary classification problem with two classes (+1 and -1) in feature space.*

### Key Advantages of SVMs

- **Robustness in high-dimensional data**: Suitable for applications with large feature spaces
- **No local optima**: Unlike neural networks, SVMs ensure reliable performance across datasets
- **Explicit regularization control**: Balance between model complexity and generalization
- **Custom kernel functions**: Extend applicability to non-traditional data types

---

## Hyperplane and Margin

SVMs operate by finding a **hyperplane** that separates observations into two classes while maximizing the **margin** between them. The hyperplane is defined as:

$$w^T x + b = 0$$

The **support vectors** are data points lying closest to the hyperplane, and they are critical because:

- The hyperplane position is entirely determined by support vectors
- They define the margin boundaries: $w^T x + b = \pm 1$
- Maximizing the margin reduces overfitting

![SVM Hyperplane and Margins](/projects/svm_project/intro.png)
*Figure 2: SVM hyperplane separating positive and negative samples with margin boundaries.*

![Hyperplane, Margins and Support Vectors](/projects/svm_project/intro2.png)
*Figure 3: Detailed visualization of the hyperplane, margins, and support vectors with class regions.*

---

## Classification with SVM

### Linearly Separable Problems

For linearly separable data, SVM finds the optimal separating hyperplane by solving the optimization problem:

$$\min \frac{1}{2} \|w\|^2$$

subject to: $y_i(w^T x_i + b) \geq 1$

### Non-Linearly Separable Problems: Soft Margins

Real-world data is often not perfectly separable. SVMs introduce **soft margins** with slack variables $\xi_i$ to allow some misclassification:

$$\min \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{N} \xi_i$$

The regularization parameter $C$ controls the trade-off:

- **Large C**: Smaller margin, fewer errors (risk of overfitting)
- **Small C**: Larger margin, more tolerance for misclassifications (better generalization)

![Soft Margin SVM](/projects/svm_project/soft_margin.png)
*Figure 4: Comparison of soft margin SVM with different C values showing the trade-off between margin size and misclassifications.*

---

## Kernel Methods

For data that cannot be linearly separated, SVMs use the **kernel trick** to map data into a higher-dimensional space where linear separation becomes possible.

![Kernel Method Visualization](/projects/svm_project/kernel_method.png)
*Figure 5: The kernel trick maps non-linearly separable data into a higher-dimensional feature space where a linear hyperplane can separate the classes.*

### Common Kernel Functions

| Kernel | Formula |
|--------|---------|
| **Linear** | $K(x_i, x_j) = x_i^T x_j$ |
| **Polynomial** | $K(x_i, x_j) = (\gamma x_i^T x_j + r)^d$ |
| **RBF (Gaussian)** | $K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2)$ |
| **Sigmoid** | $K(x_i, x_j) = \tanh(\gamma x_i^T x_j + r)$ |

![Non-linear Separation](/projects/svm_project/nonlinear.png)
*Figure 6: Non-linear separation in input space (left) becomes linear in feature space via kernel transformation (right).*

---

## SVM for Regression (SVR)

While classification assigns discrete labels, **Support Vector Regression** predicts continuous values. The key difference lies in how data is handled relative to the margin:

- **Classification**: Data points placed *outside* the margin
- **Regression**: Data points allowed *within* an ε-tube around the prediction

The SVR optimization problem becomes:

$$\min \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{N} (\xi_i + \xi_i^*)$$

subject to deviations constrained by tolerance parameter $\epsilon$.

![Classification vs Regression](/projects/svm_project/class_reg.png)
*Figure 7: Comparison between SVM for classification (left) and regression (right) showing the different margin interpretations.*

![Stock Price Prediction](/projects/svm_project/stockpred.png)
*Figure 8: Example of Support Vector Regression applied to stock price prediction.*

---

## Real-World Applications

SVMs have proven to be versatile tools in various domains:

### Image Processing
- **Object Detection**: Face and pedestrian detection
- **Image Classification**: Categorizing images into predefined categories

![Iris Classification](/projects/svm_project/irisclassification.png)
*Figure 9: Comparison of SVM classifiers on the Iris dataset using different kernels (Linear, LinearSVC, RBF, and Polynomial).*

### Bioinformatics
- **Gene Classification**: Cancer diagnosis from gene expression data
- **Protein Structure Prediction**: Secondary and tertiary structure prediction

### Text and Sentiment Analysis
- **Spam Detection**: Email classification
- **Sentiment Analysis**: Social media and review analysis

### Financial Applications
- **Fraud Detection**: Identifying fraudulent transactions
- **Stock Market Prediction**: Market trend and asset price prediction

---

## Project Report

For a comprehensive understanding of the mathematical foundations and technical details, the full project report is available below:

{{< pdf "/reports/PROJECT_REPORT_MIGUEL.pdf" >}}

---

## Tools and Technologies

The project was developed using **Python** with the following libraries:

- **NumPy & SciPy**: High-performance numerical computing
- **Scikit-learn**: SVM implementation and model evaluation

All figures presented in this document (except two cited images) were generated by the author using these Python libraries.

---

**Course**: Numerical Analysis for Machine Learning  
**Institution**: Politecnico di Milano  
**Academic Year**: 2024-25
