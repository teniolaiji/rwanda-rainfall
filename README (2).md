# Next-Day Heavy-Rainfall Classification for Rwanda

Classifies tomorrow's rainfall at ten sites across Rwanda as **dry**, **light/moderate** or **heavy**, and compares traditional machine learning (scikit-learn) with deep learning (TensorFlow/Keras). The goal is early warning for the heavy-rain days that trigger floods and landslides, such as the May 2023 disaster in the Western and Northern Provinces.

**Links:** Report: _TBA_ · Google Drive (raw data + saved models): _TBA_ · Video: _TBA_

## Data

| Source | Used for | Resolution |
|---|---|---|
| [CHIRPS v3](https://www.chc.ucsb.edu/data/chirps3), UC Santa Barbara Climate Hazards Center | Daily rainfall (target and past-rainfall features) | 0.05° (~5 km) |
| [NASA POWER](https://power.larc.nasa.gov/) (MERRA-2, CERES) | 16 daily weather variables | 0.5° × 0.625° |

1981-2025, ten sites (one per MERRA-2 grid cell), about 164,000 site-days. Raw data is not stored in this repository; it is in the Google Drive folder linked above.

## How to run

1. Open `rwanda_rainfall_classification.ipynb` in Google Colab.
2. In Section 2.1, set `DATA_MODE = "drive"` (the default for reproduction). The notebook downloads the zipped raw data from the shared folder.
3. Runtime → Run all.

To rebuild the raw data from the original sources instead, set `DATA_MODE = "api"` and provide a Google Earth Engine project ID (free for non-commercial use).

Random seeds are fixed (42). Dependencies are listed in `requirements.txt`.

## Acknowledgements

The data was obtained from the National Aeronautics and Space Administration (NASA) Langley Research Center (LaRC) Prediction of Worldwide Energy Resource (POWER) Project funded through the NASA Earth Science/Applied Science Program. CHIRPS data courtesy of the Climate Hazards Center, UC Santa Barbara.
