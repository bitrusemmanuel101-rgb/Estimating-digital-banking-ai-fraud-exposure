# Digital Banking Fraud Exposure: Bootstrapped Confidence Interval Analysis of NeurIPS BAF Data

*Last updated: November 2026*

## Summary

This project estimates average credit exposure across 11,029 confirmed fraudulent bank account applications from the NeurIPS Bank Account Fraud (BAF) dataset over an 8-month observation window. High-velocity automated attacks (top 25% of 6-hour application velocity) show a mean proposed credit limit of **$773.32** (95% CI: $750.02 to $796.98), compared to standard human-behavioral fraud at **$854.13** (95% CI: $840.32 to $868.32). Because the two intervals do not overlap, the data show a clear difference in fraud strategy: automated attacks rely on high volume at mid-tier credit limits, whereas human-behavioral fraud targets larger individual credit lines.

[View Executive Summary](visuals/executive-summary.jpg)

## Background & Research Questions

In 2026, financial institutions face rising risks from synthetic identities and automated bot applications. Using extreme 6-hour application velocity (`velocity_6h` > 75th percentile) as a behavioral proxy for automated attacks, this project answers two questions:

1. What is the estimated average credit exposure (`proposed_credit_limit`) per fraudulent application for high-velocity automated attacks?
2. How does that exposure compare with lower-velocity, human-behavioral fraud?

## Dataset & Methodology

- **Source:** NeurIPS 2022 Bank Account Fraud (BAF) Dataset Suite (`Base.csv`) - https://kaggle.com
- **Scope:** 1,000,000 total applications across an 8-month cycle (`month` 0–7), filtered to 11,029 confirmed fraud cases (`fraud_bool == 1`).
- **Segmentation:** Split at the 75th percentile of `velocity_6h` into High-Velocity Automated Proxy (`n = 2,757`) and Standard Human-Behavioral Fraud (`n = 8,272`).
- **Approach:** Checked skewness (0.47 and 0.35) and observed discrete credit-limit tiers; applied non-parametric bootstrapping (10,000 resamples with replacement, seed 700) to compute 95% confidence intervals for each cohort's mean exposure.

## Results

| Fraud Cohort | Fraud Count | Mean Exposure ($) | 95% Confidence Interval ($) |
| :--- | :--- | :--- | :--- |
| **High-Velocity (Automated Proxy)** | 2,757 | $773.32 | $750.02 to $796.98 |
| **Standard (Human-Behavioral)** | 8,272 | $854.13 | $x to $x |

- **Non-Overlapping Risk Profiles:** The 95% confidence intervals are separated by more than $40, confirming that standard fraud carries higher per-account credit exposure than high-velocity automated fraud.
- **Volume vs. Limit Size:** Automated scripts trade lower individual credit limits (~$773) for rapid scale, while manual fraudsters target higher individual limits (~$854).

## Recommendations & Limitations

- **Underwriting Controls:** Cap initial `proposed_credit_limit` at $500 for applications exceeding the 75th percentile of `velocity_6h` to limit automated bust-out exposure.
- **Enhanced Verification:** Trigger step-up identity verification on applications requesting high credit limits ($1,000+), even when application velocity appears normal.
- **Limitations:** High velocity is a behavioral proxy rather than direct proof of AI; financial exposure assumes 100% Exposure at Default (EAD) on approved credit limits; calendar dates are anonymized into monthly cohorts (`month` 0–7).

## Tools & Repository Structure

- **Tools:** Python (`pandas`, `NumPy`, `SciPy`, `Matplotlib`, `Seaborn`), Jupyter Notebook, Git, GitHub.
- **Structure:** `data/raw/` (`Base.csv`), `notebooks/`, `requirements.txt`, `README.md`.

*Author: Emmanuel Bitrus*
