# round-1 — Observe

**Team:** BB-013
**Queries used:** 150 / 150 (console shows "No queries left"). Only 61 queries were available for this analysis (1-58, 124, 126, 150); the other 89 are not covered.

## What we concluded
- **Decision boundary is about 0.5.** The highest DECLINE was 0.4732 (q27) and the lowest APPROVE was 0.5161 (q28).
- **Four inputs drive the score most.** Score rises with discrepancy_ratio (range 0.47), recent_seizures (0.34) and shipper_score (0.33), and falls with route_age_days (0.45).
- **Smaller effects.** declared_value falls from 0 to 50 and is flat above that (0.20). shipper_years rises slowly (0.09).
- **Port matters little, with one exception.** At the baseline, ports A to D differ by 0.007 at most. Port C returned exactly 0.0430 in two cases (q42, q50).
- **The score plateaus near the top.** The corner point (discrepancy 1, seizures 5, route age 18, shipper_score 900, years 40, declared 0) gives 0.9559, but that is not the ceiling. Interior points (q124, q126, q150) reach 0.980 to 0.981, and near there small input changes move the score by 0.001 or less.

## How we got there
- **One-at-a-time sweeps (q1-39).** We fixed a baseline (container 50, declared 50, discrepancy 0.5, port A, prior shipments 10, seizures 2.5, route age 46.5, shipper score 600, years 20, transfers 3; score 0.3544) and varied one input per query across its range.
- **Port across profiles (q40-47).** We tested ports A to D on a low profile, a baseline profile and a high profile.
- **Saturated profile (q49-58).** We tested ports and the three flat inputs at the top of the score range.
- **Files:** per-feature results are in `experiments/`, the raw table (61 rows: q1-58, 124, 126, 150), and charts are in `plots/`.

## What we ruled out
- **container_count, prior_shipments and transfers have no effect** in the three regimes tested (baseline, high and saturated). Scores were identical to four decimals across their full ranges.
- **Port is not a general driver.** Outside the port C case, it moves the score by about 0.01 or less.
- **The port C result is not a smooth effect.** An identical 0.0430 from two very different inputs points to an override, not a gradual response.

## What we are still unsure about
- **What triggers the port C override.** The only pattern in the data is that both cases had route_age_days of 32.25 or lower. Normal port C queries had 46.5 and 60.75. This is untested.
- **Whether the "inert" inputs ever matter.** They could act at extreme values or in interaction with other inputs.
- **Interactions between inputs.** Every sweep was one at a time, so none were tested directly.
- **Queries 59-123, 125 and 127-149.** They include the other near-zero drops visible in the screenshot and could change any of the above.
- **What the score means.** Higher discrepancy_ratio and recent_seizures raise the score, which is unexpected if it measures risk. Confirm what APPROVE represents.
- **Why interior points beat the corner.** q124, q126 and q150 use port D, container 65, declared 9.75-10, discrepancy 0.9-0.97, prior shipments 9.7-9.8, seizures 4.44-4.46, route age 27.5-27.55, shipper score 880-895, years 30-30.7 and transfers 4.8-4.85, yet score higher than the corner (0.9802, 0.9805, 0.9807). We don't know which input or combination does this.
- **Data gaps.** Query 1 was truncated (assumed baseline). The unlabeled row before q57 is taken as q58.
