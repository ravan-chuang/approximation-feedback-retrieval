# Approximation-Feedback Retrieval

Research project studying how retrieval approximation errors evolve when retrieved evidence is fed back into subsequent query states.

The project asks a broader question than one-shot ANN quality:

> **When do approximation-induced retrieval differences remain bounded, when do they become dynamically amplified by feedback, and can the resulting risk be predicted and selectively controlled?**

---


## Current Paper-Facing Snapshot

The current manuscript is:

> **Approximation under Feedback: Mechanism-Dependent Stability and Directional Harm in Iterative Retrieval**

The paper is in **submission-freeze preparation** after factual, reference, claim-strength, citation-support, anonymity, and reviewer-attack audits.

The current paper-facing evidence supports three central conclusions:

1. **One-shot ANN fidelity and feedback stability are distinct evaluation axes.**
2. **Approximation mechanism is a material boundary condition.** Representation approximation expands candidate and utility separation in the tested BGE/E5 settings, while two search-effort interventions—IVF `nprobe` and HNSW `efSearch`—produce positive state divergence but aggregate candidate and utility contraction.
3. **Predictive susceptibility does not guarantee intervention value.** RASF is effective at the frozen FEVER-BGE operating point, but the frozen E5 method replication does not beat matched-budget random allocation.

A particularly informative descriptive cross-mechanism comparison is:

| Setting | Reported one-shot nDCG@10 gap | Validation H3abs |
|---|---:|---:|
| E5 representation: IVF-PQ32 → IVF-SQ8 | 0.2548 | **+0.030166** |
| E5 HNSW search effort: efSearch 8 → 256 | 0.2487 | **-0.006311** |

The gaps are similar in magnitude but come from different frozen evaluation/calibration procedures, so this is **not** treated as a matched causal design. It does, however, argue against one-shot gap magnitude alone as an explanation of the opposite dynamic signs.


## Core Research Question

Approximate nearest-neighbor retrieval is usually evaluated as a one-shot operation.

This project studies a different regime:

> **Can approximation errors that are tolerable in one-shot retrieval become dynamically consequential when retrieved results modify subsequent query states?**

The working mechanism is:

```text
retrieval approximation
→
feedback contamination
→
query-state divergence
→
candidate divergence
→
utility divergence
```

The project therefore distinguishes between:

- one-shot approximation loss,
- feedback-induced state drift,
- approximation-specific excess drift,
- stable / null / amplifying / reversal regimes,
- signed harmful vs beneficial divergence,
- deployable risk prediction,
- and selective mitigation using higher-fidelity feedback.

The current evidence does **not** support a universal-instability claim.

Across the tested settings, approximation-feedback dynamics are heterogeneous. Stable/null behavior dominates some settings, while amplification becomes substantially more prevalent under others. The strongest current evidence supports a narrower conclusion:

> **Approximation-feedback dynamics are structured rather than universal; their prevalence can depend strongly on the retrieval representation, while the direction of amplified utility divergence can remain highly consistent across tested encoder families.**

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
| ARC-v0.18 | Cross-Encoder FEVER Replication | E5-small-v2 generalization test under the frozen FEVER design |
| ARC-v0.18.1 | E5 Signed-Direction Audit | Post-hoc signed construct-validity audit of v0.18 amplification events |
| ARC-v0.19 | Cross-Approximation nprobe Replication | Search-effort approximation boundary test using IVF-SQ8 nprobe 8 vs 64 |
| ARC-v0.20c | HNSW Mechanism Replication | Frozen graph-search boundary test, `efSearch 8 → 256` |
| ARC-v0.20d | HNSW Severity Sensitivity | Post-primary exploratory `32 → 256` and `64 → 256` robustness audit |
| ARC-v0.21 | E5 RASF Cross-Encoder Replication | Frozen cross-encoder controller replication; predictor transfers modestly, 25% intervention advantage does not |

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
| PQ32 ↔ SQ8 | 0.003229 | 0.005590 | 0.022308 |

The widest tested fidelity contrast produces the strongest overall H3 amplification, although the intermediate contrasts are not strictly monotonic for every endpoint.

### Held-out HotpotQA boundary prediction

The original interpretable boundary model achieved approximately:

| Metric | Validation result |
|---|---:|
| R² | 0.247 |
| ROC-AUC | 0.774 |
| Average Precision | 0.516 |
| Accuracy | 0.690 |

These results indicate that amplification susceptibility is partially predictable before later feedback unfolds, but the original model is primarily diagnostic rather than deployment-feasible because some features require richer retrieval information.

---

## Boundary-Aware Selective Fidelity Mitigation

ARC-v0.10 tests whether the HotpotQA boundary predictor can be translated into an actionable systems policy.

The evaluation keeps PQ32 as the search/evaluation retriever and changes only the feedback source:

- Always-PQ32 — PQ32 search → PQ32 feedback
- Always-SQ8 — PQ32 search → SQ8 feedback
- Random selective SQ8 — budget-matched random high-fidelity allocation
- Boundary-aware selective SQ8 — fit-only boundary model allocates SQ8 feedback

The nominal high-fidelity budgets are 10%, 25%, and 50%. The primary pre-specified operating point is 25%.

Applying the fit-frozen 25% threshold to held-out validation selects 26.47% of queries.

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

A null case is retained: for mean feedback at 10%, Δ vs random is +0.00218 with a 95% bootstrap interval of [-0.00032, 0.00477], so no reliable advantage over random is claimed at that operating point.

---

## Local M3 Max System Cost and Final Pareto Audit

ARC-v0.11 measures the systems cost of the frozen ARC-v0.10 policies on one Apple M3 Max CPU environment.

ARC-v0.11.1 performs only a deterministic aggregate join repair for baseline quality rows; no retrieval, policy, or systems benchmark is rerun.

| Feedback configuration | Selective nDCG@10 | Recovery | Δ vs random (95% CI) | Selective runtime | Random runtime | Fraction of Always-SQ8 extra runtime |
|---|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.357904 | 41.1% | +0.007084 [0.003560, 0.010677] | 12.170 s | 11.977 s | 27.13% |
| softmax, k=5, α=0.5, T=0.1 | 0.376525 | 44.1% | +0.034576 [0.026755, 0.042598] | 11.840 s | 12.078 s | 26.90% |

Relative to Always-PQ32, the selective policy recovers 41–44% of the Always-SQ8 quality improvement while using about 27% of the Always-SQ8 incremental runtime in this single-machine benchmark.

The corresponding recovery / incremental-runtime ratios are 1.516× and 1.639×.

These measurements are not universal production latency or throughput claims.

---

## Cross-Policy Boundary Transfer

ARC-v0.12 tests whether the HotpotQA boundary-risk signal learned in one feedback-policy family transfers to the other on the held-out boundary-validation split.

Before accepting the transfer result, the audit exactly reconstructs the sealed ARC-v0.9 classifier.

The reconstructed validation ROC-AUC is 0.7736072867 versus the sealed 0.7736072913, an absolute difference of 4.58e-9.

| Source → target | ROC-AUC | 95% bootstrap CI | Average Precision | AUC gap to target-family fit-only oracle |
|---|---:|---:|---:|---:|
| mean → softmax | 0.766532 | [0.756424, 0.776532] | 0.544860 | -0.016485 |
| softmax → mean | 0.755019 | [0.745265, 0.764847] | 0.435071 | +0.014342 |

Both directions retain substantial held-out discrimination.

The supported claim is limited to transferable boundary structure across the tested mean and softmax HotpotQA feedback-policy families.

---

## FEVER Boundary External Replication

ARC-v0.13 transfers the boundary/stability question to FEVER using a deterministic 50/50 FIT/validation split.

The frozen FEVER study contains:

- 6,666 DEV queries,
- 3,350 FIT queries,
- 3,316 FIT-held-out validation queries,
- 44 feedback configurations,
- four feedback gains (`alpha in {0.1,0.3,0.5,0.7`),
- mean and softmax feedback families,
- and a fixed empirical regime threshold `|H3| = 0.002`.

### Zero-mass finding

The FEVER H3 distribution contains substantial exact-zero mass.

On FIT:

- exact zero: **88.25%**
- near zero (`|H3| <= 10^{-12}`): **90.06%**
- q75: **0.0**
- q90: **0.0**

Therefore the preregistered 75th-percentile classification target is degenerate on FEVER and is retained as a negative protocol outcome rather than retuned.

The primary external-replication interpretation instead uses the independently specified `±0.002` regime definition.

### FIT / validation regime replication

| Regime / measure | FEVER FIT | FEVER validation |
|---|---:|---:|
| Exact-zero H3 | 88.25% | 87.97% |
| Stable / null | 90.11% | 89.84% |
| Amplifying | 7.73% | 7.95% |
| Reversal | 2.16% | 2.21% |
| Queries amplifying under ≥1 config | 17.58% | 17.67% |
| Queries amplifying under ≥50% configs | 7.31% | 7.96% |

The dominant outcome under the original BGE FEVER setting is therefore stability/null, not amplification.

The current claim is:

> **Approximation-feedback amplification is a minority but structured dynamical regime rather than a universal property of approximate retrieval.**

---

## FEVER Configuration Reproducibility and Feedback-Gain Dose Response

ARC-v0.14 studies the structure of the FEVER regime after the sealed v0.13 confirmation.

Across the 44 tested configurations, FIT and validation amplification fractions are highly reproducible:

| Statistic | Result |
|---|---:|
| Pearson correlation | 0.9958 |
| Spearman correlation | 0.9866 |

Because feedback gain α is a strong common driver, these correlations are interpreted as held-out reproducibility of the tested policy grid, not proof that every fine-grained policy factor transfers independently.

### Amplification vs feedback gain

| α | FIT amplification | Validation amplification |
|---:|---:|---:|
| 0.1 | 3.39% | 3.07% |
| 0.3 | 6.78% | 6.80% |
| 0.5 | 9.36% | 9.88% |
| 0.7 | 11.36% | 12.04% |

The amplification fraction increases monotonically with feedback gain in both FIT and FIT-held-out validation.

### Diagnostic prediction

A post-hoc diagnostic model using policy variables together with richer initial-state descriptors achieves:

| Metric | FEVER validation |
|---|---:|
| ROC-AUC | 0.7442 |
| PR-AUC | 0.2081 |
| Amplification prevalence | 0.0795 |

This model is interpreted diagnostically, not as a deployment-ready selector, because some inputs require information unavailable before higher-fidelity retrieval or relevance assessment.

---

## Deployable PQ32-Only Boundary Prediction

ARC-v0.15 removes the deployment circularity from the diagnostic boundary model.

The deployment-feasible selector uses only:

- PQ32 score entropy,
- PQ32 top-1 vs top-10 score margin,
- α,
- log(k),
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
| ROC-AUC | 0.7309 |
| 95% query-cluster bootstrap CI | [0.7102, 0.7525] |
| PR-AUC | 0.1793 |
| Amplification prevalence | 0.0795 |

### Risk enrichment

| Risk budget | Amplification rate | Enrichment vs prevalence |
|---|---:|---:|
| Top 10% | 22.57% | 2.84× |
| Top 25% | 18.43% | 2.32× |

At a 25% risk budget, the deployable selector captures:

- **57.99%** of all FEVER validation amplification events,
- versus **25.00%** under matched random allocation.

The matched-random 95% range is approximately [24.28%, 25.74%], with one-sided randomization p = 0.0005.

This is an event-prioritization result. It is not presented as a fully integrated production routing-and-latency benchmark.

---

## Full-Coverage Signed Harm Audit

The original H3 endpoint is unsigned:

`H3_abs = slope(|u_SQ8(t) - u_PQ32(t)|)`

Therefore, positive H3 alone proves growing utility disagreement but does not determine which trajectory is better.

ARC-v0.15.1 reconstructs signed utility for all 11,596 / 11,596 FEVER FIT-held-out validation query-policy events with:

`H3_abs > 0.002`

using:

`G_t = u_SQ8(t) - u_PQ32(t)`

The replay reuses the original v0.13 feedback implementation and verifies that reconstructed `|G_t|` exactly matches the sealed v0.13 absolute utility-gap trajectory for the accepted checkpoints.

### Signed taxonomy

| Signed outcome | Count | Fraction |
|---|---:|---:|
| Harmful final direction (`G_T>0`) | 10,933 | 94.28% |
| Beneficial divergence (`G_T<0`) | 603 | 5.20% |
| Unresolved / tied | 60 | 0.52% |

Primary full-coverage estimates:

| Metric | Estimate | 95% query-cluster bootstrap CI |
|---|---:|---:|
| `P(G_T > 0 given H3_abs > 0.002)` | 0.9428 | [0.9248, 0.9580] |
| `P(H3_signed > 0 given H3_abs > 0.002)` | 0.9469 | [0.9296, 0.9618] |
| `P(Delta G > 0 given H3_abs > 0.002)` | 0.9390 | [0.9208, 0.9544] |

Harmful fractions remain above 91% in every tested method × α stratum.

Accordingly, the project supports the limited statement:

> **Most FEVER trajectories classified as absolute amplification are directionally harmful to the lower-fidelity PQ32 trajectory, but beneficial and tied counterexamples remain and are retained.**

The project does **not** claim that amplification is always harmful.

---

## Threshold Sensitivity and Alpha-Controlled Robustness

ARC-v0.16 addresses whether:

1. the minority-amplification conclusion depends strongly on the operational threshold `epsilon=0.002`;
2. the high FIT↔validation configuration reproducibility is explained only by the strong common effect of feedback gain α.

The audit reuses the frozen ARC-v0.13 FIT and FIT-held-out validation trajectories and the ARC-v0.15.1 signed-replay artifact. It does not rerun the full FEVER retrieval sweep and does not access test data.

### Threshold sensitivity

The regime analysis is repeated for:

`epsilon in {0, 0.001, 0.002, 0.005, 0.01}`

| ε | FIT amplification | Validation amplification | FIT↔VAL Pearson | FIT↔VAL Spearman |
|---:|---:|---:|---:|---:|
| 0.000 | 8.00% | 8.21% | 0.9964 | 0.9831 |
| 0.001 | 7.74% | 7.97% | 0.9961 | 0.9875 |
| 0.002 | 7.73% | 7.95% | 0.9958 | 0.9866 |
| 0.005 | 7.65% | 7.83% | 0.9961 | 0.9882 |
| 0.010 | 7.43% | 7.59% | 0.9966 | 0.9893 |

Across this threshold range, amplification remains a minority regime and FIT/validation configuration risk remains highly reproducible.

### Within-α reproducibility

At the primary `epsilon=0.002` threshold:

| α | Configs | Pearson r | Spearman ρ |
|---:|---:|---:|---:|
| 0.1 | 11 | 0.8561 | 0.7791 |
| 0.3 | 11 | 0.9553 | 0.7018 |
| 0.5 | 11 | 0.8284 | 0.8670 |
| 0.7 | 11 | 0.8096 | 0.7757 |

After subtracting the mean amplification risk within each α separately in FIT and validation:

| Metric | ε=0.002 |
|---|---:|
| Alpha-centered Pearson r | 0.7671 |
| Alpha-centered Spearman ρ | 0.7897 |

Across all tested thresholds, alpha-centered Pearson correlations range from 0.7671 to 0.8484, while alpha-centered Spearman correlations range from 0.7670 to 0.8462.

This supports the narrower conclusion:

> **The held-out configuration-risk structure is not explained solely by the global feedback-gain effect; substantial within-α ordering transfers from FIT to FIT-held-out validation.**

It does not establish causal transfer of every individual policy factor.

---

## Deployable Predictor Feature Ablation

ARC-v0.16.1 tests whether the deployment-feasible FEVER risk model is merely learning policy variables or whether lower-fidelity query-specific PQ32 statistics contribute additional held-out discrimination.

| Model | Features | ROC-AUC | PR-AUC |
|---|---|---:|---:|
| Alpha only | α | 0.6281 | 0.1078 |
| Policy only | α, log k, family, temperature | 0.6308 | 0.1120 |
| PQ32 query only | PQ32 entropy, PQ32 margin | 0.7312 | 0.1580 |
| Full deployable | PQ32 query statistics + policy variables | 0.7309 | 0.1793 |

Primary paired query-cluster bootstrap contrast:

`Delta AUC = AUC_full - AUC_policy = 0.1001`

with 95% CI approximately [0.0830, 0.1174].

For PR-AUC:

`Delta PR-AUC ~= 0.0680`

with 95% CI approximately [0.0540, 0.0836].

At a 25% risk budget:

- policy-only captures approximately **37.88%** of FEVER validation amplification events;
- PQ32 query-only captures approximately **56.76%**;
- full deployable captures **57.99%**.

The supported interpretation is:

> **Amplification risk is not merely a configuration-level prior. Query-specific statistics observable from the lower-fidelity PQ32 retriever contain substantial held-out predictive information.**

This is a predictive-information claim, not a causal explanation.

---

## Feature Provenance Equality Audit

ARC-v0.16.1a audits the persisted FEVER PQ32 query-feature artifacts used around the frozen ARC-v0.13 run.

The frozen and later artifacts each contain:

- 6,666 rows,
- 6,666 unique query IDs,
- identical query-ID sets,
- no duplicate inconsistencies,
- exact equality for `pq32_entropy20`,
- exact equality for `pq32_margin1_10`,
- maximum absolute difference 0.0.

The two files also have the same SHA-256:

```text
5bd27981191826938ac17f394d7cdbe62e1e83169a1e05ff197bfb2ffe37d2a7
```

The audit therefore classifies the two persisted feature artifacts as value-equivalent.

---

## Risk-Aware Selective Fidelity

ARC-v0.17 closes the previously separate FEVER prediction and HotpotQA intervention branches.

The resulting control policy is **Risk-Aware Selective Fidelity (RASF)**.

RASF uses a fit-frozen deployment-feasible risk score computed only from:

- PQ32 score entropy,
- PQ32 top-1 vs top-10 margin,
- feedback gain α,
- log k,
- feedback-family indicator,
- numeric temperature.

At a fixed budget, the highest-risk validation queries receive SQ8 feedback while search and evaluation remain on PQ32. Lower-risk queries continue to use PQ32 feedback.

For a budget-selected query set `S_B`:

RASF feedback rule:

```text
F(q) = F_SQ8(q)   if q in S_B
       F_PQ32(q)  otherwise
```

The FEVER validation experiment compares:

- Always-PQ32 feedback,
- Always-SQ8 feedback,
- budget-matched random selective SQ8 feedback,
- RASF selective SQ8 feedback.

The primary budget is 25%.

### FEVER end-to-end closure at the 25% budget

| Configuration | Always-PQ32 | Random 25% | RASF 25% | Always-SQ8 | RASF − Random (95% CI) | Recovery of Always-SQ8 benefit |
|---|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.11504 | 0.11783 | 0.12189 | 0.12816 | +0.00406 [0.00189, 0.00633] | 52.2% |
| softmax, k=5, α=0.5, T=0.1 | 0.10409 | 0.11307 | 0.12473 | 0.15326 | +0.01166 [0.00793, 0.01548] | 42.0% |

Both primary 25% configurations beat the original matched-random allocation with paired query-level bootstrap intervals entirely above zero.

The 10% budget is retained as a mixed/null regime:

- mean feedback has only weak evidence over random;
- softmax feedback does not reliably beat random.

At 50%, both tested configurations again show a reliable RASF advantage over random.

The supported conclusion is budget-dependent rather than universal.

### Local measured cost

A 500-query local runtime audit shows that RASF and matched-random selective feedback have nearly the same execution cost at the 25% budget.

For the mean configuration:

- Always-PQ32: about 24.10 ms/query
- RASF 25%: about 25.18 ms/query
- matched random 25%: about 25.42 ms/query
- Always-SQ8 feedback: about 30.50 ms/query

For the softmax configuration:

- Always-PQ32: about 23.39 ms/query
- RASF 25%: about 25.15 ms/query
- matched random 25%: about 25.27 ms/query
- Always-SQ8 feedback: about 29.81 ms/query

The correct systems interpretation is:

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

For each query, the audit reuses the already measured Always-PQ32 and Always-SQ8 final utilities and evaluates 10,000 independent matched-budget random allocations per configuration and budget.

For a random subset `S` of fixed size:

`U_rand(S) = Ubar_L + (1/N) sum_{q in S}(U_q^H - U_q^L)`

### Primary 25% multi-random result

| Configuration | RASF nDCG@10 | Random expectation | Random 95% upper | RASF − random expectation | Random allocations ≥ RASF | Corrected one-sided p |
|---|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.121894 | 0.118321 | 0.119711 | +0.003573 | 0 / 10,000 | 0.00010 |
| softmax, k=5, α=0.5, T=0.1 | 0.124729 | 0.116384 | 0.118976 | +0.008344 | 0 / 10,000 | 0.00010 |

Thus, for both primary configurations:

`U_RASF > q_0.975(U_random)`

The 10% budget remains mixed; at 50%, both configurations again lie clearly above the matched-budget random-allocation distribution.

The primary RASF conclusion is therefore not an artifact of one unlucky random subset.

ARC-v0.17.1 report SHA-256:

```text
0924f090b8d0b1346cd4fa366abc37930832048fc60b605165eadb31f5796d83
```

---

# Cross-Encoder FEVER Replication

ARC-v0.18 tests whether the approximation-feedback findings obtained with `BAAI/bge-small-en-v1.5` generalize to a second encoder family without choosing the encoder or policy settings after seeing outcomes.

The frozen second encoder is:

```text
intfloat/e5-small-v2
```

with 384 dimensions, E5 `query:` / `passage:` prefixes, L2-normalized embeddings, the same FEVER corpus, the same authoritative 3,350 FIT / 3,316 FIT-held-out validation membership, and the same 44 feedback configurations.

The retrieval ladder is rebuilt under E5 using IVF-PQ32 and IVF-SQ8 with `nprobe=64`.

### One-shot E5 retrieval fidelity

| Retriever | Recall@10 | MRR@10 | nDCG@10 |
|---|---:|---:|---:|
| IVF-PQ32 | 0.6065 | 0.4641 | 0.4824 |
| IVF-SQ8 | 0.7961 | 0.7497 | 0.7372 |

The SQ8 condition remains a valid relative higher-fidelity comparator under E5.

### Aggregate cross-encoder endpoints

| Split | H1 mean | H2 mean | H3 absolute mean | H3 signed mean |
|---|---:|---:|---:|---:|
| FIT | 0.001937 | 0.007603 | 0.030382 | 0.031387 |
| Validation | **0.001882** | **0.007657** | **0.030166** | **0.030957** |

All three primary divergence directions remain positive on FIT-held-out validation.

### E5 validation regime composition

At the primary `epsilon=0.002` threshold:

| Regime | Validation fraction |
|---|---:|
| Stable / null | **45.56%** |
| Amplifying | **41.36%** |
| Reversal | **13.08%** |

This differs materially from the original BGE FEVER regime, where stable/null behavior dominated.

The preregistered v0.18 claim gate therefore returns:

```text
PARTIAL_REPLICATION
```

with **5 / 6** frozen criteria passing.

The single failed criterion is the preregistered requirement that stable/null trajectories remain a majority.

### E5 feedback-gain dose response

Untouched-validation amplification prevalence:

| α | Amplification |
|---:|---:|
| 0.1 | 21.23% |
| 0.3 | 41.23% |
| 0.5 | 50.94% |
| 0.7 | 52.05% |

Amplification incidence therefore rises strongly from α=0.1 to α=0.7, although the mean H3 magnitude is not claimed to be strictly monotonic at every gain.

### E5 FIT → validation reproducibility

| Statistic | Result |
|---|---:|
| Pearson correlation | **0.99749** |
| Spearman correlation | **0.98104** |
| Alpha-centered Pearson | **0.98900** |
| Alpha-centered Spearman | **0.95073** |

The very high alpha-centered correlations indicate that E5 configuration-level amplification structure transfers strongly from FIT to FIT-held-out validation and is not explained only by the common feedback-gain effect.

### Supported cross-encoder interpretation

ARC-v0.18 does **not** support encoder-invariant regime prevalence.

Instead, it supports the more precise statement:

> **Approximation-feedback dynamics generalize across the tested encoder families, but their regime prevalence is representation-dependent.**

The E5 result strengthens the evidence that the phenomenon is not specific to the original BGE embedding geometry while simultaneously identifying an important boundary condition: the frequency of stable, amplifying, and reversal behavior can change substantially across encoder families.

ARC-v0.18 source report SHA-256:

```text
e6e4aaabe8ef6feabe1c702c1e60b3566f7d1c45f02fb374a58532488b099221
```

No FEVER test outcomes were used.

---

# E5 Cross-Encoder Signed-Direction Audit

ARC-v0.18.1 is a post-hoc construct-validity audit of the completed E5 cross-encoder replication.

It does not alter the v0.18 `PARTIAL_REPLICATION` claim gate.

For each trajectory:

`G_t = u_SQ8(t) - u_PQ32(t)`

The primary population is all FIT-held-out validation query-policy events with:

`H3_abs > 0.002`

### Full E5 signed taxonomy

Total validation query-policy events:

```text
145,904
```

Primary amplification events:

```text
60,346  (41.36%)
```

These events involve 2,417 / 3,316 validation queries.

| Signed outcome | Count | Fraction |
|---|---:|---:|
| **Harmful to PQ32** | **58,247** | **96.52%** |
| Beneficial to PQ32 | 2,042 | 3.38% |
| Tied / unresolved | 57 | 0.095% |

Primary query-cluster bootstrap estimate:

`P(G_T > 0 | H3_abs > 0.002) = 0.9652`

with 95% CI:

95% CI: `[0.9593, 0.9706]`

Secondary signed endpoints agree:

`P(H3_signed > 0 | H3_abs > 0.002) = 0.9660`

and

`P(Delta G > 0 | H3_abs > 0.002) = 0.9605`

### Method × α robustness

Across all tested method × α strata, harmful fractions remain in the narrow range:

```text
96.04% – 97.01%
```

### Threshold robustness

| ε | Harmful final fraction |
|---:|---:|
| 0 | 96.48% |
| 0.001 | 96.50% |
| 0.002 | **96.52%** |
| 0.005 | 96.56% |
| 0.010 | 96.71% |

The signed direction is therefore highly stable across the tested threshold range.

### Query-level concentration

Among the 2,417 validation queries with at least one amplification event:

- **2,144** have all observed amplification events harmful to PQ32;
- only **54** have no harmful amplification event;
- the median harmful fraction among affected queries is **1.0**.

The v0.18.1 signed-direction gate returns:

```text
SIGNED_HARM_PRESERVED
```

The supported cross-encoder interpretation is therefore:

> **Regime prevalence is encoder-dependent, but conditional on amplification, the direction of utility divergence remains strongly preserved across the tested BGE and E5 encoder families.**

This result does not imply that amplification is always harmful.

Replay parity matches the persisted v0.18 endpoints to numerical precision.

ARC-v0.18.1 report SHA-256:

```text
1c548a7849bfca09fad417950fc8780bbe016fc0127e3e9d318b65c76ccbaeac
```

No FEVER test outcomes were used.

---

# Cross-Approximation Search-Effort Replication

ARC-v0.19 tests whether the approximation-feedback phenomenon extends beyond the original PQ32-vs-SQ8 **representation-fidelity** contrast to a different approximation mechanism: **search-effort approximation within a fixed index representation**.

The frozen comparison is:

```text
IVF-SQ8, nprobe=8
vs
IVF-SQ8, nprobe=64
```

Both conditions use:

- the same E5 encoder (`intfloat/e5-small-v2`),
- the same normalized 384-d embeddings,
- the same IVF-SQ8 representation,
- the same coarse centroids,
- the same FEVER corpus,
- the same 3,350 FIT / 3,316 FIT-held-out validation split,
- the same 44 mean / softmax feedback configurations.

Only IVF search effort changes. The higher-`nprobe` condition is a **relative higher-search-effort comparator**, not an exact-search oracle.

### One-shot search-effort fidelity

| Condition | DEV nDCG@10 |
|---|---:|
| IVF-SQ8, nprobe=8 | 0.506462 |
| IVF-SQ8, nprobe=64 | **0.737180** |

The frozen one-shot fidelity ordering passes:

```text
nprobe=64 > nprobe=8
```

### Aggregate coupled-trajectory endpoints

| Split | H1 mean | H2 mean | H3 absolute mean | H3 signed mean |
|---|---:|---:|---:|---:|
| FIT | +0.000881 | -0.006978 | -0.003679 | -0.007656 |
| Validation | **+0.000833** | **-0.007495** | **-0.003575** | **-0.007357** |

The search-effort experiment does **not** reproduce the full positive H1/H2/H3 pattern observed under the representation-fidelity contrast.

Instead, the FIT-held-out validation result is:

```text
H1 > 0
H2 < 0
H3_abs < 0
```

The low- vs high-search-effort query states diverge, while candidate and absolute utility disagreement contract on average over later feedback rounds.

This result is retained as a mechanism boundary rather than retuned.

### Validation regime composition

At the primary threshold `epsilon = 0.002`:

| Regime | Validation fraction |
|---|---:|
| Stable / null | **77.45%** |
| Amplifying | **10.34%** |
| Reversal | **12.22%** |

Amplification remains a minority regime.

Across the threshold sweep:

| epsilon | Stable / null | Amplifying | Reversal |
|---:|---:|---:|---:|
| 0.000 | 59.21% | 11.99% | 28.80% |
| 0.001 | 77.24% | 10.44% | 12.32% |
| 0.002 | 77.45% | 10.34% | 12.22% |
| 0.005 | 78.14% | 10.03% | 11.83% |
| 0.010 | 79.07% | 9.56% | 11.37% |

The qualitative interpretation remains stable across the tested thresholds.

### Feedback-gain dose response

Untouched-validation amplification prevalence:

| alpha | Amplification | Mean H3 absolute slope |
|---:|---:|---:|
| 0.1 | 3.24% | -0.000160 |
| 0.3 | 7.95% | -0.001072 |
| 0.5 | 12.25% | -0.004991 |
| 0.7 | **17.91%** | -0.008076 |

A structured minority of trajectories still amplifies, and amplification incidence rises strongly with feedback gain even though the **aggregate H3 direction is contractive**.

The paper therefore distinguishes amplification prevalence from the mean direction of the population-level dynamics.

### FIT -> validation configuration reproducibility

| Statistic | Result |
|---|---:|
| Pearson correlation | **0.996586** |
| Spearman correlation | **0.985298** |
| Alpha-centered Pearson | **0.968564** |
| Alpha-centered Spearman | **0.942160** |

The negative aggregate H2/H3 result is therefore not an isolated validation fluctuation. Configuration-level structure transfers extremely strongly from FIT to FIT-held-out validation, including after controlling for the shared feedback-gain effect.

### Secondary signed-direction endpoint

At the primary amplification threshold `H3_abs > 0.002`:

- amplification events: **15,082**
- affected validation queries: **1,540**
- harmful final direction: **65.97%**
- 95% query-cluster bootstrap CI: **[63.11%, 68.76%]**
- bootstrap replicates: **5,000**

Amplified search-effort trajectories remain more often harmful than beneficial to the lower-search-effort path, but the directional concentration is substantially weaker than under the BGE and E5 representation-fidelity experiments.

| Approximation setting | Harmful fraction conditional on amplification |
|---|---:|
| BGE, PQ32 vs SQ8 | 94.28% |
| E5, PQ32 vs SQ8 | 96.52% |
| E5, SQ8 nprobe 8 vs 64 | **65.97%** |

This identifies a second cross-mechanism boundary:

> **The strong signed-harm direction preserved across encoder families under representation approximation does not transfer unchanged to search-effort approximation.**

### Frozen claim gate

ARC-v0.19 passes **5 / 7** preregistered primary criteria:

| Criterion | Result |
|---|---|
| One-shot high-nprobe nDCG > low-nprobe | PASS |
| H1 positive | PASS |
| H2 positive | **FAIL** |
| H3 positive | **FAIL** |
| Amplification remains a minority | PASS |
| alpha=0.7 amplification > alpha=0.1 | PASS |
| FIT -> validation configuration Pearson > 0.5 | PASS |

Final gate:

```text
CROSS_APPROX_PARTIAL_REPLICATION
```

The correct interpretation is not that search-effort approximation reproduces the original dynamics in full.

Instead:

> **Approximation-feedback behavior depends materially on how approximation enters the retrieval loop. Representation approximation can produce persistent candidate and utility divergence, whereas search-effort approximation in the tested E5 IVF-SQ8 setting produces positive query-state divergence but aggregate candidate and utility contraction, alongside a reproducible minority amplification regime.**

This result directly rejects a universal cross-approximation claim and establishes an empirical boundary condition.

ARC-v0.19 protocol SHA-256:

```text
56a1a88a44593a51f04c1989a4a3ca8d0e969cdf205e2002797481d8f346c58d
```

ARC-v0.19 report SHA-256:

```text
a895acddb90449cf5029f8645e42a35936a93e52e472cf5913b8cf420919aff0
```

No FEVER test outcomes were used.

---

# HNSW Cross-Approximation Replication

ARC-v0.20c extends the search-effort boundary study from IVF `nprobe` to a graph-based ANN family using Faiss `IndexHNSWFlat`.

The frozen primary comparison is:

```text
HNSWFlat, efSearch=8
vs
HNSWFlat, efSearch=256
```

Both conditions use:

- `intfloat/e5-small-v2`,
- the same normalized 384-d FEVER embeddings,
- the same HNSW graph (`M=32`, `efConstruction=200`),
- the same 5,416,568-document corpus,
- the same authoritative 3,350 FIT / 3,316 FIT-held-out validation membership,
- the same 44 feedback configurations,
- and the same four-round evaluator.

Only HNSW search effort changes.

Before any feedback outcome was computed, a FIT-only one-shot ladder was measured:

| efSearch | FIT nDCG@10 |
|---:|---:|
| 8 | 0.567447 |
| 16 | 0.6569 |
| 32 | 0.7177 |
| 64 | 0.7632 |
| 128 | 0.7958 |
| 256 | 0.816108 |

A prespecified selection rule froze `8 → 256` as the primary contrast.

### Primary HNSW validation endpoints

| Endpoint | FIT-held-out validation mean | 95% query-cluster CI |
|---|---:|---:|
| H1 — query-state divergence | **+0.001102** | [0.001045, 0.001158] |
| H2 — candidate divergence | **-0.009808** | [-0.010269, -0.009353] |
| H3abs — absolute utility-gap slope | **-0.006311** | [-0.007706, -0.004906] |
| H3signed | **-0.010276** | [-0.011657, -0.008903] |

FIT gives the same qualitative pattern:

```text
H1       = +0.001192
H2       = -0.009507
H3_abs   = -0.006654
H3_signed= -0.011166
```

The frozen gate resolves to:

```text
HNSW_REVERSAL
```

At `epsilon=0.002`, validation regime composition is:

| Regime | Fraction |
|---|---:|
| Stable / null | **77.16%** |
| Amplifying | **9.78%** |
| Contracting | **13.06%** |

Amplification prevalence remains structured and rises with feedback gain, but the population-level H3abs mean becomes increasingly negative as `alpha` increases.

### HNSW signed direction

Conditional on amplification:

- amplification events: **14,269**
- higher-fidelity (`efSearch=256`) final-round wins: **64.06%**
- 95% query-cluster CI: **[61.48%, 66.59%]**
- lower-fidelity wins: **31.29%**
- ties: **4.65%**

This is substantially weaker directional concentration than under representation approximation:

```text
BGE representation: 94.28%
E5 representation : 96.52%
E5 IVF nprobe     : 65.97%
E5 HNSW           : 64.06%
```

The HNSW result therefore independently reproduces the IVF search-effort boundary:

> **Search-effort approximation can produce positive query-state divergence while candidate and utility disagreement contract on average.**

---

# HNSW Post-Primary Severity Sensitivity

ARC-v0.20d is a **post-primary exploratory sensitivity audit**. It does not replace or retrospectively alter the frozen ARC-v0.20c primary gate.

The same HNSW index and `efSearch=256` comparator are retained while the lower-search-effort condition is made less severe:

```text
32 → 256
64 → 256
```

The FIT one-shot nDCG@10 gaps shrink from:

```text
8  → 256 : 0.2487
32 → 256 : 0.0984
64 → 256 : 0.0530
```

### FIT-held-out validation severity results

| Contrast | H1 | H2 | H3abs | H3signed |
|---|---:|---:|---:|---:|
| `32 → 256` | +0.000444 | -0.019851 | **-0.002201** | -0.004636 |
| `64 → 256` | +0.000238 | -0.017491 | **-0.001053** | -0.002700 |

H3abs query-cluster intervals remain below zero:

```text
32 → 256 : [-0.00327, -0.00114]
64 → 256 : [-0.00195, -0.00019]
```

Validation regime composition moves toward stability as severity decreases:

| Contrast | Stable / null | Amplifying | Contracting |
|---|---:|---:|---:|
| `32 → 256` | 86.01% | 6.38% | 7.61% |
| `64 → 256` | 89.96% | 4.83% | 5.21% |

FIT reproduces the same negative H3abs pattern:

```text
32 → 256 : -0.002324
64 → 256 : -0.000915
```

The defensible interpretation is:

> **The HNSW contraction observed in the frozen primary study does not require only the extreme `8 → 256` contrast. Effect magnitude weakens and stable/null prevalence increases as the search-effort gap shrinks, while the tested milder contrasts preserve the same qualitative sign pattern.**

This is not claimed as a preregistered continuous severity law.

---

# E5 RASF Cross-Encoder Method Replication

ARC-v0.21 tests whether the RASF method remains useful after an encoder and prevalence-regime change.

This is a **method replication**, not zero-shot transfer of BGE model weights:

- the same deployment-feasible feature family is re-fit on E5 FIT;
- the logistic-regression hyperparameters are unchanged;
- the primary budget remains **25%**;
- the two inherited intervention policies remain fixed;
- the selector is frozen before E5 validation intervention outcomes.

The E5 amplification prevalence is much higher:

```text
41.36%
```

### E5 susceptibility prediction

| Metric | E5 FIT-held-out validation |
|---|---:|
| ROC-AUC | **0.6368** |
| 95% query-cluster CI | [0.6286, 0.6451] |
| PR-AUC | **0.4921** |
| 95% query-cluster CI | [0.4783, 0.5066] |
| Prevalence baseline | 0.4136 |
| Capture@25% | **31.37%** |
| Capture@25% CI | [30.77%, 31.98%] |
| Random ranking reference | 25% |

The lower-fidelity feature family therefore retains modest cross-encoder predictive signal.

### E5 25% selective-intervention result

| Policy | Always-PQ32 | Always-SQ8 feedback | RASF 25% | Random expectation | RASF − random | One-sided MC p |
|---|---:|---:|---:|---:|---:|---:|
| mean, `alpha=0.3, k=20` | 0.32217 | 0.41593 | 0.34418 | 0.34561 | **-0.00143** | **0.834** |
| softmax, `alpha=0.5, k=5, tau=0.1` | 0.27310 | 0.51890 | 0.33474 | 0.33455 | **+0.00018** | **0.467** |

Secondary 50% budgets are also nonsignificant.

The frozen v0.21 gate is therefore:

```text
E5_RASF_PRIMARY_FAILS_TO_REPLICATE
```

This negative result is retained rather than retuned.

The supported interpretation is:

> **Susceptibility prediction transfers modestly across the tested encoders, but the original amplification-risk ranking does not reliably estimate the utility value of higher-fidelity feedback under E5.**

Equivalently, prediction and control should be treated as distinct questions:

```text
P(amplification | lower-fidelity observables)
```

need not induce the same ranking as:

```text
E[utility gain from higher-fidelity feedback | lower-fidelity observables]
```

This result motivates future **value-aware** or **utility-per-cost-aware** routing, but those algorithms are outside the frozen paper contribution.

---

# Current Research Claim

The completed evidence chain is now:

```text
Phenomenon discovery
→ retriever-condition validation
→ sealed FEVER confirmation
→ sealed HotpotQA cross-dataset confirmation
→ boundary / stability analysis
→ deployable risk prediction
→ signed-direction validation
→ FEVER RASF selective-control closure
→ 10,000-allocation random robustness audit
→ E5 cross-encoder representation replication
→ E5 signed-direction replication
→ IVF nprobe mechanism falsification
→ HNSW cross-index-family replication
→ HNSW post-primary severity robustness
→ E5 RASF cross-encoder control non-replication
```

The strongest supported paper-facing claim is:

> **Approximation-induced retrieval differences that are acceptable in one-shot evaluation can become dynamically consequential under iterative feedback, but the resulting dynamics are neither universal nor determined by one-shot loss alone. Their aggregate sign, prevalence, and directional harm depend materially on how approximation enters the retrieval-feedback loop.**

The completed evidence supports the following narrower conclusions.

### 1. One-shot ANN fidelity and feedback stability are distinct

Representation-approximation studies show positive aggregate query-state, candidate-set, and utility-gap slopes, but search-effort studies show that positive query-state divergence can coexist with aggregate candidate and utility contraction.

A one-shot quality ordering therefore does not uniquely determine later feedback dynamics.

### 2. Representation approximation produces reproducible expansion in the tested BGE/E5 settings

Under the frozen representation contrasts:

- FEVER-BGE and HotpotQA-BGE pass sealed H1-H4 confirmation;
- E5 preserves positive aggregate H1, H2, and H3abs;
- effect magnitudes and regime prevalence are not claimed to be invariant.

### 3. Regime prevalence is encoder-dependent

At `epsilon=0.002` on FEVER:

```text
BGE amplification :  7.95%
E5 amplification  : 41.36%
```

The mechanism transfers more strongly than the prevalence.

### 4. Representation-amplification direction is strongly concentrated toward lower-fidelity harm

Conditional on amplification:

```text
BGE representation : 94.28% higher-fidelity final wins
E5 representation  : 96.52% higher-fidelity final wins
```

Beneficial and tied counterexamples are retained; no universal-harm claim is made.

### 5. Search-effort approximation establishes a genuine mechanism boundary

Under E5:

```text
IVF-SQ8 nprobe 8 → 64
H1       = +0.000833
H2       = -0.007495
H3_abs   = -0.003575
```

and:

```text
HNSW efSearch 8 → 256
H1       = +0.001102
H2       = -0.009808
H3_abs   = -0.006311
```

Thus two distinct search-effort interventions reproduce the same qualitative reversal across IVF and HNSW index families.

### 6. One-shot gap magnitude alone is insufficient

A descriptive cross-mechanism comparison has similar reported one-shot gaps:

```text
E5 representation PQ32 → SQ8 : 0.2548
E5 HNSW efSearch 8 → 256      : 0.2487
```

but opposite H3abs signs:

```text
representation : +0.030166
HNSW           : -0.006311
```

Because the gaps arise from different frozen evaluation/calibration procedures, this is **not** a matched causal experiment. It nevertheless argues against gap magnitude alone as a sufficient explanation.

### 7. HNSW contraction persists under milder post-primary contrasts

The exploratory `32 → 256` and `64 → 256` HNSW audits preserve:

```text
H1 > 0
H2 < 0
H3_abs < 0
```

while effect magnitude shrinks and stable/null prevalence rises.

### 8. Lower-fidelity observables contain predictive susceptibility information

Under FEVER-BGE, the deployable PQ32-only selector reaches:

```text
ROC-AUC      = 0.7309
PR-AUC       = 0.1793
Capture@25%  = 57.99%
random       = 25.00%
```

Feature ablation shows that query-specific PQ32 score geometry adds substantial predictive information beyond policy priors.

### 9. Selective fidelity is useful at the frozen FEVER-BGE operating point

At the primary 25% budget, RASF beats matched-budget random allocation for both frozen BGE intervention policies, and no sampled random allocation among 10,000 reaches the observed RASF utility.

The claim is budget- and setting-specific.

### 10. Prediction does not guarantee intervention value

Under E5, susceptibility prediction remains above chance:

```text
ROC-AUC = 0.6368
PR-AUC  = 0.4921
```

but the frozen 25% RASF intervention does not beat matched-budget random allocation for either primary policy.

The current evidence therefore supports a distinction between:

```text
susceptibility prediction
```

and:

```text
treatment / intervention value
```

---

The project supports:

- dynamic approximation-feedback effects across FEVER and HotpotQA;
- representation-mechanism replication across BGE and E5;
- encoder-dependent regime prevalence;
- strongly preserved signed-harm direction under the tested representation contrasts;
- cross-mechanism reversal under IVF `nprobe`;
- cross-index-family replication of that reversal under HNSW `efSearch`;
- post-primary robustness of HNSW reversal under milder search-effort contrasts;
- deployable lower-fidelity susceptibility prediction;
- selective higher-fidelity feedback control at the frozen FEVER-BGE operating point;
- and a frozen E5 controller non-replication establishing that prediction and intervention value are distinct.

The project does **not** claim that:

- approximation errors always amplify;
- amplification is always harmful;
- stable/null behavior dominates every encoder;
- one-shot approximation loss determines iterative feedback instability;
- similar one-shot quality gaps form a matched causal experiment across mechanisms;
- all approximation mechanisms produce the same feedback dynamics;
- the representation/search-effort boundary is a proved contraction theorem;
- signed-harm concentration is invariant across approximation mechanisms;
- the mechanism is universal across all encoders, indexes, ANN algorithms, datasets, or feedback systems;
- the current predictor fully explains query-level susceptibility;
- RASF is a universal or production-validated routing policy;
- RASF generalizes across encoders merely because amplification risk is predictable;
- the selective policy is universally optimal across datasets, encoders, ANN systems, budgets, policies, or hardware;
- higher-fidelity comparators are exact-search oracles;
- or local Apple M3 Max / Colab measurements are universal production latency or throughput claims.

---

# Repository Structure

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

# Experimental Philosophy

The project separates:

- exploratory discovery,
- retriever-condition replication,
- direct mechanism probing,
- statistical auditing,
- sealed independent confirmation,
- cross-dataset confirmation,
- post-confirmatory boundary analysis,
- held-out prediction,
- selective mitigation,
- measured systems-cost auditing,
- deterministic post-run provenance repair,
- cross-policy boundary transfer,
- external FEVER boundary replication,
- zero-mass / regime auditing,
- mechanism diagnostics,
- deployment-feasible prediction,
- full-coverage signed construct validation,
- threshold-sensitivity and alpha-controlled robustness auditing,
- deployable predictor feature-ablation auditing,
- feature provenance equality auditing,
- same-setting deployable selective-fidelity closure,
- multi-random allocation robustness auditing,
- cross-encoder external replication,
- cross-encoder signed-direction construct validation,
- cross-approximation search-effort boundary testing,
- cross-index-family HNSW mechanism replication,
- post-primary HNSW severity-sensitivity auditing,
- and cross-encoder selective-control non-replication.

Positive, null, stable, reversal, beneficial-divergence, partial-replication, and non-replication outcomes are retained rather than filtered away.

---

# Current Status

## Completed

- large-corpus memory-safe IVF-PQ infrastructure
- retriever-condition replication
- synchronized approximation-feedback trajectories
- statistical mechanism audit
- sealed FEVER H1-H4 confirmation
- audited HotpotQA retrieval rebuild
- sealed HotpotQA H1-H4 cross-dataset confirmation
- HotpotQA boundary/stability map
- boundary-aware selective fidelity mitigation
- local Apple M3 Max quality-cost / Pareto audit
- deterministic quality-cost merge audit
- cross-policy HotpotQA boundary transfer
- FEVER external boundary replication
- FEVER zero-mass / regime audit
- FEVER mechanism audit
- deployable PQ32-only FEVER risk prediction
- full-coverage FEVER signed-harm audit
- threshold-sensitivity and alpha-controlled FEVER robustness audit
- deployable predictor feature-ablation audit
- FEVER PQ32 query-feature provenance equality audit
- FEVER Risk-Aware Selective Fidelity end-to-end closure
- 10,000-allocation matched-random robustness audit
- E5 cross-encoder FEVER representation replication
- E5 full signed-direction audit with `SIGNED_HARM_PRESERVED`
- IVF `nprobe` cross-approximation boundary test
- HNSW `efSearch` cross-index-family mechanism replication with `HNSW_REVERSAL`
- HNSW post-primary `32 → 256` / `64 → 256` severity-sensitivity audit
- E5 RASF cross-encoder method replication with `E5_RASF_PRIMARY_FAILS_TO_REPLICATE`
- formal operator-decomposition interpretation in the manuscript
- factual and numerical manuscript audit
- reference audit
- claim-strength / citation-support audit
- adversarial empirical-IR / ANN-systems / theory-reviewer audit
- anonymity / comments / tracked-changes audit
- full eight-page render and layout QA

## Experimental line status

The large-scale experimental line through **ARC-v0.21 is complete and frozen for the current paper**.

Negative, partial, contracting, beneficial, exploratory, and failed-transfer outcomes are retained rather than retuned.

The paper does not convert:

- the v0.18 5/6 cross-encoder gate into full replication,
- the v0.19 5/7 nprobe gate into a positive cross-approximation replication,
- the post-primary HNSW severity audit into a preregistered severity law,
- or the v0.21 E5 RASF failure into a positive controller-transfer result.

## Current manuscript state

Current paper title:

> **Approximation under Feedback: Mechanism-Dependent Stability and Directional Harm in Iterative Retrieval**

The manuscript is an **8-page SIGIR full-paper submission candidate**.

Current paper-facing status:

```text
experiment freeze        : complete
claim audit               : complete
factual / numerical audit: complete
reference audit           : complete
citation-support audit    : complete
reviewer attack audit     : complete
anonymity cleanup         : complete
render / layout QA        : complete
```

## Remaining submission-facing work

- verify final compliance with the official ACM / SIGIR submission template and current call requirements;
- verify the final anonymized artifact package and links;
- perform one final PDF-level pre-upload inspection after any template conversion;
- freeze the submission PDF and artifact hashes;
- submit without further outcome-driven scientific retuning.

No new large experiment is currently required for the frozen paper unless a correctness bug is discovered.

---

# Research Scope

This repository is an active research artifact.

The intended contribution is an empirical and methodological study of approximation under retrieval feedback, not a claim that any specific ANN index, encoder, feedback policy, or mitigation strategy is universally optimal. The current evidence specifically separates representation approximation from search-effort approximation and separates susceptibility prediction from intervention value.

The project emphasizes:

- explicit separation between exploratory and confirmatory stages,
- frozen or sealed evaluation protocols where appropriate,
- held-out validation,
- query-level or query-cluster uncertainty where appropriate,
- retention of negative / null / reversal / beneficial outcomes,
- exact replay and provenance audits,
- artifact hashing and checkpoint verification,
- and conservative interpretation when evidence does not support universal claims.
