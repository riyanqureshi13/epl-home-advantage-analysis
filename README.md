# EPL Home Advantage Analysis

Does home-field advantage still hold up in the Premier League — and has it changed in recent seasons?

**🔗 Live Dashboard:** [EPL Home Advantage Analysis on Tableau Public](https://public.tableau.com/app/profile/riyan.qureshi/viz/EPL_HOME_ADVANTAGE_ANALYSIS/Dashboard1)

[![EPL Home Advantage dashboard](https://public.tableau.com/static/images/EP/EPL_HOME_ADVANTAGE_ANALYSIS/Dashboard1/1.png)](https://public.tableau.com/app/profile/riyan.qureshi/viz/EPL_HOME_ADVANTAGE_ANALYSIS/Dashboard1)

---

## Overview

Home advantage is one of the most talked-about effects in football, but I wanted to actually measure it rather than just assume it — how big is it, which teams benefit most, and is it growing or shrinking over the last few years?

This project uses 5 seasons of Premier League match data to answer three questions:
1. How much of a points advantage do teams get from playing at home?
2. Which teams benefit most (and least)?
3. Has that advantage changed over the past 5 seasons?

## Data

- **Source:** [football-data.co.uk](https://www.football-data.co.uk) — free, publicly available match-level data
- **Scope:** 5 Premier League seasons, 2021/22 through 2025/26 (1,900 matches)
- **Fields used:** match results, half-time/full-time scores, shots, corners, fouls and cards (21 of the 106–132 columns in the source files)

## Tools & Method

| Stage | Tool |
|---|---|
| Data cleaning & combining seasons | Python (Pandas) |
| Computing home/away Points-Per-Game (PPG) | Pandas `groupby` |
| Validating the calculation independently | SQL (SQLite) |
| Testing statistical significance | Paired t-test (`scipy.stats`) |
| Season trend line | NumPy (`polyfit`) |
| Dashboard & visualization | Tableau Public |

Each match result was converted to points (win = 3, draw = 1, loss = 0), then averaged per team, per season, split by home vs. away. The same calculation was rebuilt in SQL as a cross-check against the Pandas result, and all 100 team-seasons matched exactly.

## How to Run

```bash
pip install -r requirements.txt
```

Then open `Scripts/data_analysis.ipynb` in Jupyter and run all cells. It reads the five season files in `Data/` and writes two outputs back to the same folder: `epl_5seasons_combined.csv` (all 1,900 matches in one table) and `ppg_by_team_season.csv`, the data source for the Tableau dashboard.

## Key Findings

- **League-wide home PPG:** 1.56 vs. **away PPG:** 1.20 — a gap of ~0.37 points per game
- Across the 19 home games in a season, that's **~7.0 points of home advantage per season**
- A paired t-test across all 100 team-seasons confirms this gap is statistically significant (t = 9.74, **p < 0.001**) — not random variation
- Year-to-year, the gap is volatile — swinging between 0.18 and 0.59 points per game across the 5 seasons — but a straight-line trend slopes gently downward, from ~0.40 in 2021-22 to ~0.33 in 2025-26. With only five seasons, that's a hint that home advantage may be easing rather than a proven decline
- Newcastle showed the strongest home advantage over the 5 seasons (0.663 PPG), with Liverpool (0.642) and Sunderland (0.632, one season) close behind. At the other end, Ipswich (−0.421) and Watford (−0.368) actually earned more points away than at home, though each has only one season in this window; among teams with three or more seasons, Burnley and Southampton showed the least (0.123) — see Limitations

## Limitations

- Teams that were promoted or relegated during this window (e.g., Leeds, Sunderland, Burnley) have fewer seasons of data than ever-present teams like Liverpool or Man City, so their individual figures carry more noise.
- The analysis doesn't control for strength of opposition — a team's home fixtures in a given season could randomly include tougher or easier opponents than their away fixtures, which could shift the observed gap independent of a "true" home effect.

## What I Learned

I came in comfortable with Python and Pandas but had never written SQL or used Tableau before this project. Rebuilding the same PPG calculation in SQL right after doing it in Pandas was the most useful part — having a known correct answer to check against made it much easier to actually understand the GROUP BY and JOIN logic, rather than just running queries that happened to work. Tableau took a few iterations to get right: my first dashboard had a filter defaulting to a single season instead of all five (which quietly changed the actual numbers, not just the layout), and a chart that was summing values instead of averaging them once multiple seasons were selected — both easy to miss if you don't sanity-check the output against numbers you already trust. Next time, I'd validate each chart's numbers against my Python results as I build, instead of only at the end.

## Possible Next Steps

- Extend the analysis further back to see if the recent trend holds over a longer window
- Break down home advantage by referee, or by attendance/stadium capacity
- Compare against other leagues (La Liga, Bundesliga) to see if the pattern is EPL-specific

## License

The code is released under the [MIT License](LICENSE). Match data comes from [football-data.co.uk](https://www.football-data.co.uk).

---

**Author:** Riyan Qureshi · [LinkedIn](https://www.linkedin.com/in/riyan-qureshi-a7565b253/) · [GitHub](https://github.com/riyanqureshi13)
