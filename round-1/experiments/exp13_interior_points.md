# Experiment 13: interior points (q124, q126, q150)

Not one-at-a-time sweeps; these are late queries read from console screenshots. All use port D, container 65.

| query | declared | discrepancy | prior | seizures | route_age | shipper_score | years | transfers | score |
|---|---|---|---|---|---|---|---|---|---|
| 124 | 10 | 0.97 | 9.8 | 4.44 | 27.5 | 880 | 30 | 4.8 | 0.9802 |
| 126 | 9.75 | 0.97 | 9.8 | 4.44 | 27.5 | 880 | 30 | 4.8 | 0.9805 |
| 150 | 9.75 | 0.9 | 9.7 | 4.46 | 27.55 | 895 | 30.7 | 4.85 | 0.9807 |

Observations:
- All score above the saturated corner (0.9559: discrepancy 1, seizures 5, route 18, score 900, years 40, declared 0).
- Between q126 and q150 the score moved only +0.0002 despite changes in discrepancy (0.97 to 0.9), prior_shipments and seizures, so the region near 0.98 is nearly flat.
- Which input or combination lifts these above the corner is untested.
- Source note: q124 vs previous changed route_age_days; q126 vs previous changed declared_value (per console). Queries 125 and 127-149 are not available.
