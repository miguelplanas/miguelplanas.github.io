---
title: Análisis y Aplicaciones de la SVD
tags: ["udc", "python", "computer-vision", "math"]
date: 2025-11-12T12:19:52+01:00
draft: false
description: Estudio sobre la reducción de dimensionalidad, compresión de señales y extracción de componentes dinámicos usando SVD.
cover:
  image: /projects/svd_project/portada.png
  alt: Visualización de resultados SVD
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

Este repositorio documenta un estudio práctico sobre la **Descomposición en Valores Singulares (SVD)**, una herramienta fundamental en el Análisis Numérico. El proyecto no solo explora la teoría matemática detrás de la factorización $A=U\Sigma V^t$, sino que demuestra cómo esta operación abstracta es el motor de soluciones reales en ingeniería de datos, desde la compresión eficiente hasta la visión artificial.

El trabajo se divide en dos grandes bloques: una implementación constructiva del algoritmo desde cero y una serie de aplicaciones prácticas que explotan la capacidad de la SVD para separar "señal" de "ruido".

---

## 1. Implementación del Algoritmo

Aunque las librerías científicas modernas ya incluyen soluciones optimizadas, este proyecto aborda la implementación manual para comprender la estabilidad numérica y las transformaciones internas. El proceso se ha desglosado en tres etapas constructivas, tal como se detalla en la memoria técnica:

1.  **Reducción Bidiagonal:** Transformación de la matriz original $A$ a una forma bidiagonal $B$ mediante matrices ortogonales ($U_1$ y $V_1$). Esto simplifica la complejidad del problema.
2.  **SVD de la Matriz Bidiagonal:** Cálculo de los vectores singulares utilizando la relación con los autovectores de $B^TB$ y un proceso de ortonormalización.
3.  **Composición Final:** Reconstrucción de la factorización completa $A=U\Sigma V^T$.

Adicionalmente, se ha implementado la **Pseudoinversa de Moore-Penrose** ($A^+$), una aplicación directa que permite resolver problemas de mínimos cuadrados con alta precisión.

---

## 2. Aplicaciones Prácticas

La potencia de la SVD reside en su capacidad de generar aproximaciones de bajo rango. A continuación, se detallan los tres casos de uso implementados.

### Reducción de Dimensionalidad: PCA mediante SVD

El Análisis de Componentes Principales (PCA) suele asociarse a la matriz de covarianza, pero la SVD ofrece una ruta numérica más directa y eficiente.

En el notebook correspondiente, demostramos cómo los vectores singulares derechos ($V$) de una matriz de datos centrada actúan como las direcciones principales. Existe una relación directa donde los autovalores de la covarianza se derivan de los valores singulares:
$$\lambda_i = \frac{\sigma_i^2}{n-1}$$

Aplicando esto al dataset **MNIST** (dígitos manuscritos de 28x28 píxeles), logramos proyectar imágenes de alta dimensionalidad en un plano 2D. Como se observa en la figura, esto revela la estructura latente de los datos, agrupando los dígitos en clusters distinguibles sin necesidad de etiquetas previas.

![PCA sobre MNIST](/projects/svd_project/pca_mnist.png)
*Figura 1: Proyección de los dígitos de MNIST sobre los dos primeros componentes principales, mostrando la agrupación natural de las clases.*

### Compresión de Imágenes: La Energía de los Datos

Una imagen digital es, en esencia, una matriz de intensidades. La SVD nos permite descomponer esta matriz y truncar la serie de valores singulares, conservando solo los $k$ primeros. Esto se basa en que la información estructural se concentra en los primeros modos (valores singulares grandes), mientras que los detalles finos y el ruido decaen rápidamente.

En el experimento realizado con una imagen astronómica de alta resolución:
* **$k=10$:** Solo se intuye la iluminación general.
* **$k=50$:** La estructura es reconocible, pero falta nitidez.
* **$k=100$:** Se alcanza una convergencia visual casi perfecta respecto al original.

Esto supone un ahorro drástico de memoria: en lugar de almacenar $m \times n$ píxeles, solo necesitamos guardar $k(m+n+1)$ valores.

![Compresión SVD](/projects/svd_project/compresion.png)
*Figura 2: Evolución de la reconstrucción de imagen variando el rango $k$. Obsérvese cómo con $k=100$ la pérdida de información es prácticamente imperceptible al ojo humano.*

### Procesamiento de Video: Separación de Fondo

Quizás la aplicación más llamativa es el tratamiento de vídeo como una matriz gigante $M$, donde cada columna es un fotograma "aplanado".

La hipótesis es elegante: el fondo de una escena fija (como una cámara de vigilancia) es constante, lo que implica una alta correlación temporal y, por tanto, un **rango 1** (o muy bajo). Por el contrario, las personas u objetos en movimiento son perturbaciones dispersas.

El algoritmo implementado:
1.  Calcula la SVD de la matriz de vídeo.
2.  Reconstruye el fondo usando solo el **primer valor singular** ($\sigma_1$).
3.  Obtiene el primer plano (objetos en movimiento) mediante la sustracción: $M_{primer\_plano} = |M - M_{fondo}|$.

![Segmentación de fondo en vídeo](/projects/svd_project/portada.png)
*Figura 3: Izquierda: Frame original. Centro: Fondo estático recuperado (Rango 1). Derecha: Máscara de movimiento aislada.*

---

## Memoria del Proyecto

Para profundizar en la base teórica y consultar los detalles técnicos de la implementación, se adjunta el documento completo de la memoria del proyecto. En él se detallan las demostraciones matemáticas y la discusión extensa de los experimentos realizados.

{{< pdf "/projects/svd_project/memoria.pdf" >}}

---

## Estructura del Repositorio

El código está organizado secuencialmente para facilitar su estudio:

* `0_implementacion_svd.ipynb`: El "motor" del proyecto. Desarrollo del algoritmo paso a paso y validación de propiedades.
* `1_pca_usando_svd.ipynb`: Reducción de dimensionalidad aplicada al dataset de dígitos.
* `2_compresion_imagen.ipynb`: Análisis del espectro de valores singulares y compresión visual.
* `3_eliminacion_fondo.ipynb`: Separación de fuentes en secuencias de vídeo (Background Subtraction).

## Herramientas Utilizadas

La implementación se ha realizado en **Python**, combinando la potencia de cálculo matricial con herramientas de visualización:
* **NumPy y SciPy:** Para el manejo de estructuras matriciales complejas y álgebra lineal.
* **OpenCV:** Para la lectura y preprocesamiento de los flujos de vídeo e imágenes.
* **Matplotlib:** Para visualizar desde los clusters de datos hasta la comparativa visual de los fotogramas.

---
**Enlace al código:** [svd_project en GitHub](https://github.com/miguelplanas/svd_project)