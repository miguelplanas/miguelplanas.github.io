---
title: "MXP AutoScan: Vehicle Damage Detection and Segmentation (Datathon 2025 Winner)"
tags: ["computer-vision", "deep-learning", "python", "fastapi", "pytorch", "awards"]
date: 2026-01-30T12:00:00+01:00
draft: false
description: "System based on Mask R-CNN that automates vehicle appraisal: detects damages, segments components, and generates instant PDFs with repair estimates."
summary: "System based on Mask R-CNN that automates vehicle appraisal: detects damages, segments components, and generates instant PDFs with repair estimates."
cover:
    image: "/projects/mxp_autoscan/webapp.png"
    alt: "MXP AutoScan web interface showing real-time damage detection"
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
  <i>* Source code available soon.</i>
</div>

---

## Project Context

This system was developed by **Pablo, Xoel, and Miguel** as a technological proposal for the innovation challenge driven by [DIHGIGAL](https://dihgigal.com/), the [ITG Technology Center](https://itg.es/), and the [Universidade da Coruña](https://www.udc.es/). The architecture and implementation viability of MXP AutoScan were recognized with the **first prize** of the competition, standing out for its direct impact on the automotive sector and gaining [press coverage](https://www.elespanol.com/quincemil/economia/tecnologia/20260130/camino-datos-premia-mejores-soluciones-ia-universidad-coruna/1003744110955_0.html).

<div style="display: flex; justify-content: center; align-items: center; gap: 30px; margin: 20px 0; flex-wrap: wrap;">
  <img src="/projects/mxp_autoscan/logo.png" alt="MXP AutoScan Logo" style="max-width: 260px;">
  <img src="/projects/mxp_autoscan/equipo.jpg" alt="MXP AutoScan development team" style="max-width: 400px; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);">
</div>

---

## Project Description

**MXP AutoScan** is a computer vision system for the automatic detection and segmentation of vehicle damages and components. It implements a **dual Mask R-CNN** architecture —two independent models with a ResNet-50 + FPN backbone— capable of identifying **8 types of damages** and **21 categories of parts**, featuring a web interface for production use.

The goal is to optimize the appraisal processes by automatically generating technical reports and repair cost estimates.

---

## System Architecture

The system is organized into three distinct layers: a **web interface** (*Frontend*), a **REST API** implemented with FastAPI, and two independent **Mask R-CNN models** trained in PyTorch —one specialized in 8 types of damages and another in 21 part categories.

The **backend** manages the life cycle of both models, the orchestration of inference, and the generation of PDF reports using **ReportLab**. The **web interface** allows users to upload images, adjust confidence thresholds, and visualize segmentation masks overlaid on the original image.

---

## Technical Highlights

### Class Imbalance

The training dataset (*Car parts and car damages*, CC0) presents a **42:1 ratio** between the most frequent class (*Scratch*, 3,242 instances) and the least frequent (*Flaking*, 68 instances). Furthermore, **71.5% of the damages** occupy less than 1% of the image area, making their detection particularly difficult.

To mitigate this, three strategies were combined:

- **Strategic Oversampling:** 10× oversampling factor for *Cracked* and 6× for *Corrosion*, expanding the training set from 569 to 2,351 effective samples.
- **Focal Loss** to heavily penalize hard examples during training.
- **Class-specific augmentations:** *ElasticTransform*, *GridDistortion*, and *ColorJitter* to simulate adverse capture conditions.

### Multi-objective Saving

A key problem detected in early versions was that *early stopping* based solely on `val_loss` halted the training before the model could learn the minority classes. The solution was to maintain **three simultaneous checkpoints**:

| Checkpoint | Optimizes | Use Case |
|------------|-----------|----------|
| `best_minority` | Performance on difficult classes | Production (recommended) |
| `best_combined` | Overall metric balance | General use |
| `best_loss` | Minimum validation loss | Conservative backup |

This strategy enabled the first successful detection of *Cracked* (**25.18% AP50**), a category that was not detected at all in previous versions.

### Budgeting Engine

The system automatically differentiates between two cost models:

| Scenario | Criterion | Calculation |
|----------|-----------|-------------|
| **Replacement** | Missing part or critical damage | Spare part price + fixed labor cost |
| **Repair** | Mild or moderate superficial damage | Affected area (cm²) × rate based on damage type |

The area is calculated in real cm² when the system detects the vehicle's license plate, using it as a calibration reference (52 × 11 cm).

![Example of generated budget](/projects/mxp_autoscan/factura.png)
*Budget automatically generated from the detections.*

### Technical Report

Each analysis generates a PDF with the annotated image, the damage table (type, location, severity, and area), the severity distribution, and the detailed budget including VAT.

![Generated technical report](/projects/mxp_autoscan/reporte.png)
*Complete technical report exported by the system.*

---

## Technology Stack

- **Deep Learning:** PyTorch, torchvision (Mask R-CNN + ResNet-50 + FPN)
- **Backend API:** FastAPI, Uvicorn, Pydantic
- **Image Processing:** OpenCV, NumPy, Albumentations
- **Reports:** ReportLab
- **Frontend:** Vanilla JavaScript (ES6+), CSS3, HTML5

---

**MXP Team** — Pablo, Xoel, and Miguel  
January 2026 · [Universidade da Coruña](https://www.udc.es/)
