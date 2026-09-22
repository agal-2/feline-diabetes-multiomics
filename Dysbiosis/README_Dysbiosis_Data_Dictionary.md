# Feline fecal dysbiosis data and data dictionary

## Scope and current coverage

This folder contains the complete supplied 25-cat fecal dysbiosis index (DI) and bacterial taxon dataset for Table 4 and documents its relationship to cross-platform analyses in the manuscript. DI.csv contains 25 cats (16 controls and 9 diabetic cats), with complete, unique CatIDs. All 22 records in DI_revised.csv match uniquely across all ten measurements and group. The full file includes one additional control and two additional diabetic cats. 

## Available files and linked datasets

| File | Records | Role |
|---|---:|---|
| [DI.csv](DI.csv) | 25 cats, 13 columns | Complete Table 4 measurement dataset: 16 controls and 9 diabetic cats. Includes RecordID and complete CatID. |
| [Dysbiosis_Data_Dictionary.csv](Dysbiosis_Data_Dictionary.csv) | 13 definitions | Machine-readable dictionary for DI.csv. |
| [DI_revised.csv](../Targeted%20Fecal%20Analyses/DI_revised.csv) | 22 cats, 12 columns | Cross-platform DI/taxon subset: 15 controls, 7 diabetic cats. Stored once in the targeted fecal subset. |
| [fa_revised.csv](../Targeted%20Fecal%20Analyses/fa_revised.csv) | 12 cats | Targeted measurements for the Figure S8 paired analysis. Join to DI_revised.csv by CatID. |
| [FECESWIDELN20260720.csv](../Fecal%20Metabolomic%20Repository/FECESWIDELN20260720.csv) | 28 cats | Natural-log-transformed untargeted fecal abundances; a verified fecal SampleID-to-CatID crosswalk is required before joining to the DI file. |
| [FECESMETABOLITEKEY_updated.csv](../Fecal%20Metabolomic%20Repository/FECESMETABOLITEKEY_updated.csv) | Metabolite annotations | Match metabolite columns to FeatureID for untargeted correlation annotation. |
| [Targeted fecal dictionary](../Targeted%20Fecal%20Analyses/README_Targeted_Fecal_Data_Dictionary.md) | Documentation | Targeted measurement units, transformations, Figure S8 reference results, and data checks. |
| [Column dictionary](../Targeted%20Fecal%20Analyses/Targeted_Fecal_Data_Dictionary.csv) | 12 DI definitions within the combined dictionary | Filter File to DI_revised.csv for its machine-readable column definitions. |

The cross-platform files overlap; their row counts must not be added to obtain a study population. DI_revised.csv is retained in its established location for compatibility with the targeted subset. It contains no measurements additional to DI.csv. No measurement values were changed during preparation.

## Measurement column dictionary

The measurement and group definitions below apply to both DI.csv and DI_revised.csv. DI.csv additionally contains RecordID, a unique local identifier DI001–DI025 assigned in source row order. RecordID is not a cross-platform animal identifier. The 22 previously linked CatIDs were recovered by unique exact measurement-profile and group matches to the verified subset. The remaining identifiers were resolved against the laboratory workbook: DI016 = CatID 20, DI024 = CatID 30, and DI025 = CatID 31. 

| Column | Type | Definition and units |
|---|---|---|
| `CatID` | Integer identifier | Study identifier consistent across repository subsets where the same cat occurs. Complete and unique in both DI.csv and DI_revised.csv. |
| `group` | Text | `Control` = nondiabetic control; `DM` = insulin-treated diabetic cat. |
| `DI` | Numeric | Unitless fecal dysbiosis index. Under the manuscript definition, values below zero indicate no major overall microbiome shift; positive values indicate a shift of increasing magnitude. |
| `Faecalibacterium` | Numeric | Faecalibacterium abundance, log10 DNA copies/g feces. |
| `Turicibacter` | Numeric | Turicibacter abundance, log10 DNA copies/g feces. |
| `Streptococcus` | Numeric | Streptococcus abundance, log10 DNA copies/g feces. |
| `Ecoli` | Numeric | Escherichia coli abundance, log10 DNA copies/g feces. |
| `Blautia` | Numeric | Blautia abundance, log10 DNA copies/g feces. |
| `Fusobacterium` | Numeric | Fusobacterium abundance, log10 DNA copies/g feces. |
| `Clostridium_hiranonis` | Numeric | Peptacetobacter hiranonis abundance, log10 DNA copies/g feces. Historical source variable name retained. |
| `Bifidobacterium` | Numeric | Bifidobacterium abundance, log10 DNA copies/g feces. |
| `Bacteriodes` | Numeric | Bacteroides abundance, log10 DNA copies/g feces. Source misspelling retained. |

Taxon abundances are already base-10 logarithms, not percentages or raw copy counts. Negative log values are valid measurements. DI is an index, not a bacterial abundance or log-transformed concentration. Neither file has missing measurements or group codes. Neither file has missing CatIDs. CSV encoding is UTF-8, decimals use points, and one header row precedes the data.

According to the manuscript, DI was calculated using a nearest-centroid classifier based on total bacterial abundance and seven taxa: Faecalibacterium, Turicibacter, Escherichia coli, Streptococcus, Bifidobacterium, Bacteroides, and Peptacetobacter hiranonis. Blautia and Fusobacterium were measured separately and are not inputs to that DI algorithm. Total bacterial abundance is not included in this export. DI cannot be reconstructed from these nine taxon columns alone. Raw qPCR measurements and classifier reference data are not included.

## Analysis populations and joining instructions

### Table 4: group comparisons

Use all 25 records in DI.csv: 16 controls and 9 diabetic cats. Descriptive values are medians and quartiles. Effects are oriented as control minus diabetic. The 22-cat DI_revised.csv subset cannot substitute for the full Table 4 cohort.

Comparisons use pooled t-tests for DI, Faecalibacterium, Turicibacter, Blautia, Fusobacterium, and Bacteriodes; the Satterthwaite unequal-variance t-test for Bifidobacterium; and exact two-sided Wilcoxon rank-sum tests for Streptococcus, Ecoli, and Clostridium_hiranonis. Pooled versus unequal-variance results were selected using the folded F test for equality of variances at alpha = 0.05. Mean differences and Hedges' g accompany t-tests; Hodges–Lehmann shifts and rank-biserial effects accompany Wilcoxon tests. Table 4 P-values are unadjusted for multiplicity and interpreted as nominal significance.

The DI comparison has control median [Q1, Q3] -0.5 [-2.2, 0.4] and diabetic median 0.6 [-0.3, 1.5]; control minus diabetic mean difference -1.357 (95% CI -2.635 to -0.079), Hedges' g -0.885, and pooled P = 0.038. None of the nine individual bacterial taxon comparisons is significant at P < 0.05. Bifidobacterium uses the unequal-variance result (mean difference -0.129; 95% CI -1.618 to 1.360; P = 0.851). Reference inferential results follow the supplied analysis output and Table 4.

For the categorical DI comparisons reported in the manuscript:

| Threshold | Controls | Diabetic cats | Two-sided Fisher P | Cramér's V |
|---|---:|---:|---:|---:|
| DI > 0 | 5/16 (31.3%) | 6/9 (66.7%) | 0.115 | 0.343 |
| DI > 1 | 2/16 (12.5%) | 3/9 (33.3%) | 0.312 | 0.250 |

The complementary DI <= 0 counts are 11/16 controls (68.8%) and 3/9 diabetic cats (33.3%). Threshold flags are derived from DI and are not additional columns in DI.csv.

### Supplementary Figure S8: targeted correlations

Inner-join the linked DI_revised.csv and fa_revised.csv by CatID. This gives 12 unique cats, 8 controls and 4 diabetic cats. Group encodings differ: `Control`/`DM` correspond to `C`/`D`. Their assignments agree for all paired records.

Use DI and the nine taxon columns against 41 original-scale targeted measurements, excluding lathosterol_TMS, which is constant. This yields 410 two-sided Spearman correlations. Retain average ranks for ties. Apply Benjamini–Hochberg correction separately within each DI/taxon family of 41 tests and globally across all 410 tests. Figure asterisks denote within-family q < 0.05. No association survives global adjustment; the independently checked minimum global q is 0.10372.

The P. hiranonis sensitivity analyses examine lithocholic acid and secondary bile acid percentage after excluding CatID 11, CatID 2, and both. A separate partial Spearman analysis controls for diabetic status (0 = control, 1 = diabetic). The targeted dictionary provides the four reproduced within-family associations. The final rho/P/q tables are not included as separate deposited files.

### Untargeted fecal correlations

The manuscript reports 22 paired cats, ten DI/taxon variables, and 1,075 starting fecal metabolite features. Thirteen features were invariant in the paired cohort, leaving 1,062 valid features and 10,620 correlations. FDR correction was applied within DI/taxon families and globally; no association survived correction.

The DI file uses CatID, while the untargeted fecal matrix uses SampleID. **Do not join these numeric columns by equality.** A verified fecal SampleID-to-CatID mapping is not supplied in this folder. The private serum linkage does not automatically establish fecal sample numbering. Exact untargeted joins and reproduction of these results remain pending that mapping. Constant features must be checked within the verified paired cohort before excluding undefined correlations from FDR calculations.
