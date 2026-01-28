---
title: "Segmentación Semántica de Suelos con Deep Learning"
tags: ["uni", "python", "computer-vision"]
date: 2026-01-25T12:00:00+01:00
draft: false
description: "Desarrollo de arquitecturas UNet y Atrous spatial pyramid pooling enhanced CNN (ASPP) para la segmentación multiclase de terrenos"
cover:
    image: "dup_aliens.png"
    alt: "Segmentación de tipos de suelo"
    relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/anndl/tree/main/project2" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-anndl-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/anndl/tree/main/project2" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Resumen del Proyecto

Este proyecto se centra en el desarrollo de un sistema de **segmentación semántica** para la identificación automática de distintos tipos de suelo y estructuras geológicas en imágenes. Utilizando arquitecturas de última generación como **UNet** y bloques **ASPP**, el sistema es capaz de clasificar cada píxel de la imagen en categorías como roca, arena, grava o fondo, facilitando el análisis topográfico y la navegación autónoma en entornos inexplorados.

El flujo de trabajo abarca desde la limpieza rigurosa del dataset original hasta la implementación de funciones de pérdida personalizadas para combatir el desbalanceo de clases.

---

## 1. Análisis de Datos exploratorio

La calidad de las máscaras de segmentación es crítica. En la fase inicial, se realizó un **Análisis Exploratorio de Datos (EDA)** profundo que reveló la presencia de "anomalías" en el dataset de entrenamiento:

- **Limpieza de Duplicados:** Mediante Hashing Perceptual, se identificaron y eliminaron imágenes redundantes que hubieran sesgado el aprendizaje.
- **Curiosidades del Dataset:** Al igual que en otros proyectos, se detectaron entradas humorísticas infiltradas en los datos técnicos, como esta "cara de alien" repetida deliberadamente en el set de entrenamiento.

![Detección de duplicados y anomalías](dup_aliens.png)
*Figura 1: Detección de duplicados mediante hashing, revelando intrusiones humorísticas en el dataset.*

---

## 2. Arquitecturas Implementadas

Se exploraron dos enfoques principales para maximizar la precisión de los bordes y la captura de contextos a diferentes escalas:

- **UNet:** Arquitectura simétrica de codificador-decodificador con conexiones de salto (*skip connections*), ideal para recuperar la resolución espacial perdida durante la convolución.
- **ASPP (Atrous Spatial Pyramid Pooling):** Implementación de convoluciones dilatadas (*atrous convolutions*) a diferentes tasas para capturar información multiescala sin aumentar el número de parámetros de forma drástica.

---

## 3. Estrategias de Entrenamiento

Para lograr una convergencia estable y un MeanIoU (Intersection over Union) competitivo, se aplicaron técnicas avanzadas:

- **Focal Loss:** Sustitución de la entropía cruzada tradicional por Focal Loss para penalizar más los errores en las clases minoritarias (ej. rocas pequeñas frente a extensiones de arena).
- **Callbacks Dinámicos:** Uso de `ReduceLROnPlateau` para ajustar la tasa de aprendizaje al detectar estancamientos y `ModelCheckpoint` para preservar el estado con mejor validación.
- **Data Augmentation:** Implementación de transformaciones geométricas y de color (CutMix) para mejorar la robustez del modelo.

---

## 4. Monitorización y Evaluación

Todo el proceso de entrenamiento fue monitorizado en tiempo real a través de **Weights & Biases (W&B)**, permitiendo comparar visualmente la evolución de las métricas y la calidad de las predicciones en el conjunto de validación.

- **Evaluación Final:** Se calculó el **MeanIoU** (Intersection over Union) por clase, siendo esta la métrica estándar para medir el solapamiento entre la predicción y el suelo real.

![Resultados finales](portada.png)
*Figura 2: Resultados finales del modelo.*

---

## Stack Tecnológico

*   **Deep Learning:** Keras & TensorFlow.
*   **Gestión de Experimentos:** Weights & Biases (wandb).
*   **Procesamiento:** NumPy, SciPy & OpenCV.
*   **Métricas y Análisis:** Scikit-learn & Pandas.

---

## Memoria del Proyecto

{{< pdf "AN2DL_PROJECT2.pdf" >}}

---
**Enlace al código:** [anndl en GitHub](https://github.com/miguelplanas/anndl/tree/main/project2)
