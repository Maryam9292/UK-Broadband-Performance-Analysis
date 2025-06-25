# UK-Broadband-Performance-Analysis
Statistical analysis of a March 2023 broadband speed dataset in Python, featuring descriptive stats, distribution &amp; correlation visualizations, and non-parametric tests (Mann–Whitney U, Kruskal–Wallis, Wilcoxon) to compare latency and throughput across ISPs, technologies, regions, and rurality.
#!/usr/bin/env python3
"""
broadband_analysis.py

Performs statistical analysis on a broadband speed dataset:
- Descriptive statistics (mean, median, IQR)
- Distribution plots (histograms + KDE)
- Correlation heatmap
- Non-parametric hypothesis tests (Mann–Whitney U, Kruskal–Wallis, Wilcoxon)
- Exports summaries and figures for downstream use (e.g. Power BI)
"""

import os
import argparse
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import mannwhitneyu, kruskal, wilcoxon

def parse_args():
    parser = argparse.ArgumentParser(description="Broadband speed statistical analysis")
    parser.add_argument(
        "--input", "-i", required=True,
        help="Path to input CSV (e.g. data/broadband_march2023.csv)"
    )
    parser.add_argument(
        "--output", "-o", default="results",
        help="Directory to save outputs (stats, plots, tests)"
    )
    return parser.parse_args()

def ensure_dir(path):
    if not os.path.isdir(path):
        os.makedirs(path, exist_ok=True)

def descriptive_stats(df, metrics, groupby=None):
    """Compute descriptive stats; optionally by group."""
    if groupby is None:
        desc = df[metrics].agg(['mean', 'median', lambda x: x.quantile(0.75)-x.quantile(0.25)])
        desc.rename(index={'<lambda_0>':'IQR'}, inplace=True)
        return desc
    else:
        return df.groupby(groupby)[metrics].agg(['mean','median', lambda x: x.quantile(0.75)-x.quantile(0.25)]).rename(columns={'<lambda_0>':'IQR'})

def plot_distributions(df, metric, output_path):
    """Histogram + KDE for one metric."""
    plt.figure(figsize=(8,6))
    df[metric].plot(kind='hist', density=True, alpha=0.5, bins=30)
    df[metric].plot(kind='kde')
    plt.title(f"{metric} distribution")
    plt.xlabel(metric)
    plt.ylabel("Density")
    plt.tight_layout()
    plt.savefig(output_path)
    plt.close()

def plot_correlation_heatmap(df, metrics, output_path):
    """Correlation heatmap of selected metrics."""
    corr = df[metrics].corr()
    fig, ax = plt.subplots(figsize=(6,5))
    cax = ax.matshow(corr, vmin=-1, vmax=1)
    fig.colorbar(cax)
    ax.set_xticks(range(len(metrics)))
    ax.set_yticks(range(len(metrics)))
    ax.set_xticklabels(metrics, rotation=45, ha='right')
    ax.set_yticklabels(metrics)
    plt.title("Correlation Matrix")
    plt.tight_layout()
    fig.savefig(output_path)
    plt.close()

def run_tests(df, group_col, value_col, tests_out):
    """Run non-parametric tests across groups in group_col for value_col."""
    groups = df[group_col].dropna().unique()
    with open(tests_out, 'a') as f:
        f.write(f"\n=== Tests for {value_col} by {group_col} ===\n")
        # Mann–Whitney U for pairwise if only two groups
        if len(groups) == 2:
            g1 = df[df[group_col]==groups[0]][value_col]
            g2 = df[df[group_col]==groups[1]][value_col]
            stat, p = mannwhitneyu(g1, g2, alternative='two-sided')
            f.write(f"Mann–Whitney U between {groups[0]} and {groups[1]}: U={stat:.2f}, p={p:.3e}\n")
        # Kruskal–Wallis for >2 groups
        if len(groups) > 2:
            arrays = [df[df[group_col]==g][value_col] for g in groups]
            stat, p = kruskal(*arrays)
            f.write(f"Kruskal–Wallis across {len(groups)} groups: H={stat:.2f}, p={p:.3e}\n")
        # Wilcoxon if exactly two paired samples in a 'paired' column exists
        # (this is left as an example – adapt if you have paired data)
        # if 'paired_tech' in df.columns and len(groups)==2:
        #     stat, p = wilcoxon(g1, g2)
        #     f.write(f"Wilcoxon signed-rank: W={stat:.2f}, p={p:.3e}\n")

def main():
    args = parse_args()
    ensure_dir(args.output)
    ensure_dir(os.path.join(args.output, "plots"))

    # 1. Load data
    df = pd.read_csv(args.input, parse_dates=True)

    # Identify metrics and grouping columns
    metrics = ['latency_ms', 'download_mbps', 'upload_mbps']
    isp_col = 'isp'            # e.g. column name for provider
    tech_col = 'technology'    # e.g. fiber, cable, DSL
    region_col = 'region'       # e.g. state or area
    rural_col = 'rurality'      # e.g. 'Urban'/'Rural'

    # 2. Descriptive stats
    overall_stats = descriptive_stats(df, metrics)
    overall_stats.to_csv(os.path.join(args.output, "descriptive_overall.csv"))
    # by ISP
    isp_stats = descriptive_stats(df, metrics, groupby=isp_col)
    isp_stats.to_csv(os.path.join(args.output, "descriptive_by_isp.csv"))
    # by technology
    tech_stats = descriptive_stats(df, metrics, groupby=tech_col)
    tech_stats.to_csv(os.path.join(args.output, "descriptive_by_tech.csv"))

    # 3. Plots
    for m in metrics:
        plot_distributions(
            df, m,
            os.path.join(args.output, "plots", f"{m}_distribution.png")
        )
    plot_correlation_heatmap(
        df, metrics,
        os.path.join(args.output, "plots", "correlation_heatmap.png")
    )

    # 4. Statistical tests
    tests_file = os.path.join(args.output, "stat_tests.txt")
    # Reset tests file
    open(tests_file, 'w').close()
    # ISP comparisons
    run_tests(df, isp_col, 'latency_ms', tests_file)
    run_tests(df, isp_col, 'download_mbps', tests_file)
    # Region comparisons
    run_tests(df, region_col, 'latency_ms', tests_file)
    # Rural vs Urban
    run_tests(df, rural_col, 'download_mbps', tests_file)

    print(f"Analysis complete. Results saved in '{args.output}/'")

if __name__ == "__main__":
    main()
