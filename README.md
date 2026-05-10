# 🪨 Lithology Classification from Well Logs

## 📌 Overview

Determining lithology (rock type) from well logs is a fundamental task in petroleum geoscience. Traditionally done by hand by petrophysicists, this project automates that process using **Random Forest** and **XGBoost** classifiers trained on wireline log measurements.

The model learns to distinguish 11 distinct lithofacies — from Sandstone to Halite — directly from physical log readings, without ever looking at a core sample.

This project was inspired and guided by the tutorials of [Ahmed Abd Elgawad](https://www.youtube.com/@AhmedAbdElgawad-petroAnalyst) on the **Petro Analyst** YouTube channel, which covers applied machine learning for petroleum engineering and geoscience workflows.

---

## 🎯 Problem Statement

Given a set of geophysical measurements taken at depth inside a borehole, predict which **rock type (lithology)** exists at each depth level.

| Input | Output |
|---|---|
| Wireline log curves (GR, RHOB, NPHI, etc.) | Rock class label (e.g., Sandstone, Shale, Limestone) |

**Why it matters:** Manual lithology interpretation is time-consuming, subjective, and requires senior expertise. A reliable ML model accelerates petrophysical workflows and supports real-time geosteering decisions.

---

## 🗂️ Dataset

**Source:** [FORCE 2020 Machine Learning Competition](https://github.com/bolgebrygg/Force-2020-Machine-Learning-competition) — a public benchmark dataset from the Norwegian Continental Shelf.

**File used:** `15_9-23.csv` (a single well)

### Target Classes (Lithofacies)

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

---

## 🔬 Features Used

| Log Curve | Full Name | What it measures |
|---|---|---|
| `CALI` | Caliper | Borehole diameter — detects washouts |
| `RDEP` | Deep Resistivity | Fluid content (oil vs brine) |
| `RHOB` | Bulk Density | Rock + fluid density |
| `GR` | Gamma Ray | Clay/shale content |
| `NPHI` | Neutron Porosity | Porosity indicator |
| `PEF` | Photoelectric Factor | Mineralogy proxy |
| `DTC` | Compressional Slowness | Sonic velocity of formation |

---

## ⚙️ Workflow

```
Raw CSV
   │
   ▼
[1] Feature Selection
    → Keep 7 log curves + depth + well + group + lithology label
   │
   ▼
[2] Label Mapping
    → Numeric FORCE codes → human-readable lithology names
   │
   ▼
[3] Missing Value Imputation
    → KNNImputer (k=5) on RHOB, NPHI, PEF, DTC
   │
   ▼
[4] EDA
    → Class distribution bar plot
   │
   ▼
[5] Train / Test Split
    → 70/30 stratified split (preserves class ratios)
    → Label Encoding for XGBoost compatibility
   │
   ▼
[6] Baseline Modeling
    → Random Forest (300 trees)
    → XGBoost (300 estimators, lr=0.05)
    → Accuracy + Classification Report + Confusion Matrix
   │
   ▼
[7] Hyperparameter Tuning
    → GridSearchCV (5-fold CV, f1_weighted scoring)
    → RF: n_estimators, max_depth, min_samples_split
    → XGB: n_estimators, learning_rate, max_depth, subsample
   │
   ▼
[8] Class Imbalance Handling
    → compute_class_weight('balanced')
    → Re-train both models with class weights applied
   │
   ▼
Final Evaluation
    → Accuracy, F1-weighted, Confusion Matrix per model
```

> **Note:** Update the data path on the `pd.read_csv()` line to match your local file location.

---

## 🧠 Modeling Approach

### Why These Two Models?

**Random Forest** handles mixed-type features well, is robust to outliers (common in well log data due to borehole conditions), and requires minimal preprocessing. It also outputs native class probabilities.

**XGBoost** tends to outperform RF on structured tabular data with class imbalance when tuned properly. It requires label encoding but offers better gradient-based optimization.

### Handling Class Imbalance

Lithology datasets are inherently imbalanced — Shale and Sandstone dominate, while Coal and Tuff are rare. Two strategies are applied:

1. **Stratified train/test split** — preserves class ratios across splits.
2. **`class_weight='balanced'`** — penalizes misclassification of minority classes more heavily.

### Evaluation Metric

`f1_weighted` is used for GridSearchCV scoring instead of raw accuracy, because accuracy is misleading on imbalanced datasets — a model predicting "Shale" for everything can still score high on accuracy.

---

## 📊 Key Results (Expected)

| Model | Baseline Train Acc | Baseline Test Acc |
|---|---|---|
| Random Forest | ~0.99 | ~0.85–0.90 |
| XGBoost | ~0.95 | ~0.84–0.88 |

> Exact numbers depend on random state and hyperparameter search results. Run the notebook to reproduce.

---

## ⚠️ Known Limitations

- **Single well only** — model may not generalize to wells with different geological settings or logging tools without retraining.
- **No depth-sequence modeling** — each depth sample is treated independently. A 1D-CNN or LSTM over depth sequences would capture lithological transitions better.
- **KNN imputation on logs** — assumes spatial similarity across rows; imputed values may not honor geological contacts.
- **No feature engineering** — derived features like `ΔGR`, `RHOB-NPHI crossplot`, or `M-N plot` proxies could improve discrimination of carbonates from clastics.

---

## 📚 References

- [FORCE 2020 Competition](https://github.com/bolgebrygg/Force-2020-Machine-Learning-competition)
- Bormann, P. et al. (2020). *FORCE Machine Learning Competition Dataset*. Zenodo.
- Breiman, L. (2001). *Random Forests.* Machine Learning, 45(1), 5–32.
- Chen, T., & Guestrin, C. (2016). *XGBoost: A Scalable Tree Boosting System.* KDD '16.

---

## Acknowledgements

- **Ahmed Abd Elgawad — [Petro Analyst](https://www.youtube.com/@AhmedAbdElgawad-petroAnalyst):** The concept and approach for this project were inspired by his YouTube channel, which provides practical, applied tutorials on machine learning for petroleum engineers and geoscientists. Highly recommended for anyone working at the intersection of data science and the energy industry.

- **FORCE 2020 Machine Learning Competition:** Dataset and competition framework provided by the Norwegian petroleum directorate and Equinor. See the [official repository](https://github.com/bolgebrygg/Force-2020-Machine-Learning-competition) for full dataset terms and citation requirements.

---

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change. Suggestions for additional models (LightGBM, 1D-CNN) or multi-well generalization are especially appreciated.

---
## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

The FORCE 2020 dataset has its own terms of use. Please refer to the [competition repository](https://github.com/bolgebrygg/Force-2020-Machine-Learning-competition) before using the data in any published work.


## 📄 License

MIT License — see `LICENSE` for details.
