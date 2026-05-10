# Dataset Information

## Lithology Prediction Dataset (FORCE 2020)

This dataset contains well log data for predicting lithology (rock type) from various geophysical measurements. It was developed as part of the FORCE 2020 Lithology Prediction Competition and is widely used for machine learning and subsurface interpretation studies.

The dataset includes:

- Well log measurements
- Geological formations
- Lithology labels
- Spatial well information
- Confidence scores for lithology predictions

---

# Dataset Source

Dataset available from Kaggle:

https://www.kaggle.com/datasets/faresazzam/well-logs-dataset-for-machine-learning

---

# Dataset Columns

## Well Identification and Location

| Column | Description |
|---|---|
| WELL | Unique well identifier |
| DEPTH_MD | Measured depth (meters) |
| X_LOC | Easting coordinate |
| Y_LOC | Northing coordinate |
| Z_LOC | Vertical depth/elevation |

---

## Geological Information

| Column | Description |
|---|---|
| GROUP | Geological group |
| FORMATION | Geological formation |
| FORCE_2020_LITHOFACIES_LITHOLOGY | Lithology label (target variable) |
| FORCE_2020_LITHOFACIES_CONFIDENCE | Confidence level of lithology label |

---

## Well Log Measurements

| Log | Description |
|---|---|
| CALI | Caliper log |
| RSHA | Shallow resistivity |
| RMED | Medium resistivity |
| RDEP | Deep resistivity |
| RHOB | Bulk density |
| GR | Gamma ray |
| SGR | Spectral gamma ray |
| NPHI | Neutron porosity |
| PEF | Photoelectric factor |
| DTC | Compressional sonic log |
| SP | Spontaneous potential |
| BS | Bit size |
| ROP | Rate of penetration |
| DTS | Shear sonic log |
| DCAL | Differential caliper |
| DRHO | Density correction |
| MUDWEIGHT | Mud density |
| RMIC | Micro resistivity |
| ROPA | Processed resistivity |
| RXO | Flushed zone resistivity |

---

# Lithology Classes

| Code | Lithology |
|---|---|
| 30000 | Sandstone |
| 65030 | Sandstone/Shale |
| 65000 | Shale |
| 80000 | Marl |
| 74000 | Dolomite |
| 70000 | Limestone |
| 70032 | Chalk |
| 88000 | Halite |
| 86000 | Anhydrite |
| 99000 | Tuff |
| 90000 | Coal |
| 93000 | Basement |

---
