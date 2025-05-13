
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

> 💡 **Bayesian Inference Refresher — Contextualized to Gamma Severity Modeling**
>
> In Bayesian statistics, we update our beliefs about model parameters using **Bayes’ Theorem**:
>
> \[
> \text{Posterior} = \frac{\text{Likelihood} \times \text{Prior}}{\text{Evidence}} \quad \Rightarrow \quad p(\theta \mid x) = \frac{p(x \mid \theta) \cdot p(\theta)}{p(x)}
> \]
>
> Where:
> - \( \theta \) = model parameters (e.g., shape \(\alpha\), rate \(\beta\) of the Gamma distribution)
> - \( x \) = observed loss severities
> - \( p(\theta) \) = prior belief about parameters (e.g., HalfNormal for \(\alpha, \beta\))
> - \( p(x \mid \theta) \) = likelihood of observed data given parameters
> - \( p(\theta \mid x) \) = posterior distribution of parameters after seeing data
>
> In our case, we used:
>
> \[
> \begin{align*}
> \alpha &\sim \text{HalfNormal}(\sigma = 10) \\\\
> \beta &\sim \text{HalfNormal}(\sigma = 1 \times 10^{-4}) \\\\
> x_i &\sim \text{Gamma}(\alpha, \beta)
> \end{align*}
> \]
>
> PyMC used MCMC to sample from the posterior \( p(\alpha, \beta \mid x) \), giving us a full probabilistic picture of claim severity behavior.

