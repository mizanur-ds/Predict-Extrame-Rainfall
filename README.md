# 🌧️ Spatiotemporal Deep Learning Model for Monsoon Rainfall Prediction

## 1. Model Overview

This study presents a **spatiotemporal deep learning framework** for daily rainfall prediction based on a **ConvLSTM encoder with a spatial convolutional decoder**. The model is designed to leverage **7-day antecedent atmospheric conditions** to predict gridded precipitation fields over the study domain, with particular emphasis on **monsoon rainfall dynamics and extreme events**.

<img width="1391" height="615" alt="image" src="https://github.com/user-attachments/assets/1f55a1ed-1f6a-46c7-83b4-dd506ddde49c" />

---

## 2. Model Architecture

### 🔧 Input Configuration
- **Temporal context**: 7-day input sequence  
- **Spatial domain**: Gridded latitude × longitude  
- **Total features (12)**:
  - Instantaneous:  
    `tp`, `msl`, `sst`, `Qu`, `Qv`, `Qmag`
  - Lagged (t−1):  
    `tp_lag1`, `msl_lag1`, `sst_lag1`, `Qu_lag1`, `Qv_lag1`, `Qmag_lag1`

### 🧠 Encoder (Temporal-Spatial Learning)
- Stacked **ConvLSTM2D layers**:
  - 32 filters → 64 filters → 64 filters
- Kernel size: 3×3
- Activation: ReLU
- Batch Normalization applied after intermediate layers
- Final ConvLSTM layer outputs a single spatial feature map

### 🗺️ Decoder (Spatial Refinement)
- Conv2D layers:
  - 64 → 32 → 16 filters
- Kernel size: 3×3
- Activation: ReLU
- Final output layer:
  - Conv2D (1×1), linear activation
  - Produces daily rainfall intensity (mm/day)

### 🎯 Training Objective
- **Masked Mean Squared Error (Masked MSE)**  
  Loss is computed only where observed rainfall > 0, focusing learning on physically meaningful precipitation events.

---

## 3. Overall Predictive Performance

### 📈 Temporal Mean Metrics

| Metric | Value |
|------|------|
| RMSE | **0.298 mm/day** |
| MAE  | **0.238 mm/day** |
| Bias | **−0.158 mm/day** |
| Correlation | **0.994** |

The model exhibits **excellent temporal agreement** with observations, capturing daily rainfall variability with minimal bias and near-perfect correlation.

---

## 4. Rain / No-Rain Detection Skill

Threshold: **1 mm/day**

| Metric | Value |
|------|------|
| POD | **0.991** |
| FAR | **0.023** |
| CSI | **0.968** |

The model demonstrates **near-perfect rain detection**, with extremely low false-alarm rates.

---

## 5. Rainfall Intensity–Dependent Errors

| Rainfall Range (mm/day) | RMSE |
|-------------------------|------|
| 0–1 | 0.60 |
| 1–5 | 0.87 |
| 5–10 | 1.25 |
| 10–20 | 1.98 |
| 20–50 | 4.16 |

Errors increase with rainfall intensity, reflecting the inherent nonlinearity and spatial variability of convective precipitation.

---

## 6. Spatial Skill

- **Mean spatial correlation**: **0.975**

The model effectively reproduces spatial rainfall structures, including monsoon rainbands and regional gradients.

---

## 7. Extreme Rainfall Performance

### 7.1 Percentile-Based Skill

| Percentile | POD | FAR | CSI |
|-----------|-----|-----|-----|
| P90 | 0.84 | 0.08 | 0.81 |
| P95 | 0.84 | 0.08 | 0.79 |
| P97 | 0.81 | 0.08 | 0.75 |
| P99 | 0.73 | 0.09 | 0.68 |

The model retains strong predictive skill up to the 95th–97th percentile, with gradual degradation at the most extreme tail.

---

### 7.2 Extreme Event Detection (≥ 26.24 mm/day)

| Metric | Value |
|------|------|
| Hits | 83,978 |
| Misses | 15,836 |
| False Alarms | 7,016 |
| POD | **0.841** |
| FAR | **0.077** |
| CSI | **0.786** |

---

### 7.3 Spatial Verification of Extremes

- **Mean Fractions Skill Score (FSS)**: **0.947**

This high FSS indicates excellent spatial agreement for extreme rainfall events, with remaining errors primarily due to minor spatial displacement.

---

### 7.4 Extreme Intensity Bias

- **Bias**: **−2.38 mm/day**

The model slightly underestimates peak rainfall intensity, a common limitation of deterministic MSE-based regression approaches.

---

### 7.5 Extreme-Day Detection

- **Extreme day CSI**: **0.625**

Moderate skill is observed in predicting days dominated by widespread extreme rainfall.

---

## 8. Monsoon Dynamics Skill

### 🌧️ Monsoon Onset and Withdrawal
- **Onset error**: **0 days**
- **Withdrawal error**: **0 days**

The model accurately captures seasonal monsoon transitions.

---

### 🌊 Monsoon Intraseasonal Oscillation (MIS)

| Metric | Value |
|------|------|
| Phase correlation | **0.986** |
| Amplitude ratio | **1.12** |

The model reproduces both the phase and amplitude of MIS variability, with a slight overestimation of oscillation strength.

---

## 9. Probabilistic Verification

- **Continuous Ranked Probability Score (CRPS)**: **1.04**

This indicates good probabilistic consistency despite the model being primarily deterministic.

---

## 10. Summary and Conclusions

**Strengths**
- Excellent temporal and spatial accuracy
- Near-perfect rain/no-rain discrimination
- Strong skill for moderate and extreme rainfall events
- Accurate representation of monsoon onset, withdrawal, and intraseasonal oscillations
- High spatial fidelity for extreme rainfall patterns

**Limitations**
- Mild underestimation of peak rainfall intensity
- Reduced skill for domain-wide extreme rainfall days
- Deterministic formulation limits representation of uncertainty tails

---

### ✅ Final Conclusion

*The ConvLSTM-based spatiotemporal rainfall prediction model demonstrates robust and physically consistent performance across daily, intraseasonal, and extreme rainfall regimes. The model shows strong potential for subseasonal monsoon rainfall forecasting and impact-relevant extreme event prediction, with future improvements possible through probabilistic extensions and extreme-focused loss functions.*

---
