---
title: "MXP AutoScan: Detección y Segmentación de Daños en Vehículos (Ganador Datathon 2025)"
tags: ["computer-vision", "deep-learning", "python", "fastapi", "pytorch", "awards"]
date: 2026-01-30T12:00:00+01:00
draft: false
description: "Sistema basado en Mask R-CNN que automatiza el peritaje de vehículos: detecta daños, segmenta componentes y genera presupuestos y reportes PDF de forma instantánea."
summary: "Sistema basado en Mask R-CNN que automatiza el peritaje de vehículos: detecta daños, segmenta componentes y genera presupuestos y reportes PDF de forma instantánea."
cover:
    image: "/projects/mxp_autoscan/webapp.png"
    alt: "Interfaz web de MXP AutoScan mostrando detección de daños en tiempo real"
    relative: false
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap; margin-bottom: 8px;">
  <a href="https://github.com/xoel-maestu/mxp_autoscan" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-mxp__autoscan-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <img src="https://img.shields.io/badge/Made_with-PyTorch-EE4C2C?style=for-the-badge&logo=pytorch" style="margin: 0;" />
  <img src="https://img.shields.io/badge/Backend-FastAPI-009688?style=for-the-badge&logo=fastapi" style="margin: 0;" />
</div>
<div style="font-size: 0.85em; opacity: 0.7; margin-top: 0px; margin-bottom: 20px;">
  <i>* Código fuente disponible próximamente.</i>
</div>

---

## Contexto del Proyecto

Este sistema fue desarrollado por **Pablo, Xoel y Miguel** como propuesta tecnológica para el reto de innovación impulsado por [DIHGIGAL](https://dihgigal.com/), el [ITG Technology Center](https://itg.es/) y la [Universidade da Coruña](https://www.udc.es/). La arquitectura y viabilidad de implementación de MXP AutoScan fueron reconocidas con el **primer premio** del certamen, destacando por su impacto directo en el sector automotriz y obteniendo [cobertura en prensa](https://www.elespanol.com/quincemil/economia/tecnologia/20260130/camino-datos-premia-mejores-soluciones-ia-universidad-coruna/1003744110955_0.html).

<div style="display: flex; justify-content: center; align-items: center; gap: 30px; margin: 20px 0; flex-wrap: wrap;">
  <img src="/projects/mxp_autoscan/logo.png" alt="Logo MXP AutoScan" style="max-width: 260px;">
  <img src="/projects/mxp_autoscan/equipo.jpg" alt="Equipo desarrollador de MXP AutoScan" style="max-width: 400px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);">
</div>

---

## Descripción del Proyecto

**MXP AutoScan** es un sistema de visión por computador para la detección y segmentación automática de daños y componentes en vehículos. Implementa una arquitectura **Mask R-CNN dual** —dos modelos independientes con backbone ResNet-50 + FPN— capaz de identificar **8 tipos de daños** y **21 categorías de piezas**, con una interfaz web para su uso en producción.

El objetivo es optimizar los procesos de peritaje generando, de forma automática, reportes técnicos y estimaciones de costes de reparación.

---

## Arquitectura del Sistema

El sistema se organiza en tres capas bien diferenciadas: una **interfaz web** (*Frontend*), una **API REST** implementada con FastAPI, y dos **modelos Mask R-CNN** independientes entrenados en PyTorch —uno especializado en 8 tipos de daños y otro en 21 categorías de piezas.

El **backend** gestiona el ciclo de vida de ambos modelos, la orquestación de la inferencia y la generación de reportes PDF mediante **ReportLab**. La **interfaz web** permite subir imágenes, ajustar los umbrales de confianza y visualizar las máscaras de segmentación superpuestas sobre la imagen original.

---

## Aspectos Técnicos Destacados

### Desbalanceo de clases

El conjunto de datos de entrenamiento (*Car parts and car damages*, CC0) presenta un **ratio 42:1** entre la clase más frecuente (*Scratch*, 3.242 instancias) y la menos frecuente (*Flaking*, 68 instancias). Además, el **71,5% de los daños** ocupa menos del 1% del área de la imagen, lo que hace especialmente difícil su detección.

Para mitigarlo se combinaron tres estrategias:

- **Oversampling estratégico:** factor de sobremuestreo de 10× para *Cracked* y 6× para *Corrosion*, expandiendo el conjunto de entrenamiento de 569 a 2.351 muestras efectivas.
- **Focal Loss** para penalizar más los ejemplos difíciles durante el entrenamiento.
- **Augmentaciones específicas** por clase: *ElasticTransform*, *GridDistortion* y *ColorJitter* para simular condiciones adversas de captura.

### Guardado multi-objetivo

Un problema clave detectado en las primeras versiones fue que el *early stopping* basado únicamente en `val_loss` detenía el entrenamiento antes de que el modelo aprendiera las clases minoritarias. La solución fue mantener **tres checkpoints simultáneos**:

| Checkpoint | Optimiza | Uso |
|------------|----------|-----|
| `best_minority` | Rendimiento en clases difíciles | Producción (recomendado) |
| `best_combined` | Balance general de métricas | Uso general |
| `best_loss` | Mínima pérdida de validación | Backup conservador |

Esta estrategia permitió la primera detección exitosa de *Cracked* (**25,18% AP50**), categoría que en versiones anteriores no se detectaba en absoluto.

### Motor de presupuestación

El sistema diferencia automáticamente entre dos modelos de coste:

| Caso | Criterio | Cálculo |
|------|----------|---------|
| **Sustitución** | Pieza inexistente o daño crítico | Precio de recambio + coste fijo de mano de obra |
| **Reparación** | Daño superficial leve o moderado | Área afectada (cm²) × tarifa según tipo de daño |

El área se calcula en cm² reales cuando el sistema detecta la matrícula del vehículo, utilizándola como referencia de calibración (52 × 11 cm).

![Ejemplo de presupuesto generado](/projects/mxp_autoscan/factura.png)
*Presupuesto generado automáticamente a partir de las detecciones.*

### Reporte técnico

Cada análisis genera un PDF con la imagen anotada, la tabla de daños (tipo, localización, severidad y área), la distribución de severidad y el presupuesto desglosado con IVA.

![Reporte técnico generado](/projects/mxp_autoscan/reporte.png)
*Informe técnico completo exportado por el sistema.*

---

## Stack Tecnológico

- **Deep Learning:** PyTorch, torchvision (Mask R-CNN + ResNet-50 + FPN)
- **Backend API:** FastAPI, Uvicorn, Pydantic
- **Procesamiento de imagen:** OpenCV, NumPy, Albumentations
- **Reportes:** ReportLab
- **Frontend:** Vanilla JavaScript (ES6+), CSS3, HTML5

---

**Equipo MXP** — Pablo, Xoel y Miguel  
Enero 2026 · [Universidade da Coruña](https://www.udc.es/)
