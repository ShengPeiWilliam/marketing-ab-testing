# Marketing A/B Testing
Bayesian and Frequentist A/B testing on a digital marketing campaign dataset. 
Compared advertisement exposure (ad) versus public service announcement (psa) 
across 588,101 users, using Proportion test, Chi-square, Mann-Whitney U, 
Beta-Binomial, and Gamma-Poisson models. The ad group yields a higher conversion 
rate with 100% posterior probability, though the practical effect remains negligible.

## Motivation

After completing [Cookie Cats A/B Testing](https://github.com/ShengPeiWilliam/bayesian-ab-testing), 
the next natural question was whether the same framework could extend beyond 
a single overall comparison to **subgroup analysis**, identifying conditions 
under which an effect is strongest. This project applies that idea to a 
marketing context, examining how conversion rates vary across days and hours 
of peak ad exposure. Additionally, the dataset presents a more imbalanced 
group structure (96% ad vs 4% psa), offering a chance to explore how 
Bayesian inference handles unequal sample sizes through posterior uncertainty.

## Design Decisions

**Why Mann-Whitney for the randomization check?**

`total.ads` is heavily right-skewed with extreme values (max = 2,065). 
Mann-Whitney is a non-parametric alternative that does not assume normality, 
making it appropriate for verifying that the two groups received comparable 
ad exposure before proceeding with the primary analysis.

**Why Chi-square for subgroup analysis?**

Subgroup analysis involves multiple categories (7 days, 24 hours) against 
a binary outcome. Chi-square extends the two-sample proportion test to 
multiple groups, and standardized residuals identify which specific 
days or hours drive the overall difference.

## Key Results

The ad group achieves a conversion rate of 2.55% compared to 1.79% for psa, 
with 100% posterior probability. Despite statistical significance, Cohen's 
h = 0.053 indicates a negligible practical effect, a common pattern in 
large-sample A/B tests. Subgroup analysis identifies Monday and afternoon 
hours as the highest-performing conditions, with hour 16 reaching the peak.

**Frequentist**

| Metric | Test | p-value | Effect Size |
|--------|------|---------|-------------|
| total.ads (randomization) | Mann-Whitney U | < 0.001 | r = 0.0086 |
| converted | Proportion Test | < 0.001 | h = 0.053 |
| converted × day | Chi-square | < 0.001 | V = 0.027 |
| converted × hour | Chi-square | < 0.001 | V = 0.027 |

**Bayesian**

| Metric | P(ad > psa) | 95% HDI |
|--------|-------------|---------|
| total.ads (randomization) | 97.19% | [-0.0013, 0.1284] |
| converted | 100.00% | [0.0059, 0.0094] |

## Reflections & Next Steps

All effect sizes remain negligible despite statistical significance. With 
588,101 observations, even trivial differences reach significance, reinforcing 
that p-value alone is insufficient for business decisions.

Next steps:
- **Empirical Bayes**: estimate a data-driven prior from observed day/hour rates 
  to achieve partial pooling and more stable estimates for low-sample subgroups
- **Revenue metrics**: `converted` does not reflect revenue per user or 
  customer lifetime value, which would provide a more comprehensive evaluation 
  of advertisement effectiveness

## Repository

- `report/marketing_report.pdf`: Final report
- `code/marketing_analysis.ipynb`: Main analysis notebook
- `code/config.R`: Configuration file (data paths)

## Tools

R · ggplot2 · tidyr · dplyr · rstatix · HDInterval

## References

Vázquez, F. Marketing A/B Testing [Dataset]. Kaggle. 
https://www.kaggle.com/datasets/faviovaz/marketing-ab-testing