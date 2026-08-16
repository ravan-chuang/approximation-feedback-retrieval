# ARC-v0.10 — Boundary-Aware Selective Fidelity Mitigation

ARC-v0.10 evaluates whether the ARC-v0.9 boundary model has actionable systems value.

## Frozen design

The risk model is reconstructed from the ARC-v0.9 boundary-fit partition only. Selection thresholds are
also fixed from the fit partition before evaluating mitigation outcomes on the boundary-validation queries.

Policies:

1. Always-PQ32
2. Always-SQ8
3. Budget-matched random selective SQ8
4. Boundary-aware selective SQ8

Nominal high-fidelity budgets: 10%, 25%, and 50%. The pre-specified primary operating point is 25%.

## Primary 25% operating point

Applying the fit-frozen threshold to validation selects 26.47% of queries. This is reported as an
out-of-sample realized allocation rather than re-tuning the validation threshold to exactly 25%.

| Feedback configuration | Final nDCG@10 | Recovery | Δ vs Always-PQ32 (95% CI) | Δ vs random (95% CI) |
|---|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.357904 | 41.14% | +0.019655 [0.016285, 0.023138] | +0.007084 [0.003560, 0.010677] |
| softmax, k=5, α=0.5, T=0.1 | 0.376525 | 44.08% | +0.084190 [0.076935, 0.091748] | +0.034576 [0.026755, 0.042598] |

Both frozen feedback configurations are positive relative to Always-PQ32 and budget-matched random
allocation at the primary operating point.

## Budget sweep

Boundary-aware recovery of the Always-SQ8 benefit:

| Budget | Mean feedback | Softmax feedback |
|---|---:|---:|
| 10% | 16.10% | 20.31% |
| 25% nominal / 26.47% realized | 41.14% | 44.08% |
| 50% nominal / 52.60% realized | 68.86% | 69.04% |

A notable null case is retained: under mean feedback at the 10% budget, the boundary-aware policy
improves over random by +0.002177, but the 95% bootstrap interval [-0.000323, 0.004770] includes zero.

## Interpretation

The ARC-v0.9 susceptibility model is not merely descriptive: under the tested static policy, it supports
resource allocation that is more effective than budget-matched random allocation at the pre-specified
25% operating point.

This does not establish universal optimality, and the current public summary does not substitute an
explicit latency/QPS/index-footprint systems benchmark.

## Integrity

- Protocol SHA-256: `a48d2efbd470314de1042c85d8888312b3f61d1c68d3456f6f91a3150320817f`
- Report SHA-256: `fdf64925334500bc4823fd828510d9a3ec946719fda940cc8eabd55a8cd44589`
- TEST access: false
