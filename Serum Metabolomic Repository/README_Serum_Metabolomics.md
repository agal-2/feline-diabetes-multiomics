# Serum metabolomics data and data dictionary

## Overview

This repository subset contains individual-cat untargeted serum metabolomics data for 29 cats: 21 nondiabetic controls and 8 diabetic cats. It includes natural-log-transformed relative abundances, a Pareto-scaled analysis matrix, a metabolite annotation key, and annotated age-adjusted metabolite results. The measurements are relative abundances, not absolute metabolite concentrations. Raw mass-spectrometry instrument files are not included in this subset.

## Files

| File | Contents |
|---|---|
| `SERUMWIDELN20260716.csv` | 29 sample rows, 1,048 metabolite columns, and 11 sample/clinical metadata columns (1,059 columns total). Natural-log-transformed relative abundances before mean-centering or Pareto scaling. |
| `Serum_Pareto_Scaled_Analysis.csv` | The same 29 samples and metadata, with 1,047 mean-centered, Pareto-scaled metabolite columns (1,058 columns total). One constant feature was excluded. |
| `SERUMMETABOLITEKEY.csv` | 1,048 unique metabolite entries and 14 annotation columns. This key covers every metabolite in both measurement files. |
| `METAB_L2FC_ANNOTATED_KNOWN.csv` | 960 unique annotated metabolite results and 14 columns, including age-adjusted group means, confidence limits, P-values, q-values, fold changes, and annotations. |
| `README_Serum_Metabolomics.md` | File descriptions, data dictionary, processing information, and interpretation notes. |

The CSV files have a header row. Each measurement-file row represents one cat's serum sample; each key-file row represents one metabolite feature. Match the two measurement files by `SampleID`. Match metabolite column names to `FeatureID` in the key. The results file also joins to the key by `FeatureID`. All row counts exclude the header.

The current serum matrices retain `SampleID` values 1–29. These numbers are not the cytokine repository’s `CatID` values. A private crosswalk has been verified for all 29 cats, but has not been applied to these matrices and is not included in this subset. Do not join serum and cytokine datasets by assuming equal numeric IDs identify the same cat. Keep name, medical-record, and barcode linkage materials outside the public repository.

## Sample and clinical metadata

These columns occur in both measurement files and are not Pareto-scaled.

| Column | Definition and coding |
|---|---|
| `SampleID` | Serum analysis sample identifier, 1–29; consistent across the two serum matrices, but not interchangeable with cytokine CatID. |
| `Group` | `Control` = nondiabetic control; `Diabetes` = diabetic cat. |
| `GroupNum` | Numeric group code: 0 = Control; 1 = Diabetes. |
| `age_m` | Age in months; included as a covariate in the supplied serum metabolite-level models. |
| `BW_kg` | Body weight in kilograms. |
| `BCS` | Body condition score (1–9 scale). |
| `Fructosamine` | Serum fructosamine (µmole/L) measurement. |
| `Glucose` | Glucose (mg/dL) measurement. |
| `Cholesterol` | Serum cholesterol (mg/dL) measurement. |
| `Triglycerides` | Serum triglyceride (mg/dL) measurement. |
| `Total_T4` | Total thyroxine (nmol/L) measurement. |

The presence of a clinical variable in the file does not mean that it was included in every model. Blank clinical fields or SAS missing-value markers (`.`) indicate missing observations, not zero values. There is one missing fructosamine value and two missing total T4 values in the checked files.

## Metabolite measurements

Column names consisting of `S` followed by digits identify metabolites, for example `S30`. The numeric portion corresponds to `CHEM_ID` in the Metabolon source metadata. The prefix `S` distinguishes the serum analysis identifiers.

`SERUMWIDELN20260716.csv` contains natural logarithms of processed relative abundances. Negative values are valid and are not missing-value codes. This file supplies the age-adjusted univariate models and the sample-level data used to estimate forest-plot contrasts.

`Serum_Pareto_Scaled_Analysis.csv` contains the corresponding mean-centered, Pareto-scaled values for multivariate analyses, including PCA, PERMANOVA, PERMDISP, and PLS-DA. Positive, negative, and zero values are valid after centering.

## Metabolite key data dictionary

| Column | Definition |
|---|---|
| `FeatureID` | Unique identifier matching a metabolite column in the measurement files: `S` plus the source `CHEM_ID`. |
| `COMP_ID` | Metabolon-supplied compound identifier. |
| `SUPER_PATHWAY` | Broad metabolic category assigned by Metabolon. |
| `SUB_PATHWAY` | More specific pathway assigned by Metabolon. |
| `PATHWAY_SORTORDER` | Metabolon-supplied ordering field for arranging metabolites by pathway; not a measurement. |
| `INCHIKEY` | Chemical-structure identifier in InChIKey format, when available. |
| `SMILES` | Chemical structure represented as a SMILES string, when available. |
| `CHEMICAL_NAME` | Metabolon-supplied chemical name or unnamed-feature label. Preserve annotation suffixes, asterisks, and bracketed identifiers as supplied. |
| `PLOT_NAME` | Metabolon-supplied display label for plotting. |
| `CAS` | Chemical Abstracts Service registry identifier, when supplied. |
| `CHEMSPIDER` | ChemSpider database identifier or provider-supplied list of identifiers. |
| `HMDB` | Human Metabolome Database accession, when supplied. |
| `KEGG` | KEGG database identifier, when supplied. |
| `PUBCHEM` | PubChem database identifier or provider-supplied list of identifiers. |

The source workbook classifies 961 features as named and 87 as unnamed. These 87 unnamed features have no super-pathway or sub-pathway assignments. The compact CSV does not include the Metabolon `TYPE` column. Blank annotation fields mean that an annotation was not supplied; they do not mean that the sample measurements are missing. Unnamed features are retained in the measurement matrices unless excluded for another documented reason.

Database identifiers are text, not quantitative values. Some cells contain comma-separated lists of identifiers. Import these columns as Text in spreadsheet software to preserve punctuation and prevent date conversion, thousands-separator changes, or loss of digits.

## Preprocessing

According to the manuscript Methods, Metabolon median-scaled metabolite abundances, imputed missing values using the minimum observed abundance for each metabolite within the relevant sample set, and natural-log transformed the data before delivery. No additional normalization, imputation, or log transformation is applied before metabolite-level modeling.

For multivariate analyses, each metabolite's log-transformed values were centered using their mean across the 29 samples and divided by the square root of their sample standard deviation:

`scaled_value = (log_value - mean(log_values)) / sqrt(sample_SD(log_values))`

The sample standard deviation uses denominator `n - 1`. The constant feature `S100002806` (gabapentin) was removed from the multivariate matrix; it remains in the unscaled matrix and the key. Thus, the unscaled and scaled matrices contain 1,048 and 1,047 metabolite features, respectively.


## Statistical context

The supplied serum code fits metabolite-level models with study group and age in months, and applies Benjamini–Hochberg FDR correction. Fold changes compare diabetic cats with controls. L2FC is calculated from the difference between diabetic and control model-based group means on the natural-log scale divided by ln(2); FC = 2^L2FC. These are modeled group contrasts, not measurements stored in the two sample-level files.

Annotations support volcano labeling and pathway analyses. Pathway-analysis inputs retain features with both pathway fields populated. Statistical results, supplementary tables, software versions, and detailed model settings should be read alongside the final manuscript. This subset provides measurement inputs, annotations, and the 960-row annotated results dataset; it does not include every derived output or SAS code.

## Key provenance and verification

The CSV key was checked against the uploaded Metabolon serum metabolite key workbook, matching `FeatureID` to `S` plus `CHEM_ID`. There are no missing or duplicate feature identifiers. All 1,048 source features occur in the unscaled matrix, and all 1,047 retained scaled features have matching source annotations.

Both matrices contain identical sample identifiers, group assignments, and clinical metadata. The excluded feature is constant in the unscaled matrix. Scaled values agree with the formula above to a maximum absolute difference of approximately 6.2 × 10^-10. These checks establish file consistency; they do not independently reproduce every manuscript analysis or verify every clinical measurement. The private identity linkage was subsequently checked using age, group, and fructosamine against sample inventories and the manifest; the two cats sharing a name were distinguished using their cytokine profiles and medical-record identifiers. No identifying information is included in this dictionary.


## Annotated results data dictionary

`METAB_L2FC_ANNOTATED_KNOWN.csv` contains one row per annotated metabolite. It is a results table, not a sample-level abundance matrix.

| Column | Definition |
|---|---|
| `FeatureID` | Metabolite identifier matching the matrix column and key. |
| `Raw_P` | Unadjusted study-group effect P-value from the age-adjusted metabolite model. |
| `Q_value` | Benjamini–Hochberg FDR-adjusted P-value from the original model-results workflow, before the annotated subset was selected. Do not recompute FDR using only significant rows or assume the 960 annotated rows define the complete testing family. |
| `LSMean_1` | Age-adjusted least-squares mean for controls on the natural-log scale. |
| `LSMean_1_LCL` | Lower 95% confidence limit for the control least-squares mean. |
| `LSMean_1_UCL` | Upper 95% confidence limit for the control least-squares mean. |
| `LSMean_2` | Age-adjusted least-squares mean for diabetic cats on the natural-log scale. |
| `LSMean_2_LCL` | Lower 95% confidence limit for the diabetic least-squares mean. |
| `LSMean_2_UCL` | Upper 95% confidence limit for the diabetic least-squares mean. |
| `L2FC` | (LSMean_2 − LSMean_1) / ln(2). Positive values indicate higher estimated abundance in diabetic cats. |
| `FC` | 2^L2FC; diabetic-to-control ratio of modeled relative abundances. |
| `ChemicalName` | Chemical name from the metabolite key. |
| `SuperPathway` | Broad pathway from the metabolite key. |
| `SubPathway` | Specific pathway from the metabolite key. |

The group-mean confidence limits are not confidence limits for the group difference. Figure 2 uses model-derived contrast confidence limits; these must be obtained from the fitted model, not by subtracting the group-mean confidence limits.

## Feature coverage and relation to manuscript outputs

The key and unscaled matrix contain 1,048 features: 961 named and 87 unnamed. The constant named feature gabapentin is absent from the 1,047-feature scaled matrix and from the 960-row annotated results file. The remaining 87 features lack pathway assignments and are absent from the annotated results file, but remain in the multivariate matrix. These exclusions explain the counts; there are no features missing from the key.

| Manuscript output | Relevant repository inputs |
|---|---|
| Table 6 and Supplementary Table S2: age-adjusted metabolite comparisons | Unscaled matrix, age and group metadata, complete key, annotated results. The results file contains 46 metabolites with q<0.05. |
| Figure 2: age-adjusted forest plot | Unscaled matrix to estimate contrasts and their confidence limits; annotated results to identify and label selected metabolites. |
| Figure 3 and Supplementary Table S3: volcano analysis | Annotated results, including L2FC and q-values. |
| Figure 4, Supplementary Table S4, and Supplementary Figures S2–S3: descriptive pathway patterns | Annotated results and pathway assignments. |
| Figure 5, Supplementary Table S5, and Supplementary Figure S4: rank-based enrichment | Annotated results and pathway assignments; separate analyses by pathway level and signed/absolute scoring. |
| Figure 1, PERMANOVA, and PERMDISP | Pareto-scaled matrix and group labels. |
| Supplementary Table S1 and Supplementary Figure S1: PCA feature clusters and annotation | Pareto-scaled matrix and annotations. |
| Supplementary Table S6: PLS-DA variable importance | Pareto-scaled matrix and group labels, with annotations used to label features and select the annotated output. |

The supplied PCA macro call uses `prin_num=27`, and its clustering step explicitly uses components 1–27. PERMANOVA independently requests all nonzero components (28 for the centered 29-sample dataset); PERMDISP uses those retained scores. This distinction should be preserved when reproducing the supplied workflow. The code setting alone does not establish the provenance of a historical cluster output.


