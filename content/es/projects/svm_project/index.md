---
title: "Máquinas de Vectores de Soporte: Teoría y Aplicaciones"
tags:
  - math
  - polimi
  - python
date: 2025-03-16T12:19:52+01:00
draft: false
math: true
description: "Proyecto de desarrollo teórico sobre Máquinas de Vectores de Soporte para la asignatura Numerical Analysis for Machine Learning en el Politecnico di Milano."
cover:
  image: /projects/svm_project/kernel_method.png
  alt: Método kernel
  relative: true
---

## Resumen del Proyecto

Este proyecto presenta un estudio teórico exhaustivo de las **Máquinas de Vectores de Soporte (SVM)**, desarrollado para la asignatura *Numerical Analysis for Machine Learning* en el Politecnico di Milano durante el curso académico 2024-25. El trabajo abarca los fundamentos matemáticos de las SVM, desde la separación básica por hiperplanos hasta los métodos kernel avanzados, junto con aplicaciones prácticas en clasificación y regresión.

---

## Introducción a las Máquinas de Vectores de Soporte

Las Máquinas de Vectores de Soporte, introducidas por Boser, Guyon y Vapnik en 1992, surgieron como un potente método de clasificación basado en la **Teoría del Aprendizaje Estadístico**, desarrollada principalmente por Vladimir Vapnik y Alexey Chervonenkis. Las SVM ganaron protagonismo a finales de los años 90, convirtiéndose en un clasificador líder en diversos campos como el análisis de texto y la clasificación de datos genómicos.

![Problema de clasificación binaria](/projects/svm_project/fig1.jpg)
*Figura 1: Problema de clasificación binaria con dos clases (+1 y -1) en el espacio de características.*

### Ventajas Clave de las SVM

- **Robustez en datos de alta dimensionalidad**: Adecuadas para aplicaciones con grandes espacios de características
- **Sin óptimos locales**: A diferencia de las redes neuronales, las SVM garantizan un rendimiento fiable en diferentes conjuntos de datos
- **Control explícito de regularización**: Equilibrio entre complejidad del modelo y generalización
- **Funciones kernel personalizadas**: Amplían la aplicabilidad a tipos de datos no tradicionales

---

## Hiperplano y Margen

Las SVM funcionan encontrando un **hiperplano** que separa las observaciones en dos clases mientras maximiza el **margen** entre ellas. El hiperplano se define como:

$$w^T x + b = 0$$

Los **vectores de soporte** son los puntos de datos más cercanos al hiperplano, y son críticos porque:

- La posición del hiperplano está completamente determinada por los vectores de soporte
- Definen los límites del margen: $w^T x + b = \pm 1$
- Maximizar el margen reduce el sobreajuste

![Hiperplano y Márgenes SVM](/projects/svm_project/intro.png)
*Figura 2: Hiperplano SVM separando muestras positivas y negativas con los límites del margen.*

![Hiperplano, Márgenes y Vectores de Soporte](/projects/svm_project/intro2.png)
*Figura 3: Visualización detallada del hiperplano, márgenes y vectores de soporte con las regiones de cada clase.*

---

## Clasificación con SVM

### Problemas Linealmente Separables

Para datos linealmente separables, la SVM encuentra el hiperplano de separación óptimo resolviendo el problema de optimización:

$$\min \frac{1}{2} \|w\|^2$$

sujeto a: $y_i(w^T x_i + b) \geq 1$

### Problemas No Linealmente Separables: Márgenes Suaves

Los datos del mundo real a menudo no son perfectamente separables. Las SVM introducen **márgenes suaves** con variables de holgura $\xi_i$ para permitir cierta clasificación errónea:

$$\min \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{N} \xi_i$$

El parámetro de regularización $C$ controla el equilibrio:

- **C grande**: Margen más pequeño, menos errores (riesgo de sobreajuste)
- **C pequeño**: Margen más grande, mayor tolerancia a clasificaciones erróneas (mejor generalización)

![SVM con Margen Suave](/projects/svm_project/soft_margin.png)
*Figura 4: Comparación de SVM con margen suave para diferentes valores de C mostrando el compromiso entre tamaño del margen y clasificaciones erróneas.*

---

## Métodos Kernel

Para datos que no pueden separarse linealmente, las SVM utilizan el **truco del kernel** para proyectar los datos en un espacio de mayor dimensión donde la separación lineal es posible.

![Visualización del Método Kernel](/projects/svm_project/kernel_method.png)
*Figura 5: El truco del kernel proyecta datos no linealmente separables en un espacio de características de mayor dimensión donde un hiperplano lineal puede separar las clases.*

### Funciones Kernel Comunes

| Kernel | Fórmula |
|--------|---------|
| **Lineal** | $K(x_i, x_j) = x_i^T x_j$ |
| **Polinomial** | $K(x_i, x_j) = (\gamma x_i^T x_j + r)^d$ |
| **RBF (Gaussiano)** | $K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2)$ |
| **Sigmoide** | $K(x_i, x_j) = \tanh(\gamma x_i^T x_j + r)$ |

![Separación No Lineal](/projects/svm_project/nonlinear.png)
*Figura 6: La separación no lineal en el espacio de entrada (izquierda) se convierte en lineal en el espacio de características mediante la transformación kernel (derecha).*

---

## SVM para Regresión (SVR)

Mientras que la clasificación asigna etiquetas discretas, la **Regresión con Vectores de Soporte** predice valores continuos. La diferencia clave radica en cómo se manejan los datos respecto al margen:

- **Clasificación**: Los puntos de datos se colocan *fuera* del margen
- **Regresión**: Los puntos de datos pueden estar *dentro* de un tubo ε alrededor de la predicción

El problema de optimización SVR se convierte en:

$$\min \frac{1}{2} \|w\|^2 + C \sum_{i=1}^{N} (\xi_i + \xi_i^*)$$

sujeto a desviaciones limitadas por el parámetro de tolerancia $\epsilon$.

![Clasificación vs Regresión](/projects/svm_project/class_reg.png)
*Figura 7: Comparación entre SVM para clasificación (izquierda) y regresión (derecha) mostrando las diferentes interpretaciones del margen.*

![Predicción de Precios de Acciones](/projects/svm_project/stockpred.png)
*Figura 8: Ejemplo de Regresión con Vectores de Soporte aplicada a la predicción de precios de acciones.*

---

## Aplicaciones en el Mundo Real

Las SVM han demostrado ser herramientas versátiles en diversos dominios:

### Procesamiento de Imágenes
- **Detección de Objetos**: Detección de rostros y peatones
- **Clasificación de Imágenes**: Categorización de imágenes en categorías predefinidas

![Clasificación Iris](/projects/svm_project/irisclassification.png)
*Figura 9: Comparación de clasificadores SVM en el conjunto de datos Iris utilizando diferentes kernels (Lineal, LinearSVC, RBF y Polinomial).*

### Bioinformática
- **Clasificación Genética**: Diagnóstico de cáncer a partir de datos de expresión génica
- **Predicción de Estructura de Proteínas**: Predicción de estructuras secundarias y terciarias

### Análisis de Texto y Sentimiento
- **Detección de Spam**: Clasificación de correos electrónicos
- **Análisis de Sentimiento**: Análisis de redes sociales y reseñas

### Aplicaciones Financieras
- **Detección de Fraude**: Identificación de transacciones fraudulentas
- **Predicción del Mercado de Valores**: Predicción de tendencias y precios de activos

---

## Memoria del Proyecto

Para una comprensión exhaustiva de los fundamentos matemáticos y los detalles técnicos, la memoria completa del proyecto está disponible a continuación:

{{< pdf "/reports/PROJECT_REPORT_MIGUEL.pdf" >}}

---

## Herramientas y Tecnologías

El proyecto fue desarrollado utilizando **Python** con las siguientes bibliotecas:

- **NumPy y SciPy**: Computación numérica de alto rendimiento
- **Scikit-learn**: Implementación de SVM y evaluación de modelos

Todas las figuras presentadas en este documento (excepto dos imágenes citadas) fueron generadas por el autor utilizando estas bibliotecas de Python.

---

**Asignatura**: Numerical Analysis for Machine Learning  
**Institución**: Politecnico di Milano  
**Curso Académico**: 2024-25
