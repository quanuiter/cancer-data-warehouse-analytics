# Cancer Patient Data Warehouse & Analytics

[![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=flat&logo=microsoftsqlserver&logoColor=white)](https://www.microsoft.com/sql-server)
[![SSIS](https://img.shields.io/badge/SSIS-ETL-blue.svg)](https://learn.microsoft.com/en-us/sql/integration-services/)
[![SSAS](https://img.shields.io/badge/SSAS-OLAP%20Cube-orange.svg)](https://learn.microsoft.com/en-us/analysis-services/)
[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikitlearn&logoColor=white)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This repository presents an end-to-end healthcare data warehousing and analytics pipeline built around a **Snowflake Schema** data warehouse for cancer patient clinical records. The project covers the full analytics lifecycle: an automated **SSIS ETL pipeline** for ingestion and cleaning, a **SSAS OLAP cube** with MDX calculated measures for multi-dimensional analysis, interactive **Power BI** dashboards, and a machine learning layer for **survival prediction** and **patient segmentation**.

> 📌 Originally developed as a final-term Data Warehousing course project (Sep–Dec 2025). The dataset is a public/academic clinical dataset and does not contain real hospital patient data.

---

## Business Problem

Hospitals and clinical researchers need fast, multi-dimensional access to patient treatment data — across treatment types, age groups, regions, and comorbidities — to support treatment planning, resource allocation, and outcome analysis. Raw operational data is not structured for this kind of querying, which motivates a dedicated warehouse and analytics layer.

**Goals:**
* Enable multi-dimensional reporting on cancer patient outcomes (OLAP)
* Predict patient survival status using machine learning
* Segment patients into clinical profiles to support treatment optimization

---

## Key Contributions

* **Snowflake Schema Data Warehouse:** Designed a 9-table Snowflake Schema in SQL Server (`Cancer_DW`), resolving the many-to-many relationship between patients and comorbidities via a dedicated bridge table — avoiding data duplication while preserving query flexibility across Patient, Treatment, Region, and Time dimensions.
* **Automated SSIS ETL Pipeline:** Built an ETL pipeline in SQL Server Integration Services to extract, clean, and load 10,000 raw clinical records from CSV into the warehouse, handling multicast transformations for multi-destination routing, strict data type conversions, and cleansing of inconsistent or missing source values.
* **OLAP Cube with MDX Calculated Measures:** Constructed a SSAS OLAP cube with defined attribute relationships and a time hierarchy for drill-down analysis, plus custom MDX measures enabling fast queries such as survival rate by treatment type, region, and age group simultaneously.
* **Interactive Power BI Reporting:** Delivered dashboards covering treatment types, age groups, and regional breakdowns, giving clinical stakeholders self-service access to warehouse insights.
* **ML-Driven Survival Prediction:** Applied Random Forest and XGBoost classifiers to predict patient survival status, achieving **86% accuracy** on the held-out test set.
* **Unsupervised Patient Segmentation:** Applied K-Means clustering with PCA and t-SNE dimensionality reduction to segment patients into **3 distinct clinical profile groups**, supporting treatment protocol optimization and resource allocation.

---

## Architecture

```text
Raw CSV (10,000 clinical records)
        │
        ▼
   SSIS ETL Pipeline  ──►  Data cleaning, type conversion, multicast transforms
        │
        ▼
  SQL Server Data Warehouse (Cancer_DW)
  Snowflake Schema — 9 tables, bridge table for Comorbidities (M:N)
        │
        ├──►  SSAS OLAP Cube  ──►  MDX calculated measures, time hierarchy  ──►  Power BI Reports
        │
        └──►  Python (Pandas, Scikit-learn, XGBoost)  ──►  Survival Prediction + Patient Segmentation
```

📄 Schema reference: [`assets/erd.png`](assets/erd.png)
📸 Cube structure: [`assets/cube.png`](assets/cube.png)

---

## Experimental Results

### Survival Prediction

| Model | Accuracy | Notes |
| :--- | :---: | :--- |
| **XGBoost** | **86%** | Best-performing classifier on held-out test set |
| Random Forest | — | Baseline comparison model |

### Patient Segmentation (K-Means + PCA/t-SNE)

| Metric | Result |
| :--- | :---: |
| Number of clusters | 3 distinct clinical profiles |
| Dimensionality reduction | PCA, t-SNE for visualization |
| Application | Treatment protocol optimization, resource allocation |

---

## Interactive Reporting (Power BI)

Dashboards enabling clinical insight reporting across treatment types, age groups, and regions:

| Overview | Treatment Analysis | Regional Breakdown |
| :---: | :---: | :---: |
| `assets/report1.png` | `assets/report2.png` | `assets/report3.png` |

🎥 Full walkthrough videos:
* SSIS ETL: https://youtu.be/zpCIaDqupjY
* SSAS OLAP Cube: https://youtu.be/XWUUEFaJ5xE
* MDX + Power BI: https://youtu.be/GYzyGgTHPdY

---

## Repository Structure

```text
cancer-data-warehouse-analytics/
├── README.md
├── data/                    # Sample of the raw clinical dataset
├── sql/                     # Warehouse schema (DDL scripts)
├── source/
│   ├── ssis/                # SSIS ETL package
│   └── powerbi/             # Power BI report file (.pbix)
├── datamining/
│   └── survival_prediction.ipynb   # ML models: survival prediction + segmentation
└── assets/                  # ERD, OLAP cube, dashboard screenshots
```

---

## Getting Started

### 1. Prerequisites

* SQL Server + SQL Server Data Tools (SSIS, SSAS)
* Power BI Desktop
* Python 3.8+ with the following packages:

```bash
pip install pandas scikit-learn xgboost matplotlib seaborn jupyter
```

### 2. Setting Up the Data Warehouse

1. Run the DDL scripts in [`sql/`](sql/) to create the `Cancer_DW` Snowflake Schema in SQL Server.
2. Open and deploy the SSIS package in [`source/ssis/`](source/ssis) to load and clean the raw CSV data into the warehouse.
3. Deploy the SSAS OLAP cube on top of the warehouse and process it to enable MDX queries.
4. Connect Power BI to the SSAS cube using the report file in [`source/powerbi/`](source/powerbi).

### 3. Running the Machine Learning Pipeline

```bash
jupyter notebook datamining/survival_prediction.ipynb
```

The notebook covers data loading from the warehouse extract, preprocessing, training of Random Forest / XGBoost survival classifiers, and K-Means patient segmentation with PCA/t-SNE visualization.

---

## Tech Stack

| Layer | Tools |
| :--- | :--- |
| Data Warehouse | SQL Server, Snowflake Schema |
| ETL | SSIS |
| OLAP | SSAS, MDX |
| BI / Reporting | Power BI |
| Machine Learning | Python, Pandas, Scikit-learn, XGBoost |

---

## Notes

* Dataset is public/academic and does not contain real patient-identifiable data.
* Originally developed as part of a university Data Warehousing course (Sep–Dec 2025).

---

## License

This project is licensed under the MIT License.
