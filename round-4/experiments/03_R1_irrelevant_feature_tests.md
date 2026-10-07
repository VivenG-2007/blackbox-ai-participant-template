# 03 · R1 — Irrelevant-feature tests and the port-C anomaly

**Query range:** R1-Q053 → R1-Q074 (22 queries, 2026-10-06T09:14:10Z – 2026-10-06T09:22:41Z)

## Objective
(a) Confirm that container_count, prior_shipments and transfers have zero effect even at the high-score plateau; (b) isolate the port-C anomaly seen in `02`.

## Method
On the max-approve profile (score ≈ 0.956): toggle prior_shipments (0/20), transfers (0/6), container_count (0/50/100); then vary declared_value / recent_seizures / route_age_days / shipper_score slightly; then change port to C (Q65) and sweep declared_value (Q66–67); then a "all three irrelevant features at 0, port D" profile (Q68) and repeated toggles of transfers and prior_shipments (Q69–74).

## Raw inputs and outputs
| ID | declared_value | route_age_days | shipper_years | discrepancy_ratio | shipper_score | prior_shipments | recent_seizures | container_count | transfers | port | score | decision | request_id |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R1-Q053 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | 100 | 3 | A | 0.9559 | APPROVE | `03d15e8d-078a-4c78-a292-3300db1283be` |
| R1-Q054 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | **50** | 3 | A | 0.9559 | APPROVE | `ea46c5a4-9157-47fa-badc-ba086d6b0f3b` |
| R1-Q055 | 0 | 18 | 40 | 1 | 900 | **0** | 5 | 50 | 3 | A | 0.9559 | APPROVE | `3b45c612-fc4c-4c6f-bf5a-9f5a2302e6d6` |
| R1-Q056 | 0 | 18 | 40 | 1 | 900 | **20** | 5 | 50 | 3 | A | 0.9559 | APPROVE | `0c652fbb-17d8-471d-9509-90bf04d51233` |
| R1-Q057 | 0 | 18 | 40 | 1 | 900 | **10** | 5 | 50 | **6** | A | 0.9559 | APPROVE | `cac09234-2c79-4ff0-a968-7f600647d24f` |
| R1-Q058 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | 50 | **0** | A | 0.9559 | APPROVE | `8cd9cb64-141a-46d2-b0e2-08d2d1672368` |
| R1-Q059 | **3** | 18 | 40 | 1 | 900 | 10 | 5 | 50 | **3** | **D** | 0.9583 | APPROVE | `d671db6d-f584-43bd-987a-bf2db282e618` |
| R1-Q060 | **8** | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | D | 0.957 | APPROVE | `ff0f3e25-7840-4f8a-876d-367627c50934` |
| R1-Q061 | **0** | 18 | 40 | 1 | 900 | 10 | **4** | 50 | 3 | D | 0.9581 | APPROVE | `9153cbde-e003-41d0-bfb0-abca0704ad51` |
| R1-Q062 | 0 | **22** | 40 | 1 | 900 | 10 | **5** | 50 | 3 | D | 0.9612 | APPROVE | `bf3994dd-8afe-40f2-98e6-eb7c4b09647e` |
| R1-Q063 | 0 | **18** | 40 | 1 | **850** | 10 | 5 | 50 | 3 | D | 0.9523 | APPROVE | `9745d9c4-7bb3-4e20-949a-7fd9daed80ab` |
| R1-Q064 | 0 | **22** | 40 | 1 | 850 | 10 | 5 | 50 | 3 | D | 0.957 | APPROVE | `a97fff39-2093-4539-993b-6757aab38556` |
| R1-Q065 | **12** | **18** | 40 | 1 | **900** | 10 | 5 | 50 | 3 | **C** | 0.043 | DECLINE | `c5cdce0f-ced7-4e4a-8055-cfc63b5ca57d` |
| R1-Q066 | **25** | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | C | 0.043 | DECLINE | `27cab3d7-c626-47ba-bcba-abe4c1fb9c20` |
| R1-Q067 | **50** | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | C | 0.043 | DECLINE | `bd3a77ee-0442-4e07-8af0-c3a9b04db740` |
| R1-Q068 | **0** | 18 | 40 | 1 | 900 | **0** | 5 | **0** | **0** | **D** | 0.9563 | APPROVE | `00570a22-a5f3-4f0f-b40a-a987610c5ec8` |
| R1-Q069 | 0 | 18 | 40 | 1 | 900 | 0 | 5 | 0 | **6** | D | 0.9563 | APPROVE | `603a14de-e076-4010-9b54-39061db952f2` |
| R1-Q070 | 0 | 18 | 40 | 1 | 900 | **20** | 5 | 0 | **0** | D | 0.9563 | APPROVE | `7bda67ef-9a4b-436e-b745-7324289f1882` |
| R1-Q071 | 0 | 18 | 40 | 1 | 900 | **0** | 5 | **100** | 0 | D | 0.9563 | APPROVE | `3e215402-4de1-4172-8081-7cf98d7559c2` |
| R1-Q072 | 0 | 18 | 40 | 1 | 900 | 0 | 5 | 100 | **6** | D | 0.9563 | APPROVE | `b56b56ac-6599-4ba2-aafc-d985d7b9c3e5` |
| R1-Q073 | 0 | 18 | 40 | 1 | 900 | **20** | 5 | 100 | **0** | D | 0.9563 | APPROVE | `572e1ac8-06ab-4519-8ffb-be62a5a47516` |
| R1-Q074 | 0 | 18 | 40 | 1 | 900 | 20 | 5 | 100 | **6** | D | 0.9563 | APPROVE | `0a11b15e-92d0-4d56-a308-ac0ef03d6a30` |

*Bold = value changed vs. the previous query.*

## Evidence
- **Irrelevant features:** Q53–58 and Q68–74 change container_count (0↔50↔100), prior_shipments (0↔10↔20) and transfers (0↔3↔6) and the score stays **exactly 0.9559 / 0.9563** every time.
- Small moves of declared_value, recent_seizures, route_age_days and shipper_score (Q59–64) shift the score only in the 3rd–4th decimal (0.9523–0.9612), i.e. the model is already saturated at this corner.
- **Port-C anomaly:** Q65 (port C, route_age_days 18, shipper_score 900) → **0.043 DECLINE**, and Q66 (declared_value 25) and Q67 (declared_value 50) give the same **0.043**: once port=C & route_age=18, none of the other inputs changes the output.
