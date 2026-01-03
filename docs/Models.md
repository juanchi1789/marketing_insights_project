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

---

## 1. Difference in Means


### 1.1 Description

The *Difference in Means* is the most basic approach used to compare outcomes between two groups. It consists of estimating the effect of an intervention by computing the difference between the average outcome of a treated group and the average outcome of a control group.

This method does not rely on complex modeling assumptions and serves as the foundational building block for more advanced causal inference techniques such as A/B testing, regression analysis, and difference-in-differences.

---

### 1.2 Model Formulation

Let:
- $Y$ be the outcome variable of interest (e.g., sales, revenue, conversions),
- $T \in \{0,1\}$ be a binary indicator where $T=1$ denotes treatment and $T=0$ denotes control.

The Difference in Means estimator is defined as:

$$
\Delta = \mathbb{E}[Y \mid T=1] - \mathbb{E}[Y \mid T=0]
$$

In practice, this expectation is estimated using sample averages:

$$
\hat{\Delta} = \bar{Y}_T - \bar{Y}_C
$$

where:
- $\bar{Y}_T$ is the sample mean of the treated group,
- $\bar{Y}_C$ is the sample mean of the control group.

---

### 1.3 Interpretation

The estimator $\hat{\Delta}$ represents the **average difference in outcomes** between the treated and control groups.

Under ideal conditions (e.g., randomized assignment), this difference can be interpreted as the **causal effect** of the treatment on the outcome.

---

### 1.4 Use Cases

The Difference in Means is commonly used as:

- A baseline estimator in experimental analysis
- A preliminary diagnostic before applying more complex models
- A simple way to quantify lift or incremental impact
- A benchmark to validate more advanced causal methods

In marketing analytics, it is often applied to:
- campaign impact analysis,
- A/B test evaluation,
- lift measurement.

---

### 1.5 Assumptions

For the Difference in Means to have a causal interpretation, the following assumptions are required:

1. **Comparability**: The treatment and control groups are comparable.
2. **No systematic selection bias**: Assignment to treatment is independent of potential outcomes.
3. **Stable Unit Treatment Value Assumption (SUTVA)**: The treatment of one unit does not affect the outcome of another unit.

When these assumptions hold, the estimator identifies the Average Treatment Effect (ATE).

---

### 1.6 Advantages

- Simple and intuitive
- Easy to compute and interpret
- Transparent and explainable
- Requires minimal modeling assumptions
- Serves as a clear baseline

---

### 1.7 Limitations

- Highly sensitive to selection bias
- Cannot adjust for confounding variables
- Assumes comparability between groups
- Not suitable for observational data without additional adjustments
- Does not account for time dynamics or covariates

As a result, the Difference in Means is best suited for randomized experiments or as a descriptive comparison.

---

### 1.8 Illustrative Example

Consider a marketing campaign where a group of users is exposed to an advertisement (treatment group), while another group is not (control group).

Suppose the observed average revenue per user is:

$$
\bar{Y}_T = 120 \quad \text{and} \quad \bar{Y}_C = 100
$$

The estimated effect of the campaign is:

$$
\hat{\Delta} = 120 - 100 = 20
$$

This result indicates that, on average, treated users generated \$20 more revenue than control users.

If treatment assignment was randomized, this difference can be interpreted as the causal impact of the campaign. Otherwise, the estimate may reflect pre-existing differences between the groups rather than the true effect of the intervention.

---

### 1.9 Role in the Modeling Hierarchy

The Difference in Means serves as the conceptual starting point for most causal inference methods. More advanced models can be seen as extensions that relax its assumptions or incorporate additional structure, such as covariates, time dynamics, or heterogeneous effects.

---
## 2. Classical A/B Testing


### 2.1 Description

Classical A/B Testing is an experimental framework designed to estimate the causal effect of an intervention by randomly assigning units to a treatment group and a control group. The defining characteristic of A/B testing is **randomization**, which ensures that, in expectation, treated and control units are statistically equivalent prior to the intervention.

As a result, any systematic difference observed in outcomes after treatment can be attributed to the intervention itself rather than to confounding factors.

A/B testing is considered the gold standard for causal inference and serves as the reference point against which observational methods are evaluated.

---

### 2.2 Potential Outcomes Framework

Let:
- $Y_i(1)$ denote the potential outcome for unit $i$ under treatment,
- $Y_i(0)$ denote the potential outcome for unit $i$ under control,
- $T_i \in \{0,1\}$ indicate treatment assignment.

The observed outcome is:

$$
Y_i = T_i Y_i(1) + (1 - T_i) Y_i(0)
$$

The individual treatment effect is defined as:

$$
\tau_i = Y_i(1) - Y_i(0)
$$

Since only one of the two potential outcomes is observed for each unit, individual treatment effects are not directly observable.

---

### 2.3 Average Treatment Effect (ATE)

The primary estimand in classical A/B testing is the **Average Treatment Effect (ATE)**:

$$
ATE = \mathbb{E}[Y(1) - Y(0)]
$$

Under random assignment, the ATE can be identified as:

$$
ATE = \mathbb{E}[Y \mid T=1] - \mathbb{E}[Y \mid T=0]
$$

This equality holds because randomization guarantees independence between treatment assignment and potential outcomes:

$$
T \perp \{Y(1), Y(0)\}
$$

---

### 2.4 Estimation via Difference in Means

In practice, expectations are approximated by sample averages. Let:
- $N_T$ be the number of treated units,
- $N_C$ be the number of control units.

The ATE estimator is:

$$
\widehat{ATE} = \frac{1}{N_T} \sum_{i:T_i=1} Y_i - \frac{1}{N_C} \sum_{i:T_i=0} Y_i
$$

This estimator is:
- unbiased for the true ATE,
- consistent as sample size increases.

---

### 2.5 Sampling Variability and Standard Errors

Due to random sampling, the estimator $\widehat{ATE}$ is itself a random variable.

An estimate of its variance is given by:

$$
\mathrm{Var}(\widehat{ATE}) =
\frac{s_T^2}{N_T} + \frac{s_C^2}{N_C}
$$

where:
- $s_T^2$ is the sample variance in the treatment group,
- $s_C^2$ is the sample variance in the control group.

The standard error is:

$$
SE(\widehat{ATE}) = \sqrt{\mathrm{Var}(\widehat{ATE})}
$$

---

### 2.6 Hypothesis Testing

A/B testing typically involves testing the null hypothesis of no treatment effect:

$$
H_0: ATE = 0
$$

against the alternative:

$$
H_1: ATE \neq 0
$$

A test statistic can be constructed as:

$$
Z = \frac{\widehat{ATE}}{SE(\widehat{ATE})}
$$

Under standard regularity conditions and sufficiently large samples, this statistic is approximately normally distributed.

---

### 2.7 Confidence Intervals

A $(1-\alpha)$ confidence interval for the ATE is given by:

$$
\widehat{ATE} \pm z_{1-\alpha/2} \cdot SE(\widehat{ATE})
$$

where $z_{1-\alpha/2}$ denotes the corresponding quantile of the standard normal distribution.

Confidence intervals provide information about both:
- statistical significance,
- magnitude and uncertainty of the effect.

---

### 2.8 Assumptions

The validity of classical A/B testing relies on the following assumptions:

1. **Random Assignment**: $T_i$ is independent of $\{Y_i(1), Y_i(0)\}$.
2. **SUTVA**: No interference between units and no hidden versions of treatment.
3. **Compliance**: Units adhere to assigned treatment.
4. **Stable Environment**: No external shocks differentially affect treatment and control groups.

Violations of these assumptions can lead to biased estimates.

---

### 2.9 Advantages

- Strong causal identification
- Transparent estimation procedure
- Minimal modeling assumptions
- Straightforward interpretation
- Robust benchmark for model validation

---

### 2.10 Limitations

- Requires the ability to randomize treatment
- May be costly or operationally infeasible
- Limited external validity beyond the experimental population
- Does not capture heterogeneous treatment effects without extensions
- May fail in the presence of spillovers or interference

---

### 2.11 Illustrative Example

Consider an experiment evaluating a new marketing message. Users are randomly assigned to treatment and control groups.

Suppose the observed outcomes are:

$$
\bar{Y}_T = 150, \quad \bar{Y}_C = 135
$$

with estimated standard error:

$$
SE(\widehat{ATE}) = 5
$$

The estimated ATE is:

$$
\widehat{ATE} = 150 - 135 = 15
$$

The corresponding test statistic is:

$$
Z = \frac{15}{5} = 3
$$

This result provides strong evidence that the marketing message causally increased the outcome of interest.

---

### 2.12 Relationship to Other Models

Classical A/B testing provides the benchmark against which observational and quasi-experimental methods such as Difference-in-Differences, Synthetic Control, and GeoLift are evaluated. These methods aim to recover causal effects under weaker assumptions when randomization is not feasible.

---
### 2.13 The P-value in A/B Testing

In the context of A/B testing, the p-value provides a quantitative measure of how compatible the observed data is with the null hypothesis of no treatment effect.

---

#### 2.13.1 Definition

Let the null hypothesis be:

$$
H_0: ATE = 0
$$

and let $\widehat{ATE}$ be the estimated average treatment effect with standard error $SE(\widehat{ATE})$.

The test statistic is defined as:

$$
Z = \frac{\widehat{ATE}}{SE(\widehat{ATE})}
$$

The **p-value** is defined as the probability, under the null hypothesis, of observing a test statistic at least as extreme as the one computed from the data:

$$
p = \mathbb{P}(|Z| \ge |z_{\text{obs}}| \mid H_0)
$$

---

#### 2.13.2 Interpretation

The p-value quantifies the evidence against the null hypothesis:

- A **small p-value** indicates that the observed difference is unlikely to have occurred by random chance alone.
- A **large p-value** indicates that the observed difference is consistent with random variation under $H_0$.

Importantly, the p-value **does not** represent:
- the probability that the null hypothesis is true,
- the probability that the observed effect was caused by randomness,
- the magnitude or business relevance of the effect.

---

#### 2.13.3 Statistical Significance

In practice, the p-value is compared against a predefined significance level $\alpha$ (commonly $\alpha = 0.05$):

$$
\text{Reject } H_0 \quad \text{if} \quad p < \alpha
$$

This decision rule controls the probability of a **Type I error**, defined as rejecting the null hypothesis when it is in fact true.

---

#### 2.13.4 P-value and Effect Size

A critical limitation of the p-value is that it depends on both:
- the estimated effect size,
- the sample size.

As sample size increases, even very small effects may become statistically significant:

$$
p \downarrow \quad \text{as} \quad N \uparrow
$$

For this reason, p-values should always be interpreted jointly with:
- the estimated ATE,
- confidence intervals,
- business impact metrics.

---

#### 2.13.5 Common Misinterpretations

Common misinterpretations of the p-value include:

- Interpreting $p=0.03$ as a 97% probability that the treatment worked.
- Concluding that a non-significant result implies no effect.
- Using statistical significance as the sole decision criterion.

Such interpretations can lead to incorrect conclusions in marketing decision-making.

---

#### 2.13.6 Role in A/B Testing Decisions

In applied A/B testing, the p-value serves as a **statistical diagnostic tool**, not as a decision rule by itself.

Sound decision-making should consider:
- the magnitude of the estimated effect,
- uncertainty (confidence intervals),
- practical significance,
- costs and operational constraints.

The p-value complements, but does not replace, substantive judgment and causal reasoning.


