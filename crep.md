# Explaining CReP: Causal-oriented Representation Learning Predictor

Here in this blog post i share my summary of reading the paper : [Causal-oriented representation learning for time-series forecasting based on the spatiotemporal information transformation](https://www.nature.com/articles/s42005-025-02170-6)

## TL;DR

CReP is a self-supervised framework that jointly performs **multi-step time-series forecasting** and **causal analysis** by decomposing high-dimensional observations into orthogonal cause, effect, and non-causal representations for a chosen target variable.

## Introduction

### Objectives
- From high-dimensional time-series observations:
  - **Predict** the future trajectory of a selected target variable over multiple time steps.
  - **Identify** which observed variables are likely **drivers** of the target and which are likely **responses**.

### Problem
- Existing approaches forecast well but remain non-causal, or infer causality separately.

### Importance of the paper
- Enables **simultaneous forecasting and causal analysis** in high-dimensional time-series systems.
- Provides **interpretable causal insights** (causes vs. effects) rather than black-box predictions.

### Grounding

#### Dynamic causation
- If variable \( a \) drives \( b \) in a dynamical system, then information about \( a \) is embedded in the time series of \( b \).
- Using delay embeddings, there exists an implicit mapping (Takens):
  \[
  A_t = (a_t, \dots, a_{t+L-1})^\top,\quad
  B_t = (b_t, \dots, b_{t+L})^\top,\quad
  A_t \approx h(B_t)
  \]

#### Spatiotemporal information transformation (delay ↔ non-delay)
- STI transformation allows a learned non-delay embedding \( C_t \) to be topologically conjugate to \( B_t \):
  \[
  A_t \approx \tilde{h}(C_t)
  \]
- This motivates learning a latent \( Z_t \) from which \( Y_t \) can be predicted:
  \[
  Y_t \approx g(Z_t)
  \]

## Method

The self-supervised framework is characterized by three key features:

1. Dynamic causation detection with the STI transformation mechanism.
2. Causal-oriented representation learning for multi-step predictions through the **CReP**.
3. Causal analysis of the target variable via **\( \alpha\beta \)-LRP**.

**Figure:** Schematic illustration of the CReP framework.

### Loss
\[
\mathcal{L} =
\lambda_1 \mathcal{L}_{DS}
+ \lambda_2 \mathcal{L}_{FC}
+ \lambda_3 \mathcal{L}_{REC}
+ \lambda_4 \mathcal{L}_{ORTH}
\]

- **Determined-State Loss**: RMSE on known historical \( y \).
- **Future-Consistency Loss**: RMSE between overlapping future estimates.
- **Reconstruction Loss**: assesses information recovery
  - \( \mathcal{L}_{REC\_X} \): spatiotemporal information \( X \) from \( (S, Z, V) \)
  - \( \mathcal{L}_{REC\_S} \): latent cause representation \( S \) recovered by \( Y \)
- **Orthogonality Loss**: enforces orthogonality among \( S \), \( Z \), and \( V \).

## Results

### Simulation systems

**CReP on Lorenz 96**
(50-step history, 15-step-ahead prediction, 60D system)


**Normalized causal results on Dream4**
- Blue bars: top-10 variables by causal strength
- Purple bars: true causes/effects among them
- Red bars: true causes/effects that were missed

### Real datasets

**CReP on Hong Kong cardiovascular inpatients**
(70-day history, 25-day-ahead forecasting, 14 variables)

## Conclusion

### Key Points
- Causes leave **detectable traces** in the dynamics of what they influence.
- CReP learns to **extract these traces** from high-dimensional time series.
- These representations are used to **predict the future** and **identify causes and effects**.

### Critical discussion
- Presence of **false positives** and **missed true causes** (Dream4 results).
- Comparisons limited to relatively **basic baselines**, excluding SOTA methods.
- Different hyperparameter choices across datasets.

### Future Work
- Integrating different causal learning methods for broader applicability.
- Exploring causal detection through **active intervention** rather than passive observation.

