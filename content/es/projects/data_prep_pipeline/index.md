---
title: "Pipeline de Preparación de Datos para Permisos de Construcción"
tags:
  - python
  - polimi
date: 2024-12-01T12:19:52+01:00
draft: false
description: "Implementación de un pipeline robusto de preparación de datos para el dataset de Permisos de Construcción de San Francisco, abordando las dimensiones de calidad de datos: completitud, precisión, consistencia y validez."
cover:
  image: /projects/data_prep_pipeline/diq.png
  alt: Visualización del perfilado de datos
  relative: true
---

<div style="display: flex; justify-content: left; gap: 10px; flex-wrap: nowrap;">
  <a href="https://github.com/miguelplanas/diq" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/github-diq-black?style=for-the-badge&logo=github" style="margin: 0;" />
  </a>
  <a href="https://github.com/miguelplanas/diq" style="text-decoration: none; box-shadow: none;">
    <img src="https://img.shields.io/badge/Made_with-Python_3.11-blue?style=for-the-badge&logo=python" style="margin: 0;" />
  </a>
</div>

## Resumen del Proyecto

Este proyecto implementa un **pipeline de preparación de datos** integral para el dataset de *Permisos de Construcción de San Francisco* como parte de la asignatura Data and Information Quality en el Politecnico di Milano. El pipeline aborda todas las dimensiones principales de calidad de datos: completitud, precisión, consistencia y validez.

---

## Información del Dataset

El dataset **Building Permit Applications Data** proporciona registros detallados de solicitudes de permisos de construcción en San Francisco entre el 1 de enero de 2013 y el 25 de febrero de 2018.

| Atributo | Valor |
|----------|-------|
| **Registros** | ~200,000 |
| **Atributos** | 43 columnas |
| **Fuente** | [San Francisco Open Data](https://data.sfgov.org/) |
| **Registros limpios finales** | 181,495 |

### Atributos Principales del Dataset

- **Permit Number**: Identificador único para cada permiso
- **Permit Type**: Categoría del permiso
- **Location**: Coordenadas geográficas (latitud y longitud)
- **Dates**: Fecha de creación, emisión y expiración del permiso
- **Cost Information**: Costes estimados y revisados del proyecto

---

## Implementación del Pipeline

### 1. Exploración y Perfilado de Datos

El paso inicial involucró una exploración exhaustiva utilizando bibliotecas de Python como `pandas` y `ydata-profiling`. Los resultados clave del perfilado incluyeron:

- **Dimensiones del dataset**: 198,900 filas y 43 columnas
- **Valores faltantes**: Identificación de columnas con datos faltantes y cálculo de porcentajes
- **Distribuciones de valores**: Visualización de histogramas para columnas numéricas y categóricas

### Problemas Identificados

- Altos niveles de datos faltantes en varias columnas (ej. Street Number Suffix)
- Entradas duplicadas en columnas clave incluyendo Permit Number
- Presencia de valores atípicos en Estimated Cost y Revised Cost
- Formato inconsistente en valores categóricos

---

### 2. Evaluación de Calidad de Datos

El dataset fue evaluado en cuatro dimensiones clave de calidad:

| Dimensión | Evaluación |
|-----------|------------|
| **Completitud** | Cálculo de distribución de valores faltantes por columna |
| **Precisión** | Verificación de valores negativos/ilógicos en columnas numéricas |
| **Consistencia** | Verificación de identificadores únicos y relaciones lógicas entre fechas |
| **Validez** | Comprobación de líneas temporales lógicas en campos de fecha |

---

### 3. Transformación y Estandarización de Datos

Se aplicaron varias transformaciones para mejorar la estructura del dataset:

1. **Eliminación de columnas** con >80% valores faltantes: Street Number Suffix, Unit Suffix, Structural Notification, Voluntary Soft-Story Retrofit, Site Permit, Fire Only Permit, Unit y TIDF Compliance

2. **Combinación de columnas**: Street Suffix, Street Name y Street Number fusionadas en Street Address

3. **División de columnas**: Location dividida en Latitude y Longitude

4. **Correcciones de formato**: Fechas convertidas a formato datetime, valores categóricos a minúsculas

5. **Nuevas columnas creadas**:
   - **Cost Difference**: Diferencia entre Estimated Cost y Revised Cost
   - **On Time Completion**: Indicador binario de finalización a tiempo del permiso

---

### 4. Manejo de Valores Faltantes

Se emplearon múltiples técnicas de imputación según las características de cada columna:

| Tipo de Columna | Técnica |
|-----------------|---------|
| Fechas (Completed/Issued) | Reemplazo lógico de texto según estado |
| Numéricas (Stories, Units, Cost) | **KNNImputer** |
| Revised Cost | Interpolación lineal desde Estimated Cost |
| Latitude/Longitude | **Regresión Lineal** desde Street Number |
| Zipcode/Supervisor District | **Regresión Lineal** desde coordenadas |
| Neighborhoods | Moda (categoría más frecuente) |
| Proposed Stories/Units | Imputación por mediana |
| Categóricas (Use, Construction Type) | **IterativeImputer** |

---

### 5. Detección y Corrección de Valores Atípicos

Los valores atípicos fueron identificados y corregidos utilizando:

- **Rango Intercuartílico (IQR)**: Valores fuera de Q1 - 1.5×IQR y Q3 + 1.5×IQR fueron marcados
- **Isolation Forest**: Método de aprendizaje no supervisado para detección de anomalías en alta dimensionalidad
- **Reemplazo por mediana**: Valores atípicos en columnas numéricas reemplazados por la mediana

---

### 6. Deduplicación de Datos

La deduplicación mejoró la consistencia del dataset eliminando registros redundantes:

1. **Duplicados exactos**: Eliminados usando el método `drop_duplicates()`

2. **Cuasi-duplicados**: Detectados usando la biblioteca `recordlinkage` con:
   - Índice de Vecindario Ordenado sobre Permit Number
   - Similitud Jaro-Winkler (umbral 0.85) para campos de dirección y descripción

**Resultado**: 181,495 registros únicos conservados

---

## Evaluación Final de Calidad

Tras la ejecución del pipeline, el dataset alcanzó:

- **Completitud**: Porcentaje de datos faltantes cercano a cero
- **Precisión**: Sin valores negativos o ilógicos en columnas numéricas
- **Consistencia**: Todos los registros duplicados eliminados, identificadores únicos verificados
- **Temporalidad**: Relaciones lógicas de fechas validadas

---

## Herramientas y Tecnologías

- **pandas**: Manipulación y limpieza de datos
- **numpy**: Computación numérica
- **scikit-learn**: KNNImputer, IterativeImputer, Isolation Forest
- **ydata-profiling**: Informes completos de perfilado de datos
- **recordlinkage**: Detección de duplicados y vinculación
- **matplotlib y seaborn**: Visualización de datos

---

## Conclusión del Proyecto

Este proyecto demostró la importancia crítica de un pipeline robusto de preparación de datos para lograr datasets de alta calidad. Conclusiones clave:

- El **Perfilado de Datos** proporciona metadatos esenciales que guían los esfuerzos de limpieza específicos
- La **limpieza sistemática** que aborda valores faltantes, duplicados e inconsistencias asegura la fiabilidad de los datos
- Los **datos de alta calidad** son fundamentales para cualquier análisis posterior, minimizando sesgos y mejorando la credibilidad de los resultados

---

**Asignatura**: Data and Information Quality  
**Institución**: Politecnico di Milano  
**Curso Académico**: 2024-25

**Enlace al código:** [diq en GitHub](https://github.com/miguelplanas/diq)