---
title: "Análisis y Aplicaciones de la Descomposición en Valores Singulares"
tags: ["uni", "python", "computer-vision", "math"]
date: 2026-01-23T12:19:52+01:00
draft: false
description: "Estudio sobre la reducción de dimensionalidad, compresión de señales y extracción de componentes dinámicos usando SVD."
cover:
    image: "portada.png"
    alt: "Visualización de resultados SVD"
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

## Resumen del Proyecto

Este repositorio documenta un estudio práctico sobre la **Descomposición en Valores Singulares (SVD)** y su versatilidad en el procesamiento de datos. El objetivo central es demostrar cómo una operación fundamental del álgebra lineal permite resolver problemas complejos de ingeniería, desde la optimización de almacenamiento hasta el análisis forense de vídeo.

---

## Fundamentos Técnicos

La SVD permite factorizar una matriz $A$ de dimensiones $m \times n$ en el producto $U \Sigma V^T$. En el contexto de este proyecto, se han explorado tres vertientes principales:

### 1. Reducción de Dimensionalidad (PCA)
Mediante el cálculo de los autovectores de la matriz de covarianza (derivados de la SVD), se ha implementado el Análisis de Componentes Principales para proyectar el dataset MNIST. Esto permite identificar la estructura latente de los datos en espacios de baja dimensión.

![PCA sobre MNIST](pca_mnist.png)
*Figura 1: Proyección de los dígitos de MNIST sobre los dos primeros componentes principales.*



### 2. Aproximación de Rango Bajo y Compresión
Se analiza la retención de energía de la imagen original en función del número de valores singulares preservados. La técnica permite comprimir la información visual descartando las componentes con menor peso estadístico sin comprometer la interpretabilidad de la imagen.

![Compresión SVD](compresion.png)
*Figura 2: Reconstrucción de imagen variando el número de valores singulares (k).*



### 3. Segmentación de Fondo en Vídeo
Se ha implementado un algoritmo que trata una secuencia de vídeo como una matriz de gran tamaño. Aplicando SVD, es posible separar la información de rango bajo (el fondo estático) de las perturbaciones dispersas (objetos en movimiento), una técnica fundamental en sistemas de vigilancia.

---

## Estructura del Repositorio

* `0_implementacion_svd.ipynb`: Desarrollo de la descomposición desde cero y cálculo de la pseudoinversa de Moore-Penrose.
* `1_pca_usando_svd.ipynb`: Flujo completo de reducción de dimensionalidad.
* `2_compresion_imagen.ipynb`: Algoritmos de reconstrucción de imágenes.
* `3_eliminacion_fondo.ipynb`: Procesamiento de secuencias de vídeo.

## Herramientas Utilizadas

La implementación se ha realizado íntegramente en **Python**, apoyándose en librerías de cálculo científico y visión artificial:
* **Cálculo Numérico:** NumPy y SciPy para la manipulación matricial de alto rendimiento.
* **Visión Artificial:** OpenCV para el tratamiento de flujos de vídeo y lectura de imágenes.
* **Visualización:** Matplotlib para la generación de gráficas técnicas y análisis de resultados.

---
**Enlace al código:** [svd_project en GitHub](https://github.com/miguelplanas/svd_project)