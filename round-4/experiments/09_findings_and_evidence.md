# 09 · Consolidated findings and evidence

Every finding lists the query IDs (`R<round>-Q<index>`) that support it. Full inputs/outputs for each ID are in the experiment file noted, or in `raw_queries.csv`. **Observed** = directly shown by data; **Inferred** = a reasonable conclusion that the data does not prove.

---

## F1 · The model is deterministic (Observed)
- 368 queries, 347 unique input vectors, 18 vectors repeated; **every repeat gave the identical score**.
- Within round: R4-Q62 = R4-Q61 (0.3193), R4-Q66 = R4-Q65 (0.4791), R4-Q77 = R4-Q78 = R4-Q76 (0.8353).
- Across rounds: R1-Q129 = R4-Q5 (0.9805); R1-Q130 = R2-Q130 = R4-Q11 (0.043); R2-Q22 = R2-Q34 = R4-Q1 (0.5155); R2-Q91 = R4-Q16 (0.6566); R2-Q120 = R4-Q18 (0.5818).
- Files: `00`, `06`, `08`.

## F2 · Decision = score ≥ ~0.49 (Observed bracket, cut-off value inferred)
- Highest DECLINE score: **0.487** (R4-Q45). Lowest APPROVE score: **0.4908** (R4-Q31–35).
- No overlap among 368 rows (250 APPROVE, 118 DECLINE) → threshold ∈ (0.487, 0.4908]; 0.49 is the natural value.
- Tightest input bracket: shipper_score 561 → DECLINE (0.4856, R4-Q29) vs 562 → APPROVE (0.4911, R4-Q30), all else fixed. File: `08`.

## F3 · container_count, prior_shipments and transfers have **no effect** on the output (Observed)
- Across the whole file, pairs of queries that differ **only** in that feature: container_count **0 of 17** changed the score, prior_shipments **0 of 13**, transfers **0 of 16** (`10`).
- R1 sweeps at the base point: R1-Q1–4 (container 0/25/75/100), R1-Q13–16 (prior 0/5/15/20), R1-Q33–36 (transfers 0/1.5/4.5/6) → all 0.3544 (`01`).
- Re-confirmed at the high plateau (R1-Q52–58, R1-Q68–74 → 0.9559 / 0.9563) and at the threshold (R2-Q50–55 → 0.5155) (`02`, `03`, `05`).
- Practical consequence: these three inputs can be set to any value without changing score or decision (they are never used by the model, or their effect is below 4-decimal resolution).

## F4 · Hidden override: port **C** with route_age_days < 34 → score **0.043**, DECLINE, whatever the other inputs (Observed)
- **22 rows** have score exactly 0.043. All 22 have port = C and route_age_days between 18 and 33.5. No non-C row scores 0.043.
- All **44** port-C rows: those with route_age_days ≤ 33.5 → 0.043 (22/22); those with route_age_days ≥ 34 → normal score (22/22; e.g. R4-Q16 at 34 → 0.6566).
- Independence from other inputs: R1-Q65/66/67 (declared 12/25/50), R2-Q130 vs Q131 (discrepancy_ratio 0.97 vs 0.5), R4-Q24–26 (years, shipper_score, seizures, discrepancy_ratio changed) → all exactly 0.043.
- Boundary: R4-Q28 (33.5) → 0.043; R4-Q16 (34) → 0.6566. The cut-off is in (33.5, 34].
- Impact: flips high-scoring profiles from APPROVE (≈0.95–0.98) to DECLINE: R1-Q48/Q50 (0.9559 → 0.043), R1-Q129/Q130 (0.9805 → 0.043), R2-Q132/Q130.
- Files: `02`, `03`, `04`, `06`, `07`.
- **Inferred:** the exact constant 0.043 (not the minimum score in the file — 0.0132, 0.0393, 0.0503 occur elsewhere) and its total independence from other inputs indicate a hard-coded rule or a special leaf, not a smooth part of the learned function.

## F5 · Direction of each informative feature (Observed, R1 one-at-a-time from the base point)
| feature | effect on score | evidence |
|---|---|---|
| discrepancy_ratio | ↑ strong (0.18 → 0.65 over 0 → 1) | R1-Q9–12 |
| recent_seizures | ↑ strong (0.25 → 0.60 over 0 → 5) | R1-Q17–20 |
| shipper_score | ↑ strong (0.19 → 0.52 over 300 → 900) | R1-Q25–28 |
| route_age_days | ↓ strong (0.66 → 0.21 over 18 → 75) | R1-Q21–24 |
| shipper_years | ↑ mild (0.33 → 0.42 over 0 → 40) | R1-Q29–32 |
| declared_value | ↓ then flat: 0 → 0.5488, 25 → 0.4313, 50 → 0.3544, 75–100 ≈ 0.354 | R1-Q5–8 |
| port | tiny: A 0.3544, B 0.3478, C 0.351, D 0.3517 at base | R1-Q37–39 |
| container_count, prior_shipments, transfers | none | F3 |

Note: higher discrepancy_ratio and more recent_seizures **raise** the score and push toward APPROVE. The file does not say what APPROVE means; if it means "low-risk", that direction is counter-intuitive and worth checking against the challenge description.

## F6 · Port (other than C-override) is a small effect (Observed)
- Among 38 port-pair comparisons that exclude the override, the largest score difference is **0.0215** and the mean **0.0043** (all else identical). Examples: R2-Q56–58 (0.5112 / 0.504 / 0.5072), R2-Q96–103, R1-Q44–47 (0.6708 / 0.6684 / 0.6686 / 0.6614).

## F7 · Score behaves like a piecewise-constant, non-additive function (Observed pattern, model class Inferred)
- Flat plateaus: shipper_score 562.5 → 564.5 all 0.4908 (R4-Q31–35); five consecutive discrepancy/other tweaks at the optimum return 0.9792 (R1-Q115–120).
- Same change, different outcome: discrepancy_ratio = 0.3 → 0.5352 APPROVE at route_age 36.5 (R2-Q77) but 0.3117 DECLINE at route_age 56.5 (R2-Q79) → interactions.
- **Inferred:** behaviour is typical of a tree ensemble (random forest / gradient boosting) plus the hard-coded override of F4. Not proven.

## F8 · Extremes found (Observed)
- **Highest score 0.9807** — R1-Q150: `declared 9.75, route 27.55, years 30.7, ratio 0.9, shipper_score 895, prior 9.7, seizures 4.46, container 65, transfers 4.85, port D`. Optimum region: declared ≈ 10, route ≈ 27.5, years ≈ 30, shipper_score ≈ 880–895, seizures ≈ 4.4–4.6 (`04`).
- The 4-corner "obvious" max profile (declared 0, route 18, years 40, ratio 1, shipper_score 900, seizures 5) scores only 0.9559 (R1-Q48) → the optimum is interior, not at the corner.
- **Lowest score 0.0132** — R4-Q39 `(90, 70, 5, 0.2, 350, 10, 0.5, 50, 3, A)`.

## F9 · Decision balance by round
| round | queries | APPROVE | DECLINE |
|---|---|---|---|
| R1 | 150 | 108 | 42 |
| R2 | 138 | 102 | 36 |
| R4 | 80 | 40 | 40 |
| all | 368 | 250 | 118 |

The R4 random sample (R4-Q41–80, 40 points) is the only unbiased sample of the input space: see `08` for its decisions.

---

## Limitations
- **R3 is missing** from `findings.json`; nothing here covers it.
- One challenge only (GK-06). Queries were adaptive (not random), so decision rates above reflect search strategy, not the true population.
- Model internals are never observed; statements about tree ensembles or hard-coded rules are inferences from output patterns.
- Scores are rounded to 4 decimals, so effects smaller than 0.0001 cannot be seen.
