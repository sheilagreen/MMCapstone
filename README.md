## Key Terms

| Abbreviation | Full Name | What It Means |
|---|---|---|
| **AdjEM** | Adjusted Efficiency Margin | The difference between how many points a team scores vs. allows per 100 possessions, adjusted for opponent strength. Higher = better overall team. |
| **AdjOE** | Adjusted Offensive Efficiency | Points scored per 100 possessions, adjusted for opponent defense. Higher = better offense. |
| **AdjDE** | Adjusted Defensive Efficiency | Points allowed per 100 possessions, adjusted for opponent offense. **Lower = better defense.** |
| **eFG%** | Effective Field Goal Percentage | Shooting percentage that gives extra credit for 3-pointers (since they're worth more). |
| **TOPct** | Turnover Percentage | How often a team gives the ball away per possession. Lower = better ball security. |
| **ORPct** | Offensive Rebound Percentage | How often a team grabs its own missed shots. Higher = more second chances. |
| **FT Rate** | Free Throw Rate | How often a team gets to the free throw line relative to field goal attempts. |
| **Net Rating** | Net Rating | Similar to AdjEM — overall point differential per possession. |
| **Vulnerable Top 2 Seed** | Vulnerable Top 2 Seed | A 1 or 2 seed that the data flags as at risk of early elimination based on weaker underlying stats despite their high seed. |
| **SMOTE** | Synthetic Minority Oversampling | A technique to handle imbalanced data — since only 1.4% of teams are upsets, SMOTE creates synthetic upset examples so the model can learn from more than just 14 cases. |

---

## My Topic

I'm analyzing **what factors predict upsets in March Madness** — specifically, whether advanced team stats predict upsets better than just looking at seed alone.

---

## My Data

- **Source:** Kaggle dataset (compiled by Pilafas)
- **Size:** 8,680 records, 179 variables, 25 seasons (2002 to 2026)
- **Filtered to:** 1,603 March Madness teams only

---

## How I Defined "Upset"

A team seeded **7 or higher** that made the **Final Four**. That gave me **14 upsets** out of 1,027 seed 7+ teams (1.4% rate).

---

## What I've Done So Far (EDA)

1. Cleaned and filtered the data
2. Data quality check (76/179 columns have missing values, zero duplicates, 37 outliers kept)
3. Descriptive stats on all key variables
4. Compared upset teams vs. non-upset teams
5. Correlation analysis to find strongest predictors
6. Subgroup analysis on vulnerable seeds and tournament winners

---

## My Biggest Findings

- **Adjusted Efficiency Margin is the #1 predictor.** Upset teams had 85% higher efficiency margin than non-upset seed 7+ teams (19.17 vs. 10.35)
- **Defense matters more than offense.** Adjusted Defensive Efficiency correlation: r = -0.116 vs. AdjOE: r = +0.078
- **Traditional stats are useless.** eFG% had basically zero correlation with upsets (r = +0.004)
- **11-seeds upset the most.** 5 of 14 total upsets, highest rate at 4.27%
- **Vulnerable top-2 seeds underperform.** Only 14.3% made Final Four vs. 27.5% for non-vulnerable
- **Experience paradox.** Both upset teams and champions were slightly LESS experienced than average

---

## What's Next

- Logistic regression to predict upset probability
- Random forest to rank feature importance
- Compare advanced metrics model vs. seed-only model
- Handle class imbalance with SMOTE

---

## If He Asks About Challenges

- 76 columns have missing data, but those are advanced metrics not tracked in earlier seasons — my core variables are clean
- Class imbalance (only 1.4% upsets) will be the main modeling challenge
- Data was comprehensive enough that I didn't need to merge multiple sources

---

## One-Liner

> "I'm looking at whether advanced efficiency metrics can identify which lower-seeded teams are likely to make deep March Madness runs, and my EDA shows that adjusted efficiency margin and defensive efficiency are way more predictive than traditional stats like shooting percentage."

---
