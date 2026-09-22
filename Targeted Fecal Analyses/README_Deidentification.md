# Targeted fecal and dysbiosis datasets: identifiers

For measurement definitions, units, analysis populations, and the processing differences between the two multivariate matrices, see [the full data dictionary](README_Targeted_Fecal_Data_Dictionary.md).

These files are de-identified copies of the supplied analysis datasets. All measurements, stored numerical precision, missing-value representations, and group codes were preserved. Rows are sorted by numeric CatID.

| File | Animal records | Identifier processing |
|---|---:|---|
| `FA.csv` | 14 | Replaced name-bearing `id` with CatID; removed two completely empty trailing records. |
| `fa_revised.csv` | 12 | Replaced name-bearing `id` and source `SampleID` with CatID. |
| `DI_revised.csv` | 22 | Replaced source `SampleID` with CatID. |
| `ONE_TRANS_SCALED.csv` | 14 | Replaced name-bearing `id` with CatID. |
| `TARGETEDFECAL_PARETO.csv` | 14 | Replaced name-bearing `id` with CatID. |

CatIDs 1–29 follow the verified serum SampleID–CatID linkage and are consistent with the cytokine and GLP-1 repository identifiers. Only the relevant subset of these IDs appears in each file. Two additional cats absent from that linkage were assigned CatIDs 30 and 31 with study-author approval. Their name-to-ID mapping is maintained privately. The original verified linkage was not modified.

The source revised fecal file supplied the sample identifier needed to disambiguate a duplicated name: that fecal record maps to CatID 1. Names, source sample identifiers, and identity keys are not included in these deposited files. CatID is unique within each file and has no quantitative or ordinal meaning.

Join the revised fecal and DI files on CatID to recover the 12-cat Figure S8 cohort. Group codes were preserved: `C`/`D` in fecal files correspond to `Control`/`DM` in the DI file, and `group_num` uses 0/1 for control/diabetic. All paired group assignments agree. The same 12 CatIDs occur in the deposited GLP-1 paired file.

The two influential observations excluded in the original correlation sensitivity analyses are CatIDs **11** and **2** in these files. Any code using the old sample identifiers must be updated before reproducing those exclusions. Do not equate CatID with the SampleID numbering retained in the separate serum or fecal metabolomics matrices.

Validation confirmed exact preservation of every retained non-identifier field, unique CatIDs, complete record retention apart from the two empty rows, and consistency of the paired datasets. Original source files and private processing/audit materials remain outside this deposition folder. This note documents de-identification; it is not the full measurement and analysis data dictionary.
