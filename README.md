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
- and a fixed empirical regime threshold `|H3| = 0.002`.

### Zero-mass finding

The FEVER H3 distribution contains substantial exact-zero mass.

On FIT:

- exact zero: **88.25%**
- near zero (`|H3| <= 1e-12`): **90.06%**
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
H3_{\text{abs}}
=
\operatorname{slope}
\left(
|u_{\text{SQ8}}(t) - u_{\text{PQ32}}(t)|
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
| `P(G_T > 0 | H3_abs > 0.002)` | **0.9428** | **[0.9248, 0.9580]** |
| `P(H3_signed > 0 | H3_abs > 0.002)` | **0.9469** | **[0.9296, 0.9618]** |
| `P(ΔG > 0 | H3_abs > 0.002)` | **0.9390** | **[0.9208, 0.9544]** |

Harmful fractions remain above 91% in every tested method × α stratum.

Accordingly, the project now supports the stronger but still limited statement:

> **Most FEVER trajectories classified as absolute amplification are directionally harmful to the lower-fidelity PQ32 trajectory, but beneficial and tied counterexamples remain and are retained.**

The project does **not** claim that amplification is always harmful.

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
\text{Selective mitigation}
\rightarrow
\text{Measured quality-cost audit}
```

The strongest supported claim at this stage is:

> **Approximation-induced retrieval differences that are tolerable in one-shot evaluation can become dynamically consequential under iterative feedback. In the tested FEVER and HotpotQA settings, the dominant regime is stable/null, while a minority amplifying regime is reproducible across held-out queries, increases with feedback gain, is partially predictable from lower-fidelity retrieval statistics and policy variables, and is predominantly directionally harmful when signed utility is reconstructed. In HotpotQA, a fit-frozen boundary-aware policy selectively allocates higher-fidelity feedback and recovers a disproportionate fraction of the Always-SQ8 quality benefit at a fraction of its measured incremental runtime.**

The project does **not** claim that:

- approximation errors always amplify,
- amplification is always harmful,
- the mechanism is universal across all encoders or ANN systems,
- the current predictor fully explains query-level susceptibility,
- the FEVER deployable selector has already been validated as a full end-to-end production router,
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
17. full-coverage signed construct validation.

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

### Remaining paper-facing work

- threshold-sensitivity analysis for the empirical `|H3| = 0.002` regime definition,
- alpha-controlled / within-alpha configuration robustness analysis,
- final SIGIR full-paper methods, figures, references, and claim audit.

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
