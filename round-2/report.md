# Round 2 — Investigate

**Team:** BB-009  
**Queries used:** 36  
**Budget:** 170  

## What we concluded

We investigated the effect of the input parameters on the confidence score and tested multiple two-parameter combinations to identify interactions.

### Independent feature effects

| Feature | Observed change | Effect on confidence |
|---|---|---|
| `history_score` | Decreased from 350 to 300 | Decreases |
| `badge_age_days` | Decreased from 24 to 20 | Decreases |
| `tenure_years` | Changed from 34 to 30 | Decreases |
| `tenure_years` | Changed from 34 to 40 | Decreases |
| `linked_badges` | Decreased from 19 to 15/10 | Decreases |
| `recent_denials` | Increased from 0 to 2 | Decreases |
| `requested_zone` | Increased from 37 to 40 | Decreases |
| `requested_zone` | Decreased from 37 to 35 | Slight increase |
| `site` | Changed from A to B/C/D | Decreases |

### Features with no noticeable independent effect

The following changes produced the same score in our tests:

- `clearance_level`
- `escorts`
- `anomaly_ratio`

For example, changing `clearance_level` from 100 to 50 and then to 0 kept the score at 0.9922. Changing `escorts` from 6 to 0 also kept the score at 0.9922. Changing `anomaly_ratio` from 0 to 0.5 produced no observed score change when tested independently.

---

## How we got there

We started from the best Round 1 configuration, which produced a confidence score of **0.9922**:

```text
clearance_level = 100
badge_age_days = 24
tenure_years = 34
anomaly_ratio = 0
history_score = 350
linked_badges = 19
recent_denials = 0
requested_zone = 37
escorts = 6
site = A
