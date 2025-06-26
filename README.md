# UK-Broadband Performance Statistical Analysis

**Python script for in-depth statistical examination of UK broadband dataset of different Internet Service Providers from March 2023 — featuring descriptive stats, distribution & correlation visualizations, and non-parametric tests to compare latency and throughput across ISPs, technologies, regions, and rurality.**


## 📄 Project Overview

This repository hosts a single Python script (`Statistical Analysis.ipynb`) that:

1. **Loads** a raw broadband speed dataset (Dataset - March 2023-Broadband Performance Data.xlsm).
2. **Computes** descriptive statistics for latency, download, and upload speeds.
3. **Plots** distribution histograms with KDE overlays and a correlation heatmap.
4. **Performs** non-parametric tests (Mann–Whitney U, Kruskal–Wallis, Wilcoxon) to compare ISPs, technologies, regions, and rural vs. urban areas.
5. **Saves** figures and test summaries for review and dashboard integration.

---
## 📈 Analyses & Outputs

- **Descriptive Statistics**: overall and by group (ISP, technology).
- **Distribution Visuals**: understand skew, outliers, and spread per metric.
- **Correlation Heatmap**: reveal relationships between latency and throughput.
- **Hypothesis Tests**:
  - *Mann–Whitney U* for two-group comparisons.
  - *Kruskal–Wallis* for multi-group analysis.
  - *Wilcoxon* for paired samples (e.g., tech A vs. tech B).

---

## 🖼️ Screenshots

### 1. Data Distribution Chart

![Download Speed Distribution](images/Download_speed_24ave.png)
![Latency Distribution](images/latency_24h.png)
---

### 2. Boxplot of Latency in Rural vs Urban Areas Preview

![Boxplot of Latency in Rural vs Urban Areas Preview](images/rural_urban_latency.png)
---

### 3. Power BI Dashboard Preview

![Findings Dashboard Preview](images/broadband_performance.png)

---

## 🔮 Future Work

1. Analyse the broadband performance by upload and download speed at peak and off peak times.
2. Analysis of broadband speed for NetFlix and YouTube specifically.

---

