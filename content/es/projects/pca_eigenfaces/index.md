---
title: "Análisis de Componentes Principales y Eigenfaces"
tags:
  - math
  - polimi
  - python
  - computer-vision
date: 2024-12-20T10:00:00+01:00
draft: false
math: true
description: "Proyecto teórico y práctico sobre Análisis de Componentes Principales (PCA) aplicado al reconocimiento facial mediante Eigenfaces para la asignatura Numerical Analysis for Machine Learning en el Politecnico di Milano."
cover:
  image: /projects/pca_eigenfaces/eigenfaces_publication.png
  alt: Visualización de Eigenfaces
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

Este proyecto presenta un estudio exhaustivo del **Análisis de Componentes Principales (PCA)** aplicado al reconocimiento facial mediante el cálculo de **Eigenfaces**, desarrollado para la asignatura *Numerical Analysis for Machine Learning* en el Politecnico di Milano durante el curso académico 2024-25. El trabajo abarca los fundamentos matemáticos del PCA, la Descomposición en Valores Singulares (SVD) y aplicaciones prácticas en reducción de dimensionalidad y reconstrucción de imágenes.

---

## Introducción al PCA y Eigenfaces

El Análisis de Componentes Principales es una de las técnicas de reducción de dimensionalidad más fundamentales en machine learning y ciencia de datos. Cuando se aplica a imágenes faciales, el PCA produce lo que se conoce como **Eigenfaces** — un conjunto de imágenes base que capturan las variaciones más significativas en un conjunto de datos de rostros.

![Visión General de PCA Eigenfaces](/projects/pca_eigenfaces/eigenfaces_publication.png)
*Figura 1: Visión general del PCA aplicado a imágenes faciales mostrando rostros de muestra, rostro medio, eigenfaces, varianza explicada y calidad de reconstrucción.*

### Conceptos Clave

- **Reducción de Dimensionalidad**: Reducir el espacio de características preservando la máxima varianza
- **Componentes Principales**: Direcciones ortogonales de máxima varianza en los datos
- **Eigenfaces**: Componentes principales de conjuntos de datos de imágenes faciales
- **Reconstrucción**: Aproximar los datos originales usando representaciones reducidas

---

## Fundamentos Matemáticos

### Normalización de Datos

Antes de realizar el PCA, los datos deben centrarse restando el rostro medio de cada imagen:

$$\tilde{X} = X - \bar{X}$$

donde $\bar{X}$ es el vector del rostro medio calculado como el promedio de todas las imágenes faciales en el conjunto de datos.

### Descomposición en Valores Singulares

El PCA se calcula mediante la **Descomposición en Valores Singulares (SVD)** sobre el conjunto de datos normalizado:

$$\tilde{X} = U \Sigma V^T$$

Donde:
- $U$ es una matriz ortogonal $m \times m$ (vectores singulares izquierdos)
- $\Sigma$ es una matriz diagonal $m \times n$ de valores singulares
- $V^T$ es una matriz ortogonal $n \times n$ (vectores singulares derechos)

Las filas de $V^T$ corresponden a los **componentes principales** (eigenfaces), ordenados por su importancia (varianza explicada).

![Rostro Medio y Normalización](/projects/pca_eigenfaces/mean_face.png)
*Figura 2: Proceso de normalización de datos mostrando el rostro original, rostro medio y rostro normalizado.*

---

## Visualización de Eigenfaces

Los primeros componentes principales capturan los patrones de variación más significativos entre rostros:

| Componente | Captura |
|------------|---------|
| **PC 1** | Forma general del rostro e iluminación |
| **PC 2-4** | Variaciones estructurales principales |
| **PC 5-10** | Características faciales específicas |
| **PC 11+** | Detalles finos y ruido |

![Primeros 25 Eigenfaces](/projects/pca_eigenfaces/eigenfaces_grid.png)
*Figura 3: Los primeros 25 eigenfaces (componentes principales) mostrando la captura progresiva de características faciales.*

### Interpretación de Eigenfaces

Cada eigenface representa un "bloque de construcción" de la variación facial:
- **Eigenfaces de índice bajo**: Capturan características globales (forma del rostro, dirección de la luz)
- **Eigenfaces de índice medio**: Capturan características específicas (ojos, posición de la nariz)
- **Eigenfaces de índice alto**: Capturan detalles finos (arrugas, textura de la piel)

---

## Análisis de Varianza Explicada

La ratio de varianza explicada indica cuánta varianza total es capturada por cada componente principal:

$$\text{Ratio de Varianza Explicada}_i = \frac{\sigma_i^2}{\sum_{j=1}^{n} \sigma_j^2}$$

La **varianza explicada acumulada** ayuda a determinar cuántos componentes se necesitan para retener un porcentaje deseado de información:

![Varianza Explicada](/projects/pca_eigenfaces/variance.png)
*Figura 4: Varianza explicada acumulada mostrando el número de componentes necesarios para retener el 95% de la varianza.*

### Hallazgo Clave

Para el conjunto de datos de rostros utilizado en este proyecto:
- Se requieren **129 componentes** para retener el 95% de la varianza
- Dimensionalidad original: 1024 píxeles (imágenes de 32×32)
- Ratio de compresión: aproximadamente **10:1** preservando el 95% de la información

---

## Reducción de Dimensionalidad

### Proyección a Dimensiones Inferiores

Los datos se proyectan sobre los primeros $k$ componentes principales:

$$X_{reducido} = \tilde{X} \cdot V_k^T$$

donde $V_k$ contiene las primeras $k$ filas de $V^T$.

### Reconstrucción desde la Representación Reducida

Los datos originales pueden aproximarse proyectando de vuelta:

$$X_{reconstruido} = X_{reducido} \cdot V_k + \bar{X}$$

![Comparación de Reconstrucción](/projects/pca_eigenfaces/reconstruction_comparison.png)
*Figura 5: Comparación de la calidad de reconstrucción con diferentes números de componentes (k=5, 25, 50, 100).*

---

## Análisis del Error de Reconstrucción

El **Error Cuadrático Medio (MSE)** cuantifica la calidad de reconstrucción:

$$MSE = \frac{1}{N} \sum_{i=1}^{N} \|x_i - \hat{x}_i\|^2$$

| Componentes (k) | MSE | Ratio de Compresión |
|-----------------|-----|---------------------|
| 5 | ~700 | 204.8x |
| 10 | ~540 | 102.4x |
| 25 | ~350 | 40.96x |
| 50 | ~210 | 20.48x |
| 100 | ~110 | 10.24x |
| 200 | ~50 | 5.12x |
| 400 | ~15 | 2.56x |

![Error de Reconstrucción](/projects/pca_eigenfaces/reconstruction_error.png)
*Figura 6: El error de reconstrucción disminuye a medida que aumenta el número de componentes principales.*

---

## Aplicaciones Prácticas

### Reconocimiento Facial

Los eigenfaces pueden usarse como características para sistemas de reconocimiento facial:
1. Proyectar un nuevo rostro sobre la base de eigenfaces
2. Comparar la representación reducida con rostros conocidos
3. Clasificar basándose en la distancia mínima en el espacio reducido

### Compresión de Imágenes

El PCA proporciona almacenamiento y transmisión eficientes de imágenes faciales:
- Almacenar solo el rostro medio y los eigenfaces (compartidos entre todas las imágenes)
- Para cada rostro, almacenar solo los $k$ coeficientes de proyección
- Reconstruir rostros bajo demanda

### Preprocesamiento de Datos

Reducción de dimensionalidad antes de aplicar otros algoritmos de ML:
- Reduce la complejidad computacional
- Elimina el ruido capturado en componentes de baja varianza
- Previene el sobreajuste en espacios de alta dimensionalidad

---

## Detalles de Implementación

### Conjunto de Datos

- **Fuente**: `faces.mat` - Conjunto de datos estándar de rostros
- **Número de imágenes**: 400 imágenes faciales
- **Dimensiones de imagen**: 32 × 32 píxeles (1024 características por imagen)
- **Tipo de datos**: Valores de intensidad en escala de grises

### Pasos del Algoritmo

1. Cargar y reformatear imágenes faciales en vectores
2. Calcular el rostro medio y centrar los datos
3. Realizar SVD sobre la matriz de datos centrada
4. Extraer eigenfaces de los vectores singulares derechos
5. Proyectar datos sobre la base reducida
6. Reconstruir y analizar el error

---

## Resumen y Conclusiones

### Resultados Clave

| Métrica | Valor |
|---------|-------|
| Dimensiones Originales | 1024 |
| Dimensiones Reducidas (k=100) | 100 |
| Ratio de Compresión | 10.24x |
| Varianza Retenida | ~92% |
| Componentes para 95% Varianza | 129 |

### Conclusiones Principales

1. **El PCA captura efectivamente la estructura facial**: Los primeros eigenfaces representan las variaciones más importantes en imágenes faciales
2. **Es alcanzable una compresión significativa**: Reducir de 1024 a 100 dimensiones preserva la mayor parte de la calidad visual
3. **Existe un compromiso**: Más componentes = mejor reconstrucción pero menos compresión
4. **El rostro medio es crucial**: Captura la estructura facial promedio y debe preservarse para la reconstrucción

---

## Herramientas y Tecnologías

El proyecto fue desarrollado usando **Python** con las siguientes bibliotecas:

- **NumPy y SciPy**: Computación numérica de alto rendimiento y SVD
- **Matplotlib**: Visualización de eigenfaces y resultados
- **JAX**: Diferenciación automática (para tareas de optimización relacionadas)

Todas las figuras presentadas en este documento fueron generadas por el autor usando estas bibliotecas de Python.

---

**Autor**: Miguel Planas Díaz (Estudiante Erasmus+)  
**Asignatura**: Numerical Analysis for Machine Learning  
**Institución**: Politecnico di Milano  
**Curso Académico**: 2024-25
