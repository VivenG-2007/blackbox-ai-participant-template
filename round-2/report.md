# Round-2 — Investigation Report

| Item | Value |
| :--- | :--- |
| **Team** | BB-013 |
| **Dashboard queries used**  |
| **Evidence reviewed** | 138 supplied Round-2 observations, plus supplied Round-1 observations |
| **Method** | Controlled comparisons of black-box model outputs |
| **Best observed score** | 0.9807 |
| **Status** | Investigation ongoing |

> **Query accounting:** The dashboard reports 0 queries used. The supplied dataset contains Round-2 query IDs 1–138. Observations reviewed are not necessarily queries charged to the current reporting session. The budget limit was not supplied.

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Data Interpretation](#2-data-interpretation)
3. [What We Concluded](#3-what-we-concluded)
4. [How We Got There](#4-how-we-got-there)
5. [Interactions](#5-interactions)
6. [Decision Boundary](#6-decision-boundary)
7. [What We Ruled Out](#7-what-we-ruled-out)
8. [What We Are Still Unsure About](#8-what-we-are-still-unsure-about)
9. [Current Model Hypothesis](#9-current-model-hypothesis)
10. [Next Best Queries](#10-next-best-queries)
11. [Confidence Assessment](#11-confidence-assessment)

---

## 1. Executive Summary

The observations support a **nonlinear, context-dependent scoring function**. The approval decision is consistent with a separate, fixed score threshold.

### Main findings

- **Port C has a strong route-age-dependent effect** in the tested high-score configuration.
- **Shipper score is locally non-monotonic:** increasing it can reduce the output under certain seizure settings.
- **Recent seizures do not always increase score:** they help around the baseline but hurt when increased from 4.46 to 5 at the best observed configuration.
- **Column 3 increases score nonlinearly** in the sampled baseline region.
- **Container count increases score locally**, with smaller gains at higher tested counts.
- **Declared value, prior shipments, and transfers are locally inactive**, but global irrelevance is not established.
- All supplied decisions are consistent with:

  **0.4824 < approval threshold ≤ 0.4912**

- The highest observed score is **0.9807**. No supplied query reaches **0.99**.

> These are empirical findings, not a complete reconstruction of the hidden model.

---

## 2. Data Interpretation

### 2.1 Input mapping remains unresolved

The original schema places `discrepancy_ratio` second and `shipper_years` third. However, observed rows resemble:

```text
50,10,0.5,A,10,2.5,46.5,600,20,3
```

Column 2 contains values exceeding the stated discrepancy-ratio range of 0–1.

Swapping the two labels appears plausible, but column 2 also contains values exceeding the stated shipper-years maximum of 40.

Therefore, the report uses **raw input positions** where necessary.

| Position | Working label | Mapping status |
| :--- | :--- | :--- |
| x1 | declared_value | No identified conflict |
| x2 | Column 2; provisionally shipper_years | Label and range unresolved |
| x3 | Column 3; provisionally discrepancy_ratio | Plausible, unconfirmed |
| x4 | port | No identified conflict |
| x5 | prior_shipments | No identified conflict |
| x6 | recent_seizures | No identified conflict |
| x7 | route_age_days | No identified conflict |
| x8 | shipper_score | No identified conflict |
| x9 | container_count | No identified conflict |
| x10 | transfers | No identified conflict |

### 2.2 Evidence standard

A single-feature effect is supported only when **all other inputs remain fixed**.

Comparisons that change multiple inputs are **confounded** and cannot establish individual feature effects.

Scores are displayed to four decimal places. Determinism and threshold findings therefore apply at the reported precision.

---

## 3. What We Concluded

### Feature Relation Table

| Feature | Direction | Shape | Important region | Confidence | Evidence |
| :--- | :--- | :--- | :--- | :--- | :--- |
| x1: declared_value | Locally inactive | WEAK/NEGLIGIBLE | Baseline 25–75 | HIGH locally | 25, 50, 75 all score 0.5155 |
| x2: column 2 | Negative locally | PIECEWISE | Stronger decline 10–15 | HIGH for raw column | 5/10/15/20 → 0.5385/0.5155/0.4604/0.4546 |
| x3: column 3 | Positive locally | Nonlinear, monotonic in sampled region | 0.5–0.6 | HIGH locally | Unequal positive increments |
| x4: port | Context-dependent | Conditional categorical effect | C at short routes in strong configuration | HIGH | C–D difference changes dramatically with age |
| x5: prior_shipments | Locally inactive | WEAK/NEGLIGIBLE | Baseline 5–15 | HIGH locally | All score 0.5155 |
| x6: recent_seizures | Context-dependent | NON_MONOTONIC across contexts | Baseline 2.5–3; strong anchor 4.46–5 | HIGH | Helps locally, hurts at strong anchor |
| x7: route_age_days | Context-dependent | Nonlinear / interaction-dependent | A baseline and strong C configuration | HIGH locally | Opposite effects across tested ports |
| x8: shipper_score | Context-dependent | Locally NON_MONOTONIC | 550–600 at seizures 3 | HIGH | Reproduced output decrease |
| x9: container_count | Positive locally | PIECEWISE with flattening | 20–25 and 30–40 | HIGH locally | Stronger gain at 20–25 |
| x10: transfers | Locally inactive | WEAK/NEGLIGIBLE | Baseline 1.5–4.5 | HIGH locally | All score 0.5155 |

### Scope of conclusions

**Local relationships**

- Positive column-3 and container-count responses.
- Negative column-2 and route-age responses around the A baseline.
- Inactivity of declared value, prior shipments, and transfers.

**Interaction-dependent relationships**

- Port effects.
- Route-age effects.
- Shipper-score effects.
- Recent-seizure effects.

**Consistent across supplied decisions**

- A fixed score threshold explains every observed label.

No single-feature relationship has been established over the entire input domain.

---

## 4. How We Got There

### 4.1 Baseline and anchors

#### Stable baseline

```text
50,10,0.5,A,10,2.5,46.5,600,20,3
```

- **Score:** 0.5155
- **Decision:** APPROVE
- **Evidence:** R2-22 and R2-34

#### Best observed configuration

```text
65,9.75,0.9,D,9.7,4.46,27.55,895,30.7,4.85
```

- **Score:** 0.9807
- **Decision:** APPROVE
- **Evidence:** R1-150

#### Port-C low-score anchor

```text
67,9.8,0.97,C,9.8,4.44,27.5,880,30,4.8
```

- **Score:** 0.0430
- **Decision:** DECLINE
- **Evidence:** R1-130 and R2-130

### 4.2 Column-3 response curve

All other baseline inputs remain fixed.

| Column 3 | Score | Change from previous point |
| ---: | ---: | ---: |
| 0.300 | 0.3917 | — |
| 0.400 | 0.4353 | +0.0436 |
| 0.500 | 0.5155 | +0.0802 |
| 0.525 | 0.5325 | +0.0170 |
| 0.550 | 0.5675 | +0.0350 |
| 0.575 | 0.5856 | +0.0181 |
| 0.600 | 0.6451 | +0.0595 |
| 0.700 | 0.6841 | +0.0390 |

**Interpretation:** The response increases at every sampled point, but the rate of increase varies. This establishes local nonlinearity, not an exact discontinuity.

### 4.3 Recent-seizure reversal

At the baseline:

| Seizures | Score |
| ---: | ---: |
| 1.5 | 0.4613 |
| 2.5 | 0.5155 |
| 3.0 | 0.6305 |
| 3.5 | 0.6413 |

At the best observed configuration:

| Seizures | Score | Evidence |
| ---: | ---: | :--- |
| 4.46 | 0.9807 | R1-150 |
| 5.00 | 0.9745 | R2-138 |

Changing only seizures from 4.46 to 5 decreases score by **0.0062**.

**Conclusion:** A globally monotonic positive seizure effect is contradicted. The optimal seizure value remains unknown.

### 4.4 Reproduced shipper-score reversal

At seizures 3, all other inputs fixed:

| Shipper score | Model output | Evidence |
| ---: | ---: | :--- |
| 550 | 0.6386 | R2-88 and R2-124 |
| 600 | 0.6305 | R2-35 and R2-125 |

The output decreases by **0.0081**.

At seizures 1.5, the same input change increases output from **0.4004 to 0.4613**, a gain of **0.0609**.

**Interaction contrast:**

```text
-0.0081 - (+0.0609) = -0.0690
```

### 4.5 Port-C route-age experiment

All numeric inputs are matched except route age.

| Route age | Port C score | Port D score | C minus D |
| ---: | ---: | ---: | ---: |
| 27.5 | 0.0430 | 0.9805 | -0.9375 |
| 46.5 | 0.9392 | 0.9362 | +0.0030 |

Changing age from 27.5 to 46.5 produces:

- **Port C:** +0.8962
- **Port D:** -0.0443

**Difference-in-differences:**

```text
+0.8962 - (-0.0443) = +0.9405
```

This is the strongest identified interaction.

> Port C scores 0.6566 at age 34 under another baseline. Therefore, the collapse is not yet explainable as a universal “Port C plus short route” rule.

### 4.6 Conditional flat region

At the short-route C anchor:

- Changing column 3 from 0.97 to 0.5 leaves score at **0.0430**.
- Changing shipper score from 880 to 600 leaves score at **0.0430**.

Matched D configurations respond to both changes.

This supports a **possible conditional flat branch or override**, but does not establish global clipping or the complete trigger.

---

## 5. Interactions

| Feature A | Feature B | Evidence | Strength | Next test |
| :--- | :--- | :--- | :--- | :--- |
| port | route_age_days | Interaction contrast +0.9405 | Very strong | Intermediate matched C/D ages |
| recent_seizures | shipper_score | 550→600 effect changes sign | Strong | Add 575 at seizures 3 |
| port | column 3 | C remains flat while D responds | Strong locally | Retest after C exits collapse |
| port | shipper_score | C remains flat while D responds | Strong locally | Test longer-route C/D configurations |
| route_age_days | shipper_score | Score-input gains vary by age | Moderate | Complete matched curves |
| shipper_score | container_count | Container gains vary by score setting | Moderate | Expand controlled grid |

> These are interactions on the observed score scale. They do not necessarily prove explicit interaction terms inside the hidden model.

---

## 6. Decision Boundary

| Observation | Score | Query |
| :--- | ---: | :--- |
| Highest observed DECLINE | 0.4824 | R2-121 |
| Lowest observed APPROVE | 0.4912 | R2-20 |

Assuming:

```text
APPROVE if score >= threshold
```

the supported interval is:

```text
0.4824 < threshold <= 0.4912
```

These bounds are subject to displayed-score rounding.

A threshold of **0.5 is contradicted** by approvals below 0.5.

Additional decision logic outside tested configurations cannot be excluded, but is not required to explain the supplied labels.

---

## 7. What We Ruled Out

| Hypothesis | Assessment | Reason |
| :--- | :--- | :--- |
| Approval threshold is 0.5 | Contradicted | Scores below 0.5 are approved |
| Port C is globally unfavorable | Contradicted | Matched long-route C slightly exceeds D |
| Seizures always increase score | Contradicted | 4.46→5 lowers the strong-anchor score |
| Shipper score always increases output | Contradicted | Reproduced 550→600 reversal |
| Score is a simple raw-input linear function | Inconsistent with observations | Unequal increments and changing marginal effects |
| Locally inactive fields are globally irrelevant | Unsupported | Wider-domain behavior is untested |
| 0.0430 is a universal score floor | Unsupported | Only a conditional flat region is observed |
| 0.9807 is the global maximum | Unsupported | No exhaustive search or upper-bound proof |
| 0.99 is guaranteed attainable | Unsupported | No supplied observation reaches 0.99 |

### Corrections to earlier interpretations

- Shipper-score values 450 and 750 use column 2 = 25, not the baseline value 10.
- Route age 60 also uses column 2 = 25.
- These points cannot be appended to the column-2=10 baseline curve without introducing confounding.
- R2-137 changes several inputs simultaneously. Its score of 0.9624 cannot identify an individual feature effect.

---

## 8. What We Are Still Unsure About

1. **Input mapping:** Exact labels and valid ranges for columns 2 and 3.
2. **Port-C trigger:** Whether route age acts alone or alongside other conditions.
3. **Transition shape:** Abrupt threshold, smooth transition, or multiple regions.
4. **High-score optimum:** Best seizure, route-age, ratio, and container settings.
5. **Global inactivity:** Whether x1, x5, and x10 matter elsewhere.
6. **Architecture:** Trees, rules, nonlinear additive models, and other structures remain possible.
7. **Output ceiling:** Whether 0.99 is attainable.
8. **Global determinism:** Repeats are stable, but only limited configurations were repeated.

---

## 9. Current Model Hypothesis

```text
The score is nonlinear and context-dependent.

Under certain Port-C conditions:
    the score enters a flat low-output region near 0.0430.

Outside that region:
    numeric feature effects vary with the operating context.

The decision is consistent with a separate fixed score threshold.
```

This is a working description, not a reconstructed mathematical formula.

---

## 10. Next Best Queries

Use the existing raw column order:

```text
value, column_2, column_3, port, prior_shipments,
seizures, route_age, shipper_score, containers, transfers
```

These are proposed queries, not completed observations.

| ID | Input vector | Purpose |
| :--- | :--- | :--- |
| N1 | `50,10,.5,A,10,2.5,46.5,565,20,3` | Narrow decision boundary |
| N2 | `50,10,.5,A,10,2.5,46.5,562.5,20,3` | Probe lower boundary region |
| N3 | `50,10,.5,A,10,2.5,46.5,567.5,20,3` | Probe upper boundary region |
| N4 | `67,9.8,.97,C,9.8,4.44,37,880,30,4.8` | Test intermediate C route age |
| N5 | `67,9.8,.97,D,9.8,4.44,37,880,30,4.8` | Matched D control |
| N6 | `67,9.8,.97,C,9.8,4.44,32,880,30,4.8` | Test shorter-age C region |
| N7 | `67,9.8,.97,D,9.8,4.44,32,880,30,4.8` | Matched D control |
| N8 | `67,9.8,.97,C,9.8,4.44,42,880,30,4.8` | Test longer-age C region |
| N9 | `67,9.8,.97,D,9.8,4.44,42,880,30,4.8` | Matched D control |
| N10 | `50,10,.5,A,10,3,46.5,575,20,3` | Resolve score-reversal shape |

### Adaptive execution

1. Run **N1** to narrow the decision boundary.
2. Run **N4 and N5** to investigate the Port-C transition.
3. Select subsequent values between the nearest informative endpoints.
4. Avoid assuming that either response is linear or globally monotonic.

---

## 11. Confidence Assessment

| Conclusion | Confidence |
| :--- | :--- |
| Port × route-age interaction at tested strong anchor | HIGH |
| Reproduced shipper-score reversal | HIGH |
| Local nonlinear column-3 response | HIGH |
| Local container-count increase and flattening | HIGH |
| Seizure effect is not globally positive | HIGH |
| Fixed threshold fits supplied decisions | HIGH |
| Tested repeats are stable at displayed precision | HIGH |
| Conditional Port-C flat branch | MEDIUM |
| Exact C trigger and transition location | LOW |
| Unique model architecture | LOW |
| Accurate global prediction formula | LOW |
| Ability to achieve 0.99 | LOW / unresolved |

---

## Final Assessment

Round-2 establishes useful local response curves, a narrow decision-threshold interval, and a strong Port-C interaction.

The evidence is **not sufficient to guarantee a score of 0.99 or accurately predict arbitrary unseen configurations**.

The highest-value next steps are:

1. Confirm the input mapping.
2. Narrow the decision boundary.
3. Isolate the Port-C transition with matched controlled queries.
4. Refine high-score optimization only after local response shapes are better understood.
