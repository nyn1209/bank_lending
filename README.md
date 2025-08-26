Credit-risk analytics linking borrower income dynamics to mortgage PD/LGD, with forward scenarios and stress testing.

## Overview

This project explores how state-level household income growth relates to mortgage credit risk. It covers data prep for U.S. median household income, exploratory analysis, PD and LGD modeling, forward income-growth scenarios (2024–2029), and PD stress testing by LTV buckets. Empirically, lower/negative income growth aligns with higher PD; rising income aligns with lower PD and more stable LGD. 
 
## Data

Income source: U.S. Census “Median Household Income by State (1984–2023)” in two tables (current USD and 2023 USD). This project focuses on 2001–2015 and builds tidy panels per state/year. 

Missing years: 2004, 2009, 2010, 2013 are absent in the time window and are imputed. 

Imputation: Linear interpolation followed by forward-fill to ensure continuity. 

Time index: Model code maps time = 1..60 to years 2001–2015; checks guard against edge-case misalignment. 

Problem framing

Default definition: Binary default_time built from standard mortgage default events (30/60/90-day delinquency, foreclosure/REO, short sale, bankruptcy, loss write-down, etc.). 
 

## Goal
Model PD and LGD as functions of loan features (e.g., LTV) and macro income growth; evaluate performance and interpret cyclical patterns.

## Modeling
**PD (Probability of Default)**

- Specs: Baseline PD uses LTV; an augmented version adds income-growth features. (See notebooks.)
- Performance: Model 1 vs. Model 2 show similar AUC (0.6775 vs. 0.6792), identical Accuracy (0.9748), and Log-Loss (0.1124). The income-growth variable adds small marginal lift in AUC. 

**LGD (Loss Given Default)**
- Temporal pattern: Early periods show elevated, volatile LGD, stabilizing through mid-periods; macro shocks can raise LGD. 
- Income link: Regression indicates a negative association between income growth and LGD (e.g., coefficient −0.0902, p<0.01 in the report). 

**Scenarios (2024–2029)**

A forward-looking macro scenario projects annual income growth paths (e.g., 1.8% in 2024; 2.5% in 2025; 3.2% in 2026; 3.5% in 2027; 3.3% in 2028; 2.8% in 2029) based on disinflation, labor recovery, and productivity gains. 
<br> Implications: Under these paths, PD trends down toward ~0.022 by 2028 if growth ≥2.5%; LGD remains ~0.65–0.70, with a mild uptick possible in 2029. 
 
**Stress testing by LTV
Stress test uses the 2024 scenario (1.8% income growth) and ten LTV bins; PDs are shocked and compared with a higher-growth baseline (e.g., 2022–2023 ≥2.5%). 
 
<br> Result: Sensitivity to macro conditions varies by LTV, with larger responses typically at low and high LTV ends. 


## Repository structure
├─ data/              
├─ notebooks/           
├─ reports/             
└─ requirements.txt

## Getting started
```bash
# create env
python -m venv .venv
source .venv/bin/activate   # (Windows) .venv\Scripts\activate
pip install -r requirements.txt

# run notebooks
jupyter lab
```
