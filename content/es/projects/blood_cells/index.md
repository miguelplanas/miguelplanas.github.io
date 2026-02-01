---
title: "Clasificación de Células Sanguíneas con Deep Learning"
tags: ["polimi", "python", "computer-vision"]
date: 2024-11-24T12:00:00+01:00
draft: false
description: "Implementación de arquitecturas CNN para la identificación de tipos celulares en imágenes microscópicas de frotis sanguíneos, integrando limpieza por hashing perceptual y fine-tuning."
cover:
    image: "/projects/blood_cells/portada.png"
    alt: "Clasificación de leucocitos mediante CNN"
    relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/anndl/tree/main/project1" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-anndl-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/anndl/tree/main/project1" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Resumen del Proyecto

Este proyecto aborda la **automatización del diagnóstico hematológico** mediante el desarrollo de un sistema de visión artificial capaz de clasificar diferentes tipos de células sanguíneas (leucocitos) a partir de imágenes microscópicas. La solución implementada utiliza arquitecturas de **Redes Neuronales Convolucionales (CNN)**, modelos preentrenados y un flujo de trabajo completo para asegurar la fiabilidad del modelo.

---

## 1. Curación y Calidad del Dataset

Uno de los mayores retos en imagen médica es el sesgo introducido por datos redundantes. En este proyecto, se implementó una fase crítica de **detección de duplicados mediante Hashing Perceptual**:

- **Técnica:** Uso de `imagehash` (dHash) para identificar imágenes casi idénticas que podrían contaminar los conjuntos de entrenamiento y test.
- **Resultado:** Limpieza de imágenes corruptas o repetidas, garantizando que la precisión del modelo refleje su capacidad real de generalización.

Como se puede apreciar en la siguiente imagen, hay una imagen repetida 1600 veces y otra bastante graciosa con la cara de Rick Astley repetida 200 veces
![Duplicados](/projects/blood_cells/portada.png)
*Figura 1: Detección de duplicados mediante Hashing Perceptual.*

Antes de comenzar el entrenamiento, se analizó el desbalanceo de clases para ajustar los pesos del modelo y asegurar un aprendizaje equitativo entre tipos celulares.

![Distribución de clases](/projects/blood_cells/distribucion.png)
*Figura 2: Distribución del dataset tras la limpieza y balanceo.*

---

## 2. Modelado y Entrenamiento

En primer lugar se diseñó una arquitectura CNN optimizada para la extracción de características morfológicas. El proceso incluyó:

1.  **Aumentación de Datos:** Generación de variaciones sintéticas (rotaciones, zoom, cambios de brillo) para robustecer el modelo ante variaciones en la captura de la muestra.
2.  **Arquitectura:** Bloques convolucionales con normalización por lotes (*Batch Normalization*) y regularización por *Dropout* para prevenir el sobreajuste.
3.  **Optimización:** Monitorización continua de métricas para ajustar el *Learning Rate* dinámicamente.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/blood_cells/accuracy.png" alt="Evolución de Precisión">
    <p><em>Precisión (Accuracy)</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/blood_cells/loss.png" alt="Evolución de Pérdida">
    <p><em>Pérdida (Loss)</em></p>
  </div>
</div>

*Figura 3: Evolución de métricas durante las fases de entrenamiento y validación.*


Con el objetivo de mejorar el rendimiento del modelo, se implementó un proceso de **Fine-Tuning**, tomando como base el **modelo preentrenado MobileNetV3**, congelando las capas inferiores para proteger el aprendizaje de características morfológicas, y ajustando los pesos de las capas superiores para adaptarlas al dataset específico.

---

## 3. Evaluación y Fine-Tuning

La efectividad de esta fase se puede observar en la mejora de las **Matrices de Confusión**, donde se redujeron los falsos positivos tras el *fine-tuning* de la red.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/blood_cells/confusion_matrix_PRE_FINETUNING.png" alt="Pre Fine-Tuning">
    <p><em>Pre Fine-Tuning</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/blood_cells/confusion_matrix_FINETUNING.png" alt="Post Fine-Tuning">
    <p><em>Post Fine-Tuning</em></p>
  </div>
</div>

---

## Stack Tecnológico

*   **Deep Learning:** Keras & TensorFlow.
*   **Visión Artificial:** OpenCV & PIL.
*   **Data Science:** NumPy, Pandas & Scikit-learn.
*   **Visualización:** Matplotlib & Seaborn.

---

## Memoria del Proyecto

{{< pdf "/projects/blood_cells/AN2DL_PROJECT1.pdf" >}}

---
**Enlace al código:** [anndl en GitHub](https://github.com/miguelplanas/anndl/tree/main/project1)
