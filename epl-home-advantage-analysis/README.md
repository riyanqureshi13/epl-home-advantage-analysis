# EPL Home Advantage Analysis

Does home-field advantage still hold up in the Premier League — and has it changed in recent seasons?

**🔗 Live Dashboard:** [EPL Home Advantage Analysis on Tableau Public](https://public.tableau.com/app/profile/riyan.qureshi/viz/EPL_HOME_ADVANTAGE_ANALYSIS/Dashboard1)

---

## Overview

Home advantage is one of the most talked-about effects in football, but I wanted to actually measure it rather than just assume it — how big is it, which teams benefit most, and is it growing or shrinking over the last few years?

This project uses 5 seasons of Premier League match data to answer three questions:
1. How much of a points advantage do teams get from playing at home?
2. Which teams benefit most (and least)?
3. Has that advantage changed over the past 5 seasons?

## Data

- **Source:** [football-data.co.uk](https://www.football-data.co.uk) — free, publicly available match-level data
- **Scope:** 5 Premier League seasons, 2021/22 through 2025/26 (~1,900 matches)
- **Fields used:** match results, half-time/full-time scores, shots, corners, cards, referee

## Tools & Method

| Stage | Tool |
|---|---|
| Data cleaning & combining seasons | Python (Pandas, NumPy) |
| Computing home/away Points-Per-Game (PPG) | Pandas `groupby` |
| Validating the calculation independently | SQL (SQLite) |
| Testing statistical significance | Paired t-test (`scipy.stats`) |
| Dashboard & visualization | Tableau Public |

Each match result was converted to points (win = 3, draw = 1, loss = 0), then averaged per team, per season, split by home vs. away. The same calculation was rebuilt in SQL as a cross-check against the Pandas result.

## Key Findings

- **League-wide home PPG:** 1.55 vs. **away PPG:** 1.17 — a gap of ~0.38 points per game
- Scaled across a 38-game season, that's **~7.2 points of home advantage per season**
- A paired t-test confirms this gap is statistically significant (t = 4.087, **p = 0.00063**) — not random variation
- Year-to-year, the gap is volatile — swinging between 0.18 and 0.59 points per game across the 5 seasons — but the linear trend line slopes downward overall, from ~0.43 at the start of the window to ~0.29 by the end, suggesting home advantage may be gradually easing rather than staying fixed
- Newcastle showed the strongest home advantage over the 5 seasons (0.663 PPG), with Liverpool and Sunderland close behind. At the other end, Burnley showed almost none (0.123) — though some lower teams also have fewer seasons of data in this window (see Limitations)

## Limitations

- Teams that were promoted or relegated during this window (e.g., Leeds, Sunderland, Burnley) have fewer seasons of data than ever-present teams like Liverpool or Man City, so their individual figures carry more noise.
- The analysis doesn't control for strength of opposition — a team's home fixtures in a given season could randomly include tougher or easier opponents than their away fixtures, which could shift the observed gap independent of a "true" home effect.

## What I Learned

I came in comfortable with Python and Pandas but had never written SQL or used Tableau before this project. Rebuilding the same PPG calculation in SQL right after doing it in Pandas was the most useful part — having a known correct answer to check against made it much easier to actually understand the GROUP BY and JOIN logic, rather than just running queries that happened to work. Tableau took a few iterations to get right: my first dashboard had a filter defaulting to a single season instead of all five (which quietly changed the actual numbers, not just the layout), and a chart that was summing values instead of averaging them once multiple seasons were selected — both easy to miss if you don't sanity-check the output against numbers you already trust. Next time, I'd validate each chart's numbers against my Python results as I build, instead of only at the end.

## Possible Next Steps

- Extend the analysis further back to see if the recent trend holds over a longer window
- Break down home advantage by referee, or by attendance/stadium capacity
- Compare against other leagues (La Liga, Bundesliga) to see if the pattern is EPL-specific

---

**Author:** Riyan Qureshi
