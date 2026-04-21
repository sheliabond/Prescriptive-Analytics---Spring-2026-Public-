# Indirect Auto Lending Allocation — Prescriptive Analytics Final Project

**Student:** Shelia Bond
**Course:** Prescriptive Analytics — Spring 2026
**Date:** April 20, 2026

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/sheliabond/Prescriptive-Analytics---Spring-2026-Public-/blob/main/Assignments/Final_Project/%20Final_Assignment.ipynb)

---

## Overview

This project applies the full prescriptive analytics toolkit to a real decision problem in indirect auto lending. The goal is to build a working optimization model that determines how a credit union should allocate its quarterly indirect auto lending budget across a network of active dealerships — maximizing net yield while staying within approved risk limits and loan growth targets.

---

## The Business Problem

The VP of Consumer Lending at a federal credit union must decide how to allocate between **$18M and $25M** per quarter across **10 active dealerships**. Without a systematic approach, allocations default to habit and relationships rather than performance. Dealers vary widely in yield potential and risk profile, making the choice of who gets how much financially consequential.

**Suboptimal allocation reduces net yield and may breach the portfolio risk threshold — directly impacting the credit union's financial performance.**

---

## Model Summary

| Component | Detail |
|---|---|
| **Model Type** | Mixed-Integer Linear Program (MILP) |
| **Solver** | PuLP / CBC |
| **Decision Variable** | Dollar amount allocated to each of 10 dealers per quarter |
| **Objective** | Maximize total net yield across the portfolio |
| **Budget Floor** | Total allocation ≥ $18M per quarter (loan growth target) |
| **Budget Ceiling** | Total allocation ≤ $25M per quarter |
| **Portfolio Risk Limit** | Weighted average risk score < 2.8 |
| **Dealer Concentration Cap** | No single dealer may receive more than $3M |

**Key Tradeoff:** Higher yield comes with higher risk — maximizing return pushes the portfolio toward riskier dealers.

---

## Key Results

The optimal solution funds **9 of 10 dealers** and deploys the full **$25M** budget.

| Dealer | Yield Rate | Risk Score | Allocation |
|---|---|---|---|
| Mercedes Dealer | 7.88% | 3.40 | $3.00M |
| Nissan Dealer | 7.33% | 2.96 | $3.00M |
| Ford Dealer | 6.10% | 3.23 | $3.00M |
| Honda Dealer | 5.73% | 2.70 | $3.00M |
| Acura Dealer | 5.22% | 1.62 | $3.00M |
| Kia Dealer | 5.16% | 2.92 | $3.00M |
| Toyota Dealer | 4.85% | 2.70 | $3.00M |
| Mazda Dealer | 4.73% | 1.81 | $3.00M |
| Dodge Dealer | 4.73% | 1.81 | $1.00M |
| BMW Dealer *(excluded)* | 4.08% | 2.25 | — |

**BMW Dealer** received no allocation — it has the lowest yield in the portfolio and offers no competitive advantage over funded dealers.

**Portfolio Total:** $25.00M deployed | 5.83% weighted avg yield | 2.63 avg risk score

---

## Sensitivity Analysis

| Parameter | Scenario | Net Yield Impact | Verdict |
|---|---|---|---|
| Growth Floor (min $18M) | ±20% change | < 5% swing | ✅ Robust |
| Portfolio Risk Limit (2.8) | ±20% change | Significant shift | ⚠️ Fragile |
| Dealer Cap ($3M max) | ±20% change | Significant shift | ⚠️ Fragile |

The risk limit and dealer cap are the two levers that actually control outcomes. The VP should treat these as active decisions, not administrative defaults.

---

## Time Dimension

The current model is a **static, single-period decision**. Time would become highly relevant under these conditions:

1. **Changing Dealer Performance** — If yield rates or risk scores shift significantly across quarters, a dynamic multi-period model would be needed to re-optimize over time.
2. **Budget Fluctuations** — If the quarterly budget varies or unallocated funds roll over, a time dimension would allow the model to plan across periods and smooth allocations.
3. **Seasonality** — If demand and risk follow seasonal patterns, the model could shift allocations toward peak demand periods or away from higher-risk windows.

---

## Model Limitations

This model simplifies reality in three important ways:

- **Yield is not linear.** The model assumes every dollar allocated to a dealer produces the same yield. In practice, pushing volume past a dealer's natural borrower pool likely reduces marginal yield.
- **Risk is not fixed.** Forcing a dealer to originate above their natural capacity may cause them to loosen underwriting standards, raising actual risk scores beyond what the model assumes.
- **Data is simulated.** A production version requires real dealer yield and loss history — not random seed inputs.

---

## Recommendation

> **Do not fully deploy the model's output yet. Prioritize validation first.**

The recommendation is fragile to the risk limit and dealer cap — both actively managed parameters. The first step is to run a **pilot with a subset of dealers** to measure how yield and risk actually respond to volume changes. That real-world data will make the next model substantially more reliable.

---

## Repository Structure

```
Assignments/Final_Project/
├── Final_Assignment.ipynb   # Main notebook with all sections and code
├── README.md                # This file
└── Lending_Allocation_Presentation.pptx  # Slide deck (6 slides)
```

---

## How to Run

**Option 1 — Google Colab (recommended)**
Click the "Open in Colab" badge at the top of this README. All dependencies install automatically via the first notebook cell.

**Option 2 — Local**
```bash
pip install pulp pandas matplotlib numpy
jupyter notebook Final_Assignment.ipynb
```

---

## Dependencies

| Package | Purpose |
|---|---|
| `pulp` | Linear and integer programming solver |
| `pandas` | Data manipulation and results display |
| `numpy` | Numerical operations and data generation |
| `matplotlib` | Visualization |
