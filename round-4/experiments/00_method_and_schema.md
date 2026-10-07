# 00 · Dataset, schema and method

## Source
- File: `findings.json` — team **BB-013**, scope `instance`, count **368** queries, challenge **GK-06** in all rows.
- Rounds present: R1 (150 queries), R2 (138), R4 (80). **R3 is not in the file.**
- Time span: 2026-10-06T08:58:12.541454+00:00 → 2026-10-07T09:25:42.835704+00:00.

## What the system is
A black-box scoring model. Each query sends 10 inputs and receives a `score` in [0,1] and a `decision` (`APPROVE` / `DECLINE`). All experiments are **probing experiments**: we only see inputs → outputs, never the model internals.

## Input schema (all 10 are present in every row)
| field | type | range queried | notes |
|---|---|---|---|
| declared_value | float | 0 – 100 | |
| route_age_days | float | 18 – 75 | |
| shipper_years | float | 0 – 40 | |
| discrepancy_ratio | float | 0 – 1 | |
| shipper_score | float | 300 – 900 | |
| prior_shipments | float | 0 – 20 | |
| recent_seizures | float | 0 – 5 | |
| container_count | float | 0 – 100 | |
| transfers | float | 0 – 6 | |
| port | categorical | A, B, C, D | counts: A=187, B=29, C=44, D=108 |

Outputs: `score` (0.0132 – 0.9807), `decision`. Metadata: `round`, `challenge`, `query_index`, `request_id`, `ts`.

## Standard base point (used by R1 sweeps and R4 start)
`declared_value=50, route_age_days=46.5, shipper_years=20, discrepancy_ratio=0.5, shipper_score=600, prior_shipments=10, recent_seizures=2.5, container_count=50, transfers=3, port=A` — i.e. the midpoint of each range, giving **score 0.3544 → DECLINE** in R1.
Sweep grid used for each feature: 5 evenly spaced points across its range (e.g. discrepancy_ratio 0, .25, .5, .75, 1).

## Decision rule (observed)
- Every APPROVE has score ≥ **0.4908**; every DECLINE has score ≤ **0.487**.
- Closest pair: DECLINE at R4-Q045 (score 0.487) vs APPROVE at R4-Q031, R4-Q032 (score 0.4908). **Threshold lies in (0.487, 0.4908]; consistent with a cut-off of 0.49.** There is no score where the two decisions overlap, so `decision` is a deterministic function of `score` (plus the sentinel rule below, which also fits the same cut-off since its score is 0.043).

## Reproducibility / determinism
- 368 queries contain 347 unique input vectors. 18 vectors were queried more than once (e.g. R4-Q62 repeated, R4-Q77/Q78) and **every repeat returned an identical score** → the model is **deterministic** (no noise).
- Cross-round check: 5 exact vectors recur in more than one round (R1-Q129 = R4-Q5; R1-Q130 = R2-Q130 = R4-Q11; R2-Q22 = R2-Q34 = R4-Q1; R2-Q91 = R4-Q16; R2-Q120 = R4-Q18) and all return identical scores → no evidence the model changed between R1, R2 and R4.

## Files
| file | content |
|---|---|
| `01_R1_oat_sweeps.md` | R1 Q1–39: one-feature-at-a-time sweeps from the base point |
| `02_R1_port_corner_probes.md` | R1 Q40–52: port tested at 4 profile corners |
| `03_R1_irrelevant_feature_tests.md` | R1 Q53–74: container_count / prior_shipments / transfers + port-C anomaly |
| `04_R1_score_maximization.md` | R1 Q75–150: search for the highest score |
| `05_R2_interactions_and_boundary.md` | R2 Q1–128: pairwise interactions + decision-boundary mapping |
| `06_R2_plateau_and_port_c.md` | R2 Q129–138: plateau reproduction + port-C checks |
| `07_R4_sentinel_boundary.md` | R4 Q1–28: sentinel boundary (route_age 33.5 vs 34) |
| `08_R4_threshold_and_random_sampling.md` | R4 Q29–80: shipper_score threshold + 40 random points |
| `09_findings_and_evidence.md` | **Consolidated findings, each with evidence IDs** |
| `10_single_feature_effects.md` | Auto-extracted: every pair of queries differing in exactly one feature |
| `raw_queries.csv` | All 368 rows, machine-readable |
