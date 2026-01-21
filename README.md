# power-price-forecasting


---

# Spatial-Temporal Energy Forecasting with DLNet

This project implements **DLNet**, a high-frequency (15-minute) energy forecasting model that combines **DLinear decomposition** with **Spatial-Node aggregation**. It is specifically designed to handle the volatile, "bouncing" nature of energy data (e.g., wind/solar power and market prices).

## 🚀 Key Features

### 1. High-Resolution Spatial-Temporal Processing

* **Resolution:** Upsampled from hourly meteorological data to **15-minute intervals** using cubic interpolation for fine-grained forecasting.
* **Spatial Node Mapping:** Dynamically extracts lat/lon coordinates into a ranked grid, allowing the model to learn the importance of different geographical nodes via an **L1-regularized** fusion layer.

### 2. DLinear Decomposition Backbone

The model utilizes a **6-hour lookback window** (24 steps) and a **moving average kernel** to split signals:

* **Trend Component:** Captures low-frequency drift (e.g., steady temperature shifts).
* **Seasonal Component:** Isolates high-frequency residuals (e.g., wind gusts, cloud transients).

## 💡 Technical Insight: Capturing "Bouncing" Volatility

Standard deep neural networks often suffer from **Spectral Bias**, where non-linear layers (like ReLU) act as low-pass filters, producing over-smoothed predictions that miss critical market spikes.

**Our approach overcomes this by:**

1. **Decomposition First:** Stripping the trend allows the model to focus purely on high-frequency residuals.
2. **Linear Directness:** Using direct linear mappings for the seasonal component. Unlike deep layers that "average" signals, our linear backbone preserves the raw "energy" and "bouncing" nature of the input features.
3. **Real-time Alignment:** By including the current forecast point () in the lookback window, the model maps the latest weather dynamics directly to the immediate output.

## 🛠️ Model Architecture

The `DLNet` pipeline consists of three stages:

1. **Feature FCN:** Non-linear transformation of multi-dimensional weather features.
2. **Temporal DLinear:** Parallel processing of Trend and Seasonal paths across the 24-step window.
3. **Spatial Graph Layer:** A specialized linear aggregator that weights geographical nodes based on their predictive impact.

## 📈 Training Strategy

* **Rolling Training:** The model retrains on a **24-day sliding window** to adapt to rapid seasonal transitions.
* **Sparsity:** L1 regularization on the spatial weights ensures the model ignores redundant meteorological nodes and focuses on "influence centers."

---

