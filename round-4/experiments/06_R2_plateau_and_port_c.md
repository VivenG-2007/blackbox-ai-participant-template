# 06 · R2 — Plateau reproduction and port-C checks

**Query range:** R2-Q129 → R2-Q138 (10 queries, 2026-10-07T05:39:04Z – 2026-10-07T05:45:35Z)

## Objective
Reproduce the best R1 vector in a new round, then re-test port C and discrepancy_ratio under it.

## Raw inputs and outputs
| ID | declared_value | route_age_days | shipper_years | discrepancy_ratio | shipper_score | prior_shipments | recent_seizures | container_count | transfers | port | score | decision | request_id |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R2-Q129 | 9.8 | 27.5 | 30 | 0.97 | 880 | 9.8 | 4.44 | 67 | 4.8 | A | 0.9792 | APPROVE | `006a6fce-759c-432d-8455-f4377c3dc362` |
| R2-Q130 | 9.8 | 27.5 | 30 | 0.97 | 880 | 9.8 | 4.44 | 67 | 4.8 | **C** | 0.043 | DECLINE | `7f58c25f-ed47-4ba7-9f35-3d459b6594d5` |
| R2-Q131 | 9.8 | 27.5 | 30 | **0.5** | 880 | 9.8 | 4.44 | 67 | 4.8 | C | 0.043 | DECLINE | `d03841e3-4aa9-4b50-bda7-ab76e16d640d` |
| R2-Q132 | 9.8 | 27.5 | 30 | 0.5 | 880 | 9.8 | 4.44 | 67 | 4.8 | **D** | 0.9303 | APPROVE | `e27a307d-0de6-4317-9b97-89a1db531198` |
| R2-Q133 | 9.8 | **46.5** | 30 | **0.97** | 880 | 9.8 | 4.44 | 67 | 4.8 | **C** | 0.9392 | APPROVE | `7097a8a8-485f-4b27-a030-083b845ca9fc` |
| R2-Q134 | 9.8 | 46.5 | 30 | 0.97 | 880 | 9.8 | 4.44 | 67 | 4.8 | **D** | 0.9362 | APPROVE | `3c4e0cc0-04fe-495c-a9f5-43bde3b13de5` |
| R2-Q135 | 9.8 | **27.5** | 30 | 0.97 | **600** | 9.8 | 4.44 | 67 | 4.8 | **C** | 0.043 | DECLINE | `9e08e5ee-0b27-4487-bc0b-a3efc18c5061` |
| R2-Q136 | 9.8 | 27.5 | 30 | 0.97 | 600 | 9.8 | 4.44 | 67 | 4.8 | **D** | 0.9505 | APPROVE | `ec17e438-479d-42f3-ae59-75ab7026311b` |
| R2-Q137 | **0** | **25** | **35** | **0.7** | **900** | **10** | **3.5** | **50** | **3** | D | 0.9624 | APPROVE | `709d28d2-ea6f-414e-8426-79c5016acd50` |
| R2-Q138 | **9.75** | **27.55** | **30.7** | **0.9** | **895** | **9.7** | **5** | **65** | **4.85** | D | 0.9745 | APPROVE | `f0206997-7851-4ae2-a186-ac2ca995be75` |

*Bold = value changed vs. the previous query.*

## Evidence
- Q129 `(9.8, 27.5, 30, 0.97, 880, 9.8, 4.44, 67, 4.8, A)` → **0.9792 APPROVE**, the same score as the R1 plateau queries R1-Q115–120.
- Exact-vector reproductions across rounds (all identical scores): R1-Q129 = R4-Q5 (0.9805, port D); R1-Q130 = R2-Q130 = R4-Q11 (0.043, port C); R2-Q22 = R2-Q34 = R4-Q1 (0.5155); R2-Q91 = R4-Q16 (0.6566); R2-Q120 = R4-Q18 (0.5818).
- Q130: only port A→C → **0.043 DECLINE**. Q131: additionally discrepancy_ratio 0.97→0.5 → still **0.043** (no other input matters under the override).
- Q132: port D → 0.9303; Q133–134: route_age 46.5 + port C → **0.9392 APPROVE** (route_age above ~34 so C is normal), then D → 0.9362.
- Q135: port C, route_age 27.5, shipper_score 600 → **0.043**; Q136: port D → 0.9505.
- Q137–138: two further plateau vectors → 0.9624 and 0.9745 (APPROVE).
