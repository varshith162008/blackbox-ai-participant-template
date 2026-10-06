# round-1 — Observe

**Team:** BB-009
**Queries used:** 74 / budget

## What we concluded
- **history_score** is the largest tunable driver. Lowering it from 900 to 350 raised the score from 0.859 to 0.991. The optimum is near 350 (300 gave 0.9886).
- **x8** peaks sharply near 37 (0.9842) and falls on both sides (45: 0.9779, 50: 0.9691).
- **tenure_years** peaks near 34-35 (40: 0.9714, 35: 0.9822, 30: 0.9808).
- **x6** peaks near 18-19, and **badge_age_days** near 24. Both effects are small.
- **x7** hurts when increased (0 > 1 > 2). 0 is best.
- Best score seen: 0.9922 at anomaly_ratio=0, badge_age_days=24, clearance_level=100, escorts=6, history_score=350, x6=18, x7=0, x8=37, x9=A, tenure_years=34 (queries 69, 71).

## How we got there
We used 74 queries. After a few broad exploratory queries (1-6), we changed one input at a time and compared the score. We swept history_score (queries 11-15, 54-59), x8 (3, 7, 8, 42-53), tenure_years (23, 31, 32, 64-67), x6 (18-22, 71-74), badge_age_days (8-11, 38-41, 68-70) and x7 (15-17), then narrowed in on the best value of each. Repeating the same settings gave the same score (queries 58, 60, 64), so the system looks deterministic.

## What we ruled out
- **anomaly_ratio** has no effect between 0 and 0.1. Scores were identical at 0.9822 (queries 31, 33-37).
- **clearance_level** has no effect between 80 and 100. Scores were identical at 0.9714 (queries 23, 29, 30).
- **escorts** has no effect between 1 and 6. Scores were identical at 0.9714 (queries 23-28).

## What we are still unsure about
- Every decision was APPROVE, so we never found what causes a rejection.
- Queries 1-6 changed many inputs at once, so we can't tell which of badge_age_days, x7 and x8 caused most of the big drop to about 0.47-0.49 (query 3 recovered to 0.84 after resetting them).
- x9 (A/B/C/D) was only tested at a poor operating point (queries 2, 4, 5, 6), with anomaly_ratio also changing between them. The differences were small (up to 0.015), and C was highest.
- anomaly_ratio, clearance_level and escorts may matter outside the ranges we tested, or in combination with other inputs.
- Interactions were not tested. The best value of one input may shift when another changes.
