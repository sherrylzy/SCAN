# SCAN: Selective Contrastive Learning Against Noisy Data for Acoustic Anomaly Detection

This page provides additional experiments, visualizations, and analyses for our SCAN framework, focusing on the robustness of noisy data in acoustic anomaly detection.

---
## Table of Contents
- [2. Visualization of Latent Representations](#2-visualization-of-latent-representations)
  - [2.1 Over Training Epochs](#21-over-training-epochs)
  - [2.2 Different Noise Ratios](#22-different-noise-ratios)
- [4. Per-Category / Per-Machine Results](#4-per-category--per-machine-results)
  - [4.1 Machine-wise Performance Breakdown](#41-machine-wise-performance-breakdown)
  - [4.2 Domain Shift Analyses](#42-domain-shift-analyses)

---

## 2. Visualization of Latent Representations

### 2.1 Over Training Epochs
**(Explain how you visualize the latent feature space at different training epochs — e.g., 1, 100, 200, 400. Provide charts or t-SNE/UMAP plots here.)**

<details>
<summary>Example Discussion</summary>

- **Motivation**: Illustrate how latent representations of normal and abnormal samples evolve over time.
- **Method**: At selected epochs (1, 100, 200, 400), we extract features and apply dimensionality reduction (t-SNE/UMAP).  
- **Observation**: Early in training, points may be interspersed; by later epochs, clusters (normal vs. abnormal) become more distinct.

*Insert your figures or GIFs here.*
</details>

---

### 2.2 Different Noise Ratios
**(Show side-by-side comparisons under 0%, 2%, and 8% noise. You might have 3 sets of t-SNE/UMAP plots for each epoch.)**

<details>
<summary>Example Discussion</summary>

- **Motivation**: Show how increasing label noise impacts the feature space.  
- **Method**: Train separate models at noise ratios = 0%, 2%, 8%. At each ratio, visualize latent representations at the same chosen epochs.  
- **Observation**: With higher noise, the decision boundary may blur; SCAN’s selecting mechanism should help keep normal and abnormal clusters more separated.

*Insert your figures or side-by-side comparisons here.*
</details>

---

## 4. Per-Category / Per-Machine Results

### 4.1 Machine-wise Performance Breakdown
**(Present detailed metrics — e.g., AUC, Precision, F1 — per machine type. Discuss which machine types are most sensitive to noise.)**

<details>
<summary>Example Table or Chart</summary>

| Machine Type | AUC @0% Noise | AUC @2% Noise | AUC @8% Noise |
|:------------:|:------------:|:------------:|:------------:|
| Bearing      | 0.XX         | 0.XX         | 0.XX         |
| Fan          | 0.XX         | 0.XX         | 0.XX         |
| ...          | ...          | ...          | ...          |

- **Discussion**: 
  - Identify which machine categories degrade more drastically with increased noise.  
  - Hypothesize reasons for differences (e.g., some signals are inherently more variable, etc.).

*Alternatively, include a bar chart or multiple bar charts for each noise ratio, grouped by machine type.*
</details>

---

### 4.2 Domain Shift Analyses
**(Break down performance for source vs. target domains — or multiple domain shifts, if available. Compare how SCAN adapts under each condition.)**

<details>
<summary>Example Domain Shift Table</summary>

| Domain        | AUC @0% Noise | AUC @2% Noise | AUC @8% Noise |
|:-------------:|:-------------:|:-------------:|:-------------:|
| Source        | 0.XX          | 0.XX          | 0.XX          |
| Target        | 0.XX          | 0.XX          | 0.XX          |

- **Discussion**: 
  - Show how SCAN handles real-world changes (machine operator changes, environment differences, etc.).  
  - If performance drops drastically in the target domain, discuss the root causes and potential improvements.

</details>

---

_You can add extra sections if needed, such as “**Conclusions**,” “**Hyperparameter Settings**,” or “**Runtime Analysis**.”_

Feel free to modify the above outline, add figures, code blocks, or more detailed descriptions depending on your results. This provides a clean structure for readers to navigate your extended experimental findings.

