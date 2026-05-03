# Dual-Level Predictive Framework for Frequent Mental Distress

A two-study machine learning project predicting frequent mental distress using six years of CDC Behavioral Risk Factor Surveillance System (BRFSS) survey data (2019–2024). The framework combines population-level temporal modeling with individual-level prediction to surface both broad trends and personal risk factors.

> Course project for Data Science II — Group 18. I served as group lead and primary dataset expert; teammates contributed to data acquisition, preprocessing, and model implementation.

## Overview

Mental health research often relies on either aggregated public health statistics or individual clinical data, rarely both. This project bridges that gap with two complementary studies:

- **Study A — Population-level temporal analysis.** A Population LSTM trained on state-aggregated BRFSS indicators across 2019–2024 to capture how mental distress prevalence shifts over time and across regions.
- **Study B — Individual-level prediction.** A Hybrid LSTM that combines an ordered Adverse Childhood Experience (ACE) sequence with a feedforward network over static demographic and behavioral features, benchmarked against XGBoost, Random Forest, and Logistic Regression.

A linear regression / ARIMA baseline anchors the temporal study; classical ML models anchor the individual-level study.

## Repository Structure

```
.
├── proposal/           # Original project proposal
├── paper/              # Final write-up
├── data/               # Preprocessing scripts and harmonization logic
├── notebooks/          # Exploratory analysis and model training
├── models/             # Model architectures (Population LSTM, Hybrid LSTM, baselines)
└── results/            # Evaluation outputs and figures
```

*(Adjust this tree to match your actual repo layout.)*

## Methodology

### Data
CDC BRFSS public-use survey data, 2019–2024, accessed as `.XPT` SAS files and read via `pandas`. The dataset spans roughly 400,000 respondents per year across all U.S. states and territories.

### Cross-Year Harmonization
A non-trivial portion of this project was reconciling six annual codebooks. Several variables were renamed, added, or removed between years, and assumptions from earlier codebooks could not be carried forward unchecked. Notable harmonization decisions:

- **Heavy alcohol consumption** required a three-way merge across renamed variable versions.
- **Health insurance coverage** variables were renamed nearly every year; given the inconsistency, they were dropped from LSTM features rather than risk noisy harmonization.
- **A key stress-related variable** was dropped entirely from the 2024 codebook and excluded from the analysis.
- **ACE variables** use a three-point frequency scale, not binary yes/no — a subtle but critical detail for correct recoding.

### Models
| Model | Study | Role |
|---|---|---|
| Linear Regression / ARIMA | A | Temporal baseline |
| Population LSTM | A | Primary temporal model |
| Logistic Regression | B | Linear baseline |
| Random Forest | B | Tree-based baseline |
| XGBoost | B | Strong tabular benchmark |
| Hybrid LSTM | B | Primary individual-level model |

### Modeling Assumption: Ordered ACE Sequence
The Hybrid LSTM treats ACE responses as an ordered sequence fed through the recurrent layer. BRFSS does not collect ACEs as a true temporal sequence, so this is a deliberate modeling choice — we treat the ordering as a structural prior that lets the LSTM learn cumulative-effect patterns across the ACE dimensions, rather than a claim about real-world chronology. This decision is justified explicitly in the paper.

## Authors

Group 18, Data Science II
- **Gia Nguyen** — Group lead, dataset expertise, Hybrid LSTM, codebook harmonization, project scoping
- Juan Perez — Data acquisition and preprocessing
- Cole Jaramillo — [contribution]


## Data Source

CDC Behavioral Risk Factor Surveillance System (BRFSS), publicly available at [https://www.cdc.gov/brfss/](https://www.cdc.gov/brfss/).
