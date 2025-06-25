# UK-Broadband-Performance-Analysis
Statistical analysis of a March 2023 broadband speed dataset in Python, featuring descriptive stats, distribution &amp; correlation visualizations, and non-parametric tests (Mann–Whitney U, Kruskal–Wallis, Wilcoxon) to compare latency and throughput across ISPs, technologies, regions, and rurality.
# Broadband Speed Statistical Analysis

![GitHub repo size](https://img.shields.io/github/repo-size/yourusername/broadband-speed-analysis) ![License](https://img.shields.io/github/license/yourusername/broadband-speed-analysis)

**Python script for in-depth statistical examination of a broadband speed dataset—featuring descriptive stats, distribution & correlation visualizations, and non-parametric tests.**

---

## 🔍 Table of Contents

- [Project Overview](#project-overview)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Analyses & Outputs](#analyses--outputs)
- [Screenshots](#screenshots)
- [Contributing](#contributing)
- [License](#license)

---

## 📄 Project Overview

This repository hosts a single Python script (`broadband_analysis.py`) that:

1. **Loads** a raw broadband speed CSV (e.g., March 2023).
2. **Computes** descriptive statistics (mean, median, IQR) for latency, download, and upload speeds.
3. **Plots** distribution histograms with KDE overlays and a correlation heatmap.
4. **Performs** non-parametric tests (Mann–Whitney U, Kruskal–Wallis, Wilcoxon) to compare ISPs, technologies, regions, and rural vs. urban areas.
5. **Saves** figures and test summaries to `results/` for review or dashboard integration.

---

## 🗂 Repository Structure

```text
/ 
├── broadband_analysis.py        # Main analysis script
├── data/                        # Input CSV(s)
├── results/                     # Generated stats, plots, and test outputs
├── images/                      # Add screenshots here
│   ├── distribution_chart.png
│   └── dashboard_screenshot.png
├── requirements.txt             # Python dependencies
└── README.md                    # This file
```

---

## ⚙️ Installation

```bash
git clone https://github.com/yourusername/broadband-speed-analysis.git
cd broadband-speed-analysis
pip install -r requirements.txt
```

---

## ▶️ Usage

```bash
python broadband_analysis.py \
  --input data/broadband_march2023.csv \
  --output results/
```

- **Inputs**: CSV with columns `latency_ms`, `download_mbps`, `upload_mbps`, `isp`, `technology`, `region`, `rurality`.
- **Outputs**:
  - `results/plots/*_distribution.png` (histograms + KDE)
  - `results/plots/correlation_heatmap.png`
  - `results/stat_tests.txt` (hypothesis test summaries)
  - `results/descriptive_overall.csv`, ... (aggregated stats)

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

![Data Distribution](images/distribution_chart.png)

### 2. Power BI Dashboard Preview

![Dashboard Preview](images/dashboard_screenshot.png)

---

## 🤝 Contributing

1. Fork the repo  
2. Create a branch (`git checkout -b feature-name`)  
3. Commit your changes (`git commit -m "Add feature"`)  
4. Push to branch (`git push origin feature-name`)  
5. Open a pull request  

---

## 📜 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
