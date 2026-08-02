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

### Average Treatment Effect
| Method                        | ATE          | Percentage Points |
|-------------------------------|--------------|-------------------|
| Naive                         | 0.000994    | 0.0994 pp        |
| Propensity Score Matching     | 0.000872    | 0.0872 pp        |
| Inverse Probability Weighting | 0.000811    | 0.0811 pp        |
| Doubly Robust (preferred)     | 0.000739    | 0.0739 pp        |

The ad has a positive but small average effect on conversion.

### Uplift Modeling (T-Learner)
We went beyond the average effect and estimated **individual treatment effects**.

- Users were ranked by predicted uplift (benefit from seeing the ad).
- **Qini Coefficient = 29.72** → the model is significantly better than random targeting.
- The Qini curve shows that most incremental conversions come from the top 30–40% of users.

### Business / ROI Impact
**Assumptions:** Ad cost = $0.05 | Conversion value = $40

| Strategy                    | Expected Profit per User | Decision          |
|----------------------------|---------------------------|-------------------|
| Show ad to everyone        | –$0.020                   | Not profitable    |
| Target only top 30% uplift | +$0.113                   | **Profitable**    |

**Recommendation:** Do not run the campaign on the full population. Use the uplift model to prioritize high-response users.

## How to Run
```bash
git clone https://github.com/YOUR_USERNAME/criteo-causal-ad-effect.git
cd criteo-causal-ad-effect
pip install -r requirements.txt
jupyter notebook notebooks/01_causal_analysis.ipynb