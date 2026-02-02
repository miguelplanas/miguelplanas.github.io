---
title: "Optimización por Descenso de Gradiente para Funciones de Coste Cuadráticas"
tags:
  - math
  - polimi
  - python
date: 2024-11-17T10:00:00+01:00
draft: false
math: true
description: "Proyecto teórico y práctico sobre Optimización por Descenso de Gradiente con diferenciación automática usando JAX para la asignatura Numerical Analysis for Machine Learning en el Politecnico di Milano."
cover:
  image: /projects/gradient_descent/gradient_descent_publication.png
  alt: Visualización de optimización por descenso de gradiente
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

## Resumen del Proyecto

Este proyecto presenta un estudio exhaustivo de la **Optimización por Descenso de Gradiente** para minimizar funciones de coste cuadráticas, desarrollado para la asignatura *Numerical Analysis for Machine Learning* en el Politecnico di Milano durante el curso académico 2024-25. El trabajo abarca los fundamentos matemáticos del descenso de gradiente, análisis de convergencia, selección de la tasa de aprendizaje e implementación práctica usando diferenciación automática con JAX.

---

## Introducción al Descenso de Gradiente

El Descenso de Gradiente es uno de los algoritmos de optimización más fundamentales en machine learning. Actualiza iterativamente los parámetros en la dirección de mayor descenso para minimizar una función de coste.

![Visión General del Descenso de Gradiente](/projects/gradient_descent/gradient_descent_publication.png)
*Figura 1: Visión general de la optimización por descenso de gradiente mostrando la trayectoria en gráficos de contorno y el efecto de diferentes tasas de aprendizaje en la convergencia.*

### Conceptos Clave

- **Función de Coste**: La función objetivo a minimizar
- **Gradiente**: Dirección de máximo ascenso (negativo para descenso)
- **Tasa de Aprendizaje**: Tamaño del paso para las actualizaciones de parámetros
- **Convergencia**: Cuando el algoritmo alcanza un mínimo

---

## Formulación del Problema

### Función de Coste de Mínimos Cuadrados

Consideramos minimizar una función de coste de la forma:

$$J(\mathbf{w}) = \sum_{i=1}^{n} \left( y_i - \mathbf{w}^T \mathbf{v}_i \right)^2$$

Este es un problema de **mínimos cuadrados** donde:
- $\mathbf{w}$ es el vector de parámetros a optimizar
- $\mathbf{v}_i$ son vectores de características
- $y_i$ son valores objetivo

### Parámetros del Problema

Para este proyecto, usamos valores específicos:

| Parámetro | Valor |
|-----------|-------|
| $\mathbf{v}_1$ | $[2, 1]^T$ |
| $\mathbf{v}_2$ | $[-1, 1]^T$ |
| $y_1$ | 0.5 |
| $y_2$ | 0.0 |

---

## Derivación de la Forma Cuadrática

### Conversión a Forma Estándar

Cualquier función de coste cuadrática puede escribirse como:

$$J(\mathbf{w}) = \mathbf{w}^T \mathbf{A} \mathbf{w} + \mathbf{d}^T \mathbf{w} + c$$

**Derivación:**

Expandiendo la formulación de mínimos cuadrados:

$$J(\mathbf{w}) = \underbrace{\left( (\mathbf{w}^T \mathbf{v}_1)^2 + (\mathbf{w}^T \mathbf{v}_2)^2 \right)}_{\text{término cuadrático}} - \underbrace{2 \left( y_1 \mathbf{w}^T \mathbf{v}_1 + y_2 \mathbf{w}^T \mathbf{v}_2 \right)}_{\text{término lineal}} + \underbrace{\left( y_1^2 + y_2^2 \right)}_{\text{término constante}}$$

### Parámetros de la Forma Cuadrática

La matriz $\mathbf{A}$ se construye a partir de los productos externos:

$$\mathbf{A} = \mathbf{v}_1 \mathbf{v}_1^T + \mathbf{v}_2 \mathbf{v}_2^T = \begin{bmatrix} 5 & 1 \\ 1 & 2 \end{bmatrix}$$

El vector $\mathbf{d}$ es:

$$\mathbf{d} = -2 \left( y_1 \mathbf{v}_1 + y_2 \mathbf{v}_2 \right) = \begin{bmatrix} -2 \\ -1 \end{bmatrix}$$

La constante $c = y_1^2 + y_2^2 = 0.25$

![Superficie de Coste Cuadrática](/projects/gradient_descent/cost_surface.png)
*Figura 2: Visualización 3D de la superficie de la función de coste cuadrática mostrando la forma de cuenco convexo.*

---

## Solución Exacta (Analítica)

### Derivando la Solución Óptima

Para una función cuadrática, el gradiente es:

$$\nabla J(\mathbf{w}) = 2\mathbf{A}\mathbf{w} + \mathbf{d}$$

Igualando el gradiente a cero y resolviendo:

$$2\mathbf{A}\mathbf{w}^* + \mathbf{d} = 0$$

$$\mathbf{w}^* = -\frac{1}{2}\mathbf{A}^{-1}\mathbf{d}$$

### Solución Calculada

Para nuestro problema:

$$\mathbf{w}^* = \begin{bmatrix} 0.1667 \\ 0.1667 \end{bmatrix}$$

Con coste mínimo $J(\mathbf{w}^*) \approx 0$

---

## Algoritmo de Descenso de Gradiente

### Regla de Actualización

La actualización del descenso de gradiente en la iteración $k$ es:

$$\mathbf{w}^{(k+1)} = \mathbf{w}^{(k)} - \eta \nabla J(\mathbf{w}^{(k)})$$

donde $\eta$ es la **tasa de aprendizaje** (tamaño del paso).

### Implementación con JAX

Usar la diferenciación automática de JAX elimina la necesidad de calcular gradientes manualmente:

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

![Trayectoria del Descenso de Gradiente](/projects/gradient_descent/trajectory.png)
*Figura 3: Trayectoria del descenso de gradiente en el gráfico de contorno de la función de coste, mostrando la convergencia desde el punto inicial al mínimo.*

---

## Análisis de Convergencia

### Tasa de Aprendizaje Máxima

Para funciones cuadráticas, la tasa de aprendizaje máxima estable está determinada por los autovalores de la matriz Hessiana:

$$\mathbf{H} = 2\mathbf{A}$$

La **tasa de aprendizaje máxima** para convergencia garantizada es:

$$\eta_{max} = \frac{2}{\lambda_{max}(\mathbf{H})} = \frac{1}{\lambda_{max}(\mathbf{A})}$$

### Análisis de Autovalores

| Métrica | Valor |
|---------|-------|
| $\lambda_{max}(\mathbf{A})$ | 5.303 |
| $\lambda_{min}(\mathbf{A})$ | 1.697 |
| $\eta_{max}$ | 0.189 |
| Número de Condición $\kappa$ | 3.12 |

### Idea Clave

- **Si $\eta < \eta_{max}$**: El algoritmo converge
- **Si $\eta > \eta_{max}$**: El algoritmo diverge
- **Menor número de condición**: Convergencia más rápida

---

## Efecto de la Tasa de Aprendizaje

La tasa de aprendizaje afecta críticamente el comportamiento de convergencia:

| Tasa de Aprendizaje | Comportamiento | Iteraciones |
|---------------------|----------------|-------------|
| $\eta = 0.01$ | Convergencia lenta | No converge en 100 |
| $\eta = 0.05$ | Convergencia moderada | 78 |
| $\eta = 0.10$ | Convergencia rápida | 35 |
| $\eta = 0.15$ | Convergencia más rápida | 31 |
| $\eta = 0.30$ | **Diverge** | N/A |

![Comparación de Tasas de Aprendizaje](/projects/gradient_descent/learning_rates.png)
*Figura 4: Comparación del comportamiento de convergencia para diferentes tasas de aprendizaje, mostrando el compromiso entre velocidad y estabilidad.*

### Observaciones

1. **$\eta$ demasiado pequeño**: Convergencia lenta, muchas iteraciones necesarias
2. **$\eta$ óptimo**: Convergencia rápida, comportamiento estable
3. **$\eta$ demasiado grande**: Divergencia, el coste crece exponencialmente

---

## Análisis de Divergencia

Cuando $\eta > \eta_{max}$, el algoritmo sobrepasa el mínimo y diverge:

![Ejemplo de Divergencia](/projects/gradient_descent/divergence.png)
*Figura 5: Función de coste explotando cuando la tasa de aprendizaje excede el valor máximo estable.*

### Explicación Matemática

Para $\eta > \eta_{max}$:
- El paso de actualización es demasiado grande
- El algoritmo "salta por encima" del mínimo
- Cada iteración se aleja más del óptimo
- El coste crece exponencialmente

---

## Visualización de Contornos

El gráfico de contorno revela la geometría del paisaje de optimización:

![Gráfico de Contorno con Trayectoria](/projects/gradient_descent/contour_trajectory.png)
*Figura 6: Líneas de contorno de la función de coste con trayectoria del descenso de gradiente mostrando el camino desde el inicio (verde) al óptimo (azul).*

### Interpretación

- **Contornos elípticos**: Indican el número de condición del problema
- **Elipses más alargadas**: Mayor número de condición, convergencia más lenta
- **Contornos circulares**: Condicionamiento óptimo, convergencia más rápida

---

## Resumen de Resultados Clave

### Solución del Problema

| Parámetro | Valor |
|-----------|-------|
| $\mathbf{w}^*$ óptimo | $[0.167, 0.167]^T$ |
| Coste mínimo $J(\mathbf{w}^*)$ | $\approx 0$ |
| Tasa de aprendizaje máxima $\eta_{max}$ | 0.189 |
| Número de condición $\kappa$ | 3.12 |

### Comparación de Convergencia

| Método | Iteraciones para Converger |
|--------|---------------------------|
| Exacto (analítico) | 1 (cálculo directo) |
| GD con $\eta = 0.15$ | 31 |
| GD con $\eta = 0.10$ | 35 |
| GD con $\eta = 0.05$ | 78 |

---

## Recomendaciones Prácticas

Basándonos en el análisis, recomendamos:

1. **Usar tasas de aprendizaje muy por debajo de $\eta_{max}$** para convergencia estable
2. **Tasas de aprendizaje más altas convergen más rápido** pero pueden oscilar
3. **El número de condición indica** cuán "alargada" está la superficie de coste
4. **Considerar tasas de aprendizaje adaptativas** (ej. Adam, RMSprop) para problemas no cuadráticos

---

## Implementación con JAX

### Diferenciación Automática

JAX proporciona diferenciación automática eficiente:

```python
import jax

# Definir función de coste
def J_func(w):
    term1 = (y1 - jnp.dot(w, v1)) ** 2
    term2 = (y2 - jnp.dot(w, v2)) ** 2
    return term1 + term2

# Calcular gradiente automáticamente
grad_J = jax.grad(J_func)
```

### Ventajas de JAX

- **Gradientes automáticos**: Sin cálculo manual de derivadas
- **Compilación JIT**: Velocidad de ejecución optimizada
- **Aceleración GPU**: Escalado transparente a problemas grandes
- **Programación funcional**: Código limpio y componible

---

## Conclusiones

### Conclusiones Principales

1. **Las funciones cuadráticas tienen soluciones analíticas**: Pero el descenso de gradiente proporciona insights sobre optimización iterativa
2. **La selección de la tasa de aprendizaje es crítica**: Muy pequeña = lento, muy grande = divergencia
3. **Existen límites de convergencia**: $\eta_{max}$ proporciona garantía teórica
4. **El número de condición importa**: Mayor condición = convergencia más lenta
5. **La diferenciación automática simplifica la implementación**: JAX elimina el cálculo manual de gradientes

### Extensiones

Este análisis se extiende a problemas más complejos:
- **Funciones no cuadráticas**: Mínimos locales, puntos de silla
- **Optimización de alta dimensión**: Aplicaciones de deep learning
- **Descenso de gradiente estocástico**: Aproximaciones por mini-lotes
- **Métodos adaptativos**: Adam, AdaGrad, RMSprop

---

## Herramientas y Tecnologías

El proyecto fue desarrollado usando **Python** con las siguientes bibliotecas:

- **NumPy**: Computación numérica de alto rendimiento
- **JAX**: Diferenciación automática y compilación JIT
- **Matplotlib**: Visualización de trayectorias de optimización

Todas las figuras presentadas en este documento fueron generadas por el autor usando estas bibliotecas de Python.

---

**Autor**: Miguel Planas Díaz (Estudiante Erasmus+)  
**Asignatura**: Numerical Analysis for Machine Learning  
**Institución**: Politecnico di Milano  
**Curso Académico**: 2024-25
