# 07 · R4 — Sentinel (port-C) boundary

**Query range:** R4-Q001 → R4-Q028 (28 queries, 2026-10-07T07:52:29Z – 2026-10-07T08:50:51Z)

## Objective
Round 4 targets two things: where the port-C "0.043" override begins and ends in `route_age_days`, and the fine decision threshold in `shipper_score` (continued in `08`).

## Method
- Q1–4: base (declared 10, route 46.5, years 20, ratio .5, shipper_score 600) and shipper_score 565/567/569.
- Q5–11: plateau vector with port D, then port C on base with route_age 27.5, then route_age 34/40/45 on the plateau vector (Q7–9), then seizures changes on the C vector.
- Q12–20: positive controls with port C and route_age ≥ 34 (Q16, Q19, Q20).
- **Q21–28: route_age_days scan under port C — 32.5, 33, 33.25, 33.5 (all 0.043 DECLINE) while Q16 (34) is APPROVE.**

## Raw inputs and outputs
| ID | declared_value | route_age_days | shipper_years | discrepancy_ratio | shipper_score | prior_shipments | recent_seizures | container_count | transfers | port | score | decision | request_id |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R4-Q001 | 10 | 46.5 | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | A | 0.5155 | APPROVE | `fbb5a6b8-7545-4f69-b181-c862300ea1ef` |
| R4-Q002 | 10 | 46.5 | 20 | 0.5 | **565** | 10 | 2.5 | 50 | 3 | A | 0.4929 | APPROVE | `375bdb0e-8922-4ff5-b678-009e1a1a9566` |
| R4-Q003 | 10 | 46.5 | 20 | 0.5 | **567** | 10 | 2.5 | 50 | 3 | A | 0.4938 | APPROVE | `9657f8dd-9ba0-4f03-b1be-cad8f5eb036b` |
| R4-Q004 | 10 | 46.5 | 20 | 0.5 | **569** | 10 | 2.5 | 50 | 3 | A | 0.4937 | APPROVE | `bc81a92e-22d4-4b8c-ad42-ec7540d5bbd4` |
| R4-Q005 | **9.8** | **27.5** | **30** | **0.97** | **880** | **9.8** | **4.44** | **67** | **4.8** | **D** | 0.9805 | APPROVE | `f18c4a01-d333-4f18-9c59-767778e198ff` |
| R4-Q006 | **10** | 27.5 | **20** | **0.5** | **600** | **10** | **2.5** | **50** | **3** | **C** | 0.043 | DECLINE | `0ea82863-abb8-4493-b069-81e080ae2e2c` |
| R4-Q007 | **9.8** | **34** | **30** | **0.97** | **880** | **9.8** | **4.44** | **67** | **4.8** | C | 0.9728 | APPROVE | `cf8f105c-a5d8-4270-be20-910762e039e0` |
| R4-Q008 | 9.8 | **40** | 30 | 0.97 | 880 | 9.8 | 4.44 | 67 | 4.8 | C | 0.9701 | APPROVE | `ec9fa6f2-61c2-44de-b8fa-8ec24f3a493e` |
| R4-Q009 | 9.8 | **45** | 30 | 0.97 | 880 | 9.8 | 4.44 | 67 | 4.8 | C | 0.9492 | APPROVE | `8c70e3f7-fdd8-4d6f-a2c5-3205b9837fd0` |
| R4-Q010 | 9.8 | **27.5** | 30 | 0.97 | 880 | 9.8 | **2.5** | 67 | 4.8 | C | 0.043 | DECLINE | `1dfdf421-516c-443f-80cc-1f8d51c3284b` |
| R4-Q011 | 9.8 | 27.5 | 30 | 0.97 | 880 | 9.8 | **4.44** | 67 | 4.8 | C | 0.043 | DECLINE | `6c480db7-2434-436b-967c-e141dce323ce` |
| R4-Q012 | **10** | **46.5** | **20** | **0.5** | **750** | **10** | **2.5** | **50** | **3** | **A** | 0.6182 | APPROVE | `f9d0fbe7-f818-4f13-8d6a-7c7ecef7ae4e` |
| R4-Q013 | 10 | 46.5 | 20 | 0.5 | **900** | 10 | 2.5 | 50 | 3 | A | 0.6536 | APPROVE | `c0733147-0c9c-4261-96c3-54a1de8dbdcb` |
| R4-Q014 | 10 | 46.5 | 20 | 0.5 | **575** | 10 | **3** | 50 | 3 | A | 0.6413 | APPROVE | `4d44a4d1-6ebd-4b1a-a681-efbb1478315a` |
| R4-Q015 | 10 | 46.5 | 20 | 0.5 | 575 | 10 | **2.5** | 50 | 3 | A | 0.4944 | APPROVE | `c2bb0f24-0a2e-4d60-b170-5b8739624604` |
| R4-Q016 | 10 | **34** | 20 | 0.5 | **600** | 10 | 2.5 | 50 | 3 | **C** | 0.6566 | APPROVE | `0f18707f-00df-41b0-bdb9-19bb78291302` |
| R4-Q017 | **11** | **36** | 20 | **0.52** | **605** | **11** | **2.7** | **51** | 3 | C | 0.6951 | APPROVE | `8c4d9926-f28b-48d1-8f9c-bbe5915d8020` |
| R4-Q018 | **10** | **46.5** | **25** | **0.5** | **550** | **10** | **2.5** | **50** | 3 | **A** | 0.5818 | APPROVE | `80bb33a6-c042-4eee-9a73-d3ecf2c1b72a` |
| R4-Q019 | **37** | **40** | **37** | **0.75** | **587** | **17** | **3.5** | **25** | **5.88** | **C** | 0.8005 | APPROVE | `d6ac93ac-d4bc-484e-ad38-d02847251c95` |
| R4-Q020 | 37 | 40 | 37 | 0.75 | 587 | 17 | 3.5 | 25 | 5.88 | **D** | 0.8002 | APPROVE | `c90da37b-764d-4a3f-9705-5736ff32c2c2` |
| R4-Q021 | **10** | **32.5** | **20** | **0.5** | **600** | **10** | **2.5** | **50** | **3** | **C** | 0.043 | DECLINE | `99bd838d-1a1e-4f9f-aa00-f9460ad9746e` |
| R4-Q022 | 10 | **33** | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | C | 0.043 | DECLINE | `1e5c3e89-892b-4e77-a1f0-38da77da3359` |
| R4-Q023 | 10 | **33.25** | 20 | 0.5 | 600 | 10 | 2.5 | 50 | 3 | C | 0.043 | DECLINE | `94e65a68-e5f4-4b5c-80fc-fb29018ead39` |
| R4-Q024 | 10 | **32.5** | **30** | 0.5 | **880** | 10 | **4.44** | 50 | 3 | C | 0.043 | DECLINE | `f2334c0f-5b3b-4290-b1e8-9b16312a11bc` |
| R4-Q025 | **9.8** | 32.5 | 30 | 0.5 | 880 | 10 | 4.44 | 50 | 3 | C | 0.043 | DECLINE | `517bd73d-1f5d-43e3-949c-64fb91fab86a` |
| R4-Q026 | 9.8 | **33** | 30 | **0.97** | 880 | 10 | 4.44 | 50 | 3 | C | 0.043 | DECLINE | `dde26b78-ce8b-4ce2-ab3b-119014b7bdaf` |
| R4-Q027 | 9.8 | **33.25** | 30 | 0.97 | 880 | 10 | 4.44 | 50 | 3 | C | 0.043 | DECLINE | `9d9681a2-1fe1-424e-b6ad-743771ba3d11` |
| R4-Q028 | 9.8 | **33.5** | 30 | 0.97 | 880 | 10 | 4.44 | 50 | 3 | C | 0.043 | DECLINE | `9a5679d5-4cac-43da-91ea-8a37337885f6` |

*Bold = value changed vs. the previous query.*

## Evidence
- Every query with port C and route_age_days ≤ **33.5** scored exactly **0.043** (R4-Q10, 11, 21–28), including after changing years, shipper_score, recent_seizures, discrepancy_ratio and declared_value (Q24–26) → the override ignores every other input.
- Every query with port C and route_age_days ≥ **34** (Q16: 0.6566, Q19: 0.8005, Q7–9 at 34/40/45 with port D 0.9728/0.9701/0.9492) gives a normal score.
- Dataset-wide check (all 368 rows): **22 rows scored 0.043; all 22 have port = C and route_age_days in [18, 33.5]; no port-C row with route_age ≥ 34 scored 0.043; no non-C row scored 0.043.**
- Boundary is therefore in **(33.5, 34]** for port C. (Plausible hidden rule: `port == 'C' and route_age_days < 34 → score 0.043`.)
