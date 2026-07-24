# A/B Test Case Study: Ad Platform CTR

End-to-end statistical analysis of an advertising-platform A/B experiment: split validation, sensitivity methods, bootstrap inference, and segment-level interpretation via an EM mixture model.

Course homework (ПСМО), rewritten here as a **portfolio case study** — the notebook still contains the original assignment wording.

## Problem

Given user-level experiment data (`id`, three candidate splits, demographics/interests proxies, `views`, `clicks`, `CTR`):

1. Choose a valid randomization split (homogeneity of pre-experiment covariates).
2. Test whether treatment changes CTR / clicks / views.
3. Strengthen inference (post-stratification, delta method, bootstrap variants).
4. Interpret heterogeneous effects using an EM clustering of users by `views`.

Data source used in the notebook:  
`https://raw.githubusercontent.com/ZolotarevStat/psmo_22_23/2023/df_hw3.csv`

## Methods

| Block | Techniques |
|-------|------------|
| Split QA | Mann–Whitney, Ansari–Bradley, Spearman / Kendall independence checks |
| Primary tests | Two-sample *t*-tests on CTR, clicks, views |
| Sensitivity | Post-stratification by `is_teenager`, delta method for ratio metrics |
| Bootstrap | Percentile, *t*-bootstrap, BCa (90% CIs for mean uplift) |
| Segmentation | EM for Gaussian mixture on `views` (3 components) |

## Key findings

- **`split_2`** is the only candidate that passes homogeneity checks at α = 0.1 (shift / scale / independence).
- Treatment increases **clicks** and **CTR**; **views** show no clear effect.
- Post-stratification and bootstrap CIs agree with the parametric tests.
- EM segments matter: uplift is clearer in clusters 2–3; cluster 1 is noisier / riskier — suggests segment-aware rollout rather than a blanket ship.

## Repo layout

```
HW3_PSMO_AB.ipynb   # full analysis with plots, tests, and business conclusions
```

## How to run

```bash
pip install pandas numpy scipy statsmodels plotly matplotlib tqdm
jupyter notebook HW3_PSMO_AB.ipynb
```

## Notes

- CUPED bonus is discussed but not applicable (no pre-period metric in the dataset).
- This is educational experiment data, not a live production experiment.
