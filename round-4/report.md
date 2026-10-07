# round-4 — Reconstruct

**Team:** BB-009  
**Queries used:** 111 observations from Round 1 and Round 2

## What we concluded

We reconstructed the observed behavior of the GK-05 building access control system using the available Round 1 and Round 2 observations.

The reconstruction uses a Random Forest Regression model to predict the GK-05 confidence score from the observed input features.

The model was trained using 111 collected observations and 13 input features after preprocessing.

The internal evaluation achieved:

- R² Score: 0.8557
- R² Percentage: 85.57%
- Mean Absolute Error (MAE): 0.0151
- Root Mean Squared Error (RMSE): 0.0454

A known baseline configuration produced a GK-05 confidence score of 0.9922. Our reconstructed model predicted 0.9918, giving an absolute difference of 0.0004.

The reconstruction therefore provides a useful approximation of the observed GK-05 scoring behavior.

## How we got there

We collected the observed GK-05 inputs and confidence scores from Round 1 and Round 2.

The input features used were:

- clearance_level
- badge_age_days
- tenure_years
- anomaly_ratio
- history_score
- linked_badges
- recent_denials
- requested_zone
- escorts
- site

The categorical `site` feature was converted using one-hot encoding.

A Random Forest Regression model was selected because the observed black-box behavior appeared nonlinear and could involve interactions between multiple features.

Feature importance analysis indicated that the most influential observed features were:

1. recent_denials
2. badge_age_days
3. anomaly_ratio
4. requested_zone
5. history_score
6. linked_badges

The reconstruction was also checked against a known GK-05 baseline configuration.

## What we ruled out

Based on the observed experiments, several features showed little or no clear independent effect in the tested configurations.

Clearance level showed little observed effect when changed from the baseline.

The number of escorts also showed little observed effect across the tested values.

Anomaly ratio showed limited effect in several controlled experiments, although interactions cannot be completely ruled out.

We also found that the decision output should not be assumed to use a simple 0.5 confidence threshold because an observed confidence score below 0.5 was still returned with an APPROVE decision.

Therefore, the reconstruction focuses primarily on predicting the confidence score rather than assuming a fixed decision threshold.

## What we are still unsure about

The original GK-05 scoring algorithm is a black box, so the reconstructed Random Forest model is an approximation rather than an exact reproduction.

The available observations do not cover every possible combination of input features.

Some features may have nonlinear effects or interactions that are not fully captured by the collected observations.

In particular, the effect of site appeared to depend on the number of linked badges, suggesting feature interactions.

Therefore, additional unseen queries would be useful to further validate the generalization of the reconstructed model.
