# Next-Day Heavy-Rainfall Classification for Rwanda

Classifies tomorrow's rainfall at ten sites across Rwanda as **dry**, **light/moderate**, or **heavy**, and compares traditional machine learning (scikit-learn, XGBoost) with deep learning (TensorFlow/Keras). The aim is early warning for the heavy-rain days that trigger floods and landslides, such as the May 2023 disaster in the Western and Northern Provinces.

**Links**
- Report: see `report/` in this repository (or the submitted document)
- Google Drive (raw data, zipped + saved models): https://drive.google.com/drive/folders/1O1uLyIHMrhSwFn3Oy5MhFwvRVW8qpQDm?usp=sharing
- Video walkthrough: https://youtu.be/Vzmx3AY9oIw

## Problem

Given today's weather at a location, predict whether tomorrow will be dry (< 1 mm), light/moderate, or heavy rain, where "heavy" is the top 10% of wet days at that site. The target is framed for early warning: a categorical forecast one day ahead.

## Data

The dataset is built by fusing two open sources (it is **not** a downloaded CSV):

| Source | Role | Resolution |
|---|---|---|
| [NASA POWER](https://power.larc.nasa.gov/) (MERRA-2 reanalysis, CERES) | 16 daily weather predictors | 0.5° × 0.625° (~55 km) |
| [CHIRPS v3](https://www.chc.ucsb.edu/data/chirps) (UC Santa Barbara) | Rainfall target + past-rainfall features | 0.05° (~5 km) |

Ten sites (one per MERRA-2 grid cell) across all five provinces, 1984–2025, about 153,400 site-days after preprocessing. **Raw data is not committed to this repository**; it lives in the Google Drive folder above.

## How to run

1. Open `rwanda_rainfall_classification.ipynb` in Google Colab.
2. In Section 2.1, set `DATA_MODE = "drive"` (the default for reproduction). The notebook downloads the zipped raw data from the shared Drive folder, so no accounts are needed.
3. Runtime → Run all. The notebook runs top to bottom and regenerates every figure, the experiment log, and the saved models.

To rebuild the raw data from the original sources instead, set `DATA_MODE = "api"` and supply a free Google Earth Engine project ID (used only for CHIRPS).

Random seeds are fixed (42). Experiment metrics are written to `experiments.csv` and to TensorBoard logs under `logs/`, so every row of the results table is traceable to a logged run.

## Repository structure

```
rwanda_rainfall_classification.ipynb   # the full pipeline, sections 1–8
requirements.txt                       # dependencies
logs/                                  # experiments.csv + TensorBoard runs
README.md
```

Raw data and the saved best models (`.joblib`, `.keras`/`.h5`) are in the Google Drive folder, not in git.

## Method notes

- **Time-based split** (train 1984–2014, validate 2015–2019, test 2020–2025) to prevent leakage; the test period contains the May 2023 floods.
- Heavy-rain thresholds, the feature scaler, and feature selection are fitted on **training years only**.
- Class imbalance (heavy rain is ~5% of days) is handled with balanced class weights and a tuned decision threshold; models are selected on heavy-class PR-AUC, not accuracy.

## Requirements

See `requirements.txt`. Colab already includes most of these; the notebook pins nothing and uses Colab's tested versions.

## Acknowledgements

Data obtained from the NASA Langley Research Center POWER Project, funded through the NASA Earth Science/Applied Science Program. CHIRPS data courtesy of the Climate Hazards Center, UC Santa Barbara.
