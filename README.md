# TMEM and Breast Cancer Metastasis Analysis

This repository contains the R Markdown code used to reproduce the statistical analyses, summary tables, and Kaplan-Meier curves for the TMEM doorway, MenaCalc, and MenaINV breast cancer metastasis study.

Primary analysis file:

`TMEM and BrCA Metastasis_analysis.Rmd`

## Purpose

The analysis evaluates associations of three biomarkers with breast cancer outcomes in NSABP B-28:

- TMEM doorway score (`tmem_score`)
- MenaCalc score (`mena_calc_score`)
- MenaINV score (`mena_inv_score`)

Primary survival outcomes are distant disease-free survival (DDFS) and disease-free survival (DFS), evaluated over the full follow-up period and with 5-year truncation. Analyses are performed in all eligible patients and in the ER+/HER2- subgroup.

## Outcome Definitions

Overall survival (OS) is defined as the time from curative surgery to death from any cause. The OS variables are `tos` (time) and `os` (event).

Disease-free survival (DFS) events include local, regional, or distant recurrence; contralateral breast cancer; non-breast second primary cancers; and death before recurrence. Patients are censored at the end of follow-up or at loss to follow-up. The DFS variables are `tdfs` (time) and `dfs` (event).

Distant disease-free survival (DDFS) focuses on distant metastasis. Events other than distant recurrence are treated as censoring events. The DDFS variables are `tddfs` (time) and `ddfs` (event).

Variables ending in `_5ys` indicate outcomes administratively truncated at 5 years. For these variables, follow-up time is capped at 60 months; events occurring after 60 months are treated as censored at 60 months. The 5-year variables are `tos_5ys`/`os_5ys`, `tdfs_5ys`/`dfs_5ys`, and `tddfs_5ys`/`ddfs_5ys`.

## Main Input File

The R Markdown analysis reads:

- `data/NSABP Master File.xlsx`
  - `Chart Review`
  - `Master with Scores`
  - `97 HER2 Restains`

If the source data are stored outside this folder, update `data_dir` in the setup chunk of the R Markdown file before running the analysis.

## Cohort Definition

The analytic cohort is restricted to patients who were:

- eligible,
- had tumor block received,
- had TMEM staining successfully completed.

This produces an analytic cohort of `N = 814`.

## Biomarker Definitions

Biomarkers are categorized into tertiles using cutoffs derived from all eligible patients:

- TMEM doorway score: `1.169908`, `10.408976`
- MenaCalc score: `-0.11642614`, `0.07681679`
- MenaINV score: `13.82770`, `18.16060`

Rounded cutoffs are 1.2/10.4, -0.12/0.08, and 13.8/18.2.

## Models

Adjusted Cox models include:

- age,
- tumor grade,
- positive lymph nodes,
- estrogen receptor status,
- tumor size,
- surgery type,
- HER2 status,
- treatment arm.

ER+/HER2- subgroup models omit ER and HER2 because the subgroup is defined by those variables.

Two analysis datasets are used. The original dataset (`df`) is retained for descriptive tables and unmodified summaries. The modeling dataset (`df2`) is used for Cox regression models; in `df2`, missing tumor size is imputed with the cohort median, and missing tumor grade and HER2 status are retained as explicit `Unknown` categories so that patients with missing covariate values are not excluded from adjusted models.

The ER+/HER2- subgroup is defined in both datasets: `df_sub` is used for subgroup descriptive summaries, and `df2_sub` is used for subgroup adjusted Cox models.

## Note

Word table outputs are best viewed in landscape orientation for readability.

For distribution tables, p-values for categorical variables were calculated using chi-square tests with Monte Carlo simulation when asymptotic assumptions were not met.
