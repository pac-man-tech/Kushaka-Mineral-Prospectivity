# Project documentation

## Research context

This project investigates mineral prospectivity mapping for gold in the Kushaka Schist Belt, northwestern Nigeria. The manuscript describes an integrated workflow drawing on airborne magnetic and radiometric data from the Nigerian Geological Survey Agency, ASTER imagery, and geological information. It describes magnetic processing (including reduction to the equator, first vertical derivative, and analytic signal amplitude) and radiometric alteration products (including K/eTh, Kd, and F-parameter maps).

The notebook begins downstream of those GIS and raster-processing steps. It uses prepared tabular evidence values and therefore does not, by itself, reproduce the complete geospatial workflow described in the manuscript.

## Data dictionary

The labelled table, `data/processed/NEW SVM-ET DATA.csv`, contains 70 rows and the following columns:

| Column | Description |
|---|---|
| `X`, `Y` | Spatial coordinates. The coordinate reference system and units are not specified in the table; confirm them from the source data before mapping. |
| `LINEAMENT` | Lineament-related evidence value. |
| `LIN. INT` | Lineament-intersection evidence value. |
| `LINEAMENT DENSITY` | Lineament-density evidence value. |
| `HYDROTHERMAL` | Hydrothermal alteration evidence value. |
| `LITHOLOGY` | Lithology evidence/category value. |
| `SLOPE` | Slope evidence value. |
| `G/NG` | Gold/no-gold target used for model development. Confirm the numeric class mapping against the source documentation before reuse. |

Other tabular files:

| File | Role indicated by its contents/name |
|---|---|
| `data/processed/RESEARCH DATA.csv` | Evidence features and coordinates, with an `FID` field; used by the notebook's prediction workflow. |
| `data/prediction/TO BE PREDICTED.csv` | Unlabelled evidence rows for prediction. The sample includes `-9999` in `HYDROTHERMAL`, which should be checked and handled as a missing-data value if appropriate. |
| `LAIDE_G_NO_G_DATA.xlsx`, `NEW NEW.xlsx` | Supporting workbooks retained in the project root pending confirmation of whether they are original or processed data. Their sheets, units, provenance, and relationship to the CSV files need to be documented. |

The notebook removes coordinate fields from model features in its labelled-data workflow. Preserve coordinates separately when joining predictions back to spatial locations, and ensure the same feature order, units, encodings, and missing-value handling are used for training and inference.

## Notebook workflow

The notebook includes data inspection and correlation plots, a stratified train/test split, feature scaling and an SVR-based binary classification experiment, Optuna tuning, metrics and plots, model serialization, spatial prediction, and an Extra Trees experiment with its own tuning and results.

The saved figures are in `outputs/figures/`; saved prediction and comparison workbooks are in `outputs/predictions/`; model artifacts are in `models/`. Notebook outputs or model files may reflect a previous execution and are not proof that the current notebook can be run from a clean environment.

## Reported results

The manuscript reports:

- SVM-RBF: 92.86% accuracy and ROC AUC 0.959; 60% of known active and historical mine sites were reportedly identified.
- ETC: 71.43% accuracy and ROC AUC 0.917; 46% of known mine sites were reportedly identified.
- Favourable zones are reported near Tsohon Birnin Gwari, Naganda, Bassawa, Sabon Layi, Chigwagwa Hill, Faleli, Gidan Mafidaji, and Guran Dutse.

These values are transcribed as manuscript claims and should be checked against the final analysis and publication version before citation. The supplied labelled dataset is small (70 rows); the test split shown in the notebook represents 14 rows.

## Reproducibility and methodological caveats

1. **Holdout-set tuning:** the SVR Optuna objective evaluates candidate models on `X_test`/`y_test`. Repeatedly selecting hyperparameters against the holdout makes it part of model selection, so its score is not an independent final test result. Use cross-validation within the training partition and reserve a final untouched test set for a stronger evaluation.
2. **Model terminology:** the notebook fits `sklearn.svm.SVR` (regression) and thresholds its continuous output to classify labels, while the manuscript calls the approach SVM-RBF. Confirm that this implementation matches the intended research method before describing it as a support-vector classifier.
3. **Preprocessing at inference:** a saved estimator alone does not document the scaler or other preprocessing needed to reproduce training. The notebook applies preprocessing in separate steps; package the fitted transformations and estimator together, or persist the complete preprocessing/model pipeline.
4. **Missing values and feature alignment:** inspect sentinel values such as `-9999`, missing values, class encodings, units, coordinate reference systems, and feature ordering before fitting or predicting. Apply identical preprocessing to training, validation, and prediction data.
5. **Spatial validation:** nearby samples can be spatially dependent. A random split can overstate performance when training and test samples are geographically close. Consider spatially separated validation and report the split design.
6. **GIS inputs:** source rasters, GIS processing scripts, and a complete account of how tabular values were extracted are not included in the current folder. The manuscript-level workflow cannot be fully reproduced from the notebook and current tables alone.
7. **Serialized models:** only load `.pkl`/joblib files from a trusted source. They are implementation-dependent and may execute code when loaded.

## Before publishing to GitHub

- Confirm permission and redistribution terms for all input data, maps, plots, and generated outputs.
- Confirm with all co-authors and any relevant publisher before publishing the manuscript. It is ignored by `.gitignore` by default.
- Review spreadsheets and prediction tables for sensitive coordinates or information that should not be public.
- Add a license only after the rights holders choose terms; without one, reuse permissions are not granted by default.
- Record the Python/package versions and the exact notebook execution environment used for any results you intend to claim as reproducible.
- Re-run the workflow from a clean environment and document any required preprocessing, outputs, and known issues.

## Manuscript reference

Nasiru, O. T., Alagbe, O. A., Folajimi, V., & Oladapo, K. *Gold Prospectivity Mapping Using Airborne Geophysical, Remote Sensing, and Machine Learning Techniques over Kushaka Schist Belt, Northwestern Nigeria*. Add venue, year, and DOI when available.
