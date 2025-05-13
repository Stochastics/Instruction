
# Real-World Application: D&O Insurance Claims Modeling

During my time at **Aon**, I worked on predictive modeling for Directors & Officers (D&O) insurance claims. This required applying principles from probability, statistical distributions, and expected value modeling to support business decisions around risk and pricing.

## 🔄 Workflow Overview

1. **Historical Loss Adjustment**  
   We trended historical claims using CPI inflation adjustments to normalize values across years.

2. **Claim Probability Estimation**  
   We estimated the probability that a company would file a claim, modeled as a Bernoulli distribution. For example:
   - `P(claim) ≈ 0.25`

3. **Loss Severity Distribution Modeling**  
   For companies with claims, we fit various statistical distributions to model claim size:

   - **Lognormal Distribution**
   - **Gamma Distribution**
   - **Weibull Distribution**

4. **Expected Loss Calculation**  
   The expected loss was calculated as:

   \[
   \text{Expected Loss} = P(\text{Claim}) \times \mathbb{E}[\text{Severity}]
   \]

---

## 📊 Histogram of Simulated Claim Severities

![Histogram](histograms.png)

---

## 📈 Q-Q Plots for Distribution Fit

Used to visually assess goodness-of-fit of each distribution.

![QQ Plots](qq_plots.png)

---

## 📋 Expected Loss Comparison Table

| Distribution | Expected Loss |
|--------------|----------------|
| Lognormal    | $7,537 |
| Gamma        | $24,825 |
| Weibull      | $33,576 |

---

## 🔍 Summary of Statistical Concepts Used

- Probability theory (Bernoulli process)
- Distribution fitting (Lognormal, Gamma, Weibull)
- Goodness-of-fit analysis (visual Q-Q plots)
- Expected value calculation
- Inflation-adjusted normalization (CPI trend)

This project directly leveraged foundational statistics, making it a perfect real-world example of the power of probability, hypothesis testing, and distribution modeling in the insurance industry.


---

## ✅ Goodness-of-Fit: Kolmogorov-Smirnov Test

We evaluated the fit of each distribution using the **Kolmogorov-Smirnov (KS) test**. The lower the KS statistic and the higher the p-value, the better the distribution fits the data.

| Distribution | KS Statistic | p-value |
|--------------|--------------|---------|
| Lognormal    | 0.04278     | 0.74874 |
| Gamma        | 0.03691     | 0.88279 |
| Weibull      | 0.03166     | 0.96158 |

➡️ **Weibull** provided the best fit to our data, showing the lowest KS statistic and highest p-value.


---


---

## 🧠 Bayesian Distribution Fitting Extension

In addition to classical distribution fitting, we extended our analysis using **Bayesian inference** to model D&O insurance claim severities. This approach allowed us to not only estimate point values for parameters but also to quantify **uncertainty** around them.

### 🧮 Why Use Bayesian Methods?

Bayesian modeling has several advantages in the insurance domain:

- **Flexibility**: We can specify any prior knowledge about the claim distribution's shape and update it based on observed data.
- **Uncertainty Quantification**: Posteriors provide full probability distributions for parameters instead of single estimates.
- **Small Sample Resilience**: Particularly helpful when data is sparse, skewed, or heavy-tailed.

### 📐 Implementation Overview

We used **Approximate Bayesian Computation (ABC)** to estimate parameters for loss distributions such as Gamma and Weibull:

- Defined priors for parameters like shape and scale:
  - Gamma(α, β), Weibull(λ, k)
- Generated synthetic samples using candidate parameters
- Computed distances between empirical and synthetic summaries (e.g., quantiles, means)
- Accepted parameters with the closest fit, iterating to build posterior distributions

This method was particularly inspired by:

> **Goffard, P.-O., & Laub, P. J. (2020)**. *Approximate Bayesian Computations to fit and compare insurance loss models*.  
> [https://arxiv.org/abs/2007.03833](https://arxiv.org/abs/2007.03833)

### 📈 Practical Use

The posterior predictive distributions were used to:
- Simulate **future losses** with uncertainty bands
- Generate **credible intervals** for expected loss
- Guide **model comparison** with Bayes factors or posterior log-likelihoods


### 🆚 Bayesian vs. Classical (Frequentist) Approach

| Feature                  | Classical (Frequentist)                      | Bayesian                                           |
|--------------------------|----------------------------------------------|----------------------------------------------------|
| **Model Output**         | Point estimates and confidence intervals     | Full posterior distributions                       |
| **Interpretation**       | Probability is long-run frequency            | Probability expresses belief/uncertainty           |
| **Uncertainty**          | Confidence intervals around point estimates | Credible intervals from posterior distributions    |
| **Incorporates Priors**  | No                                           | Yes, through prior distributions                   |
| **Flexibility**          | Limited to parametric forms, assumptions     | Can incorporate domain knowledge and be nonparametric |
| **Parameter Estimation** | Via MLE or method of moments                 | Via Bayes’ Rule using observed data                |

**Example**:  
When fitting a Gamma distribution to model claim severities:
- The classical approach uses maximum likelihood to estimate shape and scale parameters, yielding point estimates and standard errors.
- The Bayesian approach defines priors for shape and scale, then uses observed data to update these beliefs, resulting in full posterior distributions.

This allowed us to produce more nuanced forecasts of extreme loss events—especially important when working with small or skewed insurance datasets where frequentist assumptions may fail.

---

---

## 📊 Bayesian Model Output

### 📌 Posterior Distributions for Parameters

These show the uncertainty in our estimates of the Gamma distribution’s shape (`alpha`) and rate (`beta`) parameters.

![Posterior Distributions](posterior_distributions.png)

### 📌 Posterior Predictive Distribution

This histogram shows simulated claim severities drawn from the Bayesian model. It reflects both the data and the prior assumptions.

![Posterior Predictive](posterior_predictive.png)


> 💡 **Bayesian Inference — Simple Example with Normal Likelihood and Normal Prior**
>
> Let's say we observe data from a Normal distribution with **known variance** (σ² = 1), but we **don’t know the mean** (μ). Our goal is to estimate μ using Bayesian inference.
>
> ---
> ### 🔍 Step 1: Likelihood
> Assume we observe one data point:
>
> ```
> x = 6.0
> ```
>
> We model this as:
>
> ```
> x ~ Normal(μ, σ²=1)
> ```
>
> The **likelihood function** is:
>
> ```
> p(x | μ) ∝ exp( -0.5 * (x - μ)² )
> ```
>
> ---
> ### 🎯 Step 2: Prior
> Suppose we believe μ is around 0, but we're uncertain. We use a **Normal prior**:
>
> ```
> μ ~ Normal(0, τ²=4)
> ```
>
> The **prior distribution** is:
>
> ```
> p(μ) ∝ exp( -0.5 * (μ - 0)² / 4 )
> ```
>
> ---
> ### 🧠 Step 3: Posterior ∝ Likelihood × Prior
> We combine the two:
>
> ```
> p(μ | x) ∝ exp( -0.5 * (x - μ)² ) × exp( -0.5 * (μ² / 4) )
> ```
>
> Multiply the exponents:
>
> ```
> p(μ | x) ∝ exp( -0.5 * [ (x - μ)² + μ² / 4 ] )
> ```
>
> Plug in `x = 6.0`:
>
> ```
> p(μ | x=6) ∝ exp( -0.5 * [ (6 - μ)² + μ² / 4 ] )
> ```
>
> This is the **unnormalized posterior**. It also turns out to be a Normal distribution! (Because the Normal is conjugate to itself.)
>
> ---
> ### ✅ Result
> If we do the math, the posterior turns out to be:
>
> ```
> μ | x ~ Normal(mean = 4.8, variance = 0.8)
> ```
>
> So after seeing the data, our updated belief about μ is centered near 4.8, with much tighter uncertainty than our original prior.
