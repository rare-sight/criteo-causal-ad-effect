# Causal Effect of Online Advertising on Conversion  
### Criteo Uplift Dataset – Propensity Score Analysis

## Business Question
What is the **true causal effect** of showing an online advertisement on the probability that a user converts?

A simple comparison of conversion rates between users who saw the ad and those who did not can be misleading if the two groups differ systematically. This project estimates the Average Treatment Effect (ATE) using modern causal inference methods and compares them to the naïve estimate.

## Dataset
- **Source**: [Criteo Uplift Modeling Dataset v2.1](https://ailab.criteo.com/criteo-uplift-prediction-dataset/)
- **Size**: ~14 million rows (analysis performed on a random sample of ~400k rows)
- **Treatment**: Binary indicator of ad exposure
- **Outcome**: Binary conversion
- **Features**: 12 anonymized continuous covariates (`f0`–`f11`)

## Methods
1. **Naive ATE** – Simple difference in conversion rates
2. **Covariate balance diagnostics** – Standardized Mean Differences (SMD)
3. **Propensity Score estimation** – Logistic regression
4. **Propensity Score Matching** – Nearest neighbor
5. **Inverse Probability Weighting (IPW)** – Stabilized weights
6. **Doubly Robust estimator** – Combines outcome regression + IPW

## Key Results

| Method                        | ATE Estimate | Percentage Points |
|-------------------------------|--------------|-------------------|
| Naive                         | 0.000994    | 0.0994 pp        |
| Propensity Score Matching     | 0.000872    | 0.0872 pp        |
| Inverse Probability Weighting | 0.000811    | 0.0811 pp        |
| Doubly Robust                 | 0.000739    | 0.0739 pp        |

### Main Findings
- All methods produce very similar estimates (0.074 – 0.099 percentage points).
- Covariates were already well balanced (|SMD| < 0.1 for all features), consistent with the randomized nature of the original experiment.
- Propensity scores were tightly concentrated around the overall treatment rate (~0.85) with excellent overlap.
- **Conclusion**: Ad exposure increases conversion probability by approximately **0.07–0.10 percentage points** (relative lift of roughly 35–47% against the control baseline).

## How to Run
```bash
git clone https://github.com/YOUR_USERNAME/criteo-causal-ad-effect.git
cd criteo-causal-ad-effect
pip install -r requirements.txt
jupyter notebook notebooks/01_causal_analysis.ipynb