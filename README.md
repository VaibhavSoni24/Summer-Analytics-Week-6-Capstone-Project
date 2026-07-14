# 🌍 Celonis x Summer Analytics 2026: Decarbonizing Travel Capstone

![Status](https://img.shields.io/badge/Status-Completed-success)
![ML Model](https://img.shields.io/badge/Model-CatBoost-orange)
![Task](https://img.shields.io/badge/Task-Binary_Classification%20%7C%20EDA-blue)

Welcome to the repository for the **Summer Analytics 2026 Capstone Project**, presented in partnership with **Celonis**. This project tackles a real-world corporate sustainability problem: identifying, analyzing, and predicting high-carbon business trips to support the ambitious **Net Zero by 2030** target.

This repository contains the complete end-to-end solution, including deep exploratory data analysis (EDA), executive-level presentations, and a robust machine learning pipeline.

---

## 📋 Project Overview

The challenge is divided into two primary parts:

1. **Part 1: Process Analysis & Dashboarding (Executive Pitch)**
   - **Goal:** Analyze the provided travel event logs and public trip datasets to uncover process inefficiencies, regional benchmarks, and emission hotspots.
   - **Output:** A consultant-style PowerPoint presentation (`Vaibhav_Soni_Capstone.pptx`) providing quantified recommendations and long-term strategic roadmaps to reduce corporate travel emissions.

2. **Part 2: High-Carbon Trip Prediction Challenge**
   - **Goal:** Build a predictive machine learning model to classify whether a business trip will be high-carbon (`HighCarbon = 1`) before it is booked, without using direct carbon emission features.
   - **Output:** A Jupyter Notebook (`capstone_part2.ipynb`) documenting the modeling pipeline and a CSV of predictions (`submission.csv`) for the hidden private test set.

---

## 📂 Repository Structure

```text
├── Part 1/
│   ├── README_ Sustainability Capstone - Decarbonizing Travel.pdf
│   ├── Sustainability Capstone Outline_ Decarbonizing Travel.pdf
│   ├── Sustainability Data Dictionary.xlsx
│   ├── _FullName__Capstone.pptx           # Original Template
│   └── Vaibhav_Soni_Capstone.pptx         # Final Output Presentation
├── Part 2/
│   ├── Part2.pdf                          # ML Challenge Instructions
│   ├── sample_submission.csv
│   ├── capstone_part2.ipynb               # Final ML Pipeline Notebook
│   └── submission.csv                     # Final Predictions for Private Set
├── Public Dataset/                        # Training Data & Event Logs
├── Private dataset/                       # Testing Data & Event Logs
├── Part2_Submission.zip                   # Packaged deliverables for Part 2
└── README.md                              # You are here!
```

---

## 🚀 Part 1: Exploratory Data Analysis & Strategic Pitch

Using **Python (Pandas, Matplotlib, Seaborn)**, a deep dive into the corporate travel process was conducted to answer 5 Key Business Questions. The findings were dynamically compiled into an executive presentation template using `python-pptx`.

### Key Insights Identified:
- **Emissions Hotspots:** Identified specific Business Units and short-haul flights that disproportionately contribute to the company's overall Scope 3 emissions footprint.
- **Process Inefficiencies:** Analyzed event attributes (`public_trip_event_attributes.csv`) and found strong correlations between high emissions and "out-of-policy" last-minute bookings.
- **Quantified Recommendations:** Proposed a 10% shift of short-distance flights to rail transport, leading to a conservatively estimated reduction of thousands of metric tons of CO2e annually.

---

## 🧠 Part 2: Machine Learning Methodology

### 1. Data Preprocessing & Feature Engineering
- **Data Merging:** Standard tabular trip data was merged with event attributes to capture process nuances (like booking delays and policy exceptions).
- **Prohibited Features:** Strictly removed all direct leakage features (`Departure_CO2e`, `Return_CO2e`, `Hotel_CO2e`, `Spend_CO2e`, `TotalCO2e`, and the target `HighCarbon`) from the feature space.
- **Imputation:** Handled missing categorical values with strict string constants (`Missing`) and numerical nulls with zero-padding to maintain matrix density.

### 2. Modeling Strategy
- **Algorithm:** Leveraged the **CatBoost Classifier**, chosen for its superior native handling of high-cardinality categorical variables (like `City`, `Country`, `BusinessUnit`) without requiring extensive One-Hot Encoding.
- **Cross-Validation:** Implemented a robust **5-Fold Stratified K-Fold** cross-validation to prevent overfitting and guarantee generalization to the unseen private test set.
- **Evaluation Metric:** Optimized for **ROC-AUC** (Receiver Operating Characteristic - Area Under Curve), matching the competition's primary evaluation criteria.

---

## 🛠️ Reproducibility

To reproduce the Machine Learning pipeline and predictions:

1. **Install Dependencies:**
   ```bash
   pip install pandas numpy scikit-learn catboost jupyter nbformat matplotlib seaborn python-pptx
   ```

2. **Run the Notebook:**
   Navigate into `Part 2/` and execute all cells in `capstone_part2.ipynb`. The notebook will automatically train the CatBoost models, generate predictions, and output `submission.csv`.

---
*Developed for Summer Analytics 2026. Data used is fictional and provided purely for educational purposes.*
