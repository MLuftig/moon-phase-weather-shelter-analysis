# moon-phase-weather-shelter-analysis
Statistical analysis and non-parametric hypothesis testing evaluating the operational impact of lunar cycles and inclement weather on animal shelter intake volumes.


# Environmental and Lunar Influences on Animal Shelter Operational Volume

## Executive Summary
This project bridges 15 years of veterinary clinical intuition with data analytics to investigate a long-held belief in emergency medicine: that lunar cycles and environmental conditions impact patient intake surges and critical outcomes. By merging longitudinal animal shelter data with localized historical weather records from Austin, Texas, this analysis applies non-parametric statistical methods to evaluate operational patterns.

* **Business Goal:** Enable animal shelter networks and emergency veterinary facilities to optimize staffing, resource allocation, and budget planning based on predictable environmental surge factors.
* **Key Result:** Statistical testing successfully debunked the "Full Moon Effect," revealing no significant correlation with intake surges. However, severe weather indicators (barometric pressure drops and extreme temperatures) were identified as highly statistically significant drivers of shelter volume spikes.

## The Data
* **Source:** Relational integration of Austin Animal Center Intake/Outcome records (2013–2021) and localized Austin historical climatology data (2000–2023) sourced from Kaggle.
* **Dataset Scope:** Multi-year tracking filtered strictly to canine and feline populations, isolating operational episodes and matching terminal outcomes (euthanasia, died, disposal) into a unified critical mortality baseline.
* **Feature Transformations:** Continuous metrics were engineered into meaningful operational thresholds. Lunar illumination (0.0 - 1.0) was discretized into standard lunar phases. Environmental variables (precipitation, visibility, humidity, and barometric pressure) were categorized into standard meteorological bins.

## Methodology & Architecture
1. **Statistical Assumption Testing:** Executed Quantile-Quantile (Q-Q) plots and Shapiro-Wilk testing to evaluate feature distributions. The target volume data significantly violated normality assumptions (p < 0.05), mandating a transition away from traditional parametric analysis.
2. **Non-Parametric Analysis:** Implemented Kruskal-Wallis Rank Sum testing to evaluate differences in median intake and mortality rates across non-normal categorical lunar phases.
3. **Post-Hoc Pairwise Testing:** Conducted Pairwise Wilcoxon Rank Sum tests with adjusted p-values to isolate specific environmental thresholds driving significant volume variances.

### Project Directory Structure
```text
├── data/               # Raw and filtered shelter and weather CSVs
├── notebooks/          # Exploratory data analysis and scatterplot drafting
├── src/                # Modular pipeline execution scripts
│   ├── data_extraction.py
│   ├── data_engineering.py
│   └── statistical_analysis.py
├── requirements.txt    # Managed package dependencies
└── README.md
```

## Technical Decision Rationale

### SQL Relational Join Strategy
A naive join across timelines would create duplicate row fragments due to animals with multiple historical shelter visits. To preserve strict schema integrity, data engineering logic utilized Common Table Expressions (CTEs) alongside analytical window ranking functions (`ROW_NUMBER() OVER (PARTITION BY ...)`). This safely isolated clean, chronological stay-to-discharge episodes without corrupting the historical intake baseline.

### Feature Selection and Dimension Reduction
Initial Ordinary Least Squares (OLS) regression mapping revealed identical, highly correlated slopes between overall intake volume and patient mortality. Furthermore, solar energy, solar radiation, and absolute temperature presented extreme multicollinearity. To prevent model redundancy, the workflow dropped the mortality vector to focus purely on intake surges, selecting "Feels-Like Temperature" as the single representative metric for heat-related stress.

### Non-Parametric Framework Selection
Because both the intake and transformation datasets failed the fundamental assumptions of normal distribution normality (p < 0.05), classical ANOVA testing was rejected. Implementing the Kruskal-Wallis test ensured mathematically robust conclusions that do not rely on a symmetric bell-curve distribution.

## Key Findings & Operational Insights
* **The Lunar Myth:** Median intake volume and mortality rates showed no statistically significant variance across moon phases, successfully rejecting the hypothesis of a full-moon behavior surge.
* **Barometric Pressure Surges:** Sea-level barometric pressure drops (1000–1020mb vs 1020–1030mb) yielded an incredibly strong statistical significance (p = 2.58e-23), proving that arriving weather fronts actively trigger influxes of stray or surrendered animals.
* **Inclement Weather Dynamics:** Significant spikes occurred during heavy rainfall events (p = 0.037) and low-visibility conditions (p = 0.011). This highlights a clear "human variable" where the public proactively brings vulnerable animals to safety during adverse weather.

## Getting Started & Installation

### Prerequisites
Establish a local virtual environment and install the verified data science stack:
```bash
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt
```

### Usage
Execute the workflow step-by-step by opening the Jupyter Notebooks inside the source directory:

1. **Step 1: Ingestion & Filtering** — Open and run `src/moonphase-and-weather-data-extraction.ipynb` to process raw historical metrics.
2. **Step 2: SQL Merging & Binning** — Open and run `src/moonphase-and-weather-data-engineering.ipynb` to build your relational analytical tables.
3. **Step 3: Statistical Hypothesis Testing** — Open and run `src/moonphase-and-weather-data-analysis.ipynb` to evaluate distributions and execute Kruskal-Wallis tests.

