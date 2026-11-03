# Gold Prospectivity Mapping in the Kushaka Schist Belt

Research materials for gold prospectivity mapping in the Kushaka Schist Belt, northwestern Nigeria, using geophysical, remote-sensing, geological, and machine-learning evidence.

The project combines a Jupyter notebook, tabular training and prediction data, exported prediction tables, model files, and plotted results. The accompanying manuscript describes a broader workflow involving airborne magnetic and radiometric data, ASTER imagery, and ancillary geological information.

## Study and workflow

The study evaluates gold/no-gold prospectivity using the following evidence layers: lineament intensity (`LINEAMENT`), lineament intersections (`LIN. INT`), lineament density, hydrothermal alteration, lithology, and slope. The manuscript describes magnetic filtering, radiometric alteration indices, and GIS-based integration. The notebook works with already-prepared tabular data; the source raster data and GIS preprocessing workflow are not included here.

The notebook explores two approaches:

- An RBF support-vector-regression model whose continuous predictions are thresholded into binary classes. The manuscript refers to the approach as SVM-RBF.
- An Extra Trees Classifier (ETC).

The manuscript reports the following model results:

| Model | Reported accuracy | Reported ROC AUC | Known mine-site validation |
|---|---:|---:|---:|
| SVM-RBF | 92.86% | 0.959 | 60% |
| ETC | 71.43% | 0.917 | 46% |

These are results reported in the manuscript and project outputs, not independently reproduced results. The labelled input table has 70 rows, so the notebook's 20% holdout contains only 14 rows. In addition, the SVR hyperparameter search scores candidates on that same holdout set. The reported holdout metrics should therefore be treated as preliminary rather than as an unbiased estimate of generalization performance.

### Saved evaluation figures

The following figures are existing project outputs:

| SVM-RBF / SVR ROC | ETC ROC |
|---|---|
| ![SVR ROC curve](<outputs/figures/SVR ROC AUC.png>) | ![ETC ROC curve](<outputs/figures/ETC ROC AUC.png>) |

They are included as reported outputs, not independently regenerated results.

## Repository contents

| File or pattern | Purpose |
|---|---|
| `notebooks/Laide Gold exploration using machine learning.ipynb` | Exploratory analysis, model fitting/evaluation, and prediction workflow. |
| `data/processed/NEW SVM-ET DATA.csv` | Labelled tabular data used for model development (`G/NG` is the target). |
| `data/processed/RESEARCH DATA.csv` | Spatial/evidence table used for a prediction and validation workflow. |
| `data/prediction/TO BE PREDICTED.csv` | Unlabelled rows for prediction. |
| `LAIDE_G_NO_G_DATA.xlsx`, `NEW NEW.xlsx` | Supporting workbooks retained in the project root until their raw/processed provenance is confirmed. |
| `outputs/predictions/` | Exported predictions and test-set comparisons. |
| `models/` | Serialized model artifacts. Only load pickle/joblib files from sources you trust. |
| `outputs/figures/` | Saved correlation, feature-importance, ROC, and confusion-matrix figures. |
| `manuscript/` | Research manuscript source; excluded from Git by default. |
| `docs/PROJECT_DOCUMENTATION.md` | Methods, data notes, limitations, and reproducibility details. |

## Getting started

Use Python with the packages listed in [`requirements.txt`](requirements.txt). From this project directory, create and activate a virtual environment, then install the packages:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Run Jupyter from the project root. The notebook locates the project root using `requirements.txt`, so its paths work whether the kernel starts in the root or the `notebooks` folder:

```powershell
jupyter notebook "notebooks\Laide Gold exploration using machine learning.ipynb"
```

Review the notebook and data notes before running all cells. Several cells write Excel workbooks under `outputs/predictions/` and model files under `models/`; rerunning those cells can replace existing outputs.

## Data and responsible use

Check the source, redistribution terms, and any confidentiality restrictions for every dataset and figure before making this repository public. The manuscript is excluded by default because it contains author contact details and may require co-author or publisher approval before redistribution. No open-source license has been selected for this project; do not assume others may reuse its code or data until the owners choose and add an appropriate license.

Prospectivity predictions are research outputs, not confirmation of mineral resources or a substitute for field investigation. Interpret them alongside geological evidence and appropriate validation.

## Documentation and citation

See [`docs/PROJECT_DOCUMENTATION.md`](docs/PROJECT_DOCUMENTATION.md) for the data dictionary, workflow notes, caveats, and a pre-publication checklist.

The research context is described in the manuscript, *Gold Prospectivity Mapping Using Airborne Geophysical, Remote Sensing, and Machine Learning Techniques over Kushaka Schist Belt, Northwestern Nigeria*, by Olaide Taofeek Nasiru, Olufemi Adigun Alagbe, Victor Folajimi, and Kunle Oladapo. Add publication details or a DOI here if/when they are finalized.
