# Approximation-Feedback Retrieval

Research project studying how retrieval approximation errors propagate when retrieved evidence is fed back into subsequent query states.

The project now focuses on a broader question than one-shot ANN quality:

> **When do approximation errors become dynamically amplified by retrieval feedback, when do they remain bounded, and can the resulting risk be predicted and controlled?**

---

## Core Research Question

Approximate nearest-neighbor retrieval is usually evaluated as a one-shot operation.

This project studies a different regime:

> Can approximation errors that are tolerable in one-shot retrieval become amplified when retrieved results modify subsequent query states?

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
- stable / null / reversal regimes,
- and selective mitigation using higher-fidelity feedback.

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
| ARC-v0.12 | Cross-Policy Boundary Transfer | Held-out transfer of boundary-risk signal across feedback-policy families |
| ARC-v0.13 | FEVER Boundary External Replication | Cross-dataset boundary/stability external-validity audit (in progress) |

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

The invalid artifacts were quarantined, the 5.23M-document corpus was re-encoded, and the retrieval infrastructure was rebuilt from verified embedding shards.

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

This gives a second, independent cross-dataset confirmation of the tested mechanism.

The effect magnitudes are not identical across datasets: HotpotQA shows somewhat smaller state/candidate divergence effects but substantially larger downstream utility-gap and intervention effects.

This motivates the next research question:

> **What controls the transition between stable, amplifying, and reversal regimes?**

---

## Boundary and Stability Analysis

ARC-v0.9 is explicitly post-confirmatory.

It does not replace or modify the sealed v0.8 result.

The study introduces a deterministic 50/50 boundary-fit / boundary-validation split and evaluates:

- retrieval-fidelity dose response,
- feedback gain and concentration,
- query-level initial-state features,
- stable / null / reversal regimes,
- and held-out prediction of later amplification.

### Fidelity dose response

Using the same frozen feedback policies:

| Fidelity contrast | H1 slope | H2 slope | H3 slope |
|---|---:|---:|---:|
| PQ32 ↔ PQ64 | 0.002632 | 0.001596 | 0.012274 |
| PQ64 ↔ SQ8 | 0.001514 | 0.005360 | 0.013828 |
| PQ32 ↔ SQ8 | **0.003229** | **0.005590** | **0.022308** |

The largest fidelity contrast produces the strongest overall amplification, although the intermediate contrasts are not strictly monotonic for every endpoint.

### Held-out boundary prediction

A simple interpretable first-order model trained only on the boundary-fit half achieved approximately:

| Metric | Validation result |
|---|---:|
| $R^2$ | 0.247 |
| ROC-AUC | 0.774 |
| Average Precision | 0.516 |
| Accuracy | 0.690 |

These results indicate that amplification susceptibility is **partially predictable before feedback unfolds**, but not fully explained by the current first-order features.

### Heterogeneous regimes

The boundary sweep retains all outcomes rather than keeping only positive cases.

Across feedback configurations, queries fall into three empirical regimes:

- **amplifying**
- **stable / null**
- **reversal**

Even in strong amplification settings, a substantial fraction of queries remain stable or reverse direction.

Accordingly, the current claim is **not** that approximation errors always amplify.

A more precise interpretation is:

> **Approximation-feedback amplification is a heterogeneous dynamical regime whose risk depends on retrieval fidelity, feedback gain/concentration, and query-specific retrieval geometry.**

---

## Boundary-Aware Selective Fidelity Mitigation

ARC-v0.10 tests whether the v0.9 boundary predictor can be translated into an actionable systems policy.

The evaluation keeps PQ32 as the search/evaluation retriever and changes only the feedback source:

1. **Always-PQ32** — PQ32 search → PQ32 feedback
2. **Always-SQ8** — PQ32 search → SQ8 feedback
3. **Random selective SQ8** — budget-matched random high-fidelity allocation
4. **Boundary-aware selective SQ8** — v0.9 fit-only risk model allocates SQ8 feedback

The nominal high-fidelity budgets are 10%, 25%, and 50%. The primary pre-specified operating point is 25%.

Applying the fit-frozen 25% threshold to the held-out validation partition selects **26.47%** of queries.
The threshold is not re-tuned on validation.

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

A negative/null boundary case is retained: for mean feedback at 10%, Δ vs random is +0.00218 with a
95% bootstrap interval of [-0.00032, 0.00477], so the experiment does not claim a reliable advantage
over random at that operating point.

Protocol SHA-256:
`a48d2efbd470314de1042c85d8888312b3f61d1c68d3456f6f91a3150320817f`

Report SHA-256:
`fdf64925334500bc4823fd828510d9a3ec946719fda940cc8eabd55a8cd44589`

Status: **complete**.

---

## Local M3 Max System Cost and Final Pareto Audit

ARC-v0.11 measures the systems cost of the frozen ARC-v0.10 policies on a local Apple M3 Max CPU environment. ARC-v0.11.1 performs a deterministic aggregate join repair for the baseline quality rows; no retrieval, policy, or systems benchmark is rerun.

The primary nominal 25% threshold selects **26.47%** of held-out validation queries for SQ8 feedback.

| Feedback configuration | Selective nDCG@10 | Recovery | Δ vs random (95% CI) | Selective runtime | Random runtime | Fraction of Always-SQ8 extra runtime |
|---|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.357904 | 41.1% | +0.007084 [0.003560, 0.010677] | 12.170 s | 11.977 s | 27.13% |
| softmax, k=5, α=0.5, T=0.1 | 0.376525 | 44.1% | +0.034576 [0.026755, 0.042598] | 11.840 s | 12.078 s | 26.90% |

The boundary-aware and budget-matched random policies have nearly identical measured runtime at the primary operating point. The measured quality gain therefore is not explained by boundary-aware allocation using more high-fidelity work.

Relative to Always-PQ32, the selective policy recovers **41–44%** of the Always-SQ8 quality improvement while using about **27%** of the Always-SQ8 incremental runtime. The corresponding recovery / incremental-runtime ratios are **1.516×** and **1.639×**.

ARC-v0.11 report SHA-256:
`d6b49ced27062a76d64b234e8711c652a9d293f464d7a747a6ad2ab5ef13c5f8`

ARC-v0.11.1 final audit SHA-256:
`e01c4145dfca90ac289bbdd8402b9c4497db9c88b09c28dba37982e4dacd5973`

These are single-machine Apple M3 Max CPU measurements and are not universal production latency or throughput claims.

---


## Cross-Policy Boundary Transfer

ARC-v0.12 tests whether the post-confirmatory boundary-risk signal learned in one HotpotQA feedback-policy family transfers to the other family on the held-out boundary-validation split.

Before accepting the transfer result, the audit reconstructs the sealed ARC-v0.9 classifier. The reconstructed validation ROC-AUC is **0.7736072867** versus the sealed **0.7736072913**, an absolute difference of `4.58e-9`.

| Source → target | ROC-AUC | 95% bootstrap CI | Average Precision | AUC gap to target-family fit-only oracle |
|---|---:|---:|---:|---:|
| mean → softmax | **0.766532** | **[0.756424, 0.776532]** | 0.544860 | -0.016485 |
| softmax → mean | **0.755019** | **[0.745265, 0.764847]** | 0.435071 | +0.014342 |

Both directions retain substantial held-out discrimination. The supported claim is limited to **transferable boundary structure across the tested mean and softmax feedback-policy families**; it is not a universal transfer claim across datasets, encoders, ANN families, or deployment settings.

Protocol SHA-256:
`5789f053aa51a1bc830b7bb55a0ae95dc7f6dee91211e2353403ccb5b835848d`

Report SHA-256:
`77e8582c74c71748b8deca85f1f5d652b2db517d5927339706677db4fd87ab01`

ARC-v0.13 then extends the external-validity question to FEVER. That experiment is currently running and **no FEVER boundary outcome is claimed yet**.

---

## Current Research Claim

The project currently supports the following evidence chain:

```math
\text{Phenomenon}
\rightarrow
\text{Sealed confirmation}
\rightarrow
\text{Cross-dataset replication}
\rightarrow
\text{Fidelity / stability boundary}
\rightarrow
\text{Held-out susceptibility prediction}
\rightarrow
\text{Selective mitigation}
\rightarrow
\text{Measured quality-cost audit}
\rightarrow
\text{Cross-policy boundary transfer}
```

The strongest supported claim at this stage is:

> Approximation errors that are tolerable in one-shot retrieval can become dynamically consequential under iterative feedback. The effect replicates across the tested FEVER and HotpotQA settings, varies across fidelity and feedback regimes, is partially predictable from initial retrieval-state features, can be selectively mitigated in the tested HotpotQA setting with a favorable measured quality-cost tradeoff, and the tested boundary-risk signal retains substantial held-out discrimination across the mean and softmax HotpotQA feedback-policy families.

The project does **not** claim that:

- approximation errors always amplify,
- the mechanism is universal across all encoders or ANN systems,
- the current boundary model fully explains query-level susceptibility,
- the selective policy is universally optimal across datasets, encoders, ANN systems, or hardware,
- or the Apple M3 Max measurements are universal production latency/throughput claims.

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
13. cross-dataset boundary external-validity replication.

Positive, null, and reversal outcomes are retained.

The primary statistical unit in confirmatory experiments is the query.

Post-confirmatory analyses are labeled explicitly and are not retroactively treated as confirmatory evidence.

---

## Reproducibility

Where applicable, experiments retain:

- deterministic seeds,
- frozen feedback configurations,
- ANN index configurations,
- protocol SHA-256 hashes,
- report SHA-256 hashes,
- immutable or audited corpus representations,
- query-level paired inference,
- bootstrap confidence intervals,
- paired sign-flip tests,
- Holm multiple-comparison correction,
- fit / validation separation for boundary modeling,
- measured single-machine latency / throughput / memory audits,
- deterministic aggregate-join repair with source hashes,
- and public-facing aggregate evidence artifacts.

---

## Research Status

Active research project.

### Completed

- FEVER sealed H1–H4 confirmation
- HotpotQA corpus/index repair and validity audit
- HotpotQA full baseline validation
- HotpotQA sealed H1–H4 cross-dataset confirmation
- fidelity dose-response analysis
- boundary/stability mapping
- held-out query-level amplification prediction
- boundary-aware selective-fidelity mitigation
- local Apple M3 Max system-cost / Pareto audit
- v0.11.1 final quality-cost merge audit and SHA-256 seal
- ARC-v0.12 cross-policy boundary transfer

### In progress

- ARC-v0.13 FEVER boundary external replication
- final full-paper framing and external-validity checks

The conclusions and framing may change as mitigation and additional external-validity experiments are completed.
