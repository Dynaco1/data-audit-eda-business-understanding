# Capstone Part 1: Data Audit, EDA & Business Understanding

This repository contains **Part 1** of the D2C Customer Churn Intelligence & Retention API project. In this phase, we perform a comprehensive audit of the raw data, execute exploratory data analysis (EDA) to find churn drivers, and construct business hypotheses.

## Repository Contents

*   `eda_audit.ipynb`: Jupyter notebook containing the full data loading, schema inspection, left-joins, exploratory charts, and comments.
*   `data_quality_report.md`: Detailed report on duplicate-like records, gross amount outliers, missing values, date consistency, and potential data leakage.
*   `business_memo.md`: Concise, data-backed memo for leadership summarizing key findings and recommended investigations before launching campaigns.
*   `generate_eda_plots.py`: Python script used to programmatically generate the 6 high-quality EDA charts saved in `images/`.
*   `images/`: Directory containing pre-generated EDA charts for easy reference.
*   `requirements.txt`: Python package requirements for reproducibility.

## Key Insights

1.  **High Churn Risk:** The overall customer churn rate is **47.0%**.
2.  **Recency is Dominant:** Churn rates scale from **4.9%** (customers active within 15 days) to **89.3%** (inactive for over 120 days).
3.  **Digital Engagement Decay:** Drop in app/web sessions is a strong leading indicator of churn (0 sessions in last 30 days = 48.9% churn).
4.  **Silent Churners:** Customers who raise tickets actually churn at slightly lower rates than silent customers, proving engagement is positive. However, poor ticket resolution or returns trigger immediate churn.

## Installation & Setup

1.  Clone this repository or navigate to this folder.
2.  Make sure you have Python 3.11+ installed.
3.  Install the dependencies:
    ```bash
    pip install -r requirements.txt
    ```

## Running the Notebook

To open and run the Jupyter notebook:
```bash
jupyter notebook eda_audit.ipynb
```
The notebook will load the datasets relative to the `../dataset/` directory.
