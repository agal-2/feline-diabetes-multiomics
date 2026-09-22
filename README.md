# Feline diabetes multiomics data

## Associated manuscript

**Serum and fecal metabolomic profiling identifies altered gut–liver bile acid and lipid metabolism in cats with diabetes mellitus**

This repository contains de-identified datasets and data dictionaries supporting the manuscript. The study enrolled 31 cats: 21 nondiabetic controls and 10 insulin-treated diabetic cats. Sample availability differs across analyses; the dataset counts below overlap and must not be added together.

## Folder guide

| Folder and dictionary | Contents | Analysis population |
|---|---|---|
| [Serum metabolomics](Serum%20Metabolomic%20Repository/README_Serum_Metabolomics.md) | Untargeted serum metabolite abundances, Pareto-scaled analysis matrix, annotations, and annotated differential-abundance results | 29 cats: 21 controls, 8 diabetic |
| [Fecal metabolomics](Fecal%20Metabolomic%20Repository/README_Fecal_Metabolomics.md) | Untargeted fecal metabolite abundances, Pareto-scaled analysis matrix, and annotations | 28 cats: 21 controls, 7 diabetic |
| [Cytokines](Cytokines/README_Cytokine_Data_Dictionary.md) | Original and processed cytokine concentrations, with SDF-1 multiple-imputation datasets | 29 cats: 21 controls, 8 diabetic; repeated imputation rows are not additional animals |
| [GLP-1](GLP-1/README_GLP1_Data_Dictionary.md) | Serum total GLP-1 and paired GLP-1–fecal bile-acid measurements | GLP-1: 28 cats, 21 controls and 7 diabetic; paired file: 12 cats, 8 controls and 4 diabetic |
| [Targeted fecal analyses](Targeted%20Fecal%20Analyses/README_Targeted_Fecal_Data_Dictionary.md) | Targeted bile acids, fatty acids, sterols, processed matrices, and the DI correlation subset | Full targeted panel: 14 cats, 8 controls and 6 diabetic; Figure S8 paired cohort: 12 cats, 8 controls and 4 diabetic |
| [Dysbiosis](Dysbiosis/README_Dysbiosis_Data_Dictionary.md) | Corrected complete Table 4 dysbiosis index and nine bacterial taxon measurements | 25 cats: 16 controls, 9 diabetic |

Read each folder's dictionary before analysis. It describes units, group coding, missing values, transformations, analysis subsets, and limitations. CSV dictionaries, where supplied, define individual columns.

## Linking cats across datasets

[Cross_Platform_Cat_Linkage.csv](Cross_Platform_Cat_Linkage.csv) contains one row for each of the 31 study cats. Its first column, `CatID`, is the common anonymous animal identifier. Other identifier columns are named as `folder/filename::identifier` so that each ID is explicitly tied to its deposited file.

1. For a dataset containing `CatID`, match that column to the crosswalk's first column.
2. For a serum or fecal metabolomics dataset, match its `SampleID` to the crosswalk column naming that exact file, then carry the corresponding `CatID` into the analysis.
3. The crosswalk also connects `Dysbiosis/DI.csv::RecordID` to `CatID`. RecordID identifies a local DI record; it is not the cross-platform key.
4. Join the resulting datasets on `CatID` and retain only cats with the measurements required for the intended analysis. Check group agreement and the resulting sample count.

Blank identifier cells mean that the cat is absent from that deposited file, not that the cat's identity is unresolved. Identifiers have no numerical or ordinal interpretation. Never equate CatID with SampleID, assume that equal SampleIDs in different files represent the same animal, or join records by row position.

Serum SampleID-to-CatID links come from the verified private linkage. Fecal SampleID links are metadata-based: all 28 fecal records matched uniquely to serum records across ten shared clinical/group fields. These different linkage bases are stated in the crosswalk. Names, medical record numbers, laboratory barcodes, and the private identity keys are not included in the crosswalk.

For cytokine multiple-imputation files, retain `_Imputation_` alongside CatID to distinguish repeated draws. Joining two long imputation files on CatID alone multiplies records; equal imputation numbers from different imputation models do not identify paired draws.

## Reproducing the intended analysis populations

- **Table 4:** use all 25 records in `Dysbiosis/DI.csv`. CatID 20 is classified as control in this corrected dataset. All numerical DI/taxon measurements were preserved.
- **Table 5 and targeted multivariate analyses:** use the 14-cat targeted files specified in their dictionary. The final Pareto-scaled matrix and the intermediate matrix are not interchangeable.
- **Figure S8:** join `Targeted Fecal Analyses/fa_revised.csv` to `Targeted Fecal Analyses/DI_revised.csv` on CatID, producing 12 cats. The 22-cat DI_revised subset contains 15 controls and 7 diabetic cats and does not replace the full Table 4 cohort. CatID 20 is absent from it, so the DI group correction does not change this paired cohort.
- **Untargeted fecal–DI correlations:** use the crosswalk to link fecal metabolomics SampleID to CatID, then select the 22 cats in DI_revised.csv. The crosswalk supplies metadata-based fecal links; readers should retain that provenance when reusing them.
- **GLP-1–bile-acid associations:** use the 12-cat `GLP-1/GLP_BA_PAIRED.csv` file rather than treating all cats in the separate platform datasets as paired.

The crosswalk is the repository-wide identifier reference. Earlier folder-specific notes stating that an untargeted fecal crosswalk is not supplied are superseded by this file, with the metadata-based qualification above.

## Data interpretation and scope

Untargeted metabolomics files contain processed relative abundances; original-scale targeted measurements, log-transformed measurements, and Pareto-scaled matrices must be interpreted according to their dictionaries. DI is unitless, and the deposited bacterial taxon abundances are already log10 DNA copies/g feces.

Raw mass-spectrometry instrument files, raw qPCR data, private identity keys, and a complete executable analysis-code package are not included. The available datasets and dictionaries document the deposited measurements and their analysis context; they are not a claim that every reported result can be reproduced without additional analysis specifications.

Repository: https://github.com/agal-2/feline-diabetes-multiomics
