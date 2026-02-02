---
title: "Gradient Descent Optimization for Quadratic Cost Functions"
tags:
  - math
  - polimi
  - python
date: 2024-11-17T10:00:00+01:00
draft: false
math: true
description: "Theoretical and practical project on Gradient Descent Optimization with automatic differentiation using JAX for the Numerical Analysis for Machine Learning course at Politecnico di Milano."
cover:
  image: /projects/gradient_descent/gradient_descent_publication.png
  alt: Gradient Descent Optimization visualization
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

This project presents a comprehensive study of **Gradient Descent Optimization** for minimizing quadratic cost functions, developed for the *Numerical Analysis for Machine Learning* course at Politecnico di Milano during the 2024-25 academic year. The work covers the mathematical foundations of gradient descent, convergence analysis, learning rate selection, and practical implementation using JAX automatic differentiation.

---

## Introduction to Gradient Descent

Gradient Descent is one of the most fundamental optimization algorithms in machine learning. It iteratively updates parameters in the direction of steepest descent to minimize a cost function.

![Gradient Descent Overview](/projects/gradient_descent/gradient_descent_publication.png)
*Figure 1: Overview of gradient descent optimization showing the trajectory on contour plots and the effect of different learning rates on convergence.*

### Key Concepts

- **Cost Function**: The objective function to minimize
- **Gradient**: Direction of steepest ascent (negative for descent)
- **Learning Rate**: Step size for parameter updates
- **Convergence**: When the algorithm reaches a minimum

---

## Problem Formulation

### Least Squares Cost Function

We consider minimizing a cost function of the form:

$$J(\mathbf{w}) = \sum_{i=1}^{n} \left( y_i - \mathbf{w}^T \mathbf{v}_i \right)^2$$

This is a **least squares** problem where:
- $\mathbf{w}$ is the parameter vector to optimize
- $\mathbf{v}_i$ are feature vectors
- $y_i$ are target values

### Problem Parameters

For this project, we use specific values:

| Parameter | Value |
|-----------|-------|
| $\mathbf{v}_1$ | $[2, 1]^T$ |
| $\mathbf{v}_2$ | $[-1, 1]^T$ |
| $y_1$ | 0.5 |
| $y_2$ | 0.0 |

---

## Quadratic Form Derivation

### Converting to Standard Form

Any quadratic cost function can be written as:

$$J(\mathbf{w}) = \mathbf{w}^T \mathbf{A} \mathbf{w} + \mathbf{d}^T \mathbf{w} + c$$

**Derivation:**

Expanding the least squares formulation:

$$J(\mathbf{w}) = \underbrace{\left( (\mathbf{w}^T \mathbf{v}_1)^2 + (\mathbf{w}^T \mathbf{v}_2)^2 \right)}_{\text{quadratic term}} - \underbrace{2 \left( y_1 \mathbf{w}^T \mathbf{v}_1 + y_2 \mathbf{w}^T \mathbf{v}_2 \right)}_{\text{linear term}} + \underbrace{\left( y_1^2 + y_2^2 \right)}_{\text{constant term}}$$

### Quadratic Form Parameters

The matrix $\mathbf{A}$ is constructed from the outer products:

$$\mathbf{A} = \mathbf{v}_1 \mathbf{v}_1^T + \mathbf{v}_2 \mathbf{v}_2^T = \begin{bmatrix} 5 & 1 \\ 1 & 2 \end{bmatrix}$$

The vector $\mathbf{d}$ is:

$$\mathbf{d} = -2 \left( y_1 \mathbf{v}_1 + y_2 \mathbf{v}_2 \right) = \begin{bmatrix} -2 \\ -1 \end{bmatrix}$$

The constant $c = y_1^2 + y_2^2 = 0.25$

![Quadratic Cost Surface](/projects/gradient_descent/cost_surface.png)
*Figure 2: 3D visualization of the quadratic cost function surface showing the convex bowl shape.*

---

## Exact (Analytical) Solution

### Deriving the Optimal Solution

For a quadratic function, the gradient is:

$$\nabla J(\mathbf{w}) = 2\mathbf{A}\mathbf{w} + \mathbf{d}$$

Setting the gradient to zero and solving:

$$2\mathbf{A}\mathbf{w}^* + \mathbf{d} = 0$$

$$\mathbf{w}^* = -\frac{1}{2}\mathbf{A}^{-1}\mathbf{d}$$

### Computed Solution

For our problem:

$$\mathbf{w}^* = \begin{bmatrix} 0.1667 \\ 0.1667 \end{bmatrix}$$

With minimum cost $J(\mathbf{w}^*) \approx 0$

---

## Gradient Descent Algorithm

### Update Rule

The gradient descent update at iteration $k$ is:

$$\mathbf{w}^{(k+1)} = \mathbf{w}^{(k)} - \eta \nabla J(\mathbf{w}^{(k)})$$

where $\eta$ is the **learning rate** (step size).

### Implementation with JAX

Using JAX's automatic differentiation eliminates the need to manually compute gradients:

```python
import jax
import jax.numpy as jnp

def gradient_descent(x0, f, lr=0.1, tol=1e-6, max_iter=1000):
    grad = jax.grad(jax.jit(f))
    x = x0
    for i in range(max_iter):
        grad_val = grad(x)
        if jnp.linalg.norm(grad_val) < tol:
            return x
        x = x - lr * grad_val
    return x
```

![Gradient Descent Trajectory](/projects/gradient_descent/trajectory.png)
*Figure 3: Gradient descent trajectory on the contour plot of the cost function, showing convergence from initial point to minimum.*

---

## Convergence Analysis

### Maximum Learning Rate

For quadratic functions, the maximum stable learning rate is determined by the eigenvalues of the Hessian matrix:

$$\mathbf{H} = 2\mathbf{A}$$

The **maximum learning rate** for guaranteed convergence is:

$$\eta_{max} = \frac{2}{\lambda_{max}(\mathbf{H})} = \frac{1}{\lambda_{max}(\mathbf{A})}$$

### Eigenvalue Analysis

| Metric | Value |
|--------|-------|
| $\lambda_{max}(\mathbf{A})$ | 5.303 |
| $\lambda_{min}(\mathbf{A})$ | 1.697 |
| $\eta_{max}$ | 0.189 |
| Condition Number $\kappa$ | 3.12 |

### Key Insight

- **If $\eta < \eta_{max}$**: Algorithm converges
- **If $\eta > \eta_{max}$**: Algorithm diverges
- **Lower condition number**: Faster convergence

---

## Effect of Learning Rate

The learning rate critically affects convergence behavior:

| Learning Rate | Behavior | Iterations |
|---------------|----------|------------|
| $\eta = 0.01$ | Slow convergence | Does not converge in 100 |
| $\eta = 0.05$ | Moderate convergence | 78 |
| $\eta = 0.10$ | Fast convergence | 35 |
| $\eta = 0.15$ | Fastest convergence | 31 |
| $\eta = 0.30$ | **Diverges** | N/A |

![Learning Rate Comparison](/projects/gradient_descent/learning_rates.png)
*Figure 4: Comparison of convergence behavior for different learning rates, showing the trade-off between speed and stability.*

### Observations

1. **Too small $\eta$**: Slow convergence, many iterations needed
2. **Optimal $\eta$**: Fast convergence, stable behavior
3. **Too large $\eta$**: Divergence, cost increases exponentially

---

## Divergence Analysis

When $\eta > \eta_{max}$, the algorithm overshoots the minimum and diverges:

![Divergence Example](/projects/gradient_descent/divergence.png)
*Figure 5: Cost function exploding when learning rate exceeds the maximum stable value.*

### Mathematical Explanation

For $\eta > \eta_{max}$:
- The update step is too large
- The algorithm "jumps over" the minimum
- Each iteration moves further from the optimum
- The cost grows exponentially

---

## Contour Visualization

The contour plot reveals the geometry of the optimization landscape:

![Contour Plot with Trajectory](/projects/gradient_descent/contour_trajectory.png)
*Figure 6: Contour lines of the cost function with gradient descent trajectory showing the path from start (green) to optimum (blue).*

### Interpretation

- **Elliptical contours**: Indicate the condition number of the problem
- **More elongated ellipses**: Higher condition number, slower convergence
- **Circular contours**: Optimal conditioning, fastest convergence

---

## Key Results Summary

### Problem Solution

| Parameter | Value |
|-----------|-------|
| Optimal $\mathbf{w}^*$ | $[0.167, 0.167]^T$ |
| Minimum cost $J(\mathbf{w}^*)$ | $\approx 0$ |
| Maximum learning rate $\eta_{max}$ | 0.189 |
| Condition number $\kappa$ | 3.12 |

### Convergence Comparison

| Method | Iterations to Converge |
|--------|----------------------|
| Exact (analytical) | 1 (direct computation) |
| GD with $\eta = 0.15$ | 31 |
| GD with $\eta = 0.10$ | 35 |
| GD with $\eta = 0.05$ | 78 |

---

## Practical Recommendations

Based on the analysis, we recommend:

1. **Use learning rates well below $\eta_{max}$** for stable convergence
2. **Higher learning rates converge faster** but may oscillate
3. **The condition number indicates** how "elongated" the cost surface is
4. **Consider adaptive learning rates** (e.g., Adam, RMSprop) for non-quadratic problems

---

## Implementation with JAX

### Automatic Differentiation

JAX provides efficient automatic differentiation:

```python
import jax

# Define cost function
def J_func(w):
    term1 = (y1 - jnp.dot(w, v1)) ** 2
    term2 = (y2 - jnp.dot(w, v2)) ** 2
    return term1 + term2

# Compute gradient automatically
grad_J = jax.grad(J_func)
```

### Advantages of JAX

- **Automatic gradients**: No manual derivative computation
- **JIT compilation**: Optimized execution speed
- **GPU acceleration**: Seamless scaling to large problems
- **Functional programming**: Clean, composable code

---

## Conclusions

### Main Takeaways

1. **Quadratic functions have analytical solutions**: But gradient descent provides insights into iterative optimization
2. **Learning rate selection is critical**: Too small = slow, too large = divergence
3. **Convergence bounds exist**: $\eta_{max}$ provides theoretical guarantee
4. **Condition number matters**: Higher condition = slower convergence
5. **Automatic differentiation simplifies implementation**: JAX eliminates manual gradient computation

### Extensions

This analysis extends to more complex problems:
- **Non-quadratic functions**: Local minima, saddle points
- **High-dimensional optimization**: Deep learning applications
- **Stochastic gradient descent**: Mini-batch approximations
- **Adaptive methods**: Adam, AdaGrad, RMSprop

---

## Tools and Technologies

The project was developed using **Python** with the following libraries:

- **NumPy**: High-performance numerical computing
- **JAX**: Automatic differentiation and JIT compilation
- **Matplotlib**: Visualization of optimization trajectories

All figures presented in this document were generated by the author using these Python libraries.

---

**Author**: Miguel Planas Díaz (Erasmus+ Student)  
**Course**: Numerical Analysis for Machine Learning  
**Institution**: Politecnico di Milano  
**Academic Year**: 2024-25
