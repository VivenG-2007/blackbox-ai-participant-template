# Experiment 12: port C anomaly (0.0430)

| query | route_age_days | shipper_score | discrepancy | score |
|---|---|---|---|---|
| 42 | 32.25 | 450 | 0.25 | 0.0430 |
| 50 | 18 | 900 | 1.0 | 0.0430 |
| 38 | 46.5 | 600 | 0.5 | 0.3510 (normal) |
| 46 | 60.75 | 750 | 0.75 | 0.6686 (normal) |

Only shared factor among the triggers: port C with route_age_days <= 32.25. Test next: port C, saturated profile, sweep route_age_days 18, 25, 30, 35, 40, 46.5 to find the threshold.
