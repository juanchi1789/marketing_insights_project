# Theoretical Framework

# Index

## 1. Introduction and Scope

## 2. Probability, Random Variables and Expectations
- Random variables and distributions
- Expectation and variance  
- Conditional expectation  
- Law of iterated expectations  

## 3. Correlation vs Causation

## 4. The Potential Outcomes Framework
- Treatment and control  
- Potential outcomes: \( Y(1), Y(0) \)  
- Individual Treatment Effect (ITE)  
- Average Treatment Effect (ATE)  

## 5. The Fundamental Problem of Causal Inference

## 6. Experimental Data and Randomization
- Randomized experiments  
- A/B testing logic  
- Identification through randomization  

## 7. Observational Data and Bias
- Selection bias  
- Confounding variables  
- Omitted variable bias  

## 8. Causal Estimation vs Predictive Modeling

## 9. Introduction to Quasi-Experimental Methods
- Identification strategies  
- Common assumptions  

## 10. Time and Aggregation in Marketing Data
- Temporal dependence  
- Geographic aggregation  
- Lagged effects  

---

## 1. Introduction and Scope

The objective of this document is to provide a **mathematical and theoretical perspective** on the analysis of marketing performance.

Rather than focusing on the implementation details or specific tools, this section aims to formalize the core concepts that underpin modern marketing analytics, including:

- Measurement of campaign effectiveness  
- Incremental impact and causal effects  
- Experimental and quasi-experimental designs  
- Statistical foundations  
- Connections between classical inference and machine learning approaches  

The main goal is to build a rigorous conceptual foundation that supports the empirical analyses presented in the accompanying notebooks.

---

## 2. Probability, Random Variables and Expectations

## 2. Random Variables and Expectations

In this section, we introduce the basic statistical concepts that are required to formally describe and analyze data arising from marketing activities. The focus is on intuitive definitions and operational interpretations rather than on abstract probability theory.

---

### 2.1 Random Variables

A random variable represents a **quantitative outcome** of an uncertain process. In applied settings, random variables are used to model observable quantities whose exact value cannot be determined in advance.

In the context of marketing analytics, common examples of random variables include:

- Weekly sales  
- Revenue per user  
- Number of conversions  
- Click-through rate  
- Campaign exposure indicators  

Formally, a random variable is denoted as:

$$
Y \in \mathbb{R}
$$

where $Y$ represents a numerical outcome observed across individuals, time periods, or geographic units.

---

### 2.2 Measures of Central Tendency

Measures of central tendency summarize the typical or representative value of a random variable.

---

#### 2.2.1 Mean (Arithmetic Average)

The **mean** is the most commonly used measure of central tendency and serves as the foundation for many statistical estimators used in marketing performance analysis.

Given a sample $\{Y_1, Y_2, \dots, Y_N\}$, the sample mean is defined as:

$$
\bar{Y} = \frac{1}{N} \sum_{i=1}^{N} Y_i
$$

**Interpretation:**
- Represents the average outcome across observations
- Sensitive to extreme values (outliers)

**Marketing interpretation:**
- Average conversion rate  
- Average revenue per user (ARPU)  
- Average campaign lift  

---

#### 2.2.2 Median

The **median** is the value that separates the ordered data into two equal halves.

Formally, the median is defined as the 50th percentile of the empirical distribution:

$$
\text{Median}(Y) = \text{middle value of the ordered sample}
$$

**Interpretation:**
- Robust to outliers
- Often more representative of a typical unit when distributions are skewed

**Marketing interpretation:**
- Typical user revenue  
- Typical transaction value  

---

#### 2.2.3 Mean vs Median

The difference between the mean and the median provides insight into the shape of the distribution:

- Mean $\approx$ Median: symmetric distribution  
- Mean $>$ Median: right-skewed distribution  
- Mean $<$ Median: left-skewed distribution  

This distinction is particularly relevant in marketing data, where revenue and spend variables often exhibit heavy tails.

---

### 2.3 Measures of Dispersion

Measures of dispersion quantify the variability of a random variable around its central value.

---

#### 2.3.1 Variance

The **variance** measures the average squared deviation from the mean.

It is defined as:

$$
\mathrm{Var}(Y) = \frac{1}{N} \sum_{i=1}^{N} (Y_i - \bar{Y})^2
$$

**Interpretation:**
- Quantifies uncertainty or noise in the data
- Larger variance implies more heterogeneous outcomes

---

#### 2.3.2 Standard Deviation

The **standard deviation** is the square root of the variance:

$$
\sigma_Y = \sqrt{\mathrm{Var}(Y)}
$$

**Interpretation:**
- Expressed in the same units as the original variable
- Widely used to construct confidence intervals and statistical tests

---

### 2.4 The Expectation Operator

The expectation operator provides a formal way to describe long-run average outcomes.

---

#### 2.4.1 Expectation as a Long-Run Average

The expectation of a random variable $Y$ is defined as:

$$
\mathbb{E}[Y] = \lim_{N \to \infty} \frac{1}{N} \sum_{i=1}^{N} Y_i
$$

**Interpretation:**
- Represents the average value that would be observed over repeated realizations
- Serves as the theoretical counterpart of the sample mean

---

#### 2.4.2 Conditional Expectation

The **conditional expectation** represents the expected value of a random variable given a condition or subgroup.

Formally:

$$
\mathbb{E}[Y \mid T = 1], \quad \mathbb{E}[Y \mid T = 0]
$$

where $T$ typically denotes a treatment or exposure indicator.

**Interpretation:**
- Average outcome among treated units
- Average outcome among control units

Conditional expectations form the basis of experimental analysis, difference-in-differences estimators, and uplift modeling.

---

### 2.5 Hypothesis Testing and Statistical Significance

Statistical hypothesis testing provides a framework for assessing whether observed differences in data can plausibly be attributed to random variation.

---

#### 2.5.1 Null and Alternative Hypotheses

A statistical test begins with the specification of two competing hypotheses:

- Null hypothesis ($H_0$): no effect or no difference  
- Alternative hypothesis ($H_1$): presence of an effect  

Example:

$$
H_0: \mathbb{E}[Y_T] = \mathbb{E}[Y_C]
$$

$$
H_1: \mathbb{E}[Y_T] \neq \mathbb{E}[Y_C]
$$

---

#### 2.5.2 The P-value

The **p-value** measures the degree of compatibility between the observed data and the null hypothesis.

Formally, the p-value is defined as the probability of observing a result **at least as extreme as the one obtained**, assuming that the null hypothesis is true.

**Interpretation:**
- Small p-values indicate evidence against $H_0$
- Large p-values indicate insufficient evidence to reject $H_0$

Importantly, the p-value:
- Is **not** the probability that $H_0$ is true
- Does **not** measure the size of an effect

---

#### 2.5.3 Limitations of the P-value

While widely used, p-values have important limitations:

- They do not convey effect magnitude  
- They are sensitive to sample size  
- Statistical significance does not imply practical relevance  

For these reasons, modern marketing analysis emphasizes effect sizes, confidence intervals, and causal interpretation alongside hypothesis testing.

---

### 2.6 From Descriptive Statistics to Causal Analysis

Descriptive statistics summarize observed data, while causal analysis seeks to explain how outcomes would change under different interventions.

The expectation operator serves as the conceptual bridge between these two perspectives, enabling formal definitions of treatment effects and incremental impact, which will be developed in subsequent sections.

---
### 2.7 Distributions

A distribution characterizes how a random variable can take values. In practice, distributions help us:
- model uncertainty,
- choose appropriate summary statistics,
- justify statistical tests and confidence intervals,
- define likelihood-based models.

---

#### 2.7.1 Probability Mass Function (PMF) and Probability Density Function (PDF)

**Discrete** random variables are described by a *probability mass function* (PMF):

$$
p_Y(y) = \mathbb{P}(Y = y), \qquad \sum_{y} p_Y(y) = 1.
$$

**Continuous** random variables are described by a *probability density function* (PDF):

$$
f_Y(y) \ge 0, \qquad \int_{-\infty}^{\infty} f_Y(y)\,dy = 1.
$$

For a continuous random variable, probabilities are computed by integrating the density:

$$
\mathbb{P}(a \le Y \le b) = \int_{a}^{b} f_Y(y)\,dy.
$$

---

#### 2.7.2 Cumulative Distribution Function (CDF)

For both discrete and continuous variables, the *cumulative distribution function* (CDF) is:

$$
F_Y(y) = \mathbb{P}(Y \le y).
$$

The CDF is non-decreasing and satisfies:

$$
\lim_{y \to -\infty} F_Y(y) = 0, \qquad \lim_{y \to \infty} F_Y(y) = 1.
$$

---

#### 2.7.3 The Normal Distribution

The normal distribution is widely used due to its analytical convenience and its role as an approximation for many aggregated metrics.

Notation:

$$
Y \sim \mathcal{N}(\mu, \sigma^2).
$$

PDF:

$$
f_Y(y) = \frac{1}{\sigma \sqrt{2\pi}} \exp\!\left( -\frac{(y-\mu)^2}{2\sigma^2} \right).
$$

Key moments:

$$
\mathbb{E}[Y] = \mu, \qquad \mathrm{Var}(Y) = \sigma^2.
$$

---

#### 2.7.4 Bernoulli and Binomial Distributions (Binary outcomes)

Binary outcomes are central in marketing analytics (e.g., conversion yes/no).

**Bernoulli distribution:**

$$
Y \sim \mathrm{Bernoulli}(p), \qquad Y \in \{0,1\}.
$$

PMF:

$$
\mathbb{P}(Y=y) = p^y (1-p)^{1-y}, \quad y \in \{0,1\}.
$$

Moments:

$$
\mathbb{E}[Y] = p, \qquad \mathrm{Var}(Y) = p(1-p).
$$

**Binomial distribution** (number of successes in $n$ independent Bernoulli trials):

$$
S \sim \mathrm{Binomial}(n,p).
$$

PMF:

$$
\mathbb{P}(S=k) = \binom{n}{k} p^k (1-p)^{n-k}, \quad k=0,1,\dots,n.
$$

Moments:

$$
\mathbb{E}[S] = np, \qquad \mathrm{Var}(S) = np(1-p).
$$

---

#### 2.7.5 Poisson Distribution (Counts)

Counts like events per time unit are often modeled with a Poisson distribution.

Notation:

$$
Y \sim \mathrm{Poisson}(\lambda), \qquad Y \in \{0,1,2,\dots\}.
$$

PMF:

$$
\mathbb{P}(Y=k) = \frac{\lambda^k e^{-\lambda}}{k!}, \quad k=0,1,2,\dots
$$

Moments:

$$
\mathbb{E}[Y] = \lambda, \qquad \mathrm{Var}(Y) = \lambda.
$$

---

#### 2.7.6 Lognormal Distribution (Skewed positive variables)

Revenue-like variables are often positive and right-skewed. A common model is the lognormal distribution.

Definition:

$$
Y \sim \mathrm{Lognormal}(\mu, \sigma^2) \quad \Longleftrightarrow \quad \log(Y) \sim \mathcal{N}(\mu,\sigma^2), \; Y>0.
$$

Mean and variance:

$$
\mathbb{E}[Y] = \exp\!\left(\mu + \frac{\sigma^2}{2}\right),
$$

$$
\mathrm{Var}(Y) = \left(\exp(\sigma^2)-1\right)\exp\!\left(2\mu + \sigma^2\right).
$$

---

#### 2.7.7 Quantiles and Percentiles

A quantile describes the value below which a given proportion of observations falls.

The $\tau$-quantile (for $\tau \in (0,1)$) can be defined using the CDF:

$$
q_\tau = F_Y^{-1}(\tau).
$$

The median is the special case $\tau=0.5$:

$$
\mathrm{Median}(Y) = q_{0.5}.
$$

---

#### 2.7.8 Why Distributional Shape Matters in Marketing Data

Marketing variables often exhibit:
- skewness (mean far from median),
- heavy tails (large outliers),
- heteroscedasticity (variance changes with scale).

These properties affect:
- interpretation of averages,
- robustness of statistical tests,
- modeling choices (e.g., transformations, robust estimators, or alternative likelihoods).

---
### 3. Correlation vs Causation

