# ⚡ Multi-Paradigm Hourly Energy Consumption Forecasting (PJM East Benchmarking) 

This project provides a comprehensive multi-paradigm benchmark for multi-step time series forecasting, predicting **24 hours** of hourly electricity demand (MW) on the **PJM East (PJME)** grid using **96 hours** of historical context.

We design, train, and evaluate **9 distinct models** spanning 4 architectural paradigms on unified data pipelines, demonstrating how modern State Space Models (SSMs) and Transformers compare against traditional deep learning and machine learning approaches.

---

## 📊 Official Benchmark Evaluation Table
All models were evaluated on the test set using standard paper-grade metrics: Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and Mean Absolute Percentage Error (MAPE).

| Rank | Paradigm | Model | MAE (MW) | RMSE (MW) | MAPE (%) | Train Time (s) |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: |
| **1** | **State Space Model (SSM)** | **Mamba (State Space)** | **1347.30** | **1951.25** | **4.15%** | ~1467s |
| **2** | **Transformer Family** | Transformer (Encoder-Only) | 1350.88 | 1951.84 | 4.17% | ~861s |
| **3** | **Traditional DL** | 1D-CNN | 1373.58 | 2000.38 | 4.23% | **~48s** |
| **4** | **Transformer Family** | Transformer (Decoder-Only) | 1425.66 | 2023.60 | 4.40% | ~855s |
| **5** | **Traditional DL** | LSTM | 1426.20 | 2054.49 | 4.40% | ~300s |
| **6** | **Traditional DL** | 1D U-Net | 1436.36 | 2037.87 | 4.51% | ~96s |
| **7** | **Traditional ML** | LightGBM | 1728.63 | 2396.48 | 5.58% | ~97s |
| **8** | **Traditional ML** | XGBoost | 1734.09 | 2427.88 | 5.58% | ~65s |
| **9** | **Traditional ML** | Random Forest | 2020.25 | 2681.98 | 6.59% | **~21s** |

---

## 💡 Key Insights & Outcomes

### 1. The SSM and Transformer Breakthrough
* **Mamba (State Space Model)** achieves the **lowest overall prediction error** with an MAE of **1347.30 MW** (MAPE of **4.15%**).
* The **Encoder-Only Transformer** follows extremely closely behind with an MAE of **1350.88 MW** (MAPE of **4.17%**).
* These results show that architectures capable of long-range, high-capacity sequence modeling capture complex daily and weekly grid demand cycles much better than standard models.

### 2. The Efficiency Champion: 1D-CNN
* **1D-CNN** represents the absolute sweet spot for cost-to-performance ratio. It achieved an exceptional **1373.58 MW** MAE while completing training in just **47.7 seconds** (nearly **30x faster** than Mamba).
* It easily outperformed more complex architectures like LSTM, 1D U-Net, and Decoder-Only Transformers while using a fraction of the computational footprint.

### 3. ML vs. DL Paradigm Split
* Deep Learning approaches (SSM, Transformers, CNNs, LSTMs) dominate Traditional Machine Learning tabular models by a margin of **300 to 700 MW** in MAE.
* Among Traditional ML baselines, **LightGBM** and **XGBoost** show virtually identical predictive performance (~1730 MW MAE), but LightGBM scales slightly better on sequence patterns with custom limits.

---

## 🛠️ Project Pipeline Structure
1. **Data Ingestion & Cleaning**: Direct streaming of the 145k hourly energy records. Chronological sorting, deduplication, strict hour-frequency alignment, and forward/backward imputation.
2. **Feature Engineering**: Cyclical time features extracted via sine/cosine projections of daily (24h), weekly (7d), and seasonal (365.25d) cycles.
3. **Scaling & Splitting**: Rigorous chronological train/val/test splitting (70/10/20) with separate `StandardScaler` fits on features and targets to completely avoid data leakage.
4. **Sliding Windows**: Standardized sliding window generator projecting $T = 96$ input steps to a forecast horizon of $H = 24$ steps.
