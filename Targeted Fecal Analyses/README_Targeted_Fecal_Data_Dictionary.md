# Targeted fecal bile acid, fatty acid, sterol, and dysbiosis data dictionary

## Scope and files

This subset contains targeted fecal measurements supporting Table 5, the targeted multivariate analyses and Supplementary Figure S5, and the dysbiosis-panel correlations in Supplementary Figure S8 of the manuscript. These are targeted measurements, distinct from the untargeted fecal Metabolon dataset. Instrument files, chromatograms, calibration curves, and a complete set of statistical-result exports are not included.

| File | Cats | Columns | Contents and use |
|---|---:|---:|---|
| `FA.csv` | 14 (8 control, 6 diabetic) | 44 | Original-scale values for all 42 Table 5 measurements, plus CatID and group. Use for original-scale summaries and group comparisons. |
| `fa_revised.csv` | 12 (8 control, 4 diabetic) | 44 | Exact subset of FA.csv available for the paired correlations; all 42 variables are retained, including lathosterol. |
| `DI_revised.csv` | 22 (15 control, 7 diabetic) | 12 | DI and nine bacterial taxa, plus CatID and group. Join with fa_revised.csv to obtain the 12-cat Figure S8 cohort. |
| `TARGETEDFECAL_PARETO.csv` | 14 (8 control, 6 diabetic) | 29 | Final 26-analyte natural-log, mean-centered, Pareto-scaled matrix, plus CatID and two group encodings. Reproduces the reported Figure S5 PC1/PC2 variance. |
| `ONE_TRANS_SCALED.csv` | 14 (8 control, 6 diabetic) | 28 | Intermediate 26-analyte matrix with S_-prefixed names. Two columns are only natural-log transformed; see the processing note below before reuse. |
| `Targeted_Fecal_Data_Dictionary.csv` | 157 definitions | 9 | One definition per column per measurement file, including file-specific units and transformations. |
| `README_Deidentification.md` | — | — | Identifier processing and linkage notes. |

Each measurement row is one cat. Counts exclude headers. The files overlap and must not be stacked as independent animals. CSVs use UTF-8, decimal points, a single header row, and numeric CatID sort order. There are no blank or dot-coded missing values in the five deposited measurement files. Original zeros, if present in a measurement, are not a missing-value code; transformations and analytical eligibility must be handled separately.

## Identifiers and study groups

`CatID` is an integer study identifier, unique within each file. IDs 1–29 follow the verified linkage and are consistent with the cytokine and GLP-1 subsets. CatIDs 30 and 31 are two additional targeted-panel cats absent from that linkage. Names and private linkage materials are excluded. CatID numbers carry no biological or ordinal meaning and need not be consecutive within a subset.

`group` uses `C`/`D` in the targeted files and `Control`/`DM` in the DI file. Both mean nondiabetic control/insulin-treated diabetic. `group_num`, present in both processed matrices, uses 0/1 for control/diabetic. These are equivalent encodings, not additional outcomes. ONE_TRANS_SCALED.csv has only the numeric group encoding.

Join by CatID, never by row position. The 12 cats in fa_revised.csv all occur in DI_revised.csv and in the GLP-1 subset's GLP_BA_PAIRED.csv; group assignments agree. The DI file's other ten cats do not have targeted measurements in this subset. Do not equate CatID with the separate serum or fecal metabolomics SampleID numbering. The two influential cats in the Figure S8 sensitivity analyses are CatIDs **11** and **2**.

## Original-scale targeted variables

The following 42 variables occur in both FA.csv and fa_revised.csv. Units follow Table 5; total BA/PBA/SBA units are µg/mg. Concentrations, percentages, and a ratio are all present, so a single unit must not be assigned to the entire panel. The reviewed materials do not establish a wet- versus dry-fecal-mass denominator. Source derivative labels and spellings are retained rather than replaced with inferred instrument annotations.

### Bile acid measurements

| Column | Definition | Units | In 26-analyte matrix |
|---|---|---|---|
| `cholic_acid` | Cholic acid | ng/mg | Yes |
| `chenodeoxycholic_acid` | Chenodeoxycholic acid | ng/mg | Yes |
| `lithocholic_acid` | Lithocholic acid | ng/mg | Yes |
| `deoxycholic_acid` | Deoxycholic acid | ng/mg | Yes |
| `ursodeoxycholic_acid` | Ursodeoxycholic acid | ng/mg | Yes |
| `Total_PBA` | Total primary bile acids | µg/mg | No |
| `Total_SBA` | Total secondary bile acids | µg/mg | No |
| `Total_BA` | Total bile acids | µg/mg | No |
| `Secondary_BA_percent_total` | Secondary bile acids as a percentage of total bile acids | % | No |
| `Primary_BA_percent_total` | Primary bile acids as a percentage of total bile acids | % | No |
| `Cholic_Acid_percent_total` | Cholic acid as a percentage of total bile acids | % | No |
| `Chenodeoxycholic_Acid_percent_to` | Chenodeoxycholic acid as a percentage of total bile acids | % | No |
| `Lithocholic_Acid_percent_total` | Lithocholic acid as a percentage of total bile acids | % | No |
| `Deoxycholic_Acid_percent_total` | Deoxycholic acid as a percentage of total bile acids | % | No |
| `Ursodeoxycholic_Acid_percent_tot` | Ursodeoxycholic acid as a percentage of total bile acids | % | No |

### Fatty acid measurements

| Column | Definition | Units | In 26-analyte matrix |
|---|---|---|---|
| `butyl_myristate` | Myristic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_palmitate` | Palmitic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_oleate` | Oleic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_inoleate` | Linoleic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_alpha_linolenate` | α-Linolenic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_cis_vaccenate` | cis-Vaccenic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_stearate` | Stearic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_arachidonate` | Arachidonic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_gondoate` | Gondoic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_docosanoate` | Docosanoic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_erucate` | Erucic acid (butyl-derivative label) | µg/mg | Yes |
| `butyl_nervonate` | Nervonic acid (butyl-derivative label) | µg/mg | Yes |
| `Total_FA` | Total fatty acids | ng/mg | No |

### Sterol measurements

| Column | Definition | Units | In 26-analyte matrix |
|---|---|---|---|
| `coprostanol_TMS` | Coprostanol (TMS label) | µg/mg | Yes |
| `cholesterol_TMS_vs_chol` | Cholesterol (TMS label) | µg/mg | Yes |
| `cholestanol_TMS` | Cholestanol (TMS label) | µg/mg | Yes |
| `brassicasterol_TMS` | Brassicasterol (TMS label) | µg/mg | Yes |
| `lathosterol_TMS` | Lathosterol (TMS label) | µg/mg | No |
| `campesterol_TMS` | Campesterol (TMS label) | µg/mg | Yes |
| `stigmasterol_TMS` | Stigmasterol (TMS label) | µg/mg | Yes |
| `fusosterol_TMS` | Fusosterol (TMS label) | µg/mg | Yes |
| `beta_sitosterol_TMS` | β-Sitosterol (TMS label) | µg/mg | Yes |
| `sitostanol_TMS` | Sitostanol (TMS label) | µg/mg | Yes |
| `Total_Phytosterols` | Total phytosterols | ng/mg | No |
| `Total_Zoosterols` | Total zoosterols | ng/mg | No |
| `Total_Sterols` | Total sterols | ng/mg | No |
| `Phyto_Zoo_ratio` | Phytosterol-to-zoosterol ratio | unitless | No |

The source spelling `butyl_inoleate` denotes the Table 5 linoleate variable. `cholesterol_TMS_vs_chol` is the cholesterol concentration column, not a deposited cholesterol ratio. `TMS` denotes a trimethylsilyl derivative label. Percentages are stored on a 0–100 scale, not as fractions. `Phyto_Zoo_ratio` is unitless. The precise component formulas for aggregate sterol/fatty-acid totals are not specified in these files; supplied totals and percentages were not recomputed.

Lathosterol (`lathosterol_TMS`) is constant at 0.01 µg/mg across the 14 cats. It remains in the original files and Table 5, but is excluded from the 26-analyte multivariate matrix and the 41-variable Figure S8 correlations.

For CatID 9, Total_PBA = 119 and Total_SBA = 916, whereas Total_BA = 1034 µg/mg; the components sum to 1035. The original one-unit discrepancy is preserved and its cause is not established. The remaining 13 cats have exact numeric agreement between Total_BA and the sum of those two components. Different units apply to individual bile acids and class totals as documented above; no conversion or correction has been imposed.

## Dysbiosis index and bacterial taxa

DI_revised.csv contains ten numeric measurement columns. Taxon abundances are already expressed as **log10 DNA copies/g feces**, as defined in Table 4. They are not percentages and should not be interpreted as raw DNA copy numbers. Negative log10 values, where present, are valid numerical values and are not missing-value indicators. DI is a separate, unitless index. DI_revised.csv contains the 22-cat cross-platform analysis subset (15 controls and 7 diabetic cats), not the complete Table 4 cohort, which includes 25 cats (16 controls and 9 diabetic cats). 

| Column | Definition |
|---|---|
| `DI` | Fecal dysbiosis index. Under the manuscript definition, values below zero indicate no major overall microbiome shift; positive values indicate a shift of increasing magnitude. |
| `Faecalibacterium` | Faecalibacterium abundance. |
| `Turicibacter` | Turicibacter abundance. |
| `Streptococcus` | Streptococcus abundance. |
| `Ecoli` | Escherichia coli abundance. |
| `Blautia` | Blautia abundance. |
| `Fusobacterium` | Fusobacterium abundance. |
| `Clostridium_hiranonis` | Peptacetobacter hiranonis abundance; historical source name retained. |
| `Bifidobacterium` | Bifidobacterium abundance. |
| `Bacteriodes` | Bacteroides abundance; source spelling retained. |

The manuscript describes DI as a nearest-centroid classifier based on total bacterial abundance and seven taxa. Blautia and Fusobacterium were measured separately and are included in the correlation panel but are not components of that DI algorithm. Total bacterial abundance is not a column in this export, so DI cannot be recalculated from these nine taxon columns alone. This 22-cat file supplies the cross-platform subset; it should not automatically be treated as the complete cohort underlying Table 4.

## Processed matrices and transformations

The 26-analyte matrix includes five individual bile acids, 12 fatty acids, and nine sterols, as marked in the tables above. Totals, percentages, the phytosterol/zoosterol ratio, and lathosterol are excluded. A processed column with its original name represents a transformed value, not an original concentration.

The stated preparation rules treat nonpositive concentrations as missing, retain an analyte when observed in at least 50% of cats in at least one group, and replace missing values with one-fifth of its minimum positive value. No sample-wise normalization is applied. **All 26 retained analytes in the deposited original 14-cat dataset are complete and positive**, so no replacement is needed for these particular cells.

For each retained analyte, across all 14 cats:

`L = ln(original numeric concentration)`

`processed = (L − mean(L)) / sqrt(sample_SD(L))`

The sample SD uses denominator n−1. Natural logarithms use the numeric values in the original column's stated unit. No offset is added. Centering/scaling is across the combined cohort, not separately within groups. CatID and group metadata are not transformed. Negative and zero processed values are valid and do not indicate missing measurements. Processed values no longer retain the original concentration units.

Every analyte column in TARGETEDFECAL_PARETO.csv agrees with this formula to a maximum absolute difference of approximately **4.9 × 10⁻¹⁰**. Covariance-based PCA gives **27.53994%** and **23.71531%** variance for PC1 and PC2, matching Figure S5's 27.54% and 23.72%. Correlation-based PCA would change the intended scaling.

### Important distinction: ONE_TRANS_SCALED.csv

Remove the `S_` prefix to match its analyte names to the dictionary above. Twenty-four analyte columns are identical to their corresponding TARGETEDFECAL_PARETO.csv columns. Two contain **uncentered natural logarithms of original concentrations**, without Pareto scaling:

- `S_butyl_alpha_linolenate`
- `S_cholesterol_TMS_vs_chol`

These two columns agree with ln(original concentration) to within 5 × 10⁻¹⁰. Thus, despite its inherited filename, this file is an intermediate matrix with mixed processing. It must not be used unchanged as the final Figure S5 input. It also does not establish the exact final input used for the reported PLS-DA results. The complete processed values are available in TARGETEDFECAL_PARETO.csv. Both supplied exports have been retained without altering their values, and the machine-readable dictionary identifies the transformation of every column separately.

## Which file supports which analysis?

| Analysis | Deposited input and interpretation |
|---|---|
| Table 5 descriptive statistics and group comparisons | FA.csv, 14 cats. Summaries are on the original scale; comparisons use Wilcoxon rank-sum tests, Hodges–Lehmann shifts, and rank-biserial effects. Differences are control minus diabetic. P-values are unadjusted for multiplicity. |
| Targeted PCA and Supplementary Figure S5 | TARGETEDFECAL_PARETO.csv, 14 cats and 26 features. Use covariance PCA. Component signs may reverse without changing variance or geometry. |
| Targeted PERMANOVA | Same final processed matrix. The manuscript uses all 13 nonzero components, 9,999 permutations, and seed 12345 in its associated analysis specification. Reported pseudo-F = 1.486, R² = 0.110, P = 0.176. These permutation results were not rerun during dictionary preparation. |
| Targeted PLS-DA | The manuscript reports a one-component model with Q² = −0.077 and permutation P = 0.244 for R²Y and 0.186 for Q². Exact final PLS input provenance is not established by the intermediate ONE_TRANS_SCALED.csv export; those model results are not independently reproduced here. |
| Figure S8 correlations | Inner-join fa_revised.csv and DI_revised.csv on CatID, producing 12 cats. Correlate the 41 original-scale targeted variables other than lathosterol against DI and nine taxa. Do not use the 26-feature processed matrix for this figure. |
| Figure S8 sensitivity analyses | Same 12-cat joined data. For P. hiranonis versus lithocholic acid and secondary BA percentage, exclude CatID 11, CatID 2, and both; separately compute partial Spearman correlations controlling for diabetes status. |
| GLP-1–chenodeoxycholic acid associations | The existing GLP-1 repository file GLP_BA_PAIRED.csv contains the paired measurements. Its CatIDs correspond to this 12-cat subset. |

Figure S8 contains 410 two-sided Spearman tests: 41 targeted measurements × 10 DI/taxon variables. Use average ranks for ties. Benjamini–Hochberg adjustment is applied within each DI/taxon family of 41 tests and separately across all 410 tests. These are distinct q-values. Asterisks in the figure indicate within-family q < 0.05; no association survives global adjustment. Sample size is 12 for every correlation in the deposited complete data. Partial Spearman sensitivity analyses adjust ranks for diabetic status (control = 0, diabetic = 1).

Independent checks of the deposited paired data reproduce the four reported within-family associations with P. hiranonis:

| Targeted variable | Spearman rho | Two-sided P | Within-family q |
|---|---:|---:|---:|
| Lithocholic acid | 0.86014 | 0.000332 | 0.01037 |
| Secondary bile acids (% total) | 0.80779 | 0.001481 | 0.01518 |
| Cholic acid (% total) | −0.84711 | 0.000506 | 0.01037 |
| Primary bile acids (% total) | −0.80779 | 0.001481 | 0.01518 |

The minimum global q-value is 0.10372, agreeing with the manuscript's 0.104. These reference results are included for verification; the repository files themselves contain measurements, not rho/P/q result columns.

## Provenance, verification, and reuse

Definitions and units were reconciled with the manuscript, Tables 4 and 5, and Supplementary Figure S5/S8 legends. Analysis specifications were checked against the associated targeted and correlation analysis materials. All 157 measurement-file columns have a file-specific entry in Targeted_Fecal_Data_Dictionary.csv.

The revised 12-cat targeted file is an exact subset of FA.csv, including every measurement and group value. The two processed matrices contain the same 14 CatIDs as FA.csv. Unique identifiers, group correspondence, missingness, source-value preservation, and the paired cohort were checked. Two completely empty source rows were removed during de-identification; no cat records were discarded. The original source files and private identity keys are not part of this subset.

No measurements were changed to create this dictionary. The intermediate-matrix difference and the one-unit total discrepancy are explicitly documented rather than silently corrected. 
