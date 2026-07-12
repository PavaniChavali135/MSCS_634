# MSCS_634_ProjectDeliverable_1: Healthcare Data Mining

## Project Overview
This repository contains the first deliverable for the Advanced Big Data and Data Mining course project. The objective of this phase is to ingest, clean, and visually explore a complex healthcare informatics dataset to establish a robust, noise-free foundation for future machine learning modeling (including predictive regression, classification, and patient clustering).

## Dataset Summary
**Dataset:** `healthcare_dataset.csv`  
**Description:** This dataset contains 55,500 patient encounters across 15 attributes, capturing demographic profiles, clinical data (medical condition, test results), and administrative metrics (billing amounts, admission types). It was selected because health informatics provides a rich environment for data mining, offering diverse data types (temporal, categorical, and continuous) that mirror real-world operational challenges in hospital management.

## Major Data Cleaning Steps
1. **Deduplication:** Scanned the dataset and successfully removed over 500 exact duplicate patient records that would have artificially skewed future algorithm training.
2. **Text Standardization:** Corrected severe casing inconsistencies in string columns (e.g., patient names) using title-casing transformations to ensure data uniformity.
3. **Noise Neutralization:** Identified logically impossible data entries, specifically negative values in the `Billing Amount` column. These noisy entries were successfully imputed using the median of valid, positive billing records to preserve the dataset's statistical integrity without discarding valuable clinical rows.
4. **Temporal Feature Engineering:** Converted raw string dates for admission and discharge into operational datetime objects. This allowed for the creation of a new, highly valuable mathematical feature: `Length_of_Stay`.

## Key Insights from Exploratory Data Analysis (EDA)
* **Demographics:** The histogram analysis revealed a relatively uniform distribution of patient ages, indicating the hospital treats a balanced demographic rather than heavily leaning toward a single age cohort.
* **Financial Variance:** Box plots tracking `Billing Amount` against `Medical Condition` showed highly overlapping interquartile ranges. This implies that the specific medical condition alone is not the sole driver of hospital bills; other factors (like length of stay or insurance provider) are likely compounding variables.
* **Correlations:** The correlation heatmap indicated that linear correlations between raw numeric variables (Age, Billing Amount, Length of Stay) are exceptionally low (near 0.00). 

## Challenges and Resolutions
**Challenge:** Discovering that the foundational numeric features exhibited almost zero linear correlation, which can initially make predictive regression modeling seem daunting.  
**Resolution:** This lack of simple linear correlation is a classic challenge in healthcare data. It directly informs the strategy for Deliverable 2: simple linear regression will likely fail. We will need to rely heavily on advanced feature engineering (e.g., One-Hot Encoding categorical variables like `Insurance Provider` and `Admission Type`) and eventually utilize non-linear models (like Random Forests or Gradient Boosting) to capture the complex relationships dictating patient outcomes and billing.