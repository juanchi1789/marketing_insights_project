# Models Overview

This document provides a structured overview of the main models used in marketing performance analysis and causal inference, ordered from simple baseline approaches to more advanced and flexible methods.

---

## 1. Difference in Means
Basic comparison of average outcomes between treatment and control groups.

$$
\mathbb{E}[Y \mid T=1] - \mathbb{E}[Y \mid T=0]
$$

---

## 2. Classical A/B Testing
Randomized controlled experiments used to estimate causal effects under ideal conditions.

---

## 3. Lift Tests
Application of experimental methods to measure the incremental impact of marketing campaigns.

$$
\text{Lift} = \mathbb{E}[Y_T] - \mathbb{E}[Y_C]
$$

---

## 4. Simple Linear Regression 
Linear model with a single explanatory variable, often used to represent treatment effects.

$$
Y = \alpha + \beta T + \varepsilon
$$

---

## 5. Multiple Linear Regression and Logistic regrassion
Extension of linear regression including multiple control variables to adjust for confounding.

$$
Y = \alpha + \beta T + \gamma^\top X + \varepsilon
$$

---

## 6. Difference-in-Differences (DiD)
Quasi-experimental method comparing changes over time between treated and control groups.

$$
\left(Y_{T,post} - Y_{T,pre}\right) - \left(Y_{C,post} - Y_{C,pre}\right)
$$

---

## 7. Event Study
Dynamic extension of Difference-in-Differences that estimates treatment effects across multiple time periods.

---

## 8. Synthetic Control
Method that constructs a weighted combination of control units to approximate the counterfactual outcome.

---

## 9. Geo Experiments
Experimental designs where treatment is assigned at a geographic level.

---

## 10. GeoLift
Specialized geo-experimental framework combining spatial and temporal variation to estimate incremental impact.

---

## 11. Marketing Mix Models (MMM)
Aggregate-level regression models that relate marketing spend to business outcomes over time.

---

## 12. Bayesian Marketing Mix Models
Bayesian formulation of MMM allowing uncertainty quantification and prior information.

---

## 13. Rule-Based Attribution Models
Heuristic methods that assign credit to marketing touchpoints using fixed rules.

---

## 14. Data-Driven Attribution Models
Statistical or machine learning approaches that infer attribution patterns from data.

---

## 15. Uplift Modeling
Models that estimate heterogeneous treatment effects at the individual or segment level.

$$
\mathbb{E}[Y(1) - Y(0) \mid X]
$$

---

## 16. Causal Machine Learning
Machine learning methods explicitly designed to estimate causal effects and treatment effect heterogeneity.
