# The Impact of China's DRG Payment Reform on Hospital Incentives and Inpatient Spending

**Authors:** Shijie He, Yuang Fu, Zejun Zhou
**Institution:** University of California, Los Angeles (UCLA)

## Overview

This project studies how China's Diagnosis-Related Group (DRG) payment reform has reshaped hospital behavior and inpatient healthcare spending. The reform replaces the traditional fee-for-service reimbursement system with a prospective payment mechanism: hospitals now receive a fixed payment for each diagnosis group, rather than being reimbursed for each individual service they provide. This change is intended to improve cost efficiency, but it also creates new incentives that may alter how hospitals treat and admit patients.

Using a province-year panel dataset and a difference-in-differences framework, the study estimates how variation in DRG exposure across Chinese provinces affects three outcomes: average inpatient cost per case, inpatient discharge volume, and total inpatient expenditure.

## Research Questions

The paper asks whether the DRG reform actually reduces aggregate healthcare spending or whether hospitals respond to the new incentives in ways that offset the intended savings. This reflects a classic tension in prospective payment systems: fixed per-case reimbursement can reduce treatment intensity, but providers may expand service volume to maintain revenue.

## Empirical Strategy

The model is a difference-in-differences panel regression with province and year fixed effects:

> Outcome_it = β × Exposure_post_it + Province FE + Year FE + ε_it

where Exposure_post is the interaction between a province's share of population covered by DRG implementation and a post-2019 indicator. Outcomes are logged (log average inpatient cost, log inpatient discharges, log total inpatient expenditure). A second specification adds province-level controls (GDP per capita, aging rate, physicians per 1,000 people, hospital beds per 1,000 people) as a robustness check.

Key data-construction choices:

- The 2019 pilot covered 30 cities. Exposure in 2019–2021 is the share of a province's population living in those pilot cities.
- After 2021, the nationwide rollout follows a 40% / 30% / 30% schedule, giving 40% exposure in 2022 and 70% in 2023 for provinces in the expansion phase.
- Beijing, Tianjin, Shanghai, and Chongqing are assigned 100% exposure starting in 2019 (Beijing is treated as fully exposed throughout the sample because it implemented a DRG-type system in 2011).
- Data from 2020 are excluded to avoid distortions from the COVID-19 pandemic.

## Main Findings

The baseline regression (province and year fixed effects, no controls) yields:

| Outcome               | Coefficient | Std. Error | p-value | R² | N |
|-----------------------|-------------|------------|---------|------|-----|
| Log avg inpatient cost  | −0.0274     | 0.0168     | 0.0498  | 0.9842 | 186 |
| Log inpatient discharges | +0.0455     | 0.0239     | 0.0566  | 0.9969 | 186 |
| Log total expenditure   | +0.0181     | 0.0211     | 0.3920  | 0.9958 | 186 |

Greater DRG exposure is associated with lower average inpatient cost per case and higher inpatient discharge volume. These two effects work in opposite directions, so the net effect on total inpatient expenditure is small and statistically insignificant. In other words, DRG reform improves cost efficiency on a per-case basis, but hospitals appear to offset those savings by treating more patients, limiting the reform's ability to reduce overall healthcare spending in the short run.

The specification with additional controls produces coefficients with the same signs but weaker statistical significance, which the paper attributes to the short panel and limited residual variation. The fixed-effects-only model is treated as the main result.

## Policy Implications

The findings have three implications for policymakers. First, admission behavior should be monitored so that increases in patient volume do not undermine cost-containment goals. Second, because fixed reimbursement can create incentives to avoid complex or high-cost patients, risk adjustment within the DRG classification system matters. Third, cost-control incentives should be paired with quality monitoring, such as readmission tracking, to make sure efficiency gains do not come at the expense of care quality.

## Repository Structure

```
china-drg-payment-reform/
├── README.md
├── data/
│   └── data_drg - Main Data.csv    # Province–year panel (2016–2024)
├── code/
│   └── analysis.ipynb              # Jupyter notebook with cleaning, DiD, and plots
└── paper/
    └── Econ 421 Final Project.pdf  # Full write-up
```

## Data

The analysis uses a province-year panel constructed from official Chinese statistical yearbooks. Variables include:

- `year`, `province`
- `inpatient_discharges` — total inpatient discharges in the province-year
- `avg_inpatient_cost` — average inpatient cost per case (yuan)
- `total_expenditure` — total inpatient expenditure (yuan), constructed as `avg_inpatient_cost × inpatient_discharges`
- `exposure` — province-level DRG exposure, constructed from pilot-city population shares and the 2021 rollout schedule
- `population`, `pilot_population`, `elderly_population`
- Controls: `gdp_per_capita`, `aging_rate`, `physicians_per_1000`, `beds_per_1000`

Primary data sources:

- National Bureau of Statistics of China — *National Data: Annual Population by Province.* https://data.stats.gov.cn/easyquery.htm?cn=E0103
- National Healthcare Security Administration — *Notice on the Publication of the List of National Pilot Cities for DRG Payment Reform* (2019). https://www.nhsa.gov.cn/art/2019/6/5/art_37_1362.html
- National Healthcare Security Administration — *Notice on Issuing the DRG/DIP Payment Reform Three-Year Action Plan* (2021). https://www.nhsa.gov.cn/art/2021/11/26/art_104_7413.html
- National Healthcare Security Administration — *Policy Interpretation of the Notice on Issuing the DRG/DIP Payment Reform Grouping Plan (Version 2.0)* (2024). https://www.nhsa.gov.cn/art/2024/7/23/art_105_13316.html

## How to Run the Code

### Requirements

- Python 3.9+
- `pandas`, `numpy`, `statsmodels`, `matplotlib`

Install dependencies:

```bash
pip install pandas numpy statsmodels matplotlib jupyter
```

### Run the notebook

```bash
jupyter notebook code/analysis.ipynb
```

The notebook does the following in order: (1) loads `data/data_drg - Main Data.csv`, (2) cleans numeric columns (stripping commas and percent signs, converting percentages to decimals), (3) builds the `post` dummy and the `exposure × post` interaction, (4) logs the outcome variables, (5) estimates the DiD regressions with and without controls, and (6) produces the descriptive time-series plots used in the paper.

If you cloned the repository and are running the notebook locally, update the `path` variable in the first cell to point to the local CSV (e.g., `path = "../data/data_drg - Main Data.csv"` when running from `code/`).

## References

- Boucher, A., Bowman, S., Piselli, C., & Scichilone, R. (2006). Evolution of DRGs. *Journal of AHIMA.*
- Busse, R. et al. (2013). Diagnosis-related groups in Europe: moving towards transparency, efficiency, and quality in hospitals? *BMJ* 346, f3197.
- Silverman, E., & Skinner, J. (2004). Medicare upcoding and hospital ownership. *Journal of Health Economics* 23(2), 369–389.
- Yip, W.C., Hsiao, W.C., Chen, W., Hu, S., Ma, J., & Maynard, A. (2012). Early appraisal of China's huge and complex health-care reforms. *Lancet* 379(9818), 833–842.

## License

This repository is provided for academic and educational use.
