---
title: "HackUDC 2026: Reconocimiento Visual de Productos de Moda"
tags:
  - python
  - computer-vision
  - deep-learning
  - fastapi
  - awards
date: 2026-03-01T12:00:00+01:00
draft: false
description: "Pipeline de IA para asociar prendas en fotos de moda con un catálogo de 27k productos usando SigLIP, DINOv2 y fine-tuning contrastivo — desarrollado en HackUDC 2026 para el reto de INDITEXTECH."
summary: "Pipeline de IA para asociar prendas en fotos de moda con un catálogo de 27k productos usando SigLIP, DINOv2 y fine-tuning contrastivo — desarrollado en HackUDC 2026 para el reto de INDITEXTECH."
cover:
  image: /projects/hackudc_2026/landing.png
  alt: "Demo de MXNJ mostrando outfit intelligence con productos asociados"
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/HackUDC2026" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-HackUDC2026-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/HackUDC2026" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.12-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Resumen del Proyecto

**MXNJ** es un sistema de reconocimiento visual de productos desarrollado durante **HackUDC 2026** para el **reto de INDITEXTECH**. Dada una fotografía de moda con uno o varios modelos, el sistema identifica cada prenda y recupera las referencias correspondientes de un catálogo de 27.000 artículos — alcanzando un **69.66% de Recall@15**. Todo el stack funciona con modelos open source, sin ninguna llamada a APIs cerradas.

Equipo: [Xoel García](https://github.com/xoel-maestu) · [Jacobo Núñez](https://github.com/jacobo-nu) · [Nicolás Aller](https://github.com/nicolasallerponte) · [Miguel Planas](https://github.com/miguelplanas)

**Premios:** Mejor Proyecto de IA Open Source en HackUDC 2026 y 2ª Posición en el reto de INDITEXTECH (1 de marzo de 2026).

![Equipo MXNJ en HackUDC](/projects/hackudc_2026/foto1.jpg)
*Figura 1: El equipo durante el evento HackUDC 2026.*

---

## 1. Arquitectura del Pipeline

El sistema procesa cada foto de moda (denominada *bundle*) a través de cinco bloques:

1. **Ingeniería de Datos** — Extracción de SKU y timestamps de las URLs de las imágenes, filtrado de ruido (más de 40 categorías irrelevantes eliminadas) y restricciones por sección aprendidas de las etiquetas de entrenamiento.
2. **Segmentación Anatómica** — YOLOv8x-seg detecta personas y divide la imagen en 5 recortes por persona: cabeza, torso, piernas, pies y vista global.
3. **Embeddings Visuales** — SigLIP SO400M/384 (1152d) y DINOv2 ViT-L/14 (1024d) extraen vectores de características complementarios de cada recorte.
4. **Fine-Tuning Contrastivo** — Projection heads entrenadas con InfoNCE + Triplet loss en validación cruzada de 5 folds, ensamblando los 3 mejores.
5. **The Beast Search Engine** — Recuperación de candidatos con puntuación 85% SigLIP + 15% DINOv2, potenciada por heurísticas de timestamp, prefijo SKU y co-ocurrencia. Un bucle semi-supervisado de auto-expansión B2B enriquece el índice en 4 pasadas.

---

## 2. Stack de IA Open Source

Todos los modelos y frameworks son 100% open source:

| Modelo | Propósito | Dimensiones |
|---|---|---|
| **YOLOv8x-seg** (Ultralytics) | Detección de personas y segmentación anatómica | — |
| **SigLIP SO400M/384** (OpenCLIP) | Embeddings visuales primarios | 1152d |
| **DINOv2 ViT-L/14** (timm / Meta) | Embeddings visuales de ensemble | 1024d |
| **PyTorch** | Entrenamiento, inferencia, DataParallel | — |

**¿Por qué SigLIP + DINOv2?** SigLIP utiliza una función de pérdida sigmoide en lugar de softmax, produciendo representaciones visuales más ajustadas e ideales para la asociación de prendas a nivel fino. DINOv2 captura rasgos de textura y estructura mediante aprendizaje auto-supervisado. Juntos forman un ensemble complementario que supera a cualquiera de los modelos por separado.

---

## 3. Fine-Tuning Contrastivo

En lugar de usar los modelos como cajas negras, entrenamos projection heads sobre ambos embeddings:

- **Head SigLIP:** 1152 → 2048 → 2048 → 1152
- **Head DINOv2:** 1024 → 1536 → 1536 → 1024
- **Pérdida:** InfoNCE con negativos in-batch (batch size 1024) + 0.5 × Triplet Margin Loss con minería de negativos difíciles entre los top-200 productos visualmente similares que no coinciden
- **Entrenamiento:** AdamW con OneCycleLR (cosine annealing, 2 épocas de warmup), early stopping con paciencia=5

Esto desplaza el espacio genérico de preentrenamiento hacia un espacio de embeddings específico del dominio de moda, sin modificar los pesos del backbone — haciéndolo reproducible en hardware modesto.

---

## 4. Aplicación Demo

Una aplicación web independiente con FastAPI que visualiza los resultados del pipeline sobre el conjunto de test.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/hackudc_2026/outfits.png" alt="Galería de bundles mostrando todos los outfits">
    <p><em>Galería de bundles</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/hackudc_2026/outfit.png" alt="Detalle de predicción para un outfit">
    <p><em>Detalle de predicción con los top-15 productos asociados</em></p>
  </div>
</div>

---

## Stack Tecnológico

- **Deep Learning:** PyTorch, Ultralytics (YOLOv8), OpenCLIP (SigLIP), timm (DINOv2)
- **Data Science:** NumPy, Pandas, Scikit-learn
- **Demo:** FastAPI, Uvicorn, Jinja2
- **Infraestructura:** Kaggle (2× T4 GPUs), DataParallel

---

**Enlace al código:** [HackUDC2026 en GitHub](https://github.com/miguelplanas/HackUDC2026)
