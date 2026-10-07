# round-4 — Reconstruct

**Team:** BB-013
**Queries used:** 368 / budget (the export doesn't state the budget). Rounds: R1 150, R2 138, R4 80. 21 are exact repeats, so 347 distinct inputs.

*Evidence base: `findings.json` only. No synthetic or surrogate-generated labels are used anywhere in this report.*

## What we concluded

The system is deterministic and returns a continuous `score` plus an `APPROVE` / `DECLINE` decision. Main findings:

1. **Decision rule:** one score threshold explains every decision. The largest DECLINE score is **0.4870** and the smallest APPROVE score is **0.4908**, so the threshold lies in **(0.4870, 0.4908]**. It is not 0.5.
2. **Port-C override:** when `port == "C"` and `route_age_days < 34`, the output is exactly `{score: 0.043, decision: DECLINE}`. All 22 port-C rows with route age below 34 show it, and none of the other 127 sub-34 rows (ports A, B, D) do. The cliff sits between route age 33.5 (override) and 34.0 (normal score).
3. **Inactive inputs:** `prior_shipments`, `container_count` and `transfers` have no effect. At the mid-point, sweeping each across its full range left the score at 0.3544.
4. **Active inputs (direction of effect on score):**
   - Increases with `discrepancy_ratio`, `recent_seizures`, `shipper_score` and `shipper_years`.
   - Decreases with `route_age_days` and `declared_value`.
   - `port` shifts the score by only about ±0.01 outside the override.
5. **Model behaviour on original data only:** 5-fold cross-validation, grouped by input so repeats never straddle train and test. The best model so far is ExtraTrees on the 6 active inputs plus port, with a learned port-C gate.

| Model (CV on original data) | MAE | RMSE | R² | Decision accuracy |
|---|---|---|---|---|
| Ridge, all features | 0.1056 | 0.1605 | 0.689 | 84.2% |
| Ridge + C-gate (active features) | 0.0686 | 0.1035 | 0.871 | 92.8% |
| RandomForest (active features) | 0.0389 | 0.0747 | 0.933 | 94.5% |
| GradientBoosting + C-gate | 0.0259 | 0.0721 | 0.937 | 96.3% |
| **ExtraTrees + C-gate (active features)** | **0.0288** | **0.0765** | **0.929** | **97.1%** |

For ExtraTrees + C-gate, 24.5% of rows match to 4 decimals, 34.3% fall within ±0.001 and 52.7% within ±0.01. The largest error is 0.93, which is a missed override. 11 rows in the plain ExtraTrees fit (active features, no gate) land on the wrong side of the threshold.

These are my own cross-validation numbers, not your Colab notebook's printed scores. XGBoost wasn't available here, so it wasn't tested. Decision accuracy uses the midpoint 0.4889 as the cut-off.

## How we got there

1. **R1, one-factor sweeps around a mid-point** (declared_value 50, route_age 46.5, shipper_years 20, discrepancy 0.5, shipper_score 600, prior_shipments 10, seizures 2.5, containers 50, transfers 3, port A). Moving one input at a time shows each input's direction cleanly. It also exposed the three flat inputs: four different values each of `container_count`, `prior_shipments` and `transfers` gave identical scores.
2. **R1, port swaps at the same input.** B/C/D differ from A by only 0.003–0.007, so port is a minor effect, except for the C anomaly below.
3. **R1 and R2, short-route probes on port C.** The first 0.043 outputs appeared here. Multi-feature probes (declared_value 0–25, route 18–32) kept returning exactly 0.043, which pointed to a gate and not a smooth effect.
4. **R2, a declared_value sweep (5–25) and mixed multi-feature probes.** The sweep exposed a non-smooth drop between 10 and 15 (0.5155 → 0.4604). The mixed probes checked that single-factor findings hold when several inputs move at once.
5. **R4, targeted port-C route-age probes at 32.5, 33, 33.25, 33.5 and 34.** Each was repeated with other inputs changed (declared_value 9.8–10, discrepancy 0.5–0.97, seizures 2.5–4.44). This showed the override ignores the other inputs and pinned the cliff to (33.5, 34.0]. Probes at 34–40 returned normal scores, so the override is bounded above.
6. **R4, 80 scattered inputs across the full ranges, then CV.** These give the generalisation estimate above. Plain linear models explain only about 69% of variance, so the surface is non-linear. A tree model with a port-C gate does best.

## What we ruled out

- **Decision threshold = 0.5.** Rejected: outputs of 0.4870 and below are DECLINE, and outputs of 0.4908 and above are APPROVE.
- **Decision depends on anything beyond the score.** Rejected: there are zero contradictions in 368 rows, and a single threshold separates all 250 APPROVE from all 118 DECLINE.
- **`prior_shipments`, `container_count`, `transfers` matter.** Rejected: identical scores across their full ranges at the mid-point. In CV, dropping any of the three changes ExtraTrees MAE by about 0.002 (0.0342 → 0.036), which is noise.
- **The 0.043 override happens on other ports.** Rejected: 0 of 127 non-C rows with route below 34 show it.
- **The override depends on the other inputs.** Rejected: it fired for declared_value 0–25, discrepancy 0.25–0.97, seizures 1.25–4.44 and shipper_years 10–40, always with the same score.
- **Noise or randomness.** Rejected: 21 exact repeats returned identical outputs, and no input produced two different scores.
- **An additive linear formula.** Rejected: Ridge reaches R² 0.69 versus 0.93 for tree models, so interactions or non-linearity are present. Adding the gate improves Ridge to 0.87, which shows the override alone explains a lot of the linear model's failure.

## What we are still unsure about

- **The exact override cliff.** It lies in (33.5, 34.0]. We never probed between those two values, and each side was tested with only a few input combinations.
- **Other hidden gates.** We found one (port C, short route). Other ports or input corners may have gates we haven't tested, such as extreme `shipper_score`, `recent_seizures` near 5, or `discrepancy_ratio` near 0 or 1.
- **The exact decision threshold.** It lies in (0.4870, 0.4908], and no probe has landed inside that gap.
- **The score formula.** Even the best model matches only about 25% of rows to 4 decimals (about 34% within ±0.001). We have not recovered the true functional form.
- **The declared_value step.** At the mid-point the score falls by about 0.055 between declared_value 10 and 15 (0.5155 → 0.4604), versus about 0.01 per 5 units from 0 to 10 and about 0.006 per 5 units from 15 to 20. This could be a threshold or a kink, and it is probed at one point only. Between 75 and 100 the score rises very slightly (0.3535 → 0.3546), so it isn't strictly monotone either.
- **Interactions between active inputs.** Effects were measured mostly one at a time around one mid-point. Whether and how they combine is only inferred from model fit.
- **The port effect.** The ±0.01 differences are real but small, and their pattern isn't consistent enough across inputs for us to state a rule.
- **Input domain.** Bounds such as 18–75 for `route_age_days` come from the sampled ranges and your configuration. Behaviour outside them is unknown.