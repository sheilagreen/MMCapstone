# Identifying Key Predictors of NCAA Tournament Upsets in March Madness

## Executive Overview

March Madness is defined by single-elimination volatility. Each year, lower-seeded underdogs defeat heavily favored programs, defying public bracket consensus. While tournament seeding assigned by the NCAA selection committee serves as the standard proxy for team strength, it relies on subjective committee evaluations that can obscure quantitative mismatches.

This capstone project investigates whether advanced pre-tournament team performance metrics (efficiency ratings, wins above bubble, and shooting/possession profiles) provide superior predictive power for Round of 64 upset outcomes compared to committee seeding alone.

Analyzing 578 Round of 64 matchups from the 2008 through 2026 NCAA Tournaments, this study evaluates baseline seed-only models against full multi-variable models utilizing team metrics from KenPom and Barttorvik.

---

## Key Terms

| Abbreviation | Full Name | What It Means |
|---|---|---|
| AdjEM | Adjusted Efficiency Margin | The difference between how many points a team scores vs. allows per 100 possessions, adjusted for opponent strength. Higher = better overall team. |
| AdjOE | Adjusted Offensive Efficiency | Points scored per 100 possessions, adjusted for opponent defense. Higher = better offense. |
| AdjDE | Adjusted Defensive Efficiency | Points allowed per 100 possessions, adjusted for opponent offense. Lower = better defense. |
| eFG% | Effective Field Goal Percentage | Shooting percentage that gives extra credit for 3-pointers (since they're worth more). |
| TOPct | Turnover Percentage | How often a team gives the ball away per possession. Lower = better ball security. |
| ORPct | Offensive Rebound Percentage | How often a team grabs its own missed shots. Higher = more second chances. |
| FT Rate | Free Throw Rate | How often a team gets to the free throw line relative to field goal attempts. |
| Net Rating | Net Rating | Similar to AdjEM: overall point differential per possession. |
| Vulnerable Top 2 Seed | Vulnerable Top 2 Seed | A 1 or 2 seed that the data flags as at risk of early elimination based on weaker underlying stats despite their high seed. |
| SMOTE | Synthetic Minority Oversampling | A technique to handle imbalanced data. Since only 1.4% of teams are upsets, SMOTE creates synthetic upset examples so the model can learn from more than just 14 cases. |

---

## Key Findings

- Predictive Superiority: A Logistic Regression model trained on five selected advanced metrics achieved an AUC-ROC of 0.7440, outperforming the seed-only baseline (0.7402), while delivering gains in classification Accuracy (+3.5%) and F1 Score (+4.5%).
- Primary Upset Drivers: KenPom Adjusted Efficiency Margin (KADJ EM) and Wins Above Bubble (WAB) emerged as the strongest predictors, together accounting for over 50% of total feature importance.
- The Cinderella Profile: Historical upset winners (Cinderellas) closely match favorites in core execution metrics such as Effective Field Goal Percentage (1.3% differential) and Defensive eFG% (1.0% differential), but are systematically under-seeded due to weaker conference strength of schedule (Elite SOS).

---

## Dataset and Scope

- Scope: NCAA Division I Men's Basketball Tournament, Round of 64 matchups (1-vs-16 through 8-vs-9 seed pairings).
- Timeframe: 2008 to 2026 (18 tournaments, 578 total games).
- Target Variable: UPSET (Binary: 1 if lower-seeded team won, 0 if higher-seeded favorite won).
- Class Balance: 153 Upsets (26.47%) vs. 425 Expected Wins (73.53%).
- Data Sources: Kaggle March Machine Learning Mania, KenPom.com, Barttorvik.com, Sports Reference.

---

## Methodology and Modeling Pipeline

The analysis follows a structured pipeline with the following stages:

1. Raw Matchup Data Collection (578 Games, 2008 to 2026)
2. Feature Engineering: Favorite-Minus-Cinderella Differential (FIX_DIFF_)
3. Feature Selection (Multicollinearity Reduction to Top 5 Predictors)
4. Stratified Train/Test Split (75% Train / 25% Test)
5. Class Imbalance Mitigation: SMOTE on Training Set Only (433 to 636 Samples)
6. Model Evaluation (Logistic Regression vs. Random Forest)
7. Interactive March Madness Prediction Tool Development

### 1. Data Leakage Remediation

Initial modeling achieved an artificial 0.98 AUC due to target leakage in difference features computed as Winner minus Loser. All metrics were re-engineered to strictly reflect Favorite minus Cinderella (FIX_DIFF_), ensuring the sign of the difference does not encode game outcomes.

### 2. Feature Selection

To prevent multicollinearity and overfitting across 16 correlated variables, feature selection isolated five orthogonal dimensions of team performance:

1. FIX_DIFF_WAB: Wins Above Bubble (overall resume quality)
2. FIX_DIFF_KADJ EM: KenPom Efficiency Margin (tempo-adjusted point differential per 100 possessions)
3. FIX_DIFF_EFG%: Effective Field Goal Percentage (perimeter and interior shooting efficiency)
4. FIX_DIFF_TOV%: Turnover Rate (possession security and ball handling)
5. FIX_DIFF_OREB%: Offensive Rebound Percentage (second-chance scoring generation)

### 3. Class Imbalance Handling

Synthetic Minority Over-sampling Technique (SMOTE) was applied to the training partition only, expanding training observations from 433 to 636 to prevent majority-class bias without distorting the real-world test distribution.

---

## Performance Benchmark

| Model Configuration | Accuracy | F1 Score | Test AUC-ROC | 5-Fold CV AUC |
| :--- | :--- | :--- | :--- | :--- |
| Logistic Regression (5 Advanced Metrics) | 0.6414 | 0.5357 | 0.7440 | 0.7518 |
| Logistic Regression (Seed Difference Baseline) | 0.6069 | 0.5128 | 0.7402 | 0.7238 |
| Random Forest (5 Advanced Metrics) | 0.6345 | 0.4536 | 0.6877 | 0.6634 |
| Random Forest (Seed Difference Baseline) | 0.6069 | 0.5128 | 0.6546 | 0.6875 |

---

## Historical Upset Distribution (Round of 64)

| Seed Matchup | Total Games | Total Upsets | Historical Upset Rate | Volatility Profile |
| :--- | :--- | :--- | :--- | :--- |
| 1 vs 16 | 74 | 2 | 2.7% | High Certainty |
| 2 vs 15 | 71 | 7 | 9.9% | Low Volatility |
| 3 vs 14 | 72 | 8 | 11.1% | Low Volatility |
| 4 vs 13 | 72 | 15 | 20.8% | Moderate Risk |
| 5 vs 12 | 72 | 28 | 38.9% | High Volatility |
| 6 vs 11 | 74 | 33 | 44.6% | High Volatility |
| 7 vs 10 | 71 | 27 | 38.0% | High Volatility |
| 8 vs 9 | 72 | 33 | 45.8% | Coin Flip |

---

## Interactive Prediction Tool

The accompanying Jupyter Notebook includes an interactive upset evaluation tool developed with ipywidgets:

- Real-time matchup simulation using live KenPom/Barttorvik metric inputs.
- Auto-populating default statistical baselines across all seed pairings (1 to 16).
- Four-tier alert output: Favorite Holds, Watch Out, Upset Likely, and High Cinderella Alert.

---

## Repository Structure

- CapstoneModel.ipynb: End-to-end data validation, feature engineering, SMOTE balancing, model evaluation, and interactive widget code.
- Identifying Key Predictors of NCAA Tournament Upsets in March Madness.pdf: Complete academic report and analysis.

---

## Tech Stack

- Language: Python 3
- Environment: Google Colab / Jupyter
- Core Libraries: pandas, numpy, scikit-learn, imblearn, matplotlib, seaborn, ipywidgets

---

## Project Metadata

- Author: Sheila Green
- Institution: Fairfield University, Dolan School of Business
- Course: DATA 6999 - Capstone: Business Analytics Applications
