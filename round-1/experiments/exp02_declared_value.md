# Experiment 2: one-at-a-time sweep of `declared_value`

Baseline: {'container_count': 50, 'declared_value': 50, 'discrepancy_ratio': 0.5, 'port': 'A', 'prior_shipments': 10, 'recent_seizures': 2.5, 'route_age_days': 46.5, 'shipper_score': 600, 'shipper_years': 20, 'transfers': 3}

| declared_value | score |
|---|---|
| 0.0 | 0.5488 |
| 25.0 | 0.4313 |
| 50.0 | 0.3544 |
| 75.0 | 0.3535 |
| 100.0 | 0.3546 |

Score range: 0.1953. Pattern: **non-monotonic**.
