# regional-marketing-campaign

# Difference-in-Differences: Simulated Regional Marketing Campaign

**SQL (BigQuery) + Python analysis practicing the Difference-in-Differences (DiD) causal inference method** on the public `thelook_ecommerce` dataset. This project practices the a DiD workflow. Specifically, treatment simulation, parallel trends validation, regression as well as finally placebo testing.

**Business question:** Did a regional marketing campaign in California and Texas increase weekly orders and revenue, compared to states that did not receive campaign?

**Why was the treatment  simulated?** `thelook_ecommerce` is an open source and synthetic dataset with no "real" marketing campaigns or policy shocks to exploit as a "natural experiment". Therefore, a treatment effect (a 15% revenue uplift) was simulated and injected into the data. This is a common approach to practice causal inference.

## What's in this notebook

1. **SQL panel construction**: weekly orders and revenue per state, pulled from Google BigQuery
2. **Treatment simulation**: a 15% uplift was injected into treated states post-treatment. Therefore, a DiD estimate can be compared with the true injected effect
3. **DiD regression**: with clustered standard errors.
4. **Parallel trends validation**: pre-treatment trend plots
5. **Placebo testing** : A regression was ran with a hypothecical, earlier treatment date to check the design is sound
6. **Systematic control-group screening**: all available US states by trend correlation were scored and slope-divergence against the treated group, instead of  picking a control group by hand
7. **Event-study plot**: treated-control gap by week, relative to treatment

## Key finding

The parallel trends assumption was tested with two different control groups. Initially, a group (New York, Florida, Illinois) was  hand-picked. Next, a second group (Georgia, North Carolina ) were selected via a trend-matching screen across all US states. Both constellations failed the placebo test (p < 0.001). California and Texas showed a pre-existing growth divergence from every available control state. This could be explained because the states are larger than the others or have fast-growing markets.

## Result
The core result of this project is not a treatment effect estimate. However, it's checking a violated causal inference assumption, to not report a biased estimate. In a real-world setting, the next step could be to either construct a separate synthetic control for each treated state. This could be performed by e.g. assigning weights to control states or select a different treated group. Both approaches are beyond the scope of this practice.


## Methods demonstrated

- Difference-in-Differences regression (`treated × post` interaction)
- Clustered standard errors
- Parallel trends validation (raw + indexed/normalised trend comparison)
- Placebo testing
- Event-study (leads and lags) analysis
- Systematic, data-driven control-group selection (vs. ad-hoc selection)

## Limitations

- The treatment effect is simulated as the dataset has no marketing campaign to analyse
- Parallel trends assumption does not hold for the treated states tested here, so no causal effect estimate is reported
- Small number of treated units (2 states) limits how well any control group can be matched
- Synthetic control (a natural next step for this exact problem) was not implemented in this notebook

## Tools used

BigQuery (SQL) · Python (pandas, statsmodels, matplotlib/seaborn)
