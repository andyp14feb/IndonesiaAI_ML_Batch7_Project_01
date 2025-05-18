# 🧠 Project Summary: Time Series Sales Forecasting (Grocery I)

## 🎯 Project Objective

Forecast daily sales for the **GROCERY I** category in store number 5 using:

* **VARMAX** (for multivariate relationships)
* **LSTM** (for capturing complex sequential patterns)

---

## 🔄 Step-by-Step Workflow

### 1️⃣ Project Setup

* Defined forecasting target and approach (multivariate + deep learning).
* Evaluation metrics: RMSE, MAE, MAPE.

---

### 2️⃣ Load & Explore Data

* Loaded `store5.csv`.
* Explored data structure (`date`, `sales`, `onpromotion`, `dcoilwtico`).
* Filtered to focus only on `GROCERY I` family.

---

### 3️⃣ Feature Engineering

* Created time-based features: `weekdays`, `week_of_the_year`, `month`, `day`.
* Created flags for:

  * `isHoliday` (Dec 25 & Jan 1)
  * `isWeekend` (Saturday & Sunday)
* Detected and inserted missing dates (Dec 25 each year).

---

### 4️⃣ Handle Missing Values

* `dcoilwtico` had many missing values (esp. on weekends).
* Used linear interpolation → created `oil_fill`.
* Applied backfill to handle edge NaNs.

---

### 5️⃣ Exploratory Data Analysis (EDA)

* Visualized trends: `sales`, `onpromotion`, `oil price`.
* Generated heatmaps and scatter plots.
* Measured correlations:

  * `sales` vs `onpromotion`: weak positive
  * `sales` vs `isWeekend`: moderately strong positive
  * `sales` vs `oil_fill`: very weak

---

### 6️⃣ Stationarity Check

* ADF test:

  * Stationary: `sales`, `isWeekend`, `isHoliday`
  * Non-stationary: `onpromotion`, `oil_fill` → differencing applied

---

### 7️⃣ Differencing Transformation

* Created:

  * `onpromotion_differencing`
  * `oil_fill_differencing`
* Ensured all time series variables were stationary for modeling.

---

### 8️⃣ Seasonality Analysis

* Visual inspection using:

  * ACF/PACF plots
  * Seasonal Decomposition (weekly and yearly)
* Identified weekly seasonality, especially on weekends.

---

### 9️⃣ VARMAX Modeling

* Input: `sales` and `onpromotion_differencing`
* Exogenous: `oil_fill_differencing`, `isWeekend`, `isHoliday`
* Grid search for optimal (p, q)
* Evaluation:

  * **RMSE Sales**: 603.96
  * **RMSE Onpromotion**: 28.29
  * **Weighted RMSE**: 0.6587

---

### 🔟 LSTM Modeling

* Input sequence: 7-day window
* Network: LSTM(64) → Dropout → LSTM(32) → Dropout → Dense(2)
* Used `MinMaxScaler`
* Trained with EarlyStopping
* Evaluation:

  * **RMSE Sales**: 597.98
  * **RMSE Onpromotion**: 16.76
  * **Weighted RMSE**: 0.4663

---

## 🥊 Model Comparison

| Metric            | VARMAX | LSTM         |
| ----------------- | ------ | ------------ |
| RMSE (Sales)      | 603.96 | 597.98       |
| RMSE (Promotion)  | 28.29  | 16.76        |
| **Weighted RMSE** | 0.6587 | **0.4663** ✅ |

➡️ **Conclusion**: **LSTM outperforms VARMAX** in all metrics and is recommended for future forecasting use.

---

## ✅ Final Insights

* Sales are **significantly higher on weekends**.
* No sales occur on major holidays (Dec 25 & Jan 1).
* Oil prices show very little direct impact on sales.
* LSTM is more effective in learning temporal dynamics.
* VARMAX is still useful for interpretable insights and policy simulations.


