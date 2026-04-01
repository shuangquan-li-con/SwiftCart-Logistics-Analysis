## Project Overview: Causal Inference and Statistical Analysis

This notebook demonstrates several key statistical and causal inference techniques, including bootstrapping for confidence intervals, permutation testing for hypothesis testing, and propensity score matching for estimating treatment effects in observational studies.

### 1. Bootstrapping for Median Confidence Interval

**Objective:** Estimate the 95% confidence interval for the median of a highly skewed, zero-inflated dataset (simulated driver tips) using bootstrapping.

**Methodology:**
- Generated a dataset `driver_tips` with a significant number of zeros and a right-skewed exponential distribution.
- Performed 10,000 bootstrap resamples from the `driver_tips` dataset.
- Calculated the median for each bootstrap sample.
- Constructed a 95% confidence interval using the 2.5th and 97.5th percentiles of the bootstrap medians.

**Results:**
- Sample Median: `0.755`
- 95% Confidence Interval: `(0.264, 1.364)`
- The resulting confidence interval is asymmetric, accurately reflecting the skewed and zero-inflated nature of the original data.

### 2. Permutation Testing for Difference in Means

**Objective:** Test if there is a statistically significant difference between the means of two simulated groups (control and treatment) using a permutation test.

**Methodology:**
- Generated `control` data from a normal distribution and `treatment` data from a lognormal distribution.
- Calculated the `observed_diff` between the means of the control and treatment groups.
- Performed 5,000 permutation samples:
  - Combined the `control` and `treatment` data.
  - Shuffled the combined data and randomly assigned observations to pseudo-control and pseudo-treatment groups of the original sizes.
  - Calculated the difference in means for each permutation.
- Calculated the p-value as the proportion of permutation differences whose absolute value was greater than or equal to the absolute `observed_diff`.

**Results:**
- Observed Difference: `2.265`
- p-value: `0.0004`
- The very low p-value suggests that the observed difference is statistically significant, indicating a real difference between the control and treatment groups.

### 3. Propensity Score Matching for Causal Effect Estimation

**Objective:** Estimate the Average Treatment Effect on the Treated (ATT) for subscriber status on `post_spend` using propensity score matching, accounting for confounding variables.

**Data:** `swiftcart_loyalty.csv` containing subscriber status, pre-spend, account age, support tickets, and post-spend.

**Methodology:**
1.  **Naive Difference in Means:** Calculated the simple difference in `post_spend` between subscribers (treated) and non-subscribers (control) without accounting for confounders.
    - Naive SDO: `17.57`

2.  **Propensity Score Estimation:**
    - Used Logistic Regression to model the probability of being a subscriber (`subscriber`) based on pre-treatment covariates (`pre_spend`, `account_age`, `support_tickets`).
    - Calculated propensity scores for each individual.

3.  **Nearest Neighbor Matching:**
    - For each treated individual (subscriber), found the closest control individual (non-subscriber) based on their propensity score using 1-to-1 nearest neighbor matching.

4.  **Average Treatment Effect on the Treated (ATT):**
    - Calculated the difference in `post_spend` between each treated individual and their matched control.
    - Averaged these differences to estimate the ATT.
    - ATT after matching: `9.91`

5.  **Balance Check (Love Plot):**
    - Calculated Standardized Mean Differences (SMDs) for covariates before and after matching.
    - A Love Plot visually demonstrates the reduction in covariate imbalance after matching, showing that SMDs for covariates are much closer to zero post-matching.

**Conclusion:**
Propensity score matching significantly reduced the imbalance in covariates between the treated and control groups. The estimated ATT after matching (`9.91`) is lower than the naive SDO (`17.57`), suggesting that part of the naive difference was due to pre-existing differences (confounding) rather than solely the effect of subscription. The Love Plot confirms that the matching process effectively balanced the observable covariates.# SwiftCart-Logistics-Analysis
