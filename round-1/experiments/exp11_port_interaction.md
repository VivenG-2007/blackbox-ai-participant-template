# Experiment 11: port across four input profiles

| profile | A | B | C | D |
|---|---|---|---|---|
| center (q36-39) | 0.3544 | 0.3478 | 0.351 | 0.3517 |
| low (q40-43) | 0.2247 | 0.2351 | 0.043 | 0.2462 |
| high (q44-47) | 0.6708 | 0.6684 | 0.6686 | 0.6614 |
| saturated (q49-51,54) | 0.9559 | 0.9546 | 0.043 | 0.9563 |

Port C returns exactly 0.0430 in two profiles (q42, q50) but behaves normally in the other two. The identical value suggests an override/floor, not a smooth function.

Hypothesis (untested): override fires for port C when route_age_days is low (q42: 32.25, q50: 18 vs normal C at 46.5 and 60.75).
