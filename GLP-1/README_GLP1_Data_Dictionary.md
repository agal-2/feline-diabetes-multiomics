# Feline diabetes GLP-1 data and data dictionary

## Scope

This subset contains serum total glucagon-like peptide-1 (GLP-1) concentrations for 28 cats: 21 nondiabetic controls and 7 insulin-treated diabetic cats. A second file contains paired serum GLP-1 and targeted fecal bile-acid measurements from 12 of those cats: 8 controls and 4 diabetic cats. These files support the GLP-1 group comparison and exploratory GLP-1–chenodeoxycholic acid associations reported in the main manuscript.

The wider study enrolled 31 cats. The 28 cats with GLP-1 measurements, 29 cats in the serum metabolomics/cytokine subsets, and 14 cats in the full targeted fecal panel are different analysis populations. The 12-row paired file is a subset of the GLP-1 file, not an additional independent cohort. “Paired” means that serum and fecal measurements belong to the same cat; it does not indicate repeated visits, before/after treatment, or matched control–diabetic pairs.

## Files and row structure

| File | Data rows | Columns | One row represents | Purpose |
|---|---:|---:|---|---|
| `GLP1.csv` | 28 | 4 | One cat with a serum total GLP-1 measurement | Original-scale concentrations and group comparison. |
| `GLP_BA_PAIRED.csv` | 12 | 10 | One cat with both serum GLP-1 and targeted fecal bile-acid measurements | Unadjusted and diabetes-adjusted correlations and the supplied log-linear sensitivity analysis. |
| `GLP1_Data_Dictionary.csv` | 14 | 7 | One column definition in one measurement file | Machine-readable companion to this dictionary. |
| `README_GLP1_Data_Dictionary.md` | — | — | Documentation | Measurement definitions, processing, analysis context, and reuse notes. |

CSV files use UTF-8 encoding, comma delimiters, decimal points, and one header row. Counts exclude the header. Both measurement files are sorted by numeric `CatID`. No missing fields remain in either deposited measurement file. No missing-animal rows were invented and no concentrations were imputed. If missing numeric values are introduced in future versions, use empty fields rather than zero.

## Identifiers and joining files

`CatID` is the verified study identifier used in the cytokine repository. The GLP-1 file contains 28 of the 29 CatIDs in the verified linkage; CatID 3 has no GLP-1 record in the supplied source and is absent here. Numbers have no biological or ordinal meaning and need not be consecutive in a subset.

Join these two files on `CatID`; the identifier is unique within each file. `Total_GLP1` in the paired file equals `Concentration_pM` in the main file for every matched cat. `group`, `GroupFA`, and `GroupDM` encode the same study membership. When joining to a long cytokine imputation file, retain `_Imputation_`: repeated CatIDs there represent imputation draws, not additional animals.

**Do not equate CatID with serum or fecal metabolomics SampleID.** Those sample identifiers use separate numbering systems. Cross-platform matching to those matrices requires a verified crosswalk; equal numeric values do not establish identity. No name, medical-record, barcode, or private linkage file is included in this GLP-1 subset.

## GLP1.csv dictionary

| Column | Type | Units or coding | Definition |
|---|---|---|---|
| `CatID` | Integer identifier | Verified study CatID | Unique cat identifier consistent with the cytokine CatID. |
| `Age` | Text | Years and months | Original age notation, such as `8y` or `9y10m`. Omitted months mean zero. Convert to months as 12 × years + months.  |
| `Concentration_pM` | Numeric | pM (pmol/L) | Serum total GLP-1 concentration on the original assay scale. This is not a measurement of active GLP-1 alone. |
| `group` | Text | `C` = nondiabetic control; `D` = insulin-treated diabetic | Study group. |

The original `Cat` and `MR` fields were removed and replaced by `CatID`. All ages, concentration values, and group assignments were retained exactly as supplied. Age is descriptive metadata; the supplied GLP-1 group comparison does not adjust for age.

## GLP_BA_PAIRED.csv dictionary

| Column | Type | Units or coding | Definition |
|---|---|---|---|
| `CatID` | Integer identifier | Verified study CatID | Same identifier as in `GLP1.csv`. |
| `chenodeoxycholic_acid` | Numeric | ng/mg | Targeted fecal chenodeoxycholic acid concentration, not percentage of total bile acids or untargeted relative abundance. |
| `Total_PBA` | Numeric | µg/mg | Total primary fecal bile-acid concentration as supplied. |
| `Total_SBA` | Numeric | µg/mg | Total secondary fecal bile-acid concentration as supplied. |
| `Total_BA` | Numeric | µg/mg | Total fecal bile-acid concentration as supplied. |
| `GroupFA` | Text | `C` = control; `D` = diabetic | Study group inherited from the targeted fecal data. |
| `Total_GLP1` | Numeric | pM (pmol/L) | Serum total GLP-1; identical to `Concentration_pM` for the corresponding CatID. |
| `GroupDM` | Integer | `0` = control; `1` = diabetic | Diabetes-status covariate used for partial correlation. |
| `Log_GLP1` | Numeric | Natural-log scale | `ln(Total_GLP1)`, using the numeric concentration expressed in pM. |
| `Log_CDCA` | Numeric | Natural-log scale | `ln(chenodeoxycholic_acid)`, using the numeric concentration expressed in ng/mg. |

The source `FACat`, `SampleKey`, and `GLPCat` fields contained names or name-derived identifiers. All three were removed and replaced by one `CatID`. Every remaining source value, including the stored precision of the logarithms, was retained.

The units for `Total_PBA`, `Total_SBA`, and `Total_BA` are **µg/mg**, as labeled in Table 5. The individual chenodeoxycholic acid measurement is in **ng/mg**. No unit conversion was performed. The reviewed materials do not specify whether the fecal mass denominator is wet or dry mass, and the paired CSV does not contain the individual bile acids needed to reconstruct each class total. These totals should therefore be treated as supplied measurements, not recomputed from this subset.

For **CatID 9**, `Total_PBA = 119`, `Total_SBA = 916`, and `Total_BA = 1034` µg/mg. The components sum to 1035, one unit above the supplied total. The source values are preserved; the cause is not established. The other 11 rows satisfy `Total_BA = Total_PBA + Total_SBA` exactly. This discrepancy does not affect the supplied GLP-1–chenodeoxycholic acid analyses, which do not use these total columns.

## Measurement and processing

According to the manuscript's “Serum total GLP-1” Methods section, total GLP-1 was measured in 50 µL of thawed frozen serum using the Multi-Species GLP-1 Total ELISA kit (Millipore). The manuscript reports an assay range of 4.1–1000 pM and a detection limit of 1.5 pM, with absorbance read at 450 and 590 nm using a SpectraMax ID3 reader. Deposited concentrations range from 6.735 to 120.424 pM, all within the reported range. The CSV contains assay-derived concentrations, not raw absorbance, replicate wells, or calibration curves.
These GLP files were not Pareto-scaled, mean-centered, or subjected to the imputation used in the separate targeted fecal PCA pipeline. The paired file retains original-scale measurements and two explicitly labeled log columns.

## Analysis context and reference results

| Analysis | Input and method | Reference result |
|---|---|---|
| GLP-1 descriptive summaries | `GLP1.csv`; original-scale median [Q1, Q3] | Controls: 22.3 [10.4, 34.4] pM, n=21; diabetic: 57.7 [28.6, 79.1] pM, n=7. |
| Between-group GLP-1 comparison | Independent-samples pooled t-test on ln(`Concentration_pM`), as reported in the manuscript | Control/diabetic geometric mean ratio 0.39 (95% CI 0.20–0.78); Hedges' g = −1.181; P = 0.01. |
| Unadjusted GLP-1–CDCA association | `GLP_BA_PAIRED.csv`; two-sided Spearman correlation of `Total_GLP1` with `chenodeoxycholic_acid` | n=12; ρ = 0.30; P = 0.342. |
| Diabetes-adjusted GLP-1–CDCA association | Same variables; partial Spearman correlation controlling for `GroupDM` | n=12; partial ρ = 0.19; P = 0.579. |
| Log-linear sensitivity analysis | `Log_GLP1 = GroupFA + Log_CDCA`, with control as reference group, in SAS PROC GLM | No estimate from this model is reported in the manuscript text. |

`GLP1.csv` contains concentrations on the original pM scale. The reported between-group comparison uses a pooled t-test on natural-log-transformed concentrations. The geometric mean ratio is expressed as control/diabetic, and Hedges' g describes the standardized difference on the log scale. A ratio below 1 and a negative g indicate lower GLP-1 concentrations in controls. Expressed as diabetic/control, the same comparison gives a geometric mean ratio of approximately 2.56 and a positive g.

The two correlation analyses use the original concentration columns. The separate log-linear model uses the stored log columns. No age covariate or multiple-testing adjustment is specified for these GLP analyses. The additional bile-acid totals are retained for transparency and reuse but were not included in these correlation or regression models.

The full targeted fecal panel used in Table 5 has 14 cats (8 controls, 6 diabetic cats). Its group comparisons cannot be reproduced from this 12-cat paired subset alone. Likewise, this subset does not contain the full targeted panel needed for its PCA or other fecal analyses.

## Preparation and verification

All 28 GLP-1 records and all 12 paired records mapped uniquely to verified CatIDs. Group and age checks passed; all paired GLP-1 values exactly match the main file. No animal records were dropped during de-identification. All retained source fields were checked for exact value preservation, and all measurement-file fields have entries in the machine-readable dictionary. Stored logs agree with recomputed natural logarithms to less than 5 × 10⁻¹¹.

Independent numerical checks reproduce the GLP-1 medians and quartiles, control/diabetic geometric mean ratio (0.3912), pooled t-test P-value (0.00982), Hedges' g (−1.1807), unadjusted Spearman result (ρ=0.3007, P=0.3423), and partial Spearman result (ρ=0.1884, P=0.5791). These checks support consistency with the reported results; they are not a rerun of the SAS code. The reference confidence interval above is transcribed from the manuscript.
