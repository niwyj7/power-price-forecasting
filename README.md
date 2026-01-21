# power-price-forecasting

---

## 🚀 Model Architecture: DLNet (Spatial-Temporal Forecasting)

This repository implements a **Spatial-Temporal DLinear** model for 15-minute energy forecasting. It maps multi-node weather features to a single regional target (e.g., Grid Load/Price).

### 1. Data Pipeline

* **Resolution:** 15-minute frequency via **Cubic Interpolation**.
* **Spatial Mapping:** Extracts lat/lon coordinates into a ranked grid of geographical nodes.
* **Feature Engineering:** Includes wind-cube physics, time-of-day encoding, and log-transforms for variance stabilization.

### 2. Temporal Logic (The DLinear Backbone)

The model processes a **6-hour lookback window** () including the current forecast point ().

* **Series Decomposition (Kernel 7):** Uses a moving average to split signals into **Trend** and **Seasonal** components.
* **Why Trend matters?** It captures the "inertia" of weather patterns (e.g., steady warming), ensuring the model isn't misled by instantaneous noise while predicting "now."

### 3. Spatial Aggregation

* **Node Fusion:** A specialized `fcn_graph` layer aggregates features from all geographical grid points.
* **Sparse Learning:** Applied **L1 Regularization** on spatial weights to automatically identify and prioritize the most influential weather stations/grid nodes.

### 4. Training Strategy

* **Rolling Training:** The model retrains on a **24-day sliding window** to adapt to seasonal shifts.
* **Loss Function:** MSE with L1 Sparsity penalty for robust spatial-temporal feature selection.

---

