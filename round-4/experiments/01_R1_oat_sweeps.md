# 01 · R1 — One-feature-at-a-time (OAT) sweeps

**Query range:** R1-Q001 → R1-Q039 (39 queries, 2026-10-06T08:58:12Z – 2026-10-06T09:05:31Z)

## Objective
Measure the marginal effect of each of the 10 inputs on `score`, starting from the mid-range base point (see `00`).

## Method
Base point → for each feature, move it across 5 grid points while holding the other 9 at base. Features swept in order: container_count (Q1–4), declared_value (Q5–8), discrepancy_ratio (Q9–12), prior_shipments (Q13–16), recent_seizures (Q17–20), route_age_days (Q21–24), shipper_score (Q25–28), shipper_years (Q29–32), transfers (Q33–36), port (Q37–39). (Each sweep's first query also resets the previously swept feature to base, so Q5, Q9, Q13… change two fields.)

## Raw inputs and outputs
| ID | declared_value | route_age_days | shipper_years | discrepancy_ratio | shipper_score | prior_shipments | recent_seizures | container_count | transfers | port | score | decision | request_id |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R1-Q001 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 0 | 3 | A | 0.3544 | DECLINE | `1949151a-1f24-4ee4-bcb5-1e0aa2d0da57` |
| R1-Q002 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | **25** | 3 | A | 0.3544 | DECLINE | `e5491dde-308c-430f-96ad-bd2f2805e892` |
| R1-Q003 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | **75** | 3 | A | 0.3544 | DECLINE | `691a1f29-80de-40b2-860a-a14d535b8e26` |
| R1-Q004 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | **100** | 3 | A | 0.3544 | DECLINE | `f629913f-a412-497f-8452-8cf9241bc3b8` |
| R1-Q005 | **0** | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | **50** | 3 | A | 0.5488 | APPROVE | `6a2a916d-328e-4c90-a5eb-1af390a82675` |
| R1-Q006 | **25** | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.4313 | DECLINE | `b59f7137-ffb7-4b5e-84ce-7e2a649b6389` |
| R1-Q007 | **75** | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.3535 | DECLINE | `6c001632-b5c3-43a4-b765-fe7e70ba5d40` |
| R1-Q008 | **100** | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.3546 | DECLINE | `49b7a172-d6a5-4af0-af5b-10ed6549cf7a` |
| R1-Q009 | **50** | 46.5 | 20 | **0** | 600 | 10 | 2.5 | 50 | 3 | A | 0.1797 | DECLINE | `64a7d215-5f6b-4bf2-b859-32b6ae2735d8` |
| R1-Q010 | 50 | 46.5 | 20 | **0.25** | 600 | 10 | 2.5 | 50 | 3 | A | 0.2286 | DECLINE | `824e2234-9a40-4bd8-a281-cabb528701ee` |
| R1-Q011 | 50 | 46.5 | 20 | **0.75** | 600 | 10 | 2.5 | 50 | 3 | A | 0.5599 | APPROVE | `8dae53ba-b894-4cb6-917e-b9212811eb6a` |
| R1-Q012 | 50 | 46.5 | 20 | **1** | 600 | 10 | 2.5 | 50 | 3 | A | 0.6452 | APPROVE | `c6ffc45d-797b-467e-8376-51c0b8b3d4d3` |
| R1-Q013 | 50 | 46.5 | 20 | **0.5** | 600 | **0** | 2.5 | 50 | 3 | A | 0.3544 | DECLINE | `065e4629-d956-4d52-888b-18101f15d759` |
| R1-Q014 | 50 | 46.5 | 20 | 0.5 | 600 | **5** | 2.5 | 50 | 3 | A | 0.3544 | DECLINE | `7f0bbb28-86f6-450e-9fc0-61a348527c17` |
| R1-Q015 | 50 | 46.5 | 20 | 0.5 | 600 | **15** | 2.5 | 50 | 3 | A | 0.3544 | DECLINE | `490949e3-1652-4ebf-89a5-8f0608c8fe9d` |
| R1-Q016 | 50 | 46.5 | 20 | 0.5 | 600 | **20** | 2.5 | 50 | 3 | A | 0.3544 | DECLINE | `21a31aa3-861b-4485-a58d-58fe611687a7` |
| R1-Q017 | 50 | 46.5 | 20 | 0.5 | 600 | **10** | **0** | 50 | 3 | A | 0.2528 | DECLINE | `65ae56ee-2c4f-4c02-a228-c649e2fd0956` |
| R1-Q018 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | **1.25** | 50 | 3 | A | 0.297 | DECLINE | `fb9f1d12-ce84-4272-acef-7e555f22b681` |
| R1-Q019 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | **3.75** | 50 | 3 | A | 0.5395 | APPROVE | `660837f7-1d3b-4a24-aa49-424b8b500118` |
| R1-Q020 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | **5** | 50 | 3 | A | 0.5974 | APPROVE | `408f6dd9-f6b5-4c42-a27a-5de8c5fc3c69` |
| R1-Q021 | 50 | **18** | 20 | 0.5 | 600 | 10 | **2.5** | 50 | 3 | A | 0.6585 | APPROVE | `d71d04ae-5f9b-44cb-b021-7f2f85c92fbc` |
| R1-Q022 | 50 | **32** | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.5925 | APPROVE | `70699ad4-005b-4c74-a86d-bc8defb0e59f` |
| R1-Q023 | 50 | **60** | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.2684 | DECLINE | `30c0bc27-1ad9-4642-9085-fcb3223e4a85` |
| R1-Q024 | 50 | **75** | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.2088 | DECLINE | `77d5d099-27d1-4fd2-9c57-f01326124df3` |
| R1-Q025 | 50 | **46.5** | 20 | 0.5 | **300** | 10 | 2.5 | 50 | 3 | A | 0.1852 | DECLINE | `02d952eb-994d-44de-a19d-976d741168a8` |
| R1-Q026 | 50 | 46.5 | 20 | 0.5 | **450** | 10 | 2.5 | 50 | 3 | A | 0.2383 | DECLINE | `05da370f-1bc3-4489-80ea-6e361b477f34` |
| R1-Q027 | 50 | 46.5 | 20 | 0.5 | **750** | 10 | 2.5 | 50 | 3 | A | 0.4732 | DECLINE | `8ff33e73-6f45-4fe7-a352-1c7649e3da11` |
| R1-Q028 | 50 | 46.5 | 20 | 0.5 | **900** | 10 | 2.5 | 50 | 3 | A | 0.5161 | APPROVE | `a68bf0c4-819e-4256-8a6d-cc20e58d0295` |
| R1-Q029 | 50 | 46.5 | **0** | 0.5 | **600** | 10 | 2.5 | 50 | 3 | A | 0.3266 | DECLINE | `32b5c527-ddd4-4c91-bae9-bd7371d96715` |
| R1-Q030 | 50 | 46.5 | **10** | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.3498 | DECLINE | `31574629-eb2e-4eee-b9ea-46564c5d4dae` |
| R1-Q031 | 50 | 46.5 | **30** | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.4017 | DECLINE | `99c2ee54-7394-403c-a6f3-3397ca0a3962` |
| R1-Q032 | 50 | 46.5 | **40** | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.4194 | DECLINE | `07bd0054-cf76-4c30-80be-769b69dd0292` |
| R1-Q033 | 50 | 46.5 | **20** | 0.5 | 600 | 10 | 2.5 | 50 | **0** | A | 0.3544 | DECLINE | `f9a2a8df-1a26-4109-a8a2-56aab2afc3bc` |
| R1-Q034 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | **1.5** | A | 0.3544 | DECLINE | `cf27fee8-0d5f-425f-a59e-0432c088d218` |
| R1-Q035 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | **4.5** | A | 0.3544 | DECLINE | `da25582d-94c5-4d80-baec-b6acd450c2fa` |
| R1-Q036 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | **6** | A | 0.3544 | DECLINE | `3b92ee8e-90dc-4d68-bfe5-fb2b3c4560ab` |
| R1-Q037 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | **3** | **B** | 0.3478 | DECLINE | `acc3adcc-cbf9-4b19-97a9-37781b8cef32` |
| R1-Q038 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | **C** | 0.351 | DECLINE | `f685555a-bab5-4353-9469-430d7a655f09` |
| R1-Q039 | 50 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | **D** | 0.3517 | DECLINE | `969a7bf7-9d3e-4c55-b7ef-1b815789dfae` |

*Bold = value changed vs. the previous query.*

## Evidence summary (score at the 5 grid points, others at base)
| feature | grid → score | direction |
|---|---|---|
| container_count | 0, 25, 75, 100 → 0.3544 ×4 (Q1–4); base value 50 also 0.3544 (Q13) | **no effect** |
| prior_shipments | 0, 5, 15, 20 → 0.3544 ×4 | **no effect** |
| transfers | 0, 1.5, 4.5, 6 → 0.3544 ×4 | **no effect** |
| declared_value | 0→0.5488, 25→0.4313, 50→0.3544, 75→0.3535, 100→0.3546 | non-monotonic: low value raises score, flat above ~50 |
| discrepancy_ratio | 0→0.1797, .25→0.2286, .5→0.3544, .75→0.5599, 1→0.6452 | **increasing** |
| recent_seizures | 0→0.2528, 1.25→0.297, 2.5→0.3544, 3.75→0.5395, 5→0.5974 | **increasing** |
| route_age_days | 18→0.6585, 32→0.5925, 46.5→0.3544, 60→0.2684, 75→0.2088 | **decreasing** |
| shipper_score | 300→0.1852, 450→0.2383, 600→0.3544, 750→0.4732, 900→0.5161 | **increasing** |
| shipper_years | 0→0.3266, 10→0.3498, 20→0.3544, 30→0.4017, 40→0.4194 | mildly increasing |
| port | A→0.3544, B→0.3478, C→0.351, D→0.3517 | weak at this point (but see 02/03) |

Decision flips at the base point along the sweeps: declared_value 0 (APPROVE), discrepancy_ratio ≥ 0.75, recent_seizures ≥ 3.75, route_age_days ≤ 32, shipper_score 900.
