# Cancer Patient Data Warehouse & Analytics

Healthcare data warehousing and analytics project: designing a Snowflake Schema data warehouse, building an automated ETL pipeline, an OLAP cube for multi-dimensional analysis, and applying machine learning to predict patient survival and segment patients into clinical profiles.

> 📌 This project was originally developed as a final-term Data Warehousing course project. The dataset used is a public/academic clinical dataset (not real hospital patient data).

---

## 🎯 Business Problem

Hospitals and clinical researchers need fast, multi-dimensional access to patient treatment data — across treatment types, age groups, regions, and comorbidities — to support treatment planning, resource allocation, and outcome analysis. Raw operational/clinical data is not structured for this kind of analysis.

**Goal:** Build a data warehouse and analytics pipeline that enables:
- Multi-dimensional reporting on cancer patient outcomes (OLAP)
- Prediction of patient survival status (Machine Learning)
- Patient segmentation to support treatment optimization (Clustering)

---

## 🏗️ Architecture

```
Raw CSV (10,000 clinical records)
        │
        ▼
   SSIS ETL Pipeline  ──► Data cleaning, type conversion, multicast transforms
        │
        ▼
  SQL Server Data Warehouse (Cancer_DW)
  Snowflake Schema — 9 tables, bridge table for Comorbidities (M:N)
        │
        ├──► SSAS OLAP Cube ──► MDX calculated measures, time hierarchy ──► Power BI Reports
        │
        └──► Python (Pandas, Scikit-learn, XGBoost) ──► Survival Prediction + Patient Segmentation
```

*(Replace with an actual architecture diagram image if available — see `assets/`)*

---

## 🧱 1. Data Warehouse Design

- Designed a **Snowflake Schema** in SQL Server with **9 tables**.
- Resolved the many-to-many relationship between patients and their comorbidities using a **bridge table**, avoiding data duplication and preserving query flexibility.
- Schema covers dimensions such as Patient, Treatment, Region, Time, and the Comorbidity bridge, linked to a central fact table of clinical encounters/outcomes.

📄 Schema reference: [`assets/erd.png`](assets/erd.png) *(add ERD screenshot from the final report here)*

---

## 🔄 2. ETL Pipeline (SSIS)

- Built an automated ETL pipeline in **SQL Server Integration Services (SSIS)** to extract, clean, and load 10,000 raw clinical records from CSV into the `Cancer_DW` warehouse.
- Handled:
  - **Multicast transformations** to route data to multiple destinations/dimension tables
  - **Data type conversions** to enforce warehouse schema consistency
  - Cleaning of inconsistent/missing values in the source data

📁 Package reference: [`source/ssis/`](source/ssis) *(add `.dtsx` package if available)*

---

## 📊 3. OLAP Cube (SSAS & MDX)

- Built an **OLAP cube** in SQL Server Analysis Services (SSAS) on top of the warehouse.
- Defined **attribute relationships** and a **time hierarchy** for efficient drill-down analysis.
- Wrote **calculated measures (MDX)** to support fast, multi-dimensional epidemiological queries (e.g., survival rate by treatment type, region, and age group simultaneously).

📸 Cube structure: [`assets/olap_cube.png`](assets/olap_cube.png) *(add screenshot from the final report here)*

---

## 📈 4. Interactive Reporting (Power BI)

- Delivered interactive **Power BI dashboards** enabling clinical insight reporting across:
  - Treatment types
  - Age groups
  - Regions

📸 Dashboard screenshots:

| Overview | Treatment Analysis | Regional Breakdown |
|---|---|---|
| *(add screenshot)* | *(add screenshot)* | *(add screenshot)* |

🎥 Full walkthrough video: **[YouTube link here]**

---

## 🤖 5. Advanced Analytics (Python)

**Survival Prediction**
- Applied **Random Forest** and **XGBoost** classifiers to predict cancer patient survival status.
- Achieved **86% accuracy** on the test set.

**Patient Segmentation**
- Applied **K-Means Clustering** combined with dimensionality reduction (**PCA**, **t-SNE**) to segment patients into **3 distinct clinical profile groups**.
- Segments support treatment protocol optimization and hospital resource allocation.

📁 Notebook: [`datamining/survival_prediction.ipynb`](datamining/survival_prediction.ipynb)

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Data Warehouse | SQL Server, Snowflake Schema |
| ETL | SSIS |
| OLAP | SSAS, MDX |
| BI / Reporting | Power BI |
| Machine Learning | Python, Pandas, Scikit-learn, XGBoost |

---

## 📁 Repository Structure

```
cancer-data-warehouse-analytics/
├── README.md
├── data/                   # Sample of the raw clinical dataset
├── sql/                    # Warehouse schema (DDL scripts)
├── source/                 # SSIS package, Power BI report file
├── datamining/             # Python notebook — ML models
└── assets/                 # ERD, OLAP cube, dashboard screenshots
```

---

## 📌 Notes

- Dataset is public/academic and does not contain real patient-identifiable data.
- Original project developed as part of a university Data Warehousing course (Sep–Dec 2025).
