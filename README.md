# ⚡ Short-Term Electricity Load Forecasting with XAI
### Panama Power System | 2015–2020

> **Summer Internship Project** — National School of Computer Science (ENSI), University of Manouba  
> Supervised by **Dr. Marouene Chaieb** (ENSI) & **Nabil Omri** (CapGemini France)

---

## 📌 Overview

This project addresses the **Short-Term Load Forecasting (STLF)** problem applied to the Panama power system, with a forecasting horizon of **one week (168 hours)** at hourly resolution.

The core contribution is the integration of **Explainable Artificial Intelligence (XAI)** — specifically **SHAP (SHapley Additive exPlanations)** — into the forecasting pipeline to:
- Interpret model predictions and identify the most influential features
- Perform explainability-driven feature selection
- Improve model transparency without sacrificing predictive accuracy

---

## 🗂️ Repository Structure

```
stlf-panama-xai/
│
├── notebooks/
│   ├── 01_xgboost_model.ipynb                  # Best performing model (XGBoost + SHAP)
│   ├── 02_cnn_model.ipynb                       # Multi-headed CNN architecture
│   ├── 03_hybrid_xgboost_randomforest.ipynb     # Hybrid XGBoost + Random Forest
│
│
├── data/
│   └── README.md                                # Dataset source and description
│
├── results/
│   ├── shap_summary_plot.png                    # SHAP feature importance
│   ├── forecast_comparison.png                  # Forecast vs real demand
│   └── model_performance.png                    # Before/after feature selection
│
├── requirements.txt
└── README.md
```

---

## 📊 Dataset

**Short-Term Electricity Load Forecasting — Panama**  
Source: [Kaggle Dataset](https://www.kaggle.com/datasets/ernestojaguilar/shortterm-electricity-load-forecasting-panama)

| Property | Details |
|---|---|
| Period | January 2015 – June 2020 |
| Granularity | Hourly |
| Records | 110,000+ observations |
| Target | Electricity load (MWh) |
| Features | Temperature, humidity, wind speed, solar radiation, holidays, calendar variables, lagged load |

> ⚠️ The raw dataset is not included in this repository due to its size. Please download it directly from Kaggle using the link above.

---

## 🧠 Models Explored

| Model | Test RMSE (MWh) | Test MAPE (%) |
|---|---|---|
| KNN | 74.16 | 5.17 |
| Hybrid (XGBoost + RF) | ~60.0 | 4.09 |
| Multi-headed CNN | 90.36 | 5.36 |
| **XGBoost (full features)** | **53.27** | **3.56** |
| XGBoost (SHAP-selected features) | 54.15 | 3.61 |

✅ **XGBoost** was selected as the final model based on its superior predictive performance and compatibility with SHAP-based interpretability.

---

## 🔍 XAI Pipeline

The explainability pipeline follows three phases:

1. **Model Inspection** — SHAP values computed on the test set to rank features by their average absolute impact on predictions
2. **Feature Engineering** — Low-impact features (contributing <6% of total SHAP impact) are aggregated into composite variables rather than simply dropped
3. **Retraining** — The model is retrained on the refined feature set and re-evaluated

Key features identified by SHAP:
- `MA_X-4` (4-week moving average of load) — strongest predictor
- `holiday` and `Holiday_ID`
- `T2M_toc` (temperature)
- `week_X-2`, `week_X-4` (lagged weekly load)
- `dayOfWeek`, `hourOfDay`, `weekend`

---

## ⚙️ Methodology

- **Preprocessing**: datetime construction, outlier detection (±3σ threshold), linear interpolation, Min-Max normalization, rolling window train/test split
- **Train/Test Split**: rolling window — 4 weeks training (672h) → 1 week testing (168h), with a 72-hour gap between sets
- **Hyperparameter Tuning**: Optuna framework with Bayesian optimization (TPE sampler)
- **Evaluation Metrics**: RMSE, MAPE, MAE

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python | Core language |
| XGBoost | Primary forecasting model |
| SHAP | Explainability and feature selection |
| Optuna | Hyperparameter tuning |
| scikit-learn | Metrics and preprocessing |
| pandas / NumPy | Data manipulation |
| Matplotlib | Visualization |
| Jupyter Notebook | Development environment |

---

## 👩‍💻 Authors

| Name | Institution |
|---|---|
| Chaima Zoghlami | ENSI, University of Manouba |
| Chadha Ben Said | ENSI, University of Manouba |
| Asma Mhamdi | ENSI, University of Manouba |

**Academic Supervisor**: Dr. Marouene Chaieb (ENSI)  
**Industry Supervisor**: Nabil Omri (CapGemini France)  
**Academic Year**: 2025/2026

---

## 📚 Key References

- Aguilar Madrid & Antonio (2021) — *Short-Term Electricity Load Forecasting with Machine Learning* — [DOI](https://doi.org/10.3390/info12020050)
- Van Zyl et al. (2024) — *Harnessing XAI for Feature Selection in Time Series Energy Forecasting*
- Yang et al. (2025) — *An Informer Model for Very Short-Term Power Load Forecasting*
