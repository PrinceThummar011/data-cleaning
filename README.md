# 🏥 Clinical Trials Data Accessing and Cleaning

Welcome to the **Clinical Trials Data Accessing and Cleaning** repository! This project focuses on demonstrating a complete data wrangling, cleaning, and transformation workflow for healthcare and clinical-trial-style datasets using **Python** and **Pandas**. 

## 📖 Project Overview

Real-world clinical data is often messy, missing, or improperly formatted. This repository simulates a real-world scenario where data from multiple sources (patient demographics, treatments, and adverse reactions) must be joined, cleaned, and structurally validated before it is ready for downstream analytics and reporting.

The primary implementation and step-by-step cleaning process is contained in the Jupyter Notebook: 
👉 **[`session_28_data_accessing_and_cleaning.ipynb`](session_28_data_accessing_and_cleaning.ipynb)**

---

## 📂 Repository Contents

| File | Description |
| ---- | ----------- |
| [`patients.csv`](patients.csv) | Patient-level demographic information including contact details, addresses, anthropometrics (weight, height, BMI), and exact birthdates. |
| [`treatments.csv`](treatments.csv) | Treatment records for the main cohort, containing Auralin/Novodra dosage ranges and HbA1c metrics (start, end, change). |
| [`treatments_cut.csv`](treatments_cut.csv) | Additional treatment records (same schema as `treatments.csv`) that simulate delayed data or secondary site results. |
| [`adverse_reactions.csv`](adverse_reactions.csv) | Adverse reaction labels (e.g., hypoglycemia, injection site discomfort) keyed by patient name. |
| [`session_28_data_accessing_and_cleaning.ipynb`](session_28_data_accessing_and_cleaning.ipynb) | End-to-end Jupyter Notebook handling the assessment (visual & programmatic) and cleaning of the dataset. |

---

## 🔍 Data Quality Issues Addressed (Tidiness & Quality)

The data assessment phase in the notebook identifies and resolves multiple structural (tidiness) and quality issues:

### 🧩 **Tidiness Issues (Structural)**
- **Denormalized values**: `contact` column in `patients.csv` contains both phone numbers and email addresses.
- **Data fragmentation**: `treatments` and `treatments_cut` are the same dataset split into two files.
- **Multiple variables in one column**: `auralin` and `novodra` columns in treatments contain starting and ending doses combined by a dash (`-`).
- **Data merging**: `adverse_reactions.csv` contains labels that belong naturally inside the `treatments` table.
- **Variables stored as columns instead of rows**: The treatment type (`auralin` and `novodra`) should be a single categorical dimension `treatment_type`, not two separate columns.

### 🚩 **Quality Assessment (Completeness, Validity, Accuracy, Consistency)**
- **Missing values**: Missing demographics in `patients.csv` (12 missing values for address/city/state/zip/country/contact), missing `hba1c_change` in treatment files.
- **Inconsistent formatting**: Mixed state formats (full state names vs. standard 2-letter abbreviations) and varying naming capitalization.
- **Data anomalies**: Suspected erroneous outliers (e.g., incorrect heights/weights creating impossible BMI values) and duplicate records.
- **Placeholder symbols**: Dash (`-`) representing missing treatment values instead of standard `NaN` formatting.

---

## 🛠️ Cleaning Strategy & Workflow

The notebook strictly follows a **Define → Code → Test** standard software engineering pattern. 

1. **Pre-processing**: Loading data, making deep copies (`df.copy()`) before applying transformations.
2. **Standardization**:
   - Creating derived columns correctly (e.g., parsing phone vs email using regex).
   - Unifying string formatting (e.g., lowercasing names, converting state names to standard postal abbreviations).
3. **Data imputation and derivation**:
   - Calculating missing `hba1c_change` metrics using: $$\Delta HbA1c = HbA1c_{start} - HbA1c_{end}$$
4. **Data structuring**:
   - Utilizing `pd.melt()` and string splitting (`.str.split()`) to explode dosage ranges into `dosage_start` and `dosage_end`.
   - Concatenating (`pd.concat()`) `treatments` and `treatments_cut`.
   - Merging (`pd.merge()`) the adverse reactions DataFrame smoothly into the primary treatment records.

---

## 🚀 Getting Started (Reproducibility)

### Prerequisites

To execute the data cleaning pipeline locally, ensure you have the following installed:
- Python 3.10+
- `pandas`
- `numpy`
- `jupyter` / `jupyterlab`

### Execution Steps

1. Clone this repository to your local workspace.
2. Open [`session_28_data_accessing_and_cleaning.ipynb`](session_28_data_accessing_and_cleaning.ipynb) in VS Code or Jupyter Server.
3. Select your Python kernel.
4. Execute the cells sequentially from top to bottom.
5. Review the `.info()`, `.isnull()`, and `.duplicated()` outputs provided at each checkpoint to see the data quality improve interactively.

---

## 📌 Note

This project is intended for educational and demonstrational purposes. The datasets are fictional clinical data representations created strictly for data-cleaning training.
