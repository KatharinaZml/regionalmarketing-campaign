# regional-marketing-campaign

# Difference-in-Differences: Simulated Regional Marketing Campaign

**SQL (BigQuery) + Python analysis practicing the Difference-in-Differences (DiD) causal inference method** on the public `thelook_ecommerce` dataset — including parallel trends validation, a systematic control-group screening approach, and placebo testing.

## About

This project practices the full DiD workflow end-to-end: panel data construction, treatment simulation, parallel trends validation, regression and finally placebo testing.

**Business question:** Did a regional marketing campaign in California and Texas increase weekly orders/revenue, compared to states that received no campaign?

**Why simulated treatment?** `thelook_ecommerce` is a synthetic dataset with no real marketing campaigns or policy shocks to exploit as a "natural experiment". A treatment effect (a 15% revenue uplift) was simulated and injected into the data. This is a standard approach for practicing causal inference methodology. This notebook demonstrates the DiD workflow. However, it does not make a "real" business claim about California/Texas customers.

## What's in this notebook

1. **SQL panel construction** — weekly orders and revenue per state, pulled from BigQuery
2. **Treatment simulation** — a known, documented 15% uplift injected into treated states post-treatment , so the recovered DiD estimate can be checked against the true injected effect
3. **DiD regression** with clustered standard errors
4. **Parallel trends validation** — raw and indexed and smoothed pre-treatment trend plots
5. **Placebo testing** — re-running the regression with a fake, earlier treatment date to check the design is sound
6. **Systematic control-group screening** — scoring all available US states by trend correlation and slope-divergence against the treated group, instead of picking a control group by hand
7. **Event-study plot** — treated-control gap by week, relative to treatment

## Key finding

The parallel trends assumption was tested with two different control groups — an initial hand-picked group (New York, Florida, Illinois) and a second group (Georgia, North Carolina) selected via a systematic trend-matching screen across all US states. **However, both failed the placebo test** (p < 0.001): California and Texas show a pre-existing growth divergence from every available control state, most likely because they are unusually large, fast-growing markets — not because of the simulated campaign.

**The core result of this project is not a clean treatment effect estimate — it's checking a violated causal inference assumption instead of reporting a biased estimate.** In a real-world setting, the next step would be to either construct a separate synthetic control for each treated state by assigning appropriate weights to control states or select a different treated group. Both approaches are beyond the scope of this practice notebook.


## Methods demonstrated

- Difference-in-Differences regression (`treated × post` interaction)
- Clustered standard errors
- Parallel trends validation (raw + indexed/normalized trend comparison)
- Placebo/falsification testing
- Event-study (leads and lags) analysis
- Systematic, data-driven control-group selection (vs. ad-hoc selection)

## Limitations

- Treatment effect is simulated, not real — dataset has no genuine marketing campaign to analyzze
- Parallel trends assumption does not hold for the treated states tested here, so no causal effect estimate is reported
- Small number of treated units (2 states) limits how well any control group can be matched
- Synthetic control (a natural next step for this exact problem) was not implemented in this notebook

## Tools

BigQuery (SQL) · Python (pandas, statsmodels, matplotlib/seaborn)
