---
title: Experimental Design Handbook
author: Lucas Cruz
date: 2024-05-27
category: experimentation
layout: post
permalink: /experimental-design
mermaid: true
---

This handbook provides an introductory guide to experimental design, made for data scientists and researchers. The material is based on the lecture notes from the University of Waterloo's STAT 430/830 course, which offers an in-depth exploration of statistical methods for designing and analyzing experiments. The goal is to present these concepts in a clear and precise manner, ensuring that readers can apply these principles to their own work in experimentation as I did.

## Chapter 1: Introduction

### 1.1 Notation and Nomenclature

In the context of this handbook, **an experiment refers to a systematic procedure where one or more factors are purposefully manipulated to observe their effect on a response variable**. The primary goal is to identify and quantify the differences in the response variable values across different conditions or treatments. For instance:

- **Experiment 1:** Nike, the athletic apparel company, is conducting an experiment with their mobile shopping interface to determine whether changing the user interface from a list view to a tile view will increase the proportion of customers proceeding to checkout.

- **Experiment 2:** Nixon, the watch and accessories brand, is testing four different video ad themes on Instagram. The themes include surfing, rock climbing, camping, and urban professional. The goal is to identify which ad theme is watched the longest on average.

**Metric of Interest**  
The metric of interest (MOI) is the statistic that the experiment aims to investigate. Typically, **the objective is to optimize this metric**, either by maximizing or minimizing it. For example, key performance indicators (KPIs) in business, such as click-through rates (CTRs), bounce rate, average time on page, and 95th percentile page load time, are common metrics of interest. In the Nike example, the checkout rate (COR) serves as the metric of interest, while in the Nixon example, the average viewing duration is the key metric.

**Response Variable**  
The response variable, denoted $y$, is the primary variable of interest that needs to be measured to calculate the metric of interest. For instance, in the Nike example, the response variable is a binary indicator of whether a customer checked out. In the Nixon example, it is the continuous measurement of viewing duration for each user.

**Factor**  
A factor, denoted $x$, is a secondary variable of interest that may influence the response variable. Factors are also known as covariates, explanatory variates, predictors, features, or independent variables. In the Nike experiment, the factor is the visual layout, whereas in the Nixon experiment, the factor is the ad theme.

**Experimental Conditions and Levels**  
Experimental conditions refer to the unique combinations of levels of one or more factors, also known as treatments, variants, or buckets. Levels are the specific values that a factor can take in an experiment. For Nike, the levels are tile view and list view, while for Nixon, the levels are the four ad themes: surfing, rock climbing, camping, and urban professional.

**Experimental Units**  
Experimental units are the entities assigned to the experimental conditions and on which the response variable is measured. In the Nike example, the experimental units are Nike mobile customers, and in the Nixon example, they are Instagram users. It is important to note that in many online experiments, the experimental unit is often a user or customer, but this is not always the case.

### 1.2 Experiments versus Observational Studies

An experiment involves a collection of conditions defined by purposeful changes to one or more factors. The primary goal is to identify and quantify differences in the response variable values across these conditions. In experiments, we intervene in the data collection process. For example, to determine whether a video ad’s theme significantly influences its average viewing duration, it is crucial to understand how experimental units respond when exposed to each condition. Although we cannot observe how the same unit behaves under all conditions, we can infer this by randomly assigning different units to different conditions and comparing their responses. Randomization ensures that the only difference between units in each condition is the condition itself, which helps facilitate causal inference.

In contrast, observational studies involve passively collecting data without any control over the data collection process. This limits the ability to establish causal connections between the factors and the response variable. However, observational studies are sometimes the only feasible option, especially when conducting certain experiments would be unethical or impractical. For instance, it would be unethical to force subjects to smoke to study the effects of smoking on lung cancer or to show different users different prices for the same product in dynamic pricing experiments. Similarly, it would be unethical to manipulate social media content to consistently show some users negative content and others positive content, even though such experiments have been conducted in the past.

**Advantages and Disadvantages**

|                         | Advantages                                    | Disadvantages                                    |
| ----------------------- | --------------------------------------------- | ------------------------------------------------ |
| **Experiment**          | Causal Inference is clean                     | Experiments might be unethical, risky, or costly |
| **Observational Study** | No additional cost, risk, or ethical concerns | Causal inference is muddy                        |

### 1.3 QPDAC: Answering Questions with Data

The QPDAC cycle is a structured approach for using data to answer questions. The first step is to clearly define the question, ensuring it is quantifiable and aligned with the metric of interest. For example, Nike might ask, "Which visual layout, tile view or list view, corresponds to the highest checkout rate?" Similarly, Nixon might ask, "Which ad theme, camping, surfing, rock climbing, or business, corresponds to the highest average viewing duration?"

<br>
```mermaid
graph LR;
Question-->Plan-->Data-->Analysis-->Conclusion-->Question;
```

<center>
Figure 1.1: QPDAC cycle
</center>
<br>

Next, in the planning stage, we design the experiment and address all pre-experimental questions. This involves choosing the response variable based on the question and metric of interest, identifying factors that might influence the response, and determining the experimental units, sample size, and sampling mechanism. For instance, factors could be:

1. **Design factors** that are manipulated in the experiment;
2. **Nuisance factors** that are controlled through _blocking_;
3. **Allowed-to-vary factors** that are neither controlled nor specifically considered.

Data collection follows the plan and must be executed correctly to ensure the validity of the analysis. **An A/A test can be conducted to verify the random assignment of units to conditions**. If the ratio of users between variants deviates significantly from the expected ratio, the experiment may suffer from a **sample ratio mismatch (SRM)**.

In the analysis stage, we statistically analyze the collected data to provide an objective answer to the question. This typically involves estimating parameters, fitting models, and conducting hypothesis tests. If the experiment was well-designed and the data correctly collected, the analysis should be straightforward.

Finally, in the conclusion stage, we interpret the results of the analysis and communicate the findings to all stakeholders. Clearly communicating both successful and unsuccessful outcomes helps foster a culture of experimentation.

### 1.4 Fundamental Principles of Experimental Design

**Randomization**  
Randomization is crucial in experimental design and occurs at two levels: selecting experimental units for inclusion and assigning them to experimental conditions. The first level ensures that the sample is representative of the population, allowing for generalizable conclusions. The second level balances the effects of extraneous variables, making conditions more homogeneous and facilitating causal inference.

**Replication**  
Replication involves having multiple response observations within each experimental condition, ensuring that observed results are genuine and not due to chance. For instance, in the Nike experiment, having 1000 units per condition provides more convincing results than just 2 units.

**Blocking**  
Blocking is a technique used to control nuisance factors by holding them fixed during the experiment. For example, in an email promotion experiment by GAP, different variations of the message in the subject line were tested to maximize the open rate. To control for the nuisance factor of "send time," all emails were sent at the same time of day and on the same day of the week, thereby eliminating its effect.

## Chapter 2: Experiments with Two Conditions

### 2.1 Anatomy of an A/B Test

A/B testing involves comparing two versions of a variable to determine which one performs better in terms of a specific metric. This type of experiment typically includes one design factor with two levels. For example, consider a canonical A/B test where the goal is to determine which button color, red or blue, results in a higher click-through rate (CTR).

<br>

```mermaid
graph LR;
id1["Button A"];
id2["Button B"];

style id1 fill:#fa828e
style id2 fill:#8284fa

```

<center>
Figure 2.1: Canonical Button Colour Test
</center>
<br>

Other practical examples of A/B tests include:

- **Amazon:** Testing checkout reassurances or comparing list view versus tile view.
- **Airbnb:** Testing host landing page redesigns or the next available date feature.

The primary objective of an A/B test is to decide which condition is optimal with respect to a metric of interest, denoted as $\theta$. This metric could be a mean (e.g., average time on page), a proportion (e.g., CTR, bounce rate), a variance, or a quantile (e.g., median page load time). The observation we make on the MOI is denoted as $\hat{\theta}$.

Consider a button color test where the observed CTRs for red and blue buttons are $\hat{\theta}\_1 = 0.12$ and $\hat{\theta}\_2 = 0.03$, respectively. Although $\hat{\theta}\_1 > \hat{\theta}\_2$, we need to statistically test whether $\theta_1$ is indeed greater than $\theta_2$.

#### 2.1.1 Hypothesis Testing

To determine if the observed differences are statistically significant, we formulate and test hypotheses using the data collected from the experiment. The hypotheses could be:

- $H_0: \theta_1 = \theta_2$ versus $H_A: \theta_1 \neq \theta_2$ (two-sided).
- $H_0: \theta_1 \leq \theta_2$ versus $H_A: \theta_1 > \theta_2$ (one-sided).
- $H_0: \theta_1 \geq \theta_2$ versus $H_A: \theta_1 < \theta_2$ (one-sided).

The goal is to decide whether to reject the null hypothesis ($H_0$) based on the observed data. This decision is made using a test statistic, denoted as $T$.

> ##### Definition: Test Statistic
>
> The test statistic, $T$, is a random variable that must:
>
> 1. Be a function of the observed data.
> 2. Be a function of the parameters $\theta_1$ and $\theta_2$.
> 3. Have a distribution that does not depend on $\theta_1$ or $\theta_2$.
>
> Assuming the null hypothesis is true, $T$ follows a particular distribution known as the null distribution (e.g., $\mathcal{N}(0, 1)$, $t$-distribution). We calculate the observed value of the test statistic, $t$, and evaluate its extremity relative to the null distribution.
{: .block-tip }

#### 2.1.2 Choosing the Significance Level

The significance level of the test, denoted $\alpha$, determines how extreme $t$ must be to reject $H_0$. Common choices for $\alpha$ are 0.05 and 0.01.

> ##### Definition: $p$-value
>
> The $p$-value is the probability of observing a value of the test statistic at least as extreme as the value we observed, assuming the null hypothesis is true. The $p$-value quantifies the extremity of the observed test statistic. The more extreme the value of $t$, the smaller the $p$-value, providing more evidence against the null hypothesis.
{: .block-tip }

- If $p$-value $\le \alpha$, we reject $H_0$.
- If $p$-value $> \alpha$, we do not reject $H_0$.

To choose $\alpha$, it is essential to understand the two types of errors that can occur in hypothesis testing:

- **Type I Error:** Rejecting $H_0$ when it is true.
- **Type II Error:** Not rejecting $H_0$ when it is false.

We aim to minimize both types of errors, though they have different consequences.

**Example: Pregnancy Test**

- $H_0$: The person is not pregnant.
- Type I Error: A non-pregnant person is classified as pregnant (false positive).
- Type II Error: A pregnant person is classified as not pregnant (false negative).

**Example: Courtroom Analogy**

- $H_0$: The defendant is innocent.
- Type I Error: Convicting an innocent person.
- Type II Error: Acquitting a guilty person.

> ##### Definition: Significance level
>
> The **significance level** of a test is $\alpha = \mathbb{P}$(Type I Error)
{: .block-tip }

> ##### Definition: Power
>
> The **power** of a test is the probability that the test correctly rejects a false null hypothesis. It is defined as $1- \beta$, where $\beta= \mathbb{P}$(Type II Error)
{: .block-tip }

### 2.2 Comparing Means in Two Conditions

In experiments comparing two conditions, we often measure the response variable on a continuous scale. We assume the response observations in the two conditions follow normal distributions:

$$
\begin{align*}
Y_{ij} &\sim \mathcal{N}(\mu_j, \sigma^2) \quad \forall i \in \{1, 2, \cdots, n_j\}, \forall j \in \{1,2\}
\end{align*}
$$

Where $Y_{ij}$ is the response observation for unit $i$ in condition $j$.

We test hypotheses of the form:

- $H_0: \mu_1 = \mu_2$ versus $H_A: \mu_1 \neq \mu_2$
- $H_0: \mu_1 \leq \mu_2$ versus $H_A: \mu_1 > \mu_2$
- $H_0: \mu_1 \geq \mu_2$ versus $H_A: \mu_1 < \mu_2$

#### 2.2.1 The Two-Sample $t$-Test

The Student's $t$-test is used to compare the means ($\mu_1$ and $\mu_2$) of two conditions, assuming the variances are unknown but equal.

> ##### Assumption
>
> The variances are equal, that is $\sigma_1=\sigma_2$.
{: .block-danger }

The test statistic is calculated as:

$$
\begin{align*}

T &= \frac{\text{Estimate} - \text{Hypothesis}}{\text{Standard Error of Estimate}}\\

T &= \frac{(\bar{Y}_1 - \bar{Y}_2) - \overbrace{(\mu_1-\mu_2)}^0}{\hat{\sigma} \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} \sim t(n_1 + n_2 - 2)

\end{align*}
$$

Where $\hat{\sigma}$ is the pooled standard deviation estimator, and $t(n_1+n_2-2)$ is our null distribution.

The observed version is:

$$
t=\frac{\hat{\mu_1}-\hat{\mu_2}}{\hat{\sigma}{\sqrt{\frac{1}{n_1} + \frac{1}{n_2}}}}
$$

Where:

$$
\begin{align*}
\hat{\mu_j} &= \frac{1}{n_j} \sum^{n_j}_{i=1} y_{ij} \\
\hat{\sigma}^2 &= \frac{(n_1-1)\hat{\sigma}_1^2 + (n_2-1)\hat{\sigma}_2^2}{n_1-n_2-2} \\
\hat{\sigma_j} &= \frac{1}{n_j-1} \sum^{n_j}_{i=1}(y_{ij}-\bar{y}_j)^2
\end{align*}
$$

We then compute the $p$-value to determine the significance of the results.

- For $H_0: \mu_1 = \mu_2$, versus $H_A: \mu_1 \neq \mu_2$, we compute $p$-value $= \mathbb{P}(T\ge \|t\|)+\mathbb{P}(T \le -\|t\|)$
- For $H_0: \mu_1 \le \mu_2$, versus $H_A: \mu_1 > \mu_2$, we compute $p$-value $= \mathbb{P}(T\ge t)$
- For $H_0: \mu_1 \ge \mu_2$, versus $H_A: \mu_1 < \mu_2$, we compute $p$-value $= \mathbb{P}(T\le t)$

#### 2.2.2 The Welch's $t$-Test

When the assumption of equal variances does not hold, Welch's $t$-test is a more appropriate method for comparing the means of two independent samples. The test statistic for Welch's $t$-test is given by:

$$
T = \frac{(\bar{Y}_1 - \bar{Y}_2) - \overbrace{(\mu_1-\mu_2)}^0}{\sqrt{\frac{\hat{\sigma}_1^2}{n_1} + \frac{\hat{\sigma}_2^2}{n_2}}} \sim t(\nu)
$$

Where $\hat{\sigma}_1^2$ and $\hat{\sigma}_2^2$ are the sample variances for the two groups. The degrees of freedom for the test statistic are approximated by:

$$
\nu = \frac{\left( \frac{\hat{\sigma}_1^2}{n_1} + \frac{\hat{\sigma}_2^2}{n_2} \right)^2}{\frac{\left( \frac{\hat{\sigma}_1^2}{n_1} \right)^2}{n_1 - 1} + \frac{\left( \frac{\hat{\sigma}_2^2}{n_2} \right)^2}{n_2 - 1}} \approx \min(n_1,n_2)-1
$$

The observed version is:

$$
t = \frac{\hat{\mu_1} - \hat{\mu_2}}{\sqrt{\frac{\hat{\sigma}_1^2}{n_1} + \frac{\hat{\sigma}_2^2}{n_2}}}
$$

The test statistic $T$ follows a $t$-distribution with $\nu$ degrees of freedom. The $p$-value is calculated based on this distribution to determine if there is a significant difference between the means of the two groups.

#### 2.2.3 F-Test for Variances

The F-test is used to compare the variances of two independent samples. The test statistic is defined as:

$$
F = \frac{\hat{\sigma}_1^2}{\hat{\sigma}_2^2} \sim F(n_1-1, n_2-1)
$$

Where $\hat{\sigma}_1^2$ and $\hat{\sigma}_2^2$ are the sample variances of the two groups. Under the null hypothesis that the variances are equal ($\sigma_1^2 = \sigma_2^2$, or that $\sigma_1^2/\sigma_2^2 =1$), the test statistic follows an $F$-distribution with $(n_1 - 1)$ and $(n_2 - 1)$ degrees of freedom.

The $p$-value is calculated as follows:

$$
p\text{-value} =
\begin{cases}
\mathbb{P}(F \ge f) + \mathbb{P}(F \le 1/f) ,& \text{If } t\ge 1 \\
\mathbb{P}(F \le f) + \mathbb{P}(F \ge 1/f) ,& \text{If } t < 1
\end{cases}
$$

Where $f$ is the observed value of the test statistic. The $p$-value determines whether there is a significant difference in variances between the two groups.

#### 2.2.4 Example: Instagram Ad Frequency

Suppose you are a data scientist at Instagram, and you want to investigate the influence of ad frequency on user engagement. Users currently see an ad every 8 posts, but your manager wants to increase ad frequency to every 5 posts. You hypothesize that this change will decrease user engagement, measured by average session time, in minutes, ($y$).

The Hypothesis here is:

$$
H_0: \mu_1 \le \mu_2 \text{ versus } H_A: \mu_1 > \mu_2
$$

The data summaries for the two conditions (7:1 ad frequency and 4:1 ad frequency) show that:

- Condition 1 (7:1): $n_1 = 500$, $\hat{\mu}_1 = \bar{y}_1 = 4.916$, $\hat{\sigma}_1 = s_1 = 0.963$
- Condition 2 (4:1): $n_2 = 500$, $\hat{\mu}_2 = \bar{y}_2 = 3.052$, $\hat{\sigma}_2 = s_2 = 0.995$

##### $F$-test

- $t = \hat{\sigma}^2_1/\hat{\sigma}^2_2 = (963)^2/(995)^2 = 0.938$
- $p$-value $= \mathbb{P}(T \le 0.938) + \mathbb{P}(T \ge 1/0.938) = 0.472$ where $T\sim F(499,499)$
- This $p$-value is larger than any ordinary $\alpha$, so we do not reject $H_0: \sigma^2_1 = \sigma^2_2$, and so we continue with Students's $t$-test.

##### Student's $t$-test

- $\hat{\sigma}^2 = \frac{499(0.963)^2+499(0.995)^2}{998} = (0.979)^2$
- $t = \frac{4.916-3.052}{0.979\sqrt{\frac{1}{500}+\frac{1}{500}}} = 30.101$
- $p$-value $= \mathbb{P}(T \ge 30.101) = 1.838 \times 10^{-142}$ where $T \sim t(998)$
- This $p$-value is much smaller than any typical $\alpha$, and so we reject $H_0: \mu_1 \le \mu_2$, and conclude that increasing ad frequency significantly reduces average session duration.

### 2.3 Comparing Proportions in Two Conditions

When the response variable is binary (e.g., indicating whether an action was performed), we often assume a Bernoulli distribution. The $Z$-test for proportions compares the probabilities ($\pi_1$ and $\pi_2$) that the action of interest occurs in each condition.

In cases like these, we let

$$
Y_{ij} = 
\begin{cases}
    1 \quad\text{if unit $i$ in condition $j$ performs an action of interest}         &i=1,2,\cdots,n_j \\
    0 \quad\text{if unit $i$ in condition $j$ does not perform an action of interest} &j=1,2
\end{cases} 
$$

The test statistic for the Z-test is given by:

$$
Z = \frac{(\bar{Y}_1 - \bar{Y}_2) - \overbrace{(\pi_1 - \pi_2)}^0}{\sqrt{\hat{\pi}(1 - \hat{\pi})\left( \frac{1}{n_1} + \frac{1}{n_2} \right)}} \sim \mathcal{N}(0,1)
$$

Where:
- $\hat{\pi}_1$ and $\hat{\pi}_2$ are the sample proportions for the two groups.
- $\hat{\pi}$ is the pooled proportion, calculated as:

$$
\hat{\pi} = \frac{n_1 \hat{\pi}_1 + n_2 \hat{\pi}_2}{n_1 + n_2} = \frac{\text{# units who performed action}}{\text{total # units in exp}}
$$

The observed version is:

$$
z = \frac{(\hat{\pi}_1 - \hat{\pi}_2)}{\sqrt{\hat{\pi}(1 - \hat{\pi})\left( \frac{1}{n_1} + \frac{1}{n_2} \right)}}
$$


The hypotheses for the Z-test can be formulated as:
- $H_0: \pi_1 = \pi_2$ versus $H_A: \pi_1 \neq \pi_2$ (two-sided)
- $H_0: \pi_1 \leq \pi_2$ versus $H_A: \pi_1 > \pi_2$ (one-sided)
- $H_0: \pi_1 \geq \pi_2$ versus $H_A: \pi_1 < \pi_2$ (one-sided)


The $p$-value for the Z-test is calculated based on the standard normal distribution, except that $Z \sim \mathcal{N}(0,1)$:

$$
p\text{-value} = 
\begin{cases} 
2 \cdot P(Z \geq |z|) & \text{for a two-sided test} \\
P(Z \geq z) & \text{for a right-sided test} \\
P(Z \leq z) & \text{for a left-sided test} 
\end{cases}
$$

Where $z$ is the observed value of the test statistic.

#### 2.3.1 Example: Optimizing Optimizely

Consider a scenario where Optimizely is interested in determining if a redesigned homepage leads to a significant increase in the number of new accounts created compared to the original homepage.

The hypothesis here is:

$$
H_0: \pi_1 \ge \pi_2 \text{ versus } H_A: \pi_1 < \pi_2
$$

We summarize the data from this experiment in a $2 \times 2$ contingency table:

| Conversion | Condition 1 | Condition 2 | **Total**  |
|------------|-------------|-------------|--------|
| Yes        | 280         | 399         | **679**    |
| No         | 8592        | 8243        | **16835**  |
| **Total**  | **8872**    | **8642**    | **17514** |


The conversion rates for the original and redesigned homepages are $\hat{\pi}_1 = 280/8872 = 0.032$ and $\hat{\pi}_2 = 399/8642 = 0.046$ respectively.

The pooled proportion is calculated as:

$$
\hat{\pi} = \frac{8872(0.032)+8642(0.046)}{17514} = 0.039
$$

The test statistic is:

$$
z = \frac{0.032 - 0.046}{\sqrt{0.039 \times (1 - 0.039) \left( \frac{1}{8872} + \frac{1}{8642} \right)}} = -5.007
$$

$p$-value $= \mathbb{P}(T\le -5.007) = 2.758\times 10^{-7}$, where $T \sim \mathcal{N}(0,1)$

We reject $H_0$ and conclude that the redesigned homepage significantly increases conversion rate.


### 2.4 Power Analysis and Sample Size Calculations

Power analysis helps control Type II errors and determine the required sample size for detecting a meaningful effect. The power of a test, defined as $1 - \beta$, is the probability of correctly rejecting a false null hypothesis ($H_0$). A higher power indicates a greater likelihood of detecting an effect when it exists.

#### 2.4.1 Hypothesis Testing Design

Suppose we are interested in testing the hypothesis:

$$
H_0: \theta_1 = \theta_2 \quad \text{versus} \quad H_A: \theta_1 \neq \theta_2
$$

Where $\theta_1$ and $\theta_2$ are the parameters of interest (e.g., means or proportions) in two groups.

##### Test Statistic

Assume the test statistic $T$ follows a normal distribution under the null hypothesis:

$$
T = \frac{(\bar{Y}_1 - \bar{Y}_2) - 0}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}} \sim \mathcal{N}(0, 1)
$$

Where $\bar{Y}_1$ and $\bar{Y}_2$ are the sample means, $\sigma_1^2$ and $\sigma_2^2$ are the population variances, and $n_1$ and $n_2$ are the sample sizes.

##### Rejection Region

The rejection region, $\mathcal{R}$, is defined as the set of values for the test statistic that would lead to the rejection of $H_0$:

$$
\mathcal{R} = \{t: H_0 \text{ is rejected}\}
$$

Where $z_{\alpha/2}$ is the critical value from the standard normal distribution corresponding to the significance level $\alpha$ in a two-tailed test, and it is $z_{\alpha}$ in a one-tailed test.

![Reject region in different test types](/assets/gitbook/images/experimental-design/reject-region.png)

##### Defining Type I and II Error Rates

Defining the error rates in terms of a reject region is also useful (refer to the original material for more detailed derivation):

###### Type I

$$
\alpha = \mathbb{P}(\text{Type I Error}) = \mathbb{P}(\text{Reject } H_0|H_0 \text{ is true}) = \mathbb{P}(T \in \mathcal{R}|H_0 \text{ is true})
$$

###### Type II

$$
\beta = \mathbb{P}(\text{Type II Error}) = \mathbb{P}(\text{Do not Reject } H_0|H_0 \text{ is false}) = \mathbb{P}(T \in \mathcal{R}^\complement | H_0 \text{ is false})
$$

##### Sample Size Calculation

From the definitions above, we can derive and rearrange the number of samples as:

$$
n = \frac{(z_{\alpha/2} - z_{1-\beta})^2 [\mathbb{V}(Y_1) + \mathbb{V}(Y_2)]}{\delta^2}  
$$

Where $z_{1-\beta}$ is the critical value corresponding to the desired power $1 - \beta$. $\mathbb{V}(Y_1)$ and $\mathbb{V}(Y_2)$ are the variances of the response in the two conditions, these need to be guessed or determined by historical information.

$\delta = \theta_1 - \theta_2$ is the **minimum detectable effect** (MDE).

> ##### Definition: Minimum Detectable Effect (MDE)
>
> The **minimum detectable effect**, denoted $\delta$, is the smallest difference between conditions (i.e, between $\theta_1$ and $\theta_2$) that we find to be practically relevant and we would like to detect as being statistically significant.
{: .block-tip }

#### 2.4.2 Example: Calculating Sample Size

Suppose we want to detect a difference in means between two groups with the following parameters:

- Significance level: $\alpha = 0.05$
- Desired power: $1 - \beta = 0.80$
- Population variances: $\sigma_1^2 = 1.5$ and $\sigma_2^2 = 2.0$
- Minimum detectable effect: $\delta = 0.5$


Using the critical values $z_{0.025} = 1.96$ and $z_{0.20} = 0.84$, the required sample size for each group is:

$$
n = \left( \frac{1.96 + 0.84}{0.5} \right)^2 \left( 1.5 + 2.0 \right) = \left( 5.6 \right)^2 \cdot 3.5 = 109.76 \approx 110
$$

Thus, each group should contain at least 110 participants to achieve the desired power.

Power analysis and sample size calculations are essential for designing robust experiments that can detect meaningful effects while controlling for Type I and Type II errors.

### 2.5 Permutation and Randomization Tests

All the previous tests have made some kind of distributional assumption for the response measurements, such as $Y_{ij} \sim \mathcal{N}(\mu_j, \sigma^2)$ or $Y_{ij} \sim \text{Binomial}(1, \pi_j)$. It would be preferable to have a test that does not rely on any assumptions. This is precisely the purpose of permutation and randomization tests.

These tests are non-parametric and rely on resampling. The motivation is that if $H_0: \theta_1 = \theta_2$ is true, any random rearrangement of the data is equally likely to have been observed. If $H_0$ is true, then we have a single population/distribution from which our data has been drawn. With $n_1$ and $n_2$ units in each condition, there are:

$$
\binom{n_1 + n_2}{n_1} = \binom{n_1 + n_2}{n_2}
$$

arrangements of the $n_1 + n_2$ observations into two groups of size $n_1$ and $n_2$, respectively. For example, if $n_1 = n_2 = 50$, then:

$$
\binom{100}{50} = 1.009 \times 10^{29}
$$

A true permutation test considers all possible rearrangements of the original data. The test statistic $t$ is calculated on the original data and on every one of its rearrangements. This collection of test statistic values generates the empirical null distribution.

In a randomization test, we do not consider all possible rearrangements. Instead, we consider a large number $N$ of them. We use this in practice instead of a permutation test because the exact permutation tests have too many permutations to consider.

#### 2.5.1 Randomization Test Algorithm

1. Collect response observations in each condition:
   
    $$
    \begin{align}
    \{ y_{11}, y_{21}, \ldots, y_{n_1 1} \} \rightarrow \hat{\theta}_1 \\

    \{ y_{12}, y_{22}, \ldots, y_{n_2 2} \} \rightarrow \hat{\theta}_2
    \end{align}
    $$

2. Calculate the test statistic $t$ on the original data:
   
    $$
    t = \hat{\theta}_1 - \hat{\theta}_2 \quad \text{or} \quad t = \frac{\hat{\theta}_1}{\hat{\theta}_2}
    $$

3. Pool all the observations together and randomly sample (without replacement) $n_1$ observations which will be assigned to “Condition 1” and the remaining $n_2$ observations that are assigned to “Condition 2.” Repeat this $N$ times:
   
    $$
    \begin{align}
    \{ y^*_{11}, y^*_{21}, \ldots, y^*_{n_1 1} \} \rightarrow \hat{\theta}^*_1 \\
    \{ y^*_{12}, y^*_{22}, \ldots, y^*_{n_2 2} \} \rightarrow \hat{\theta}^*_2
    \end{align}
    $$

4. Calculate the test statistic $t^*_k$ on each of the “shuffled” datasets, $k = 1, 2, \ldots, N$:
   
    $$
    t^*_k = \hat{\theta}^*_1 - \hat{\theta}^*_2 \quad \text{or} \quad t^*_k = \frac{\hat{\theta}^*_1}{\hat{\theta}^*_2}
    $$

5. Compare $t$ to $\\{ t_1^\*, t_2^\*, \cdots, t_N^\* \\}$, the empirical null distribution, and calculate the $p$-value:
   
   $$
   \begin{align*}
   p\text{-value} &= \frac{\# \text{ of } t^* \text{'s that are at least as extreme as } t}{N} \\ 
    &= \frac{1}{N} \sum_{k=1}^{N} \mathbb{I}\{ t^*_k \text{ is at least as extreme as } t \}
   \end{align*}
   
   $$

- $H_0: \theta_1 = \theta_2$ versus $H_A: \theta_1 \neq \theta_2$. If $t = \hat{\theta}_1 - \hat{\theta}_2$, then the $p$-value is:
  
  $$
  p\text{-value} = \frac{1}{N} \sum_{k=1}^{N} \mathbb{I}\{ t^*_k \geq |t| \cup t^*_k \leq -|t| \}
  $$

- $H_0: \theta_1 \geq \theta_2$ versus $H_A: \theta_1 < \theta_2$. If $t = \hat{\theta}_1 - \hat{\theta}_2$, then the $p$-value is:
  
  $$
  p\text{-value} = \frac{1}{N} \sum_{k=1}^{N} \mathbb{I}\{ t^*_k \leq t \}
  $$

- $H_0: \theta_1 \leq \theta_2$ versus $H_A: \theta_1 > \theta_2$. If $t = \hat{\theta}_1 - \hat{\theta}_2$, then the $p$-value is:
  
  $$
  p\text{-value} = \frac{1}{N} \sum_{k=1}^{N} \mathbb{I}\{ t^*_k \geq t \}
  $$