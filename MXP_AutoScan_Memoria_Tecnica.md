# MXP AutoScan

## Sistema de Detección Automática de Daños y Partes en Vehículos

### Memoria Técnica - Versión Final con Aplicación Web

**Equipo MXP** | Datathon ITG Galicia 2025

---

## Índice

1. [Contexto del Proyecto](#1-contexto-del-proyecto)
2. [Dataset y Análisis Exploratorio](#2-dataset-y-análisis-exploratorio)
3. [Arquitectura del Sistema](#3-arquitectura-del-sistema)
4. [Evolución del Modelo](#4-evolución-del-modelo)
5. [Evaluación Cuantitativa](#5-evaluación-cuantitativa)
6. [Sistema de Producción: Aplicación Web](#6-sistema-de-producción-aplicación-web)
7. [Conclusiones](#7-conclusiones)
8. [Anexos](#anexos)

---

## 1. Contexto del Proyecto

### 1.1 Descripción General

MXP AutoScan es un sistema completo de visión por computador para la detección y segmentación automática de daños en vehículos. El proyecto implementa una arquitectura basada en **Mask R-CNN** capaz de identificar y segmentar **8 tipos de daños** y **21 categorías de partes** de vehículos, con una **interfaz web profesional** para uso en producción.

**Componentes del sistema:**
- **Modelos Deep Learning:** Mask R-CNN dual (daños + partes)
- **Backend API:** FastAPI con endpoints REST
- **Frontend Web:** Interfaz interactiva HTML/CSS/JavaScript
- **Sistema de Informes:** Generación automática de PDFs

**Aplicaciones objetivo:**
- Sector asegurador: automatización de peritajes y presupuestación
- Talleres de reparación: inspección técnica y documentación
- Inspección vehicular: análisis rápido del estado de vehículos

### 1.2 Problema a Resolver

La detección automática de daños en vehículos presenta desafíos significativos:

1. **Desbalanceo extremo de clases** (ratio 42:1 entre clase mayoritaria y minoritaria)
2. **Objetos pequeños** (71.5% de daños ocupan <1% del área de imagen)
3. **Formas irregulares** (scratches elongados, corrosión difusa)
4. **Similitud visual** entre tipos de daños superficiales
5. **Requisitos de producción:** Interfaz intuitiva, rapidez, informes profesionales

### 1.3 Trabajos Relacionados

| Referencia | Aportación |
|------------|------------|
| **CarDD** (Wang et al., 2022) | Dataset de referencia con 4,000 imágenes. Scratch AP50: 34.3%, Crack AP50: 16.6%. Usan DCN+ y Focal Loss α=0.5 |
| **Focal Loss** (Lin et al., 2017) | Función de pérdida `FL(p_t) = -α(1-p_t)^γ log(p_t)` para desbalanceo de clases |
| **Mask R-CNN** (He et al., 2017) | Arquitectura base para detección + segmentación de instancias |

### 1.4 Hardware Utilizado

| Componente | Especificación |
|------------|----------------|
| GPU | NVIDIA GeForce GTX 1650 Ti (3.9 GB VRAM) |
| Framework | PyTorch 2.7.1 + CUDA 11.8 |
| Python | 3.12.9 |
| Backend | FastAPI 0.109.0 + Uvicorn |

> **Restricción importante:** La limitada VRAM condicionó el batch size (2) y la resolución máxima de entrenamiento.

---

## 2. Dataset y Análisis Exploratorio

### 2.1 Descripción del Dataset

**Fuente:** "Car parts and car damages" (Humans in the Loop, CC0 1.0)

| Aspecto | Daños | Partes |
|---------|-------|--------|
| Imágenes | 814 | 998 |
| Instancias totales | 9,084 | 15,767 |
| Clases | 8 | 21 |
| Resolución media | 685×492 px | 724×507 px |
| Instancias/imagen | 11.16 ± 10.39 | 15.80 ± 8.12 |

### 2.2 Distribución de Clases de Daños

El análisis reveló un **desbalanceo extremo** con ratio 42.66:1 entre la clase más frecuente y la menos frecuente:

| Clase | Instancias | % Total | Categoría |
|-------|------------|---------|-----------|
| Scratch | 3,242 | 35.7% | Mayoritaria |
| Dent | 1,741 | 19.2% | Mayoritaria |
| Broken part | 1,151 | 12.7% | Mayoritaria |
| Paint chip | 590 | 6.5% | Minoritaria |
| Missing part | 337 | 3.7% | Minoritaria |
| Corrosion | 277 | 3.1% | Minoritaria |
| Flaking | 68 | 0.7% | Crítica |
| **Cracked** | **76** | **0.8%** | **Crítica** |

### 2.3 Características Geométricas

El análisis de características geométricas revela desafíos significativos:

- **Área relativa media (daños):** 1.76% del área de imagen (objetos muy pequeños)
- **71.5% de daños** ocupan menos del 1% del área de imagen
- **Compacidad media:** 0.505 (formas irregulares y elongadas)
- **Puntos por polígono:** 13.2 (daños) vs 23.3 (partes)

### 2.4 Splits de Datos

| Split | Proporción | Imágenes (daños) | Imágenes (partes) |
|-------|------------|------------------|-------------------|
| Train | 70% | 569 | 698 |
| Validation | 15% | 122 | 150 |
| Test | 15% | 123 | 150 |

Semilla fija (`random_state=42`) para reproducibilidad verificada.

---

## 3. Arquitectura del Sistema

### 3.1 Arquitectura Global del Sistema

```
┌─────────────────────────────────────────────────────────────┐
│                    FRONTEND (Webapp)                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │  Subida de   │  │ Visualización│  │   Exportar   │     │
│  │   Imagen     │→ │  Resultados  │→ │  PDF/JSON    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP/REST
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                   BACKEND API (FastAPI)                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │  Endpoint    │  │   Modelo     │  │  Generación  │     │
│  │  /analyze    │→ │  Manager     │→ │    PDF       │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ↓
┌─────────────────────────────────────────────────────────────┐
│              MODELOS MASK R-CNN (PyTorch)                   │
│  ┌──────────────────────┐  ┌──────────────────────┐        │
│  │  Modelo de Daños     │  │  Modelo de Partes    │        │
│  │  (8 clases)          │  │  (21 clases)         │        │
│  │  ResNet50-FPN        │  │  ResNet50-FPN        │        │
│  └──────────────────────┘  └──────────────────────┘        │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 Modelo Base: Mask R-CNN

**Arquitectura:** ResNet-50 + FPN, pre-entrenado en COCO (80 clases, incluye 'car').

```
Input (800×800) → ResNet-50 → FPN → RPN → ROI Align → 
  ├── Box Head → Bounding Boxes + Labels
  └── Mask Head → Máscaras de Segmentación
```

### 3.3 Configuración de Entrenamiento (V3)

| Parámetro | Valor |
|-----------|-------|
| Clases (daños) | 9 (background + 8 tipos) |
| Clases (partes) | 22 (background + 21 tipos) |
| Input size | 800×800 px |
| Learning rate (heads) | 3e-5 |
| Learning rate (backbone) | 3e-6 (ratio 0.1) |
| Weight decay | 5e-4 |
| Batch size | 2 |
| Gradient accumulation | 2 (batch efectivo: 4) |
| Epochs máximos | 30 |
| Warmup | 5 epochs |
| Patience (early stopping) | 15 |

### 3.4 Técnicas Implementadas

| Técnica | Descripción | Justificación |
|---------|-------------|---------------|
| **Learning Rate Diferencial** | Backbone 10x menor que heads | Preserva features COCO mientras adapta clasificadores |
| **Oversampling por clase** | Cracked 10x, Corrosion 6x, Flaking 5x | Compensa desbalanceo extremo |
| **Augmentations agresivas** | Específicas para clases minoritarias | ElasticTransform, GridDistortion, ColorJitter |
| **Guardado multi-objetivo** | 3 checkpoints por métrica | Evita pérdida de modelos buenos para clases difíciles |

### 3.5 Sistema de Guardado Multi-Objetivo

Se guardan tres checkpoints optimizando diferentes métricas:

| Checkpoint | Métrica Optimizada | Uso Recomendado |
|------------|-------------------|-----------------|
| `best_combined_*.pth` | Cobertura × Score minoritarias / Loss | Balance general |
| `best_minority_*.pth` | Rendimiento en clases difíciles | **Producción (recomendado)** |
| `best_loss_*.pth` | Menor val_loss | Backup conservador |

**Métrica combinada:**
```python
combined = (class_coverage × minority_coverage × minority_score) / val_loss
```

### 3.6 Oversampling Aplicado

| Clase | Instancias Originales | Factor | Resultado |
|-------|----------------------|--------|-----------|
| Cracked | 76 | 10x | ~760 |
| Corrosion | 277 | 6x | ~1,662 |
| Flaking | 68 | 5x | ~340 |
| Paint chip | 590 | 3x | ~1,770 |
| Missing part | 337 | 2x | ~674 |

**Dataset de entrenamiento:** 569 → 2,351 archivos (con repeticiones)

---

## 4. Evolución del Modelo

### 4.1 Resumen de Versiones

| Versión | Cambio Principal | Problema Resuelto | Tiempo Total |
|---------|------------------|-------------------|--------------|
| V1 | Baseline COCO | - | ~10h |
| V2 | LR diferencial + Oversampling | No detecta minoritarias | ~13h |
| **V3** | **Guardado multi-objetivo** | **Early stopping prematuro** | **~38h** |

### 4.2 Problemas Resueltos en V3

**V1/V2:** Early stopping basado únicamente en `val_loss` guardaba el modelo demasiado pronto, antes de que aprendiera clases minoritarias.

**V3:** Sistema de triple guardado permite preservar modelos buenos para diferentes objetivos:
- `best_minority`: Enfocado en Cracked, Corrosion, Flaking
- `best_combined`: Balance entre todas las métricas
- `best_loss`: Backup conservador

**Resultado:** Primera versión en detectar Cracked (**25.18% AP50**).

---

## 5. Evaluación Cuantitativa

### 5.1 Métricas Globales

| Métrica | V1 | V2 | V3 Combined | **V3 Minority** |
|---------|----|----|-------------|-----------------|
| **BBox AP50** | 8.17% | 3.08% | 9.94% | **11.75%** |
| **Segm AP50** | 1.53% | 0.78% | 3.24% | **5.49%** |

> **Conclusión:** `best_minority_damage.pth` obtiene las mejores métricas.

### 5.2 Métricas por Clase - BBox AP50

| Clase | V3 Minority | Estado |
|-------|-------------|--------|
| **Missing part** | **32.07%** | Excelente |
| **Cracked** | **25.18%** | Breakthrough (era 0%) |
| Broken part | 23.41% | Bueno |
| Dent | 11.37% | Aceptable |
| Scratch | 1.09% | Problemático |
| Paint chip | 0.44% | Problemático |
| Corrosion | 0.47% | Problemático |
| Flaking | 0.00% | No detecta |

### 5.3 Comparativa con Estado del Arte

| Aspecto | CarDD (DCN+) | MXP AutoScan (V3) |
|---------|--------------|-------------------|
| Dataset | 4,000 imágenes | 814 imágenes |
| Overall AP50 | 77.7% | 11.75% |
| Scratch AP50 | 34.3% | 1.09% |
| **Crack AP50** | 16.6% | **25.18%** |

**Nota:** Superamos a CarDD en detección de Crack a pesar de tener un dataset 5x menor.

---

## 6. Sistema de Producción: Aplicación Web

### 6.1 Acceso a la Aplicación

**URL:** http://localhost:8000/app

```bash
# Iniciar servidor
python api.py

# Acceder desde el navegador
http://localhost:8000/app
```

### 6.2 Interfaz de Usuario

![Interfaz Web MXP AutoScan](imgs/demo_interfaz.png)

**Panel Izquierdo - Análisis:**
- Drag & Drop de imágenes
- Control de thresholds (Daños: 0.6 | Partes: 0.85)
- Opciones de visualización (Boxes/Masks)
- Controles de vista (All/Damages/Parts/Image)

**Panel Derecho - Resultados:**
- Resumen de métricas
- Distribución de severidad
- Lista de daños detectados
- Exportación (PDF/JSON)

### 6.3 Sistema de Informes PDF

![Ejemplo de Informe](imgs/reporte.png)

**Contenido del PDF:**

1. **Portada** - ID, fecha, resumen ejecutivo
2. **Imagen Anotada** - Vehículo con detecciones y leyenda
3. **Tabla de Daños** - Detalle por daño con área, severidad y coste
4. **Distribución** - Gráfico de severidad
5. **Presupuesto** - Desglose económico con IVA

**Sistema de Presupuestación:**

```python
# SUSTITUCIÓN (Missing part, Broken >200cm²)
Headlight Replacement: 450€ + 200€ (labor) = 650€

# REPARACIÓN (daños superficiales)
Scratch on Hood (145cm²): 150€ + (145 × 0.50€/cm²) = 222.50€
Dent on Door (92cm²): 150€ + (92 × 1.20€/cm²) = 260.40€
```

### 6.4 API Endpoints

| Endpoint | Método | Descripción | Parámetros |
|----------|--------|-------------|------------|
| `/analyze` | POST | Análisis de imagen | damage_threshold=0.6, parts_threshold=0.85 |
| `/report` | POST | Generar PDF | damage_threshold=0.6, parts_threshold=0.85 |
| `/health` | GET | Health check | - |
| `/models/status` | GET | Estado de modelos | - |

**Ejemplo de uso:**

```bash
# Analizar imagen
curl -X POST "http://localhost:8000/analyze?damage_threshold=0.6&parts_threshold=0.85" \
  -F "file=@car.jpg"

# Generar informe
curl -X POST "http://localhost:8000/report" \
  -F "file=@car.jpg" \
  --output informe.pdf
```

### 6.5 Calibración Automática de Área

El sistema busca matrículas para calibrar áreas reales:

**Referencia:** Matrícula europea estándar = 52×11 cm = 572 cm²

**Criterios de validación:**
- Confianza ≥ 0.65
- Área ≥ 150 píxeles
- Aspect ratio entre 2.2 y 6.5

Si detecta matrícula válida → Áreas en cm² reales
Si no → Áreas relativas estimadas

### 6.6 Sistema de Importancia

Combina **severidad semántica** con **contexto geométrico**:

```python
# Severidad por clase
'Broken part': 100 (critical)
'Cracked': 100 (critical)
'Corrosion': 75 (high)
'Dent': 50 (medium)
'Scratch': 25 (low)

# Modificadores
if área > 100 cm²: score × 1.3
if parte_crítica (headlight, windshield, etc.): score × 1.2
```

### 6.7 Tiempos de Respuesta

| Hardware | Tiempo por Imagen |
|----------|-------------------|
| GPU (GTX 1650 Ti) | 2-3 segundos |
| CPU (i7) | 8-12 segundos |

---

## 7. Conclusiones

### 7.1 Contribuciones del Proyecto

- **Sistema funcional end-to-end** de detección dual (daños + partes) con interfaz web profesional
- **Estrategia de guardado multi-objetivo** que resuelve el problema del early stopping prematuro
- **Pipeline de augmentation diferenciado** por clase para manejar desbalanceo extremo
- **Superación del estado del arte** en detección de Cracked (25.18% vs 16.6% CarDD)
- **API REST completa** con generación automática de informes PDF
- **Sistema de presupuestación** realista con diferenciación reparación/sustitución

### 7.2 Lecciones Aprendidas

#### Técnicas

1. **Val_loss no es suficiente** para early stopping en problemas con desbalanceo extremo
2. **El guardado multi-objetivo** es esencial para preservar modelos buenos en clases minoritarias
3. **Oversampling agresivo** (10x) es necesario para clases con <100 instancias
4. **Hardware limitado** (4GB VRAM) es viable con batch size pequeño + gradient accumulation

#### Desarrollo de Producto

5. **Interfaz intuitiva** es tan importante como el modelo subyacente
6. **Thresholds configurables** dan flexibilidad al usuario sin reentrenar
7. **Sistema de calibración automática** mejora la usabilidad
8. **Informes profesionales** son esenciales para adopción en producción

### 7.3 Limitaciones Identificadas

**Del Modelo:**
- Rendimiento insuficiente en daños superficiales elongados (Scratch: 1.09%)
- Dataset limitado (814 imágenes vs 4,000 en CarDD)
- Clases con <100 instancias siguen siendo problemáticas

**Del Sistema:**
- Calibración de área requiere detección de matrícula
- Tiempo de inferencia en CPU (~10s) puede ser lento
- Presupuestación es estimativa

### 7.4 Trabajo Futuro

| Prioridad | Mejora Propuesta | Impacto Esperado |
|-----------|------------------|------------------|
| **Alta** | Implementar Deformable Convolutions | Mejor detección de formas irregulares |
| **Alta** | Aumentar dataset | Mejora general en todas las clases |
| **Media** | Test-Time Augmentation | +2-5% AP50 |
| **Media** | Procesamiento multi-vista | Análisis 360° del vehículo |

### 7.5 Recomendaciones de Uso

**Modelos para producción:**
```bash
checkpoints/best_minority_damage.pth  # Daños
checkpoints/best_combined_part.pth    # Partes
```

**Thresholds recomendados:**

| Caso de Uso | Damage | Parts |
|-------------|--------|-------|
| **Balance (recomendado)** | **0.6** | **0.85** |
| Inspección exhaustiva | 0.3 | 0.75 |
| Alta precisión | 0.8+ | 0.9+ |

**Comando de evaluación:**
```bash
# Evaluación cuantitativa
python test.py \
    --checkpoint checkpoints/best_minority_damage.pth \
    --confidence 0.6 \
    --output-dir outputs/evaluation

# Evaluación cualitativa
python evaluate_qualitative.py \
    --checkpoint checkpoints/best_minority_damage.pth \
    --confidence 0.6 \
    --n-images 15
```

---

## Anexos

### Anexo A: Comandos de Ejecución

#### Aplicación Web

```bash
# Instalar dependencias
pip install fastapi uvicorn python-multipart reportlab

# Lanzar servidor
python api.py

# Acceder
# Webapp: http://localhost:8000/app
# API Docs: http://localhost:8000/docs
```

#### Entrenamiento

```bash
# Entrenamiento secuencial completo
python train_sequential.py --epochs 30 --batch-size 2

# Solo daños
python train.py --task damage --epochs 30 --batch-size 2

# Con opciones avanzadas
python train_sequential.py \
    --epochs 30 \
    --damage-epochs 35 \
    --parts-epochs 25
```

#### Evaluación

```bash
# Cuantitativa (COCO)
python test.py \
    --checkpoint checkpoints/best_minority_damage.pth \
    --confidence 0.6 \
    --output-dir outputs/evaluation

# Cualitativa (visual)
python evaluate_qualitative.py \
    --checkpoint checkpoints/best_minority_damage.pth \
    --confidence 0.6 \
    --n-images 15
```

### Anexo B: Estructura del Proyecto

```
mxp_autoscan/
├── api.py                      # Backend FastAPI
├── checkpoints/                # Modelos entrenados
│   ├── best_combined_part.pth
│   └── best_minority_damage.pth
├── configs/
│   ├── configs.py              # Configuración central
│   └── __init__.py
├── datasets/
│   ├── dual_dataset.py         # Dataset con oversampling
│   └── __init__.py
├── EDA.ipynb                   # Análisis exploratorio
├── evaluate_qualitative.py     # Evaluación visual
├── imgs/
│   ├── logo.png
│   ├── maserati_tunel.jpg
│   ├── demo_interfaz.png
│   └── demo_factura.png
├── logs/
│   └── training_20251227_110042.log
├── models/
│   ├── dual_mask_rcnn.py       # Mask R-CNN
│   ├── unet.py                 # U-Net (experimental)
│   └── __init__.py
├── MXP_AutoScan_Memoria_Tecnica.md
├── outputs/
│   ├── cuantitative/
│   │   └── v3_coco_bueno/
│   │       ├── best_combined_coco_metrics_damage.json
│   │       ├── best_loss_coco_metrics_damage.json
│   │       └── best_minority_coco_metrics_damage.json
│   ├── qualitative/
│   │   ├── damages/images/     # 15 visualizaciones
│   │   └── parts/images/       # 15 visualizaciones
│   └── v3_compare/best_minority/
├── Pipfile
├── Pipfile.lock
├── predict_unet.py
├── README.md
├── requirements.txt
├── scripts/
│   ├── verify_splits.py
│   └── __init__.py
├── test.py                     # Evaluación COCO
├── train.py                    # Entrenamiento individual
├── train_sequential.py         # Entrenamiento secuencial
├── train_sequential_unet.py
├── train_unet.py
├── utils/
│   ├── augmentations.py
│   ├── losses.py
│   └── __init__.py
├── verify_billing.py
└── webapp/
    ├── app.js
    ├── index.html
    └── style.css

```

### Anexo C: Referencias

1. **He, K., et al. (2017).** Mask R-CNN. _ICCV_.
2. **Lin, T.Y., et al. (2017).** Focal Loss for Dense Object Detection. _ICCV_.
3. **Wang, X., et al. (2022).** CarDD: A New Dataset for Car Damage Detection. _IEEE Access_.
4. **Jin, Y., & Sendhoff, B. (2008).** Pareto-based Multi-objective Machine Learning. _IEEE Transactions_.
5. **Loshchilov, I., & Hutter, F. (2017).** SGDR: Stochastic Gradient Descent with Warm Restarts. _ICLR_.

---

*Enero 2025*  
*Equipo MXP - Datathon ITG DIHGIGAL Galicia 2025*  