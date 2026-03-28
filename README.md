# Urban-Pulse

**Urban-Pulse** is a data engineering and machine learning pipeline built on **Apache Spark (PySpark)** and **Databricks** that predicts urban flood risk based on historical monsoon rainfall data.

## Overview

The pipeline ingests historical rainfall records for Indian regions, engineers monsoon-season features, labels years by flood risk, and trains a Logistic Regression classifier — all within a Databricks notebook environment.

## Features

- Load and inspect rainfall data from a Databricks Delta table
- Clean and cast raw columns to numeric types, handling `"NA"` values
- Engineer a `monsoon_rainfall` feature (sum of June, July, August, September rainfall)
- Label each year as flood risk (`1`) or no flood risk (`0`) based on a 1500 mm threshold
- Train/test split and Logistic Regression model training with PySpark MLlib
- Persist the enriched dataset as a Delta table (`flood_prediction_data`)
- Visualise results with scatter plots, histograms, and box plots (matplotlib & seaborn)

## Tech Stack

| Component | Technology |
|-----------|------------|
| Data processing | Apache Spark / PySpark |
| Platform | Databricks |
| Machine learning | PySpark MLlib (Logistic Regression) |
| Visualisation | matplotlib, seaborn |
| Storage | Databricks Delta Lake |

## Pipeline Steps

1. **Load** — Read the `r2_india` Delta table into a Spark DataFrame.
2. **Select** — Keep only `YEAR`, `JUN`, `JUL`, `AUG`, `SEP`, `ANNUAL` columns.
3. **Cast** — Convert rainfall columns from strings to `double`, replacing `"NA"` with `null`.
4. **Feature engineering** — Create `monsoon_rainfall = JUN + JUL + AUG + SEP`.
5. **Labelling** — Assign `flood_risk = 1` when `monsoon_rainfall > 1500`, else `0`.
6. **Clean** — Drop rows with null values.
7. **Vectorise** — Use `VectorAssembler` to create a `features` column.
8. **Train** — Split 80/20 and fit a `LogisticRegression` model.
9. **Persist** — Write the labelled dataset to a Delta table.
10. **Visualise** — Plot flood risk vs. rainfall distributions.

## Getting Started

### Prerequisites

- A Databricks workspace (Runtime 10.x or later recommended)
- A Delta table named `r2_india` in your Databricks catalog with at least the columns: `YEAR`, `JUN`, `JUL`, `AUG`, `SEP`, `ANNUAL`

### Running the Notebook

1. Import `Commands.ipynb` into your Databricks workspace.
2. Attach it to a running cluster.
3. Run all cells in order.

## Data Schema

| Column | Type | Description |
|--------|------|-------------|
| `YEAR` | string/int | Year of the rainfall record |
| `JUN` | double | June rainfall (mm) |
| `JUL` | double | July rainfall (mm) |
| `AUG` | double | August rainfall (mm) |
| `SEP` | double | September rainfall (mm) |
| `ANNUAL` | double | Annual total rainfall (mm) |

## Output

- **Delta table** `flood_prediction_data` — enriched data with `monsoon_rainfall` and `flood_risk` labels
- **Model predictions** — displayed in the notebook comparing actual vs. predicted flood risk
- **Charts** — scatter plot, histogram, and box plot of rainfall distributions by flood risk category
