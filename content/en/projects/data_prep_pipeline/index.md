---
title: "Data Preparation Pipeline for Building Permits"
tags:
  - python
  - polimi
date: 2024-12-01T12:19:52+01:00
draft: false
description: "Implementation of a robust data preparation pipeline for the San Francisco Building Permits dataset, addressing data quality dimensions including completeness, accuracy, consistency, and validity."
cover:
  image: /projects/data_prep_pipeline/diq.png
  alt: Data profiling visualization
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

## Project Overview

This project implements a comprehensive **data preparation pipeline** for the *San Francisco Building Permits* dataset as part of the Data and Information Quality course at Politecnico di Milano. The pipeline addresses all major data quality dimensions: completeness, accuracy, consistency, and validity.

---

## Dataset Information

The **Building Permit Applications Data** provides detailed records of building permit applications from San Francisco between January 1, 2013, and February 25, 2018.

| Attribute | Value |
|-----------|-------|
| **Records** | ~200,000 |
| **Attributes** | 43 columns |
| **Source** | [San Francisco Open Data](https://data.sfgov.org/) |
| **Final Clean Records** | 181,495 |

### Key Dataset Attributes

- **Permit Number**: Unique identifier for each permit
- **Permit Type**: Category of the permit
- **Location**: Geographic coordinates (latitude and longitude)
- **Dates**: Permit Creation Date, Issued Date, and Expiration Date
- **Cost Information**: Estimated and Revised costs of the project

---

## Pipeline Implementation

### 1. Data Exploration and Profiling

The initial step involved thorough exploration and profiling using Python libraries such as `pandas` and `ydata-profiling`. Key profiling outputs included:

- **Dataset Shape**: 198,900 rows and 43 columns
- **Missing Values**: Identified columns with missing data and calculated percentages
- **Value Distributions**: Visualized histograms for numerical and categorical columns

### Issues Identified

- High levels of missing data in several columns (e.g., Street Number Suffix)
- Duplicate entries in key columns including Permit Number
- Presence of outliers in Estimated Cost and Revised Cost
- Inconsistent formatting of categorical values

---

### 2. Data Quality Assessment

The dataset was evaluated across four key quality dimensions:

| Dimension | Assessment |
|-----------|------------|
| **Completeness** | Calculated missing values distribution across columns |
| **Accuracy** | Checked for negative/illogical values in numerical columns |
| **Consistency** | Verified unique identifiers and logical relationships between dates |
| **Validity** | Ensured date fields adhered to logical timelines |

---

### 3. Data Transformation and Standardization

Several transformations were applied to improve the dataset's structure:

1. **Removed columns** with >80% missing values: Street Number Suffix, Unit Suffix, Structural Notification, Voluntary Soft-Story Retrofit, Site Permit, Fire Only Permit, Unit, and TIDF Compliance

2. **Combined columns**: Street Suffix, Street Name, and Street Number merged into Street Address

3. **Split columns**: Location split into Latitude and Longitude

4. **Formatting corrections**: Dates converted to datetime format, categorical values to lowercase

5. **New columns created**:
   - **Cost Difference**: Difference between Estimated Cost and Revised Cost
   - **On Time Completion**: Binary indicator for permit completion timeliness

---

### 4. Missing Value Handling

Multiple imputation techniques were employed based on column characteristics:

| Column Type | Technique |
|-------------|-----------|
| Dates (Completed/Issued) | Logical text replacement based on status |
| Numerical (Stories, Units, Cost) | **KNNImputer** |
| Revised Cost | Linear interpolation from Estimated Cost |
| Latitude/Longitude | **Linear Regression** from Street Number |
| Zipcode/Supervisor District | **Linear Regression** from coordinates |
| Neighborhoods | Mode (most frequent category) |
| Proposed Stories/Units | Median imputation |
| Categorical (Use, Construction Type) | **IterativeImputer** |

---

### 5. Outlier Detection and Correction

Outliers were identified and corrected using:

- **Interquartile Range (IQR)**: Values outside Q1 - 1.5×IQR and Q3 + 1.5×IQR were flagged
- **Isolation Forest**: Unsupervised learning method for high-dimensional anomaly detection
- **Median replacement**: Outliers in numerical columns replaced with median values

---

### 6. Data Deduplication

Deduplication enhanced dataset consistency by removing redundant records:

1. **Exact duplicates**: Removed using `drop_duplicates()` method

2. **Near-duplicates**: Detected using the `recordlinkage` library with:
   - Sorted Neighborhood Index on Permit Number
   - Jaro-Winkler similarity (threshold 0.85) for address and description fields

**Result**: 181,495 unique records retained

---

## Final Quality Assessment

After the pipeline execution, the dataset achieved:

- **Completeness**: Near-zero missing data percentage
- **Accuracy**: No negative or illogical values in numerical columns
- **Consistency**: All duplicate records removed, unique identifiers verified
- **Timeliness**: Logical date relationships validated

---

## Tools and Technologies

- **pandas**: Data manipulation and cleaning
- **numpy**: Numerical computations
- **scikit-learn**: KNNImputer, IterativeImputer, Isolation Forest
- **ydata-profiling**: Comprehensive data profiling reports
- **recordlinkage**: Duplicate detection and linkage
- **matplotlib & seaborn**: Data visualization

---

## Project Conclusion

This project demonstrated the critical importance of a robust data preparation pipeline in achieving high-quality datasets. Key takeaways:

- **Data Profiling** provides essential metadata guiding targeted cleaning efforts
- **Systematic cleaning** addressing missing values, duplicates, and inconsistencies ensures data reliability
- **High-quality data** is foundational for any downstream analysis, minimizing biases and enhancing result credibility

---

**Course**: Data and Information Quality  
**Institution**: Politecnico di Milano  
**Academic Year**: 2024-25

**Code link:** [diq on GitHub](https://github.com/miguelplanas/diq)
