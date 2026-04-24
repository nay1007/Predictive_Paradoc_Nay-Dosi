# Predictive Paradox: Short-Term Hourly Power Demand Forecasting



## 🛠 1. Data Wrangling & Quality Assurance
The raw dataset required extensive cleaning to handle sensor errors and interruptions:
* **Outlier Correction:** Identified and capped "impossible" sensor spikes (reaching 160,000 MW) to a realistic grid capacity of 15,000 MW.
* **Temporal Reconstruction:** Employed a multi-tier strategy (including **Island Strategy** and **KNN Imputation**) to fill over 1,000 missing hourly blocks, creating a continuous timeline.
* ** Integration:** Merged three distinct data streams:
    * **Hourly Demand Logs:** Historical power usage.
    * **Weather Data:** Temperature and humidity.
    * **Annual Economic Indicators:** GDP and Population growth.

---

## 🧬 2. Feature Engineering: The "Predictive Clues"
* **Cyclical Time Features:** Applied Sine/Cosine transformations to hour and month data to preserve temporal continuity.
* **Memory Features (Lags):** Introduced `Lag_1h` and `Lag_24h` to capture immediate momentum and daily rhythms.
* **Rolling Statistics:** Calculated rolling averages to provide the model with context on recent grid volatility.

---

## 🛡 3. The "Anti-Cheating" Protocol
To ensure the model is truly forecasting and not simply "remembering" the data:
* **Temporal Splitting:** Used a hard chronological cut—training on **2015–2023** and testing on a completely blind **2024** dataset.
* **Scale Isolation:** The `StandardScaler` was fitted strictly on training data to prevent future statistics from leaking into the past.

---

## 📈 4. Forecasting Strategy
The model is configured for **$T+1$ Forecasting**:
* **Input ($X$):** Current weather, current economics, and demand at hour $T$.
* **Output ($Y$):** The predicted demand for the next hour ($T+1$).

---

## 🏆 5. Results & Performance
The **XGBoost Regressor** emerged as the optimal model for this task.

| Metric | Value |
| :--- | :--- |
| **Test MAPE (2024)** | **1.84%** |
| **Model Accuracy** | **98.16%** |

**Interpretation:** Feature Importance confirms the model relies most heavily on **Short-term Lags** for momentum and **Temperature** for peak demand spikes.
The MAPE error of 1.86 is considered good for power prediction as it can be used to cut down on the power supply, avoid wastage of power and avoid shortages.
