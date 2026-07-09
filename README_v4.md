# TMEM and Breast Cancer Metastasis Analysis

This repository contains the R Markdown code used to reproduce the statistical analyses, summary tables, and Kaplan-Meier curves for the TMEM doorway, MenaCalc, and MenaINV breast cancer metastasis study.

Primary analysis file:

`TMEM_and_BrCA_Metastasis_analysis.Rmd`

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

Prognostic performance is also evaluated using Uno's C-index for the clinical model and TMEM/Mena biomarker combinations, with results saved to `results/uno_cindex_results.xlsx`.

Treatment-arm interaction analyses estimate adjusted hazard ratios for DDFS per one-tertile increase in each biomarker within the AC and AC + PTX treatment arms. Interaction p-values are calculated using likelihood-ratio tests comparing Cox models with and without the biomarker-by-treatment interaction term. These results are reported as Supplementary Table S6.

## Generated Outputs

Running `TMEM_and_BrCA_Metastasis_analysis.Rmd` writes Word, Excel, and PDF outputs to `results/`.

### Word table outputs

- `1. Main table 1 Supplementary Table S2 S3.docx`
  - Main Table 1: baseline characteristics by TMEM doorway score tertiles.
  - Supplementary Table S2: baseline characteristics by MenaCalc score tertiles.
  - Supplementary Table S3: baseline characteristics by MenaINV score tertiles.

- `main_analysis_tables.docx`
  - Main Table 1: baseline characteristics by TMEM doorway score tertiles.
  - Table 2 / Supplementary Table S4: unadjusted and adjusted Cox models among all patients for TMEM doorway score, MenaCalc score, and MenaINV score, evaluated as tertiles and as continuous scores.
  - Tables 4 and 5: unadjusted and adjusted Cox models in the ER+/HER2- subgroup for the same biomarkers.
  - Event-count tables are included for tertile marker models.

- `table3_supplementary_tables5p_trend_tables.docx`
  - Numeric-marker p-trend Cox models for mutually adjusted marker combinations.
  - Results are shown for all patients and for the ER+/HER2- subgroup.

- `supplementary_analysis_tables.docx`
  - Supplementary Table S2 and Supplementary Table S3 baseline tables.
  - Spearman correlations among TMEM doorway score, MenaCalc score, and MenaINV score in all patients and in the ER+/HER2- subgroup.
  - Table 3 / Supplementary Table S5: mutually adjusted Cox models using tertile marker combinations, shown for all patients and for the ER+/HER2- subgroup.
  - Supplementary Table S6: adjusted HRs per tertile increase by treatment arm, with marker-by-treatment interaction p-values for all-year and 5-year DDFS.

- `table4_erpos_her2neg_adjusted_for_recurrence.docx`
  - Sensitivity analysis for manuscript Table 4 in the ER+/HER2- subgroup.
  - Models are additionally adjusted for recurrence score.
  - Missing recurrence scores are replaced with the full-cohort median.

### Excel output

- `uno_cindex_results.xlsx`
  - Uno's C-index model performance results for clinical models and TMEM/Mena biomarker combinations.
  - Includes separate worksheets for all patients, ER+/HER2- patients, and numeric results.

### Kaplan-Meier plots

Each PDF contains DDFS and DFS Kaplan-Meier curves with all-year and 5-year truncated follow-up panels.

- `km_curves/all_patients_tmem_tertile.pdf`: all patients, TMEM doorway score tertiles.
- `km_curves/all_patients_menacalc_tertile.pdf`: all patients, MenaCalc score tertiles.
- `km_curves/all_patients_menainv_tertile.pdf`: all patients, MenaINV score tertiles.
- `km_curves/er_her2_tmem_tertile.pdf`: ER+/HER2- subgroup, TMEM doorway score tertiles.
- `km_curves/er_her2_menacalc_tertile.pdf`: ER+/HER2- subgroup, MenaCalc score tertiles.
- `km_curves/er_her2_menainv_tertile.pdf`: ER+/HER2- subgroup, MenaINV score tertiles.

## Note

Word table outputs are best viewed in landscape orientation for readability.

For baseline/distribution tables, p-values for categorical variables were calculated using chi-square tests; Monte Carlo simulation was applied to Race.
