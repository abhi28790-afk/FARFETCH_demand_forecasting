# 🛍️ Store-Item Demand Forecasting — FARFETCH Business Case

**Author:** Abhisheak Singh
**Submitted:** April 2026
**Dataset:** [Store-Item Demand Forecasting — Kaggle](https://www.kaggle.com/datasets/dhrubangtalukdar/store-item-demand-forecasting-dataset/data)

---

## 📌 Project Overview

This project tackles a real-world retail forecasting challenge: **predicting the next 90 days of daily sales** across multiple stores and items using 5 years of historical data.

The analysis covers the full data science pipeline - from exploratory analysis to a production-ready XGBoost forecasting model and translates results into **strategic business recommendations** relevant to a luxury fashion retailer like FARFETCH.

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

> ⚠️ **Note:** `retail_sales.csv` is not included as it is a Kaggle dataset.
> Download from the link above and place it in the same folder as the notebook.

---

## ⚙️ How to Run

**Step 1 — Clone the repository**
```bash
git clone https://github.com/abhi28790-afk/FARFETCH_demand_forecasting.git
cd FARFETCH_demand_forecasting
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

**Insight 1 — Upward Trend with Annual Spring Peak**
Total daily sales show a consistent upward trend of **+5.9 units/day** over 5 years. There is **one annual seasonal peak per year around May-June**, followed by a winter trough in January-February. Each year's peak grows higher than the last, confirming consistent underlying demand growth. Finance projections must account for this spring peak cycle and pre-position inventory by February.

**Insight 2 — Promotional Timing Matters More Than Volume**
Promotional periods drive a **50.2% average sales lift** (promo median ~40 units vs no-promo ~25 units). Critically, the promo gap is **widest in Feb-May (~17-18 unit difference)** — the most cost-effective promotional window. The gap **narrows sharply in Aug-Oct (~8-10 units)** — summer promotions are least effective and should be deprioritised. Outliers reaching **140 units** reveal certain items are hyper-responsive. Strategy: concentrate all promotional budget in the **Feb-May window only**.

**Insight 3 — Mid-Week Leads; Mar-Apr is Peak Month**
Mid-week dominates demand — **Wednesday peaks at ~35 units, Tuesday ~34, Thursday ~32**. Weekend is the weakest window with **Saturday ~23.5 and Sunday ~24.8 units**. Weekday average is 31.3 units vs weekend average 24.2 units. Monthly peak falls in **Mar-Apr (~37 units)**, trough in **Sep-Oct (~21 units)** — a **43% seasonal drop**. Inventory must be pre-positioned by February. September-October is the optimal window for lean inventory runs.

---

## 🤖 Model — XGBoost Regressor

**Why XGBoost?**
- Handles mixed feature types (categorical IDs, continuous lags) natively
- Captures non-linear interactions (e.g. promo × seasonality)
- Scales efficiently across all store-item combinations in one model
- Provides feature importance for business explainability
- Consistently outperforms linear models on tabular retail data

**Features Used**

| Category | Features |
|---|---|
| Identity | `store_id`, `item_id` |
| Price & Promo | `price`, `promo`, `price_change`, `promo_lag_1` |
| Time | `weekday`, `month`, `quarter`, `dayofyear`, `is_weekend` |
| Lag | `lag_1`, `lag_7`, `lag_30` |
| Rolling | `rolling_mean_7`, `rolling_mean_30`, `rolling_std_7` |

**Validation:** 5-fold walk-forward cross-validation (TimeSeriesSplit) — no data leakage.

**Top-3 Most Important Features**

| Rank | Feature | Why It Matters |
|---|---|---|
| #1 | `lag_7` | Same day last week — weekly seasonality is the dominant signal |
| #2 | `promo` | Directly confirms the 50.2% promotional lift found in EDA |
| #3 | `rolling_mean_7` | Recent 7-day sales trajectory captures short-term momentum |

---

## 📊 Model Performance

| Metric | Baseline (Last Year) | XGBoost | Improvement |
|---|---|---|---|
| MAE | 6.70 | 2.55 | 61.9% |
| RMSE | 9.40 | 3.21 | 65.9% |
| MAPE | 22.99% | 10.89% | 52.6% |

**CV Mean MAE: 2.57 ± 0.06** — low variance across all 5 folds confirms model stability and no overfitting.

---

## 📈 Output Files Generated

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
- More accurate forecasts reduce over-stocking costs and markdown risk — critical for protecting luxury fashion margins
- 3-month horizon enables proactive CAPEX, logistics, and procurement planning
- Promo timing analysis (Feb-May optimal window) supports ROI-maximising campaign planning
- Spring peak cycle confirmed — pre-positioning inventory by February prevents stock-outs

**CFO Advisory**

| | Detail |
|---|---|
| 🟢 Opportunity 1 | Model-driven reorder triggers can cut excess inventory 10–15% |
| 🟢 Opportunity 2 | Elasticity map protects margin on inelastic luxury items |
| 🟢 Opportunity 3 | Concentrate promotional budget in Feb-May for maximum ROI |
| 🟢 Opportunity 4 | +5.9 units/day confirmed growth supports confident revenue projections |
| 🔴 Risk 1 | Model degrades under macro shocks (inflation, luxury demand slowdown) |
| 🔴 Risk 2 | New product lines lack lag history — cold-start problem |
| 🔴 Risk 3 | Static promo assumptions in 90-day forecast require campaign injection |
| 🔴 Risk 4 | Annual spring peak — missing February pre-positioning causes stock-outs |

---

## 🔬 Price Elasticity (Bonus)

Per-item log-log OLS regression with price variance filtering:
- **No strongly elastic items detected** — consistent with premium/luxury product positioning where customers are relatively price-insensitive
- **All items appear inelastic** (|elasticity| < 1) — demand is stable despite price changes, representing a **pricing power opportunity** for the CFO
- **Important caveat:** Limited price variation in this dataset constrains OLS detection. Results should be validated with a randomised pricing experiment in production for robust estimates

Results saved in `price_elasticity_by_item.csv`.

---

## 🚀 Next Steps for the Data Science Team

1. **Hierarchical forecasting** — reconcile item → category → company totals so all teams plan from consistent numbers
2. **External signals** — fashion-week calendars, social media trends, competitor pricing, macroeconomic indicators
3. **Per store/item tuning** — hyper-parameter optimisation at a more granular level
4. **Deep learning** — Temporal Fusion Transformer (TFT) for long-horizon accuracy with uncertainty quantification
5. **Real-time retraining** — weekly automated retraining pipeline with drift detection
6. **A/B testing** — validate promotional recommendations against model lift estimates in production

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

*Submitted as part of the FARFETCH Data Science interview process - April 2026*
