---
title: "HackUDC 2026: Fashion Visual Product Recognition"
tags:
  - python
  - computer-vision
  - deep-learning
  - fastapi
  - awards
date: 2026-03-01T12:00:00+01:00
draft: true
description: "AI pipeline for matching garments in fashion photos to a 27k-item product catalogue using SigLIP, DINOv2 and contrastive fine-tuning — built at HackUDC 2026 for the INDITEXTECH challenge."
summary: "AI pipeline for matching garments in fashion photos to a 27k-item product catalogue using SigLIP, DINOv2 and contrastive fine-tuning — built at HackUDC 2026 for the INDITEXTECH challenge."
cover:
  image: /projects/hackudc_2026/landing.png
  alt: "MXNJ demo app showing outfit intelligence with matched products"
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

## Project Summary

**MXNJ** is a visual product recognition system built during **HackUDC 2026** for the **INDITEXTECH challenge**. Given a fashion photograph with one or more models, the system identifies every garment being worn and retrieves the matching product references from a 27,000-item catalogue — achieving **69.66% Recall@15**. The entire stack runs on open-source models with zero closed-source API calls.

Team: [Xoel García](https://github.com/xoel-maestu) · [Jacobo Núñez](https://github.com/jacobo-nu) · [Nicolás Aller](https://github.com/nicolasallerponte) · [Miguel Planas](https://github.com/miguelplanas)

**Awards:** Best Open Source AI Project at HackUDC 2026 and 2nd Place in the INDITEXTECH Challenge (March 1, 2026).

![MXNJ Team at HackUDC](/projects/hackudc_2026/foto1.jpg)
*Figure 1: The team during the HackUDC 2026 event.*

---

## 1. Pipeline Architecture

The system processes each fashion photo (called a *bundle*) through five blocks:

1. **Data Engineering** — SKU and timestamp extraction from image URLs, noise filtering (40+ irrelevant categories removed), and section constraints learned from training labels.
2. **Anatomical Segmentation** — YOLOv8x-seg detects people and splits the image into 5 crops per person: head, torso, legs, feet, and a global view.
3. **Visual Embeddings** — SigLIP SO400M/384 (1152d) and DINOv2 ViT-L/14 (1024d) extract complementary feature vectors from each crop.
4. **Contrastive Fine-Tuning** — Projection heads trained with InfoNCE + Triplet loss on 5-fold cross-validation, ensembling the 3 best folds.
5. **The Beast Search Engine** — Candidate retrieval with 85% SigLIP + 15% DINOv2 scoring, boosted by timestamp, SKU prefix, and co-occurrence heuristics. A semi-supervised B2B self-expansion loop enriches the index over 4 passes.

---

## 2. Open-Source AI Stack

Every model and framework is 100% open source:

| Model | Purpose | Dimensions |
|---|---|---|
| **YOLOv8x-seg** (Ultralytics) | Person detection & anatomical segmentation | — |
| **SigLIP SO400M/384** (OpenCLIP) | Primary visual embeddings | 1152d |
| **DINOv2 ViT-L/14** (timm / Meta) | Ensemble visual embeddings | 1024d |
| **PyTorch** | Training, inference, DataParallel | — |

**Why SigLIP + DINOv2?** SigLIP uses sigmoid loss instead of softmax, producing tighter visual representations ideal for fine-grained clothing matching. DINOv2 captures texture and structural features through self-supervised learning. Together they form a complementary ensemble that outperforms either model alone.

---

## 3. Contrastive Fine-Tuning

Rather than using the models as black boxes, we train projection heads on top of both embeddings:

- **SigLIP Head:** 1152 → 2048 → 2048 → 1152
- **DINOv2 Head:** 1024 → 1536 → 1536 → 1024
- **Loss:** InfoNCE with in-batch negatives (batch size 1024) + 0.5 × Triplet Margin Loss with hard negative mining from top-200 similar non-matching products
- **Training:** AdamW with OneCycleLR (cosine annealing, 2 warmup epochs), early stopping with patience=5

This moves the generic pretraining space into a domain-specific fashion embedding space without touching backbone weights — making it reproducible on modest hardware.

---

## 4. Demo Application

A standalone FastAPI web app visualises the pipeline results on the test set.

<div style="display: flex; justify-content: space-around; gap: 20px; flex-wrap: wrap;">
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/hackudc_2026/outfits.png" alt="Bundle gallery showing all outfits">
    <p><em>Bundle gallery</em></p>
  </div>
  <div style="flex: 1; min-width: 300px; text-align: center;">
    <img src="/projects/hackudc_2026/outfit.png" alt="Prediction detail for a single outfit">
    <p><em>Prediction detail with top-15 matched products</em></p>
  </div>
</div>

---

## Tech Stack

- **Deep Learning:** PyTorch, Ultralytics (YOLOv8), OpenCLIP (SigLIP), timm (DINOv2)
- **Data Science:** NumPy, Pandas, Scikit-learn
- **Demo:** FastAPI, Uvicorn, Jinja2
- **Infrastructure:** Kaggle (2× T4 GPUs), DataParallel

---

**Link to code:** [HackUDC2026 on GitHub](https://github.com/miguelplanas/HackUDC2026)
