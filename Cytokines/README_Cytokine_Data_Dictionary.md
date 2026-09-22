# Feline diabetes cytokine repository

## Scope

These four CSV files contain serum cytokine and chemokine concentrations from 29 cats: 21 nondiabetic controls and 8 insulin-treated diabetic cats. Nineteen analytes were measured using a multiplex feline cytokine/chemokine immunoassay. Concentrations are in **pg/mL**. The original concentration file contains assay-derived concentrations, not raw fluorescence measurements or instrument calibration files.

The files support the cytokine descriptive statistics and between-group comparisons in Table 3, including multiple imputation and sensitivity analyses for SDF-1.

## Files and row structure

| File | Data rows | Columns | One row represents | Purpose |
|---|---:|---:|---|---|
| `Cytokine20260622.csv` | 29 | 21 | One cat | Original concentrations before detection-limit substitution or SDF-1 imputation. |
| `Cytokine_imputed_SAS_PMM.csv` | 29 | 44 | One cat | Completed dataset for descriptive summaries and non-SDF-1 group comparisons. The two originally missing SDF-1 values are averages across 50 full-predictor PMM imputations. |
| `STEP2_MERGED_PMM.csv` | 1,450 | 46 | One cat in one of 50 imputations | Completed datasets for the full-predictor SDF-1 analysis, retaining individual imputation draws and original values. |
| `SDF1_group_only_PMM.csv` | 1,450 | 6 | One cat in one of 50 imputations | Completed SDF-1 datasets for the group-only PMM sensitivity analysis. |

Counts exclude the header. The 1,450-row files each contain the same 29 cats repeated across 50 imputations, not 1,450 independent animals. Each imputation contains 21 controls and 8 diabetic cats.

## Identifiers, groups, and missing values

| Column | Type | Definition |
|---|---|---|
| `CatID` | Integer identifier | Unique study identifier, 1–29. The same cat has the same ID in all four files. Numbers have no biological or ordinal meaning. Names have been removed. |
| `Group` | Text | `C` = nondiabetic control; `D` = diabetic. Present in the first three files. |
| `GroupCode` | Text | Same coding as `Group`; used in `SDF1_group_only_PMM.csv`. |
| `_Imputation_` | Integer | Completed-dataset number, 1–50. Present in the two 1,450-row files. |
| `Analysis` | Text | Constant value `Group-only PMM` in `SDF1_group_only_PMM.csv`. |

Blank numeric fields denote missing measurements, not zero concentrations. All completed analyte columns have no missing values; blanks remain in columns preserving original measurements.

Use `CatID` to join the two one-row-per-cat files. Within an imputation file, the unique row key is `CatID` plus `_Imputation_`. Imputation numbers are local to each imputation model; matching numbers across the two PMM models do not identify paired donor draws. Do not join the two long files by CatID alone, which would multiply records.

## Analyte columns

All columns below contain numeric concentrations in pg/mL on the original measurement scale. No exported analyte column contains log-transformed concentrations.

| Column | Analyte |
|---|---|
| `FAS` | Fas (CD95) |
| `Flt3L` | FMS-like tyrosine kinase 3 ligand |
| `GM_CSF` | Granulocyte-macrophage colony-stimulating factor |
| `IFNg` | Interferon gamma |
| `IL1b` | Interleukin-1 beta |
| `IL2` | Interleukin-2 |
| `PGDF_BB` | Platelet-derived growth factor BB (PDGF-BB); the original code's spelling `PGDF_BB` is retained. |
| `IL12p40` | Interleukin-12 p40 |
| `IL13` | Interleukin-13 |
| `IL4` | Interleukin-4 |
| `IL6` | Interleukin-6 |
| `IL8` | Interleukin-8 |
| `KC` | Keratinocyte chemoattractant |
| `SDF1` | Stromal cell-derived factor 1 |
| `RANTES` | Regulated upon activation, normal T-cell expressed and secreted |
| `SCF` | Stem cell factor |
| `MCP1` | Monocyte chemoattractant protein 1 |
| `TNFa` | Tumor necrosis factor alpha |
| `IL18` | Interleukin-18 |

In `Cytokine20260622.csv`, these are the original measured concentrations, with blanks where measurements were missing. In the two completed full-panel files, concentrations reflect the processing described below. The group-only file contains SDF-1 only, not the other 18 analytes.

## Original-value columns and flags

For each of these 11 analytes, both completed full-panel files contain `<analyte>_original` and `<analyte>_censored_flag`:

`FAS`, `GM_CSF`, `IFNg`, `IL1b`, `IL2`, `PGDF_BB`, `IL4`, `IL6`, `KC`, `TNFa`, and `IL18`.

| Column pattern | Definition |
|---|---|
| `<analyte>_original` | Concentration before substitution, in pg/mL. Blank if the original measurement was missing. Retained for every cat, including cats whose value was not replaced. |
| `<analyte>_censored_flag` | `1` = original value was missing or below the specified LoD and was replaced with LoD/√2; `0` = no replacement. A flag of 1 does not distinguish a missing measurement from an observed sub-LoD concentration; inspect the original-value column for that distinction. |
| `SDF1_censored_flag` | In the completed full-panel files: `1` = SDF-1 was originally missing and was multiply imputed; `0` = observed SDF-1 retained. Despite the inherited column name, this flag does not indicate LoD censoring or LoD/√2 substitution for SDF-1. |
| `SDF1_original` | In `STEP2_MERGED_PMM.csv` only: SDF-1 before imputation, with blanks for the two originally missing measurements. |
| `SDF1_Observed` | In `SDF1_group_only_PMM.csv` only: SDF-1 before imputation, with blanks for the two originally missing measurements. |

`Cytokine_imputed_SAS_PMM.csv` does not include `SDF1_original`. Obtain original SDF-1 measurements from `Cytokine20260622.csv` by CatID. Other analytes without original-value/flag columns were not modified.

## Detection-limit substitutions

For the 11 analytes below, values that were missing or strictly below the specified limit of detection (LoD) were replaced with LoD/√2. Values equal to the LoD were not replaced. This deterministic substitution was performed before SDF-1 multiple imputation and is identical across all completed datasets.

| Analyte | LoD (pg/mL) | Cats with substituted values, out of 29 |
|---|---:|---:|
| FAS | 2.027 | 15 |
| GM_CSF | 2.092 | 14 |
| IFNg | 4.287 | 1 |
| IL1b | 6.881 | 14 |
| IL2 | 2.851 | 8 |
| PGDF_BB | 47.760 | 5 |
| IL4 | 10.099 | 4 |
| IL6 | 10.605 | 8 |
| KC | 0.389 | 15 |
| TNFa | 44.033 | 25 |
| IL18 | 20.677 | 8 |

These counts include both missing and observed sub-LoD values; they are not missing-value counts. FAS, KC, and TNFa were excluded from the reported between-group cytokine comparisons because of high substitution rates (51.7%, 51.7%, and 86.2%, respectively). They remain in the repository for transparency and were included among the predictors in the full-predictor SDF-1 imputation model.

## SDF-1 multiple imputation

Only SDF-1 underwent stochastic multiple imputation. Two of 29 original measurements were missing: one control and one diabetic cat. The other 27 SDF-1 measurements were preserved. Missingness was treated as missing at random for imputation; this is an assumption rather than an established property of the data.

Predictive mean matching (PMM) was implemented using fully conditional specification in SAS PROC MI (SAS 9.4), with 50 imputations, 20 burn-in iterations, seed 42, and five candidate donors (`K=5`). Modeling used natural-log-transformed concentrations, ln(x+1); imputed SDF-1 values were back-transformed with exp(x)−1.

- **Full-predictor PMM:** predictors were the other 18 ln(x+1)-transformed cytokine concentrations and group membership. Individual completed datasets are in `STEP2_MERGED_PMM.csv`.
- **Group-only PMM:** group membership was the sole predictor. Individual completed datasets are in `SDF1_group_only_PMM.csv`. Including group as a predictor did not explicitly restrict donors to the same group.

For each of the two cats with missing SDF-1, `Cytokine_imputed_SAS_PMM.csv` contains the arithmetic average of its 50 back-transformed full-predictor PMM values. These averaged values need not themselves equal any single observed donor concentration.

## Which file supports which analysis?

| Analysis or summary | Input |
|---|---|
| Original missingness and measured concentrations | `Cytokine20260622.csv` |
| Table 3 medians and quartiles | `Cytokine_imputed_SAS_PMM.csv` |
| Wilcoxon comparisons, Hodges–Lehmann shifts, and rank-biserial effects for the 15 retained non-SDF-1 analytes | `Cytokine_imputed_SAS_PMM.csv` |
| Full-predictor pooled SDF-1 mean difference, confidence interval, and P-value | `STEP2_MERGED_PMM.csv` |
| Group-only pooled SDF-1 sensitivity analysis | `SDF1_group_only_PMM.csv` |
| Observed-only SDF-1 sensitivity analysis | The 27 nonmissing SDF-1 values in `Cytokine20260622.csv` (20 controls, 7 diabetic cats) |

For SDF-1 inference, each completed dataset was analyzed separately on the original pg/mL scale. The arithmetic mean difference (control minus diabetic) and its unequal-variance standard error were combined using Rubin's rules in PROC MIANALYZE. A small-sample adjustment used the smallest Welch–Satterthwaite degrees of freedom across imputations as the complete-data degrees of freedom. This is an approximate procedure, not an exact test. The observed-only comparison used Welch's t-test.

The pooled SDF-1 effect is a mean difference, not a Hodges–Lehmann shift. Negative differences indicate higher estimated concentrations in diabetic cats. An ordinary test on the single averaged dataset does not incorporate between-imputation uncertainty and does not reproduce the reported pooled SDF-1 inference. Likewise, treating all 1,450 rows as independent observations is inappropriate.

## Reference results for reproducibility

All differences below are control minus diabetic, in pg/mL; P-values are two-sided and unadjusted for multiplicity.

| SDF-1 analysis | Mean difference | Lower 95% CI | Upper 95% CI | P-value |
|---|---:|---:|---:|---:|
| Full-predictor PMM | −457.4 | −972.3 | 57.5 | 0.0738 |
| Group-only PMM | −503.3 | −991.6 | −14.9 | 0.0449 |
| Observed only | −537.2 | −1038.2 | −36.3 | 0.0385 |

These results are from the final supplied SAS output. Statistical significance of the SDF-1 comparison depended on missing-value handling, despite a consistent direction of the estimated difference. 

## Reuse and identity protection

CatIDs are consistent across these four files; correspondence to identifiers in other repository subsets has not been established here. No name-to-ID key is included in this subset. Preserve CatID as an identifier when importing or joining files. 


