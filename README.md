# Approximation-Feedback Retrieval

Research project studying how retrieval approximation errors evolve when retrieved evidence is fed back into subsequent query states.

The project asks a broader question than one-shot ANN quality:

> **When do approximation-induced retrieval differences remain bounded, when do they become dynamically amplified by feedback, and can the resulting risk be predicted and selectively controlled?**

---

## Core Research Question

Approximate nearest-neighbor retrieval is usually evaluated as a one-shot operation.

This project studies a different regime:

> Can approximation errors that are tolerable in one-shot retrieval become dynamically consequential when retrieved results modify subsequent query states?

The working mechanism is:

```math
\text{retrieval approximation}
\rightarrow
\text{feedback contamination}
\rightarrow
\text{query-state divergence}
\rightarrow
\text{candidate divergence}
\rightarrow
\text{utility divergence}
```

The project therefore distinguishes between:

- one-shot approximation loss,
- feedback-induced state drift,
- approximation-specific excess drift,
- stable / null / amplifying / reversal regimes,
- signed harmful vs beneficial divergence,
- deployable risk prediction,
- and selective mitigation using higher-fidelity feedback.

The current evidence **does not** support a universal-instability claim. The dominant regime in the broad FEVER boundary study is stable/null.

---

## Experimental Lineage

| Version | Experiment | Role |
|---|---|---|
| ARC-v0.1 | Memory-Safe IVF-PQ | Large-corpus retrieval infrastructure |
| ARC-v0.2 | Retriever-Condition Replication | Retrieval-fidelity dose response |
| ARC-v0.3 | Error Amplification Probe | Synchronized trajectories and intervention |
| ARC-v0.4 | Statistical Mechanism Audit | Query-level statistical validation |
| ARC-v0.5 | Sealed FEVER DEV Confirmation | Independent confirmatory experiment |
| ARC-v0.6.1 | HotpotQA Cross-Dataset Replication Setup | Cross-dataset transfer and audit |
| ARC-v0.7 | HotpotQA Fidelity Index Rebuild | Rebuilt valid PQ32 / PQ64 / SQ8 retrieval conditions |
| ARC-v0.8 | Sealed HotpotQA H1–H4 Confirmation | Independent cross-dataset confirmation |
| ARC-v0.9 | Boundary & Stability Map | Fidelity dose response, stability regimes, held-out prediction |
| ARC-v0.10 | Boundary-Aware Selective Fidelity Mitigation | Held-out selective high-fidelity feedback control |
| ARC-v0.11 | Local M3 Max System Cost & Pareto Audit | Measured CPU latency, throughput, memory, and quality-cost tradeoffs |
| ARC-v0.11.1 | Final Quality-Cost Merge Audit | Deterministic baseline-join repair and sealed final Pareto evidence |
| ARC-v0.12 | Cross-Policy Boundary Transfer | Held-out transfer across tested feedback-policy families |
| ARC-v0.13 | FEVER Boundary External Replication | Cross-dataset boundary/stability external replication |
| ARC-v0.13.1 | FEVER Zero-Mass / Regime Audit | Diagnose degenerate q75 target and preserve regime interpretation |
| ARC-v0.14 | FEVER Mechanism Audit | Configuration reproducibility, dose response, diagnostic prediction |
| ARC-v0.15 | Deployable Boundary Prediction | PQ32-only deployment-feasible risk prediction |
| ARC-v0.15.1 | Full-Coverage Signed Harm Audit | Signed reconstruction of all FEVER amplification events |
| ARC-v0.16 | Threshold & Alpha-Controlled Robustness | Threshold sensitivity and within-alpha reproducibility audit |
| ARC-v0.16.1 | Deployable Predictor Feature Ablation | Query-specific PQ32 signal vs policy-only baselines |
| ARC-v0.16.1a | Feature Provenance Equality Audit | Exact equality of persisted FEVER PQ32 query-feature artifacts |
| ARC-v0.17 | Deployable Selective Fidelity Closure | FEVER end-to-end risk-aware selective SQ8 feedback |
| ARC-v0.17.1 | Multi-Random Allocation Audit | 10,000 matched-budget random-allocation robustness audit |

---

## Sealed FEVER DEV Confirmation

ARC-v0.5 evaluated four preregistered primary endpoints on the untouched FEVER DEV split.

| Endpoint | FEVER DEV effect |
|---|---:|
| H1 — Query-state divergence slope | +0.004109 |
| H2 — Candidate-divergence increment slope | +0.008157 |
| H3 — Absolute utility-gap slope | +0.007261 |
| H4 — High-fidelity feedback intervention | +0.030376 |

All four endpoints passed the preregistered query-level bootstrap, paired sign-flip randomization, and joint Holm-correction criteria.

These results established the first confirmatory evidence for the tested approximation-feedback mechanism.

---

## HotpotQA Retrieval Rebuild and Validation

During the HotpotQA replication, a persisted corpus embedding artifact was discovered to be structurally valid by file size and SHA-256 but semantically invalid because its vectors were zero-filled.

The invalid artifacts were quarantined, the approximately 5.23M-document corpus was re-encoded, and the retrieval infrastructure was rebuilt from verified embedding shards.

ARC-v0.7 then established three valid retrieval-fidelity conditions:

| Condition | Recall@10 | MRR@10 | nDCG@10 |
|---|---:|---:|---:|
| IVF-PQ32 | 0.5351 | 0.6024 | 0.4898 |
| IVF-PQ64 | 0.6422 | 0.7406 | 0.6113 |
| IVF-SQ8 | 0.6828 | 0.8006 | 0.6633 |

The rebuilt indexes passed:

- corpus-vector norm and finite-value audits,
- positive-vs-random semantic alignment checks,
- IVF population audits,
- 100-query retrieval smoke tests,
- full 5,447-query DEV evaluation,
- and artifact hash recording.

This validation stage is kept separate from the later H1–H4 mechanism confirmation.

---

## Sealed HotpotQA Cross-Dataset Confirmation

ARC-v0.8 transferred the frozen FEVER experimental design to HotpotQA without HotpotQA H1–H4-based parameter selection.

The primary statistical unit remained the query.

| Endpoint | HotpotQA DEV effect | Result |
|---|---:|---|
| H1 — Query-state divergence slope | +0.003229 | PASS |
| H2 — Candidate-divergence increment slope | +0.005590 | PASS |
| H3 — Absolute utility-gap slope | +0.022308 | PASS |
| H4 — High-fidelity feedback intervention | +0.121920 | PASS |

All four endpoints passed:

- query-level bootstrap confidence intervals,
- one-sided paired sign-flip randomization,
- and joint Holm family-wise correction.

The direction of all four effects therefore replicates across FEVER and HotpotQA, while the magnitudes remain dataset-specific.

---

## Boundary and Stability Analysis

ARC-v0.9 is explicitly post-confirmatory and does not replace the sealed v0.8 result.

Using the same frozen feedback policies, the HotpotQA fidelity-dose study found:

| Fidelity contrast | H1 slope | H2 slope | H3 slope |
|---|---:|---:|---:|
| PQ32 ↔ PQ64 | 0.002632 | 0.001596 | 0.012274 |
| PQ64 ↔ SQ8 | 0.001514 | 0.005360 | 0.013828 |
| PQ32 ↔ SQ8 | **0.003229** | **0.005590** | **0.022308** |

The widest tested fidelity contrast produces the strongest overall H3 amplification, although the intermediate contrasts are not strictly monotonic for every endpoint.

### Held-out HotpotQA boundary prediction

The original interpretable boundary model achieved approximately:

| Metric | Validation result |
|---|---:|
| $R^2$ | 0.247 |
| ROC-AUC | 0.774 |
| Average Precision | 0.516 |
| Accuracy | 0.690 |

These results indicate that amplification susceptibility is partially predictable before later feedback unfolds, but the original model is primarily diagnostic rather than deployment-feasible because some features require richer retrieval information.

---

## Boundary-Aware Selective Fidelity Mitigation

ARC-v0.10 tests whether the HotpotQA boundary predictor can be translated into an actionable systems policy.

The evaluation keeps PQ32 as the search/evaluation retriever and changes only the feedback source:

1. **Always-PQ32** — PQ32 search → PQ32 feedback
2. **Always-SQ8** — PQ32 search → SQ8 feedback
3. **Random selective SQ8** — budget-matched random high-fidelity allocation
4. **Boundary-aware selective SQ8** — fit-only boundary model allocates SQ8 feedback

The nominal high-fidelity budgets are 10%, 25%, and 50%. The primary pre-specified operating point is 25%.

Applying the fit-frozen 25% threshold to held-out validation selects **26.47%** of queries.

| Feedback configuration | Selective final nDCG@10 | Recovery of Always-SQ8 benefit | Δ vs Always-PQ32 (95% CI) | Δ vs random (95% CI) |
|---|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.3579 | 41.1% | +0.0197 [0.0163, 0.0231] | +0.0071 [0.0036, 0.0107] |
| softmax, k=5, α=0.5, T=0.1 | 0.3765 | 44.1% | +0.0842 [0.0769, 0.0917] | +0.0346 [0.0268, 0.0426] |

Both frozen feedback configurations beat Always-PQ32 and budget-matched random allocation at the primary operating point.

Across the budget sweep, boundary-aware recovery is:

| Nominal budget | Mean feedback | Softmax feedback |
|---|---:|---:|
| 10% | 16.1% | 20.3% |
| 25% | 41.1% | 44.1% |
| 50% | 68.9% | 69.0% |

A null case is retained: for mean feedback at 10%, Δ vs random is +0.00218 with a 95% bootstrap interval of `[-0.00032, 0.00477]`, so no reliable advantage over random is claimed at that operating point.

---

## Local M3 Max System Cost and Final Pareto Audit

ARC-v0.11 measures the systems cost of the frozen ARC-v0.10 policies on one Apple M3 Max CPU environment.

ARC-v0.11.1 performs only a deterministic aggregate join repair for baseline quality rows; no retrieval, policy, or systems benchmark is rerun.

| Feedback configuration | Selective nDCG@10 | Recovery | Δ vs random (95% CI) | Selective runtime | Random runtime | Fraction of Always-SQ8 extra runtime |
|---|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.357904 | 41.1% | +0.007084 [0.003560, 0.010677] | 12.170 s | 11.977 s | 27.13% |
| softmax, k=5, α=0.5, T=0.1 | 0.376525 | 44.1% | +0.034576 [0.026755, 0.042598] | 11.840 s | 12.078 s | 26.90% |

Relative to Always-PQ32, the selective policy recovers **41–44%** of the Always-SQ8 quality improvement while using about **27%** of the Always-SQ8 incremental runtime in this single-machine benchmark.

The corresponding recovery / incremental-runtime ratios are **1.516×** and **1.639×**.

These measurements are not universal production latency or throughput claims.

---

## Cross-Policy Boundary Transfer

ARC-v0.12 tests whether the HotpotQA boundary-risk signal learned in one feedback-policy family transfers to the other on the held-out boundary-validation split.

Before accepting the transfer result, the audit exactly reconstructs the sealed ARC-v0.9 classifier.

The reconstructed validation ROC-AUC is **0.7736072867** versus the sealed **0.7736072913**, an absolute difference of `4.58e-9`.

| Source → target | ROC-AUC | 95% bootstrap CI | Average Precision | AUC gap to target-family fit-only oracle |
|---|---:|---:|---:|---:|
| mean → softmax | **0.766532** | **[0.756424, 0.776532]** | 0.544860 | -0.016485 |
| softmax → mean | **0.755019** | **[0.745265, 0.764847]** | 0.435071 | +0.014342 |

Both directions retain substantial held-out discrimination.

The supported claim is limited to transferable boundary structure across the tested mean and softmax HotpotQA feedback-policy families.

---

## FEVER Boundary External Replication

ARC-v0.13 transfers the boundary/stability question to FEVER using a deterministic 50/50 FIT/validation split.

The frozen FEVER study contains:

- 6,666 DEV queries,
- 3,350 FIT queries,
- 3,316 untouched validation queries,
- 44 feedback configurations,
- four feedback gains (`α ∈ {0.1, 0.3, 0.5, 0.7}`),
- mean and softmax feedback families,
- and a fixed empirical regime threshold $|H3| = 0.002$.

### Zero-mass finding

The FEVER H3 distribution contains substantial exact-zero mass.

On FIT:

- exact zero: **88.25%**
- near zero ($|H3| \le 10^{-12}$): **90.06%**
- q75: **0.0**
- q90: **0.0**

Therefore the preregistered 75th-percentile classification target is degenerate on FEVER and is retained as a negative protocol outcome rather than retuned.

The primary external-replication interpretation instead uses the independently specified $\pm 0.002$ regime definition.

### FIT / validation regime replication

| Regime / measure | FEVER FIT | FEVER validation |
|---|---:|---:|
| Exact-zero H3 | 88.25% | 87.97% |
| Stable / null | 90.11% | 89.84% |
| Amplifying | 7.73% | 7.95% |
| Reversal | 2.16% | 2.21% |
| Queries amplifying under ≥1 config | 17.58% | 17.67% |
| Queries amplifying under ≥50% configs | 7.31% | 7.96% |

The dominant outcome is therefore **stability/null**, not amplification.

The current claim is:

> **Approximation-feedback amplification is a minority but structured dynamical regime rather than a universal property of approximate retrieval.**

---

## FEVER Configuration Reproducibility and Feedback-Gain Dose Response

ARC-v0.14 studies the structure of the FEVER regime after the sealed v0.13 confirmation.

Across the 44 tested configurations, FIT and validation amplification fractions are highly reproducible:

| Statistic | Result |
|---|---:|
| Pearson correlation | **0.9958** |
| Spearman correlation | **0.9866** |

Because feedback gain `α` is a strong common driver, these correlations are interpreted as held-out reproducibility of the tested policy grid, not proof that every fine-grained policy factor transfers independently.

### Amplification vs feedback gain

| α | FIT amplification | Validation amplification |
|---:|---:|---:|
| 0.1 | 3.39% | 3.07% |
| 0.3 | 6.78% | 6.80% |
| 0.5 | 9.36% | 9.88% |
| 0.7 | 11.36% | 12.04% |

The amplification fraction increases monotonically with feedback gain in both FIT and untouched validation.

### Diagnostic prediction

A post-hoc diagnostic model using policy variables together with richer initial-state descriptors achieves:

| Metric | FEVER validation |
|---|---:|
| ROC-AUC | **0.7442** |
| PR-AUC | **0.2081** |
| Amplification prevalence | **0.0795** |

This model is interpreted diagnostically, not as a deployment-ready selector, because some inputs require information unavailable before higher-fidelity retrieval or relevance assessment.

---

## Deployable PQ32-Only Boundary Prediction

ARC-v0.15 removes the deployment circularity from the diagnostic boundary model.

The deployment-feasible selector uses only:

- PQ32 score entropy,
- PQ32 top-1 vs top-10 score margin,
- `α`,
- `log(k)`,
- feedback-family indicator,
- and numeric temperature.

It explicitly excludes:

- qrels-derived utility,
- SQ8 scores,
- SQ8 candidates,
- and PQ32↔SQ8 divergence features.

### Untouched FEVER validation

| Metric | Result |
|---|---:|
| ROC-AUC | **0.7309** |
| 95% query-cluster bootstrap CI | **[0.7102, 0.7525]** |
| PR-AUC | **0.1793** |
| Amplification prevalence | **0.0795** |

### Risk enrichment

| Risk budget | Amplification rate | Enrichment vs prevalence |
|---|---:|---:|
| Top 10% | **22.57%** | **2.84×** |
| Top 25% | **18.43%** | **2.32×** |

At a 25% risk budget, the deployable selector captures:

- **57.99%** of all FEVER validation amplification events,
- versus **25.00%** under matched random allocation.

The matched-random 95% range is approximately `[24.28%, 25.74%]`, with one-sided randomization `p = 0.0005`.

This is an event-prioritization result. It is not presented as a fully integrated FEVER production routing-and-latency benchmark.

---

## Full-Coverage Signed Harm Audit

The original H3 endpoint is unsigned:

```math
H3_{\mathrm{abs}}
=
\mathrm{slope}\!\left(
\left|u_{\mathrm{SQ8}}(t)-u_{\mathrm{PQ32}}(t)\right|
\right)
```

Therefore, positive H3 alone proves growing utility disagreement but does not determine which trajectory is better.

ARC-v0.15.1 reconstructs signed utility for **all 11,596 / 11,596** FEVER untouched-validation query-policy events with:

```math
H3_{\text{abs}} > 0.002
```

using:

```math
G_t
=
u_{\text{SQ8}}(t)
-
u_{\text{PQ32}}(t)
```

The replay reuses the original v0.13 feedback implementation and verifies that reconstructed `|G_t|` exactly matches the sealed v0.13 absolute utility-gap trajectory for the accepted checkpoints.

### Signed taxonomy

| Signed outcome | Count | Fraction |
|---|---:|---:|
| Harmful final direction (`G_T > 0`) | **10,933** | **94.28%** |
| Beneficial divergence (`G_T < 0`) | 603 | 5.20% |
| Unresolved / tied | 60 | 0.52% |

Primary full-coverage estimates:

| Metric | Estimate | 95% query-cluster bootstrap CI |
|---|---:|---:|
| $P(G_T > 0 \mid H3_{\mathrm{abs}} > 0.002)$ | **0.9428** | **[0.9248, 0.9580]** |
| $P(H3_{\mathrm{signed}} > 0 \mid H3_{\mathrm{abs}} > 0.002)$ | **0.9469** | **[0.9296, 0.9618]** |
| $P(\Delta G > 0 \mid H3_{\mathrm{abs}} > 0.002)$ | **0.9390** | **[0.9208, 0.9544]** |

Harmful fractions remain above 91% in every tested method × α stratum.

Accordingly, the project now supports the stronger but still limited statement:

> **Most FEVER trajectories classified as absolute amplification are directionally harmful to the lower-fidelity PQ32 trajectory, but beneficial and tied counterexamples remain and are retained.**

The project does **not** claim that amplification is always harmful.

---

## Threshold Sensitivity and Alpha-Controlled Robustness

ARC-v0.16 addresses two remaining robustness questions for the FEVER boundary analysis:

1. whether the minority-amplification conclusion depends strongly on the operational threshold $\epsilon=0.002$;
2. whether the high FIT↔validation configuration reproducibility is explained only by the strong common effect of feedback gain $\alpha$.

The audit reuses the frozen ARC-v0.13 FIT and untouched-validation trajectories and the ARC-v0.15.1 signed-replay artifact. It does not rerun the full FEVER retrieval sweep and does not access test data.

### Threshold sensitivity

The regime analysis is repeated for:

```math
\epsilon
\in
\{0,\ 0.001,\ 0.002,\ 0.005,\ 0.01\}.
```

| $\epsilon$ | FIT amplification | Validation amplification | FIT↔VAL Pearson | FIT↔VAL Spearman |
|---:|---:|---:|---:|---:|
| 0.000 | 8.00% | 8.21% | 0.9964 | 0.9831 |
| 0.001 | 7.74% | 7.97% | 0.9961 | 0.9875 |
| 0.002 | 7.73% | 7.95% | 0.9958 | 0.9866 |
| 0.005 | 7.65% | 7.83% | 0.9961 | 0.9882 |
| 0.010 | 7.43% | 7.59% | 0.9966 | 0.9893 |

Across this threshold range, amplification remains a minority regime and FIT/validation configuration risk remains highly reproducible.

### Feedback-gain dose response under every threshold

For all five thresholds, amplification is **strictly increasing** with $\alpha$ in both FIT and untouched validation.

At the primary $\epsilon=0.002$ threshold:

| $\alpha$ | FIT amplification | Validation amplification |
|---:|---:|---:|
| 0.1 | 3.39% | 3.07% |
| 0.3 | 6.78% | 6.80% |
| 0.5 | 9.36% | 9.88% |
| 0.7 | 11.36% | 12.04% |

The same monotone ordering survives at $\epsilon \in \{0,0.001,0.005,0.01\}$.

### Within-$\alpha$ reproducibility

To test whether the global FIT↔validation correlation is driven only by $\alpha$, ARC-v0.16 compares configurations **within each fixed $\alpha$ level**.

At the primary $\epsilon=0.002$ threshold:

| $\alpha$ | Configs | Pearson $r$ | Spearman $\rho$ |
|---:|---:|---:|---:|
| 0.1 | 11 | 0.8561 | 0.7791 |
| 0.3 | 11 | 0.9553 | 0.7018 |
| 0.5 | 11 | 0.8284 | 0.8670 |
| 0.7 | 11 | 0.8096 | 0.7757 |

After subtracting the mean amplification risk within each $\alpha$ separately in FIT and validation, the remaining configuration-level structure still transfers:

| Metric | $\epsilon=0.002$ |
|---|---:|
| Alpha-centered Pearson $r$ | **0.7671** |
| Alpha-centered Spearman $\rho$ | **0.7897** |

Across all tested thresholds, alpha-centered Pearson correlations range from **0.7671 to 0.8484**, while alpha-centered Spearman correlations range from **0.7670 to 0.8462**.

This supports a narrower but stronger conclusion:

> **The held-out configuration-risk structure is not explained solely by the global feedback-gain effect; substantial within-$\alpha$ ordering transfers from FIT to untouched validation.**

It does not establish causal transfer of every individual policy factor.

### Signed-harm threshold sensitivity

ARC-v0.15.1 provides exact signed coverage for the primary set $H3_{\mathrm{abs}}>0.002$. Therefore signed-harm sensitivity is directly identifiable for thresholds at or above 0.002.

| $\epsilon$ | Signed events | Harmful final direction | Beneficial divergence | Tied |
|---:|---:|---:|---:|---:|
| 0.002 | 11,596 | **94.28%** | 5.20% | 0.52% |
| 0.005 | 11,420 | **94.33%** | 5.17% | 0.51% |
| 0.010 | 11,076 | **94.47%** | 5.03% | 0.50% |

The harmful fraction is therefore stable across stricter thresholds.

For $\epsilon<0.002$, ARC-v0.15.1 does not contain signed reconstructions for the additional trajectories introduced by those lower thresholds, so the corresponding harmful fractions are deliberately reported as **not identifiable from the existing signed replay** rather than extrapolated.

ARC-v0.16 report SHA-256:

```text
aee5c426bceb8293f574a374d74eb1aba21ef3ac142f1369a62961d66d3dbdb7
```


---

## Deployable Predictor Feature Ablation

ARC-v0.16.1 tests whether the deployment-feasible FEVER risk model is merely learning policy variables such as feedback gain and feedback family, or whether lower-fidelity query-specific PQ32 statistics contribute additional held-out discrimination.

Four fixed logistic models are compared on untouched FEVER validation:

| Model | Features | ROC-AUC | PR-AUC |
|---|---|---:|---:|
| Alpha only | $\alpha$ | 0.6281 | 0.1078 |
| Policy only | $\alpha$, $\log k$, family, temperature | 0.6308 | 0.1120 |
| PQ32 query only | PQ32 entropy, PQ32 margin | **0.7312** | 0.1580 |
| Full deployable | PQ32 query statistics + policy variables | **0.7309** | **0.1793** |

The primary paired query-cluster bootstrap contrast is:

```math
\Delta \mathrm{AUC}
=
\mathrm{AUC}_{\mathrm{full}}
-
\mathrm{AUC}_{\mathrm{policy}}
=
0.1001,
```

with 95% CI approximately **[0.0830, 0.1174]**.

For PR-AUC:

```math
\Delta \mathrm{PR\text{-}AUC}
\approx
0.0680,
```

with 95% CI approximately **[0.0540, 0.0836]**.

Across 2,000 paired bootstrap replicates, the probability that the full model failed to outperform policy-only was 0.

The query-only model retains essentially the full ROC discrimination, while policy variables improve precision-focused ranking at the high-risk end. At a 25% risk budget:

- policy-only captures approximately **37.88%** of FEVER validation amplification events;
- PQ32 query-only captures approximately **56.76%**;
- full deployable captures **57.99%**.

The supported interpretation is:

> **Amplification risk is not merely a configuration-level prior. Query-specific statistics observable from the lower-fidelity PQ32 retriever contain substantial held-out predictive information.**

This is a predictive-information claim, not a causal explanation of amplification.

---

## Feature Provenance Equality Audit

ARC-v0.16.1a audits the persisted FEVER PQ32 query-feature artifacts used around the frozen ARC-v0.13 run.

The frozen and later artifacts each contain:

- **6,666 rows**,
- **6,666 unique query IDs**,
- identical query-ID sets,
- no duplicate inconsistencies,
- exact equality for `pq32_entropy20`,
- exact equality for `pq32_margin1_10`,
- maximum absolute difference **0.0**.

The two files also have the same SHA-256:

```text
5bd27981191826938ac17f394d7cdbe62e1e83169a1e05ff197bfb2ffe37d2a7
```

The audit therefore classifies the two persisted feature artifacts as **value-equivalent**, so the ARC-v0.16.1 ablation results do not depend on an artifact mismatch between the frozen and later FEVER runs.

---

## Risk-Aware Selective Fidelity

ARC-v0.17 closes the previously separate FEVER prediction and HotpotQA intervention branches.

The resulting control policy is **Risk-Aware Selective Fidelity (RASF)**.

RASF uses a fit-frozen deployment-feasible risk score computed only from:

- PQ32 score entropy,
- PQ32 top-1 vs top-10 margin,
- feedback gain $\alpha$,
- $\log k$,
- feedback-family indicator,
- numeric temperature.

At a fixed budget, the highest-risk validation queries receive SQ8 feedback while search and evaluation remain on PQ32. Lower-risk queries continue to use PQ32 feedback.

Formally, for a budget-selected query set $S_B$:

```math
F(q)
=
\begin{cases}
F_{\mathrm{SQ8}}(q), & q \in S_B, \\
F_{\mathrm{PQ32}}(q), & q \notin S_B.
\end{cases}
```

The FEVER validation experiment compares:

- Always-PQ32 feedback,
- Always-SQ8 feedback,
- budget-matched random selective SQ8 feedback,
- RASF selective SQ8 feedback.

The primary budget is **25%**.

### FEVER end-to-end closure at the 25% budget

| Configuration | Always-PQ32 | Random 25% | RASF 25% | Always-SQ8 | RASF − Random (95% CI) | Recovery of Always-SQ8 benefit |
|---|---:|---:|---:|---:|---:|---:|
| mean, $k=20,\alpha=0.3$ | 0.11504 | 0.11783 | **0.12189** | 0.12816 | **+0.00406 [0.00189, 0.00633]** | **52.2%** |
| softmax, $k=5,\alpha=0.5,T=0.1$ | 0.10409 | 0.11307 | **0.12473** | 0.15326 | **+0.01166 [0.00793, 0.01548]** | **42.0%** |

Both primary 25% configurations therefore beat the original matched-random allocation with paired query-level bootstrap intervals entirely above zero.

The 10% budget is retained as a mixed/null regime:

- mean feedback has only weak evidence over random;
- softmax feedback does not reliably beat random.

At 50%, both tested configurations again show a reliable RASF advantage over random.

The supported conclusion is therefore budget-dependent rather than universal:

> **In the tested FEVER setting, deployment-feasible risk ranking becomes actionable at moderate and high selective-feedback budgets, while very small budgets do not reliably outperform random allocation across both feedback families.**

### Local measured cost

A 500-query local runtime audit shows that RASF and matched-random selective feedback have nearly the same execution cost at the 25% budget.

For the mean configuration:

- Always-PQ32: about **24.10 ms/query**,
- RASF 25%: about **25.18 ms/query**,
- matched random 25%: about **25.42 ms/query**,
- Always-SQ8 feedback: about **30.50 ms/query**.

For the softmax configuration:

- Always-PQ32: about **23.39 ms/query**,
- RASF 25%: about **25.15 ms/query**,
- matched random 25%: about **25.27 ms/query**,
- Always-SQ8 feedback: about **29.81 ms/query**.

The correct systems interpretation is not that RASF is faster than matched random. Instead:

> **At approximately matched selective-feedback cost, RASF allocates the same high-fidelity budget more effectively and achieves higher held-out retrieval utility.**

These are local single-machine measurements, not production latency or throughput guarantees.

ARC-v0.17 report SHA-256:

```text
437fb1cdc1c72d5e18eb3b8d728e3e55eac293ceddfa1bc889784d1e00729674
```

---

## Multi-Random Allocation Robustness

ARC-v0.17.1 tests whether the original single random selective allocation happened to be unusually weak.

No retrieval is rerun.

For each query, the audit reuses the already measured Always-PQ32 and Always-SQ8 final utilities and evaluates **10,000 independent matched-budget random allocations per configuration and budget**.

For a random subset $S$ of fixed size:

```math
U_{\mathrm{rand}}(S)
=
\bar U^{L}
+
\frac{1}{N}
\sum_{q \in S}
\left(
U_q^{H}
-
U_q^{L}
\right).
```

### Primary 25% multi-random result

| Configuration | RASF nDCG@10 | Random expectation | Random 95% upper | RASF − random expectation | Random allocations $\ge$ RASF | Corrected one-sided $p$ |
|---|---:|---:|---:|---:|---:|---:|
| mean, $k=20,\alpha=0.3$ | **0.121894** | 0.118321 | 0.119711 | **+0.003573** | **0 / 10,000** | **0.00010** |
| softmax, $k=5,\alpha=0.5,T=0.1$ | **0.124729** | 0.116384 | 0.118976 | **+0.008344** | **0 / 10,000** | **0.00010** |

Thus, for both primary configurations:

```math
U_{\mathrm{RASF}}
>
q_{0.975}
\left(
U_{\mathrm{random}}
\right).
```

The finite-population analytic standard deviations closely agree with the Monte Carlo standard deviations, providing an independent check of the random-allocation null reconstruction.

The 10% budget remains mixed:

- mean feedback shows only weak evidence against the random-allocation null;
- softmax feedback is consistent with the random null.

At 50%, both configurations again lie clearly above the matched-budget random-allocation distribution.

The primary RASF conclusion is therefore not an artifact of one unlucky random subset.

ARC-v0.17.1 report SHA-256:

```text
0924f090b8d0b1346cd4fa366abc37930832048fc60b605165eadb31f5796d83
```


---

## Current Research Claim

The evidence chain now is:

```math
\text{Phenomenon}
\rightarrow
\text{Sealed confirmation}
\rightarrow
\text{Cross-dataset replication}
\rightarrow
\text{Fidelity / stability boundary}
\rightarrow
\text{Held-out regime replication}
\rightarrow
\text{Cross-policy transfer}
\rightarrow
\text{Diagnostic prediction}
\rightarrow
\text{Deployable PQ32-only prediction}
\rightarrow
\text{Full-coverage signed harm audit}
\rightarrow
\text{Threshold / alpha-controlled robustness}
\rightarrow
\text{Deployable feature-ablation validation}
\rightarrow
\text{Risk-aware selective fidelity closure}
\rightarrow
\text{Multi-random allocation audit}
\rightarrow
\text{Measured quality-cost audit}
```

The strongest supported claim at this stage is:

> **Approximation-induced retrieval differences that are tolerable in one-shot evaluation can become dynamically consequential under iterative feedback. In the tested FEVER and HotpotQA settings, the dominant regime is stable/null, while a minority amplifying regime is reproducible across held-out queries, remains qualitatively stable across reasonable regime thresholds, increases with feedback gain, retains substantial configuration-level reproducibility after controlling for alpha, and is predominantly directionally harmful when signed utility is reconstructed. Lower-fidelity PQ32 query statistics provide substantial held-out predictive information beyond policy-level priors. In FEVER, the resulting fit-frozen Risk-Aware Selective Fidelity policy uses those deployment-feasible risk scores to allocate SQ8 feedback and, at the primary 25% budget, outperforms matched random allocation for both tested feedback configurations. Across 10,000 matched-budget random allocations per configuration, no random allocation reaches the observed RASF utility at the primary operating point. In HotpotQA, an earlier richer boundary-aware selector also recovered a disproportionate fraction of the Always-SQ8 quality benefit at a fraction of its measured incremental runtime. These results support selective fidelity control in the tested settings, not universal instability or universal routing optimality across retrievers, encoders, datasets, feedback mechanisms, or hardware.**

The project does **not** claim that:

- approximation errors always amplify,
- amplification is always harmful,
- the mechanism is universal across all encoders or ANN systems,
- the current predictor fully explains query-level susceptibility,
- the FEVER RASF controller is a universal or production-validated routing policy beyond the tested experimental setting,
- the selective policy is universally optimal across datasets, encoders, ANN systems, or hardware,
- or Apple M3 Max measurements are universal production latency/throughput claims.

---

## Repository Structure

```text
notebooks/          Experimental notebooks
protocols/          Sanitized experimental protocols
results/            Sanitized aggregate results
figures/            Research figures
docs/               Methodology and experiment history
src/                Reusable implementation
```

Large corpora, embedding matrices, FAISS indexes, SQLite databases, raw datasets, and private runtime artifacts are intentionally excluded from Git.

---

## Experimental Philosophy

The project separates:

1. exploratory discovery,
2. retriever-condition replication,
3. direct mechanism probing,
4. statistical auditing,
5. sealed independent confirmation,
6. cross-dataset confirmation,
7. post-confirmatory boundary analysis,
8. held-out prediction,
9. selective mitigation,
10. measured systems-cost auditing,
11. deterministic post-run provenance repair,
12. cross-policy boundary transfer,
13. external FEVER boundary replication,
14. zero-mass / regime auditing,
15. mechanism diagnostics,
16. deployment-feasible prediction,
17. full-coverage signed construct validation,
18. threshold-sensitivity and alpha-controlled robustness auditing,
19. deployable predictor feature-ablation auditing,
20. feature provenance equality auditing,
21. same-setting deployable selective-fidelity closure,
22. multi-random allocation robustness auditing.

Positive, null, stable, reversal, and beneficial-divergence outcomes are retained rather than filtered away.

---

## Current Status

### Completed

- large-corpus memory-safe IVF-PQ infrastructure
- retriever-condition replication
- synchronized approximation-feedback trajectories
- statistical mechanism audit
- sealed FEVER H1–H4 confirmation
- audited HotpotQA retrieval rebuild
- sealed HotpotQA H1–H4 cross-dataset confirmation
- HotpotQA boundary/stability map
- boundary-aware selective fidelity mitigation
- local Apple M3 Max quality-cost / Pareto audit
- v0.11.1 deterministic quality-cost repair
- cross-policy HotpotQA boundary transfer
- FEVER external boundary replication
- FEVER zero-mass / regime audit
- FEVER mechanism audit
- deployable PQ32-only FEVER risk prediction
- full-coverage FEVER signed harm audit
- threshold-sensitivity and alpha-controlled FEVER robustness audit

### Remaining paper-facing work

- integrate ARC-v0.17 / ARC-v0.17.1 into the final SIGIR full-paper methods and results,
- finalize the formal RASF algorithm description and mathematical notation,
- finalize figures / tables and ACM page-budget optimization,
- perform the final adversarial reviewer and claim audit.

---

## Research Scope

This repository is an active research artifact.

The intended contribution is an empirical and methodological study of **approximation under retrieval feedback**, not a claim that any specific ANN index, encoder, feedback policy, or mitigation strategy is universally optimal.

The project emphasizes:

- explicit separation between exploratory and confirmatory stages,
- held-out validation,
- query-level or query-cluster uncertainty where appropriate,
- retention of negative/null/reversal outcomes,
- artifact hashing and checkpoint auditing,
- and conservative interpretation when evidence does not support universal claims.
