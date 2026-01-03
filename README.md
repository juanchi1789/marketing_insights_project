# Marketing Campaign Performance Analysis  
### Statistical, Mathematical and Machine Learning Approaches

## Overview

This repository is a personal research and experimentation project focused on **evaluating and understanding the performance of marketing campaigns** using a **data-driven approach**.

The goal is to explore different **quantitative methodologies** commonly used in modern analytics and marketing science, combining:

- Mathematical and statistical reasoning  
- Programmatic implementations in Python  
- Machine Learning techniques  
- Causal inference and experimentation frameworks  

This project is intended as a hands-on learning space to test ideas, validate assumptions, and compare approaches under realistic data scenarios.

---

## Objectives

The main objectives of this project are:

- To study **campaign effectiveness and incremental impact**
- To compare **classical statistical methods vs. machine learning approaches**
- To understand **causal inference techniques** applied to marketing problems
- To build **reproducible analytical workflows** using Python

---

## Topics Covered

This repository will progressively include notebooks and experiments related to:

### 1. Experimentation & Statistical Analysis
- A/B Testing
- Lift Tests
- Hypothesis Testing
- Confidence Intervals & Power Analysis
- Bootstrap methods

### 2. Causal Inference
- Difference-in-Differences (DiD)
- Synthetic Control
- Geo-based experiments (GeoLift-style approaches)
- Treatment & control group design
- Incrementality measurement

### 3. Marketing Measurement
- Funnel analysis
- Cohort analysis
- Retention & churn
- ROI and performance metrics
- Attribution concepts

### 4. Machine Learning Applications
- Uplift modeling
- Treatment effect estimation
- Causal ML (meta-learners, causal forests)
- Regularized regression (Lasso, Ridge, Elastic Net)
- Segmentation and clustering

### 5. Time Series & Marketing Mix Modeling
- Trend and seasonality analysis
- Lagged effects (adstock, carryover)
- Saturation effects
- Marketing Mix Modeling (MMM)

---

Raw datasets are not included in this repository.

---


## Project Structure

```text
marketing_insights_project/
│
├── data/
│   ├── raw/            # Raw datasets (not tracked)
│   └── processed/      # Cleaned datasets (not tracked)
│
├── notebooks/
│   ├── 00_testing.ipynb
│   ├── 01_ab_testing.ipynb
│   ├── 02_diff_in_diff.ipynb
│   ├── 03_synthetic_control.ipynb
│   ├── 04_uplift_modeling.ipynb
│   └── 05_mmm_basics.ipynb
│
├── src/
│   └── utils.py        # Reusable helper functions
│
├── docs/
│   └── Theory.md
│
├── README.md
└── .gitignore
