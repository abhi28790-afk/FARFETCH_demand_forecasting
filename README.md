# Store-Item Demand Forecasting — FARFETCH Business Case

**Author:** Abhishek Singh  
**Submitted:** April 2026  
**Dataset:** [Store-Item Demand Forecasting — Kaggle](https://www.kaggle.com/datasets/dhrubangtalukdar/store-item-demand-forecasting-dataset/data)

---

## 📌 Project Overview

This project tackles a real-world retail forecasting challenge: **predicting the next 90 days of daily sales** across multiple stores and items using 5 years of historical data.

The analysis covers the full data science pipeline – from exploratory analysis to a production-ready XGBoost forecasting model – and translates results into **strategic business recommendations** relevant to a luxury fashion retailer like FARFETCH.

---

## 🎯 Objectives Covered

| Objective | Status |
|---|---|
| EDA — 3 key sales insights | ✅ |
| Forecasting model with justification | ✅ |
| Baseline comparison (same day last year) | ✅ |
| Business & operational impact | ✅ |
| CFO advisory & risk analysis | ✅ |
| Data Science team next steps | ✅ |
| Bonus — Price elasticity per item | ✅ |

---

## 📁 Repository Structure

```
farfetch-demand-forecasting/
│
├── store_item_demand_forecasting.ipynb   ← Main notebook (run this)
├── requirements.txt                      ← Python dependencies
└── README.md                             ← You are here
```

> ⚠️ **Note:** `retail_sales.csv` is not included in this repository as it is a Kaggle dataset.  
> Please download it from the link above and place it in the **same folder** as the notebook before running.

---

## ⚙️ How to Run

**Step 1 — Clone the repository**
```bash
git clone https://github.com/abhi28790-afk/FARFETCH_demand_forecasting.git
cd farfetch-demand-forecasting
```

**Step 2 — Install dependencies**
```bash
pip install -r requirements.txt
```

**Step 3 — Add the dataset**  
Download `retail_sales.csv` from [Kaggle](https://www.kaggle.com/datasets/dhrubangtalukdar/store-item-demand-forecasting-dataset/data) and place it in the root folder.

**Step 4 — Run the notebook**  
Open Jupyter and run `store_item_demand_forecasting.ipynb` from top to bottom.

---

## 🔍 Key Insights (EDA)

**Insight 1 — Upward Sales Trend**  
Total daily sales show a consistent upward trend over 5 years with clear annual seasonality peaks, likely driven by holiday and end-of-season cycles.

**Insight 2 — Promotional Lift**  
Promotional periods drive a measurable average sales lift. However, high variance across items suggests promo effectiveness is not uniform — critical intelligence for a luxury brand where indiscriminate discounting can damage brand equity.

**Insight 3 — Weekly & Monthly Seasonality**  
Sales follow a strong day-of-week pattern with weekend peaks and a monthly cycle with Q4 as the strongest period. These patterns are essential for weekly inventory and staffing planning.

---

## 🤖 Model — XGBoost Regressor

**Why XGBoost?**
- Handles mixed feature types (categorical IDs, continuous lags) natively
- Captures non-linear interactions (e.g. promo × seasonality)
- Scales efficiently across many store–item combinations
- Provides feature importance for business explainability
- Consistently outperforms linear models on tabular retail data

**Features Used**

| Category | Features |
|---|---|
| Identity | `store_id`, `item_id` |
| Price & Promo | `price`, `promo`, `price_change`, `promo_lag_1` |
| Time | `weekday`, `month`, `quarter`, `day of year`, `is_weekend` |
| Lag | `lag_1`, `lag_7`, `lag_30` |
| Rolling | `rolling_mean_7`, `rolling_mean_30`, `rolling_std_7` |

**Validation:** 5-fold walk-forward cross-validation (TimeSeriesSplit) — no data leakage.

---

## 📊 Model Performance

| Metric | Baseline (Last Year) | XGBoost | Improvement |
|---|---|---|---|
| MAE | 6.70 | 2.55 | 61.9% |
| RMSE | 9.40 | 3.21 | 65.9% |
| MAPE | 22.99% | 10.89% | 52.6% |


---

## 📈 Output Files Generated

After running the notebook, these files are saved automatically:

| File | Description |
|---|---|
| `test_predictions.csv` | Actual vs predicted sales on test set |
| `future_3months_forecast.csv` | 90-day forward forecast per store & item |
| `price_elasticity_by_item.csv` | Price elasticity coefficient per item |
| `eda_1_sales_trend.png` | Sales trend chart |
| `eda_2_promo_impact.png` | Promo impact chart |
| `eda_3_seasonality.png` | Seasonality chart |
| `eval_actual_vs_predicted.png` | Model evaluation chart |
| `eval_feature_importance.png` | Feature importance chart |
| `forecast_90days.png` | 90-day forecast chart |
| `elasticity_distribution.png` | Elasticity distribution chart |

---

## 💼 Business Impact

**Operational Value**
- More accurate demand forecasts reduce over-stocking costs and mark-down risk — critical for protecting luxury fashion margins
- 3-month horizon enables proactive CAPEX, logistics, and procurement planning
- Promo lift quantification supports ROI evaluation of marketing campaigns

**CFO Advisory**

| | Detail |
|---|---|
| 🟢 Opportunity 1 | Model-driven reorder triggers can cut excess inventory 10–15% |
| 🟢 Opportunity 2 | Inelastic items identified via elasticity analysis offer margin expansion potential |
| 🟢 Opportunity 3 | Price-sensitive items can absorb targeted promotions without brand equity risk |
| 🔴 Risk 1 | Model accuracy degrades under macro shocks (inflation, demand disruption) |
| 🔴 Risk 2 | New product lines lack historical lag features — cold-start problem |
| 🔴 Risk 3 | Static promo assumptions in the 90-day forecast require campaign injection |

---

## 🔬 Price Elasticity (Bonus)

Per-item log-log OLS regression identifies:
- **Elastic items** (|elasticity| > 1): demand drops sharply with price increases — handle promotions carefully
- **Inelastic items** (|elasticity| < 1): demand is stable despite price increases — opportunity to protect or grow margin

Results saved in `price_elasticity_by_item.csv`.

---

## 🚀 Next Steps for the Data Science Team

1. **External signals** — integrate fashion-week calendars, social media trends, competitor pricing, and macroeconomic indicators
2. **Per store/item tuning** — hyper-parameter optimization at a more granular level
3. **Deep learning** — explore Temporal Fusion Transformer (TFT) or N-BEATS for long-horizon accuracy
4. **Real-time retraining** — build a weekly automated retraining pipeline
5. **A/B testing** — validate promotional recommendations against model lift estimates in production

---

## 🛠️ Dependencies

```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
statsmodels
```

Install all with:
```bash
pip install -r requirements.txt
```

---

*Submitted as part of the FARFETCH Data Science interview process — April 2026*
