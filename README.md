# Predictive_Paradoc_Nay-Dosi



## 📌 Project Overview
This repository contains a production-grade machine learning pipeline designed to predict hourly electricity demand. 

By integrating weather patterns, economic indicators, and cyclical time features, this model achieves a **MAPE (Mean Absolute Percentage Error) of 1.84%** on a blind 2024 test set.

---

## 🛠 Data Reconstruction: The Three-Tier Strategy
To handle over 1,000 hours of missing data and extreme sensor outliers (spikes reaching 160,000 MW), I implemented a specialized reconstruction framework:

1. **The Island Strategy (Temporal Segmentation):**
   Segmented the dataset into continuous "Islands of Truth." This ensured that gaps were filled using context from the correct seasonal and operational periods rather than a global average.
2. **KNN Imputation (Multivariate Similarity):**
   For medium-sized gaps, K-Nearest Neighbors was utilized to find the most similar historical hours based on **Temperature** and **Relative Humidity**, preserving weather-driven demand spikes.
3. **Seasonal & Economic Imputation:**
   For large gaps, a seasonal profile (Month/Day/Hour) was reconstructed and adjusted by a yearly growth factor (GDP and Population) to reflect the increasing "floor" of energy consumption over the 9-year span.

---

## 🚀 Feature Engineering
To fulfill the requirement of predicting the **next hour's demand**, the model utilizes features strictly available up to the current hour ($T$):

* **Cyclical Encoding:** Hours, days, and months transformed into $Sine$ and $Cosine$ waves to maintain temporal continuity.
* **Momentum Lags:** Inclusion of `demand_lag_1h` and `demand_lag_24h` to capture immediate grid pressure and daily habits.
* **Weather Stress:** Real-time temperature and humidity tracking.
* **Economic Baseline:** Annual GDP and Population data providing long-term growth trends.

---

## 🤖 Model & Validation Firewall
To ensure the model is truly forecasting and not simply "remembering" the past:
* **Temporal Split:** A hard chronological cut was used. The model trained on data from **2015–2023** and was tested on a strictly blind **2024** dataset.
* **Scale Isolation:** The `StandardScaler` was fitted *only* on training data to prevent the statistics of the "future" from leaking into the training phase.

### Final Performance (XGBoost)
The model was evaluated using the Mean Absolute Percentage Error:

$$MAPE = \frac{100\%}{n} \sum_{t=1}^{n} \left| \frac{Actual_t - Forecast_t}{Actual_t} \right|$$

| Metric | Value |
| :--- | :--- |
| **Test MAPE (2024)** | **1.84%** |
| **Model Accuracy** | **98.16%** |

---

## 💻 How to Run
1. There is a jupyter notebook . All the cells have been already executed and the graphs have been plotted . If you want to rerun the file first load your data sets using the first cell.
