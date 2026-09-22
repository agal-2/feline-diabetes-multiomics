# Fecal metabolomics data and data dictionary

## Overview

These files contain individual-cat untargeted fecal metabolomics data for 28 cats: 21 nondiabetic controls and 7 diabetic cats. They include log-transformed relative abundances, the corresponding Pareto-scaled matrix, and a metabolite annotation key. They do not contain raw mass-spectrometry instrument files or absolute metabolite concentrations.

## Files

| File | Contents |
|---|---|
| `FECESWIDELN20260720.csv` | 28 sample rows, 1,075 metabolite columns, and 11 sample/clinical metadata columns. Metabolite values are natural-log-transformed relative abundances, before Pareto scaling. |
| `Feces_Pareto_Scaled_Analysis.csv` | The same 28 samples and metadata, with 1,062 mean-centered and Pareto-scaled metabolite columns after removal of 13 constant features. |
| `FECESMETABOLITEKEY.csv` | 1,075 unique metabolite entries and 15 annotation columns. This key covers every feature in both measurement files. |

CSV files have a header row. Each measurement-file row represents one cat's fecal sample. Each key-file row represents one metabolite feature. Match sample records using `SampleID` and metabolite columns to the key using `FeatureID`; do not rely on row order.

## Sample and clinical metadata

| Column | Definition and coding |
|---|---|
| `SampleID` | Analysis sample identifier, 1–28. The same identifier links the two measurement files. Cross-platform correspondence requires a verified sample mapping. |
| `Group` | `Control` = nondiabetic control; `Diabetes` = diabetic cat. |
| `GroupNum` | Numeric group code: 0 = Control; 1 = Diabetes. |
| `age_m` | Age in months. |
| `BW_kg` | Body weight in kilograms. |
| `BCS` | Body condition score (1–9 scale). |
| `Fructosamine` | Serum fructosamine (µmole/L) measurement. |
| `Glucose` | Blood/serum glucose (mg/dL) measurement. |
| `Cholesterol` | Serum cholesterol (mg/dL) measurement. |
| `Triglycerides` | Serum triglyceride measurement (mg/dL). |
| `Total_T4` | Total thyroxine measurement (nmol/L). |

Clinical metadata are retained for sample description and potential covariate analyses; their presence does not imply that all were included in a statistical model. Missing clinical values must not be interpreted as zero. Blank fields or a SAS missing-value marker (`.`) should be read as missing.

## Metabolite measurements and identifiers

Columns whose names consist of `F` followed by digits contain metabolite measurements, for example `F30`. The numeric portion corresponds to `CHEM_ID` in the Metabolon source workbook. During preparation of the key, `CHEM_ID` was renamed to `FeatureID` and the prefix `F` was added. The prefix does not change the chemical identity.

The unscaled file contains natural logarithms of processed relative abundances. Negative values can result from log transformation and are not missing-value indicators. The scaled file can also contain negative and zero values because of centering. Neither matrix reports absolute concentrations.

## Metabolite key data dictionary

| Column | Description |
|---|---|
| `FeatureID` | Unique identifier matching a metabolite column in the measurement files: `F` plus the source `CHEM_ID`. |
| `LIB_ID` | Metabolon-supplied internal library identifier, preserved from the source metadata. |
| `COMP_ID` | Metabolon-supplied compound identifier. |
| `SUPER_PATHWAY` | Broad metabolic category assigned by Metabolon. |
| `SUB_PATHWAY` | More specific metabolic pathway assigned by Metabolon. |
| `TYPE` | Metabolon classification: `NAMED` or `UNNAMED`. There are 878 named and 197 unnamed entries. |
| `INCHIKEY` | Chemical-structure identifier in InChIKey format, when supplied. |
| `SMILES` | Chemical structure represented as a SMILES string, when supplied. |
| `CHEMICAL_NAME` | Metabolon-supplied metabolite name or unnamed-feature label, such as an `X-` identifier. Preserve Metabolon suffixes and qualifiers as supplied. |
| `CAS` | Chemical Abstracts Service registry identifier, when supplied. |
| `CHEMSPIDER` | ChemSpider database identifier, when supplied. |
| `HMDB` | Human Metabolome Database accession, when supplied. |
| `KEGG` | KEGG database identifier, when supplied. |
| `PUBCHEM` | PubChem database identifier, when supplied. |
| `PLATFORM` | Metabolon-supplied analytical platform or chromatography/mass-spectrometry method label. |

Blank annotation fields indicate that the source metadata did not supply that annotation. They do not indicate that the metabolite measurement is missing. Unnamed features remain in the measurement matrices unless excluded as constant features. Database identifiers should be treated as identifiers rather than quantitative variables.

## Preprocessing

According to the manuscript Methods, Metabolon median-scaled metabolite abundances, imputed missing abundances using the minimum observed value for each metabolite within the relevant sample set, and natural-log transformed the data before delivery. 

For multivariate preparation, each log-transformed metabolite column was mean-centered and divided by the square root of its sample standard deviation across the 28 samples:

`scaled_value = (log_value - mean(log_values)) / sqrt(sample_SD(log_values))`

The sample standard deviation uses denominator `n - 1`. Sample and clinical metadata were not Pareto-scaled. The following 13 constant features were excluded from the multivariate matrix but retained in the unscaled file and annotation key:

- F100002385
- F100002806
- F100003235
- F100003236
- F100004208
- F100004322
- F100015776
- F100020975
- F100021881
- F533
- F999912262
- F999912407
- F999913695

The unscaled matrix is the input for metabolite-level modeling. The scaled matrix supplies the multivariate analyses, including PCA, PERMANOVA, PERMDISP, and PLS-DA. 

## Annotation provenance

The repository key was generated from the Metabolon analysis report, which contains matching entries for all 1,075 fecal features. `CHRO_LIB_ENTRY_ID`, `PATHWAY_SORTORDER`, and `PLOT_NAME` were omitted from the repository CSV; the Metabolon source workbook retains them.

## Verification of the supplied files

The checked copies contain identical sample identifiers, group assignments, and clinical metadata in both measurement files. All measurement features match unique entries in the key. All 13 excluded features are constant in the unscaled matrix. The scaled values agree with the formula above to a maximum absolute difference of approximately 5.7 × 10^-10. No missing or nonfinite values were found in the retained metabolite columns.
