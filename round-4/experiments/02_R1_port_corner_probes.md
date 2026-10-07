# 02 · R1 — Port tested at four profile corners

**Query range:** R1-Q040 → R1-Q052 (13 queries, 2026-10-06T09:06:19Z – 2026-10-06T09:14:02Z)

## Objective
Check whether `port` matters, and whether it interacts with the other features, by switching A→B→C→D at fully specified profiles instead of only at the base point.

## Method
Three composite profiles + one repeat:
- **Q40–43 "mid-low" profile** (25, 32.25, 10, .25, 450, 5, 1.25, 25, 1.5)
- **Q44–47 "mid-high" profile** (75, 60.75, 30, .75, 750, 15, 3.75, 75, 4.5)
- **Q48–51 "max-approve" profile** (0, 18, 40, 1, 900, 10, 5, 50, 3)
- Q52–54: return to port A on the max-approve profile and vary container_count.

## Raw inputs and outputs
| ID | declared_value | route_age_days | shipper_years | discrepancy_ratio | shipper_score | prior_shipments | recent_seizures | container_count | transfers | port | score | decision | request_id |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| R1-Q040 | 25 | 32.25 | 10 | 0.25 | 450 | 5 | 1.25 | 25 | 1.5 | A | 0.2247 | DECLINE | `621e2e53-eda6-4fa2-a468-3f17f3f558f7` |
| R1-Q041 | 25 | 32.25 | 10 | 0.25 | 450 | 5 | 1.25 | 25 | 1.5 | **B** | 0.2351 | DECLINE | `d1bfa215-51fe-4a72-a858-4c085f0100c9` |
| R1-Q042 | 25 | 32.25 | 10 | 0.25 | 450 | 5 | 1.25 | 25 | 1.5 | **C** | 0.043 | DECLINE | `167f4f70-8048-40cb-a4f2-6363b813f2e5` |
| R1-Q043 | 25 | 32.25 | 10 | 0.25 | 450 | 5 | 1.25 | 25 | 1.5 | **D** | 0.2462 | DECLINE | `2caa652b-8e3d-4232-81fd-67bb9d9bd917` |
| R1-Q044 | **75** | **60.75** | **30** | **0.75** | **750** | **15** | **3.75** | **75** | **4.5** | **A** | 0.6708 | APPROVE | `76c76d77-a6aa-4b0b-8df9-4e44fd605cc6` |
| R1-Q045 | 75 | 60.75 | 30 | 0.75 | 750 | 15 | 3.75 | 75 | 4.5 | **B** | 0.6684 | APPROVE | `92c68ebc-bbd1-4ee2-96a4-0733aa7a0cec` |
| R1-Q046 | 75 | 60.75 | 30 | 0.75 | 750 | 15 | 3.75 | 75 | 4.5 | **C** | 0.6686 | APPROVE | `78dc53a2-725c-4c47-8421-5871257ffe56` |
| R1-Q047 | 75 | 60.75 | 30 | 0.75 | 750 | 15 | 3.75 | 75 | 4.5 | **D** | 0.6614 | APPROVE | `0b5feb18-5eba-42c5-8919-902e7496533d` |
| R1-Q048 | **0** | **18** | **40** | **1** | **900** | **10** | **5** | **50** | **3** | **A** | 0.9559 | APPROVE | `1b7f23f4-00a5-48da-9520-55ef70b3ca40` |
| R1-Q049 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | **B** | 0.9546 | APPROVE | `f8cb5c6c-623c-4865-b8b5-e89551066233` |
| R1-Q050 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | **C** | 0.043 | DECLINE | `df244ab1-50b0-41d9-aa52-3d9836cb5b45` |
| R1-Q051 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | 50 | 3 | **D** | 0.9563 | APPROVE | `ae188a7f-2e58-4d8a-816c-5dfaf907d5c4` |
| R1-Q052 | 0 | 18 | 40 | 1 | 900 | 10 | 5 | **0** | 3 | **A** | 0.9559 | APPROVE | `f7ad0ccf-0e15-402c-8574-e4e0be356135` |

*Bold = value changed vs. the previous query.*

## Evidence
- Mid-low profile: A 0.2247, B 0.2351, **C 0.043**, D 0.2462 (Q40–43) → **port C collapses the score** (route_age_days = 32.25 here).
- Mid-high profile: A 0.6708, B 0.6684, C 0.6686, D 0.6614 (Q44–47) → port barely matters (route_age_days = 60.75).
- Max-approve profile: A 0.9559, B 0.9546, **C 0.043 → DECLINE**, D 0.9563 (Q48–51) → score drops from ~0.955 to 0.043, **flipping APPROVE→DECLINE** (route_age_days = 18).
- Q52–54: with port A back, container_count 0/100/50 → 0.9559 unchanged.
- **Hypothesis generated:** port C + low route_age_days triggers a hidden override (confirmed in `03`, `07`).
