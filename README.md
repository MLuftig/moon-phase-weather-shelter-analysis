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
├── images/             # Generated analytical visualizations
│   └── shelter_intakes_by_pressure.png
├── src/                # Modular pipeline execution notebooks
│   ├── moonphase-and-weather-data-extraction.ipynb
│   ├── moonphase-and-weather-data-engineering.ipynb
│   └── moonphase-and-weather-data-analysis.ipynb
├── requirements.txt    # Managed package dependencies
└── README.md
