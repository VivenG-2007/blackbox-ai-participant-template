# Experiments — Team BB-013 · Challenge GK-06

Black-box probing of a scoring model (inputs → `score` + `APPROVE`/`DECLINE`). Built from `findings.json` (368 queries; rounds R1, R2, R4; R3 absent). Every file contains the **full raw inputs and outputs** of its queries (with `request_id`) plus the evidence derived from them.

## Headline results
1. **Deterministic** model; repeats give identical scores (F1).
2. **Decision cut-off ≈ 0.49** (DECLINE ≤ 0.487, APPROVE ≥ 0.4908) (F2).
3. **container_count, prior_shipments, transfers are ignored** — 0 score changes in 46 one-feature-only comparisons (F3).
4. **Hidden override:** `port == C` and `route_age_days < 34` → score **0.043**, DECLINE, regardless of everything else (22 of 22 rows) (F4).
5. Informative features: discrepancy_ratio, recent_seizures, shipper_score (↑ raises score), route_age_days (↓), shipper_years (mild ↑), declared_value (low ↑) (F5).
6. Best score **0.9807** at R1-Q150; interior optimum, not a corner (F8).

## Reading order
| file | what it holds |
|---|---|
| `00_method_and_schema.md` | Data schema, base point, decision rule, determinism check |
| `01_R1_oat_sweeps.md` | R1 Q1–39 one-at-a-time sweeps |
| `02_R1_port_corner_probes.md` | R1 Q40–52 port at four profiles |
| `03_R1_irrelevant_feature_tests.md` | R1 Q53–74 irrelevant features + first sentinel evidence |
| `04_R1_score_maximization.md` | R1 Q75–150 search for max score |
| `05_R2_interactions_and_boundary.md` | R2 Q1–128 interactions + threshold mapping |
| `06_R2_plateau_and_port_c.md` | R2 Q129–138 reproduction + port C |
| `07_R4_sentinel_boundary.md` | R4 Q1–28 sentinel boundary 33.5 / 34 |
| `08_R4_threshold_and_random_sampling.md` | R4 Q29–80 threshold + 40 random points |
| `09_findings_and_evidence.md` | **All findings with evidence IDs and limitations** |
| `10_single_feature_effects.md` | Every single-feature-change pair, auto-extracted |
| `raw_queries.csv` | All 368 rows |

ID format: `R<round>-Q<query_index>` (e.g. `R4-Q028`). Bold cells in tables mark values that changed vs. the previous query.
