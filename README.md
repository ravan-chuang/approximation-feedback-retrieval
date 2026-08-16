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

\[
\text{retrieval approximation}
\rightarrow
\text{feedback contamination}
\rightarrow
\text{query-state divergence}
\rightarrow
\text{candidate divergence}
\rightarrow
\text{utility divergence}
\]

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
| \(R^2\) | 0.247 |
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

## Current Research Claim

The project currently supports the following evidence chain:

\[
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
\text{Selective mitigation study}
\]

The strongest supported claim at this stage is:

> Approximation errors that are tolerable in one-shot retrieval can become dynamically consequential under iterative feedback. The effect replicates across the tested FEVER and HotpotQA settings, varies across fidelity and feedback regimes, and is partially predictable from initial retrieval-state features.

The project does **not** claim that:

- approximation errors always amplify,
- the mechanism is universal across all encoders or ANN systems,
- the current boundary model fully explains query-level susceptibility,
- or selective mitigation is effective before ARC-v0.10 is completed.

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
9. selective mitigation.

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

### In progress

- final full-paper framing and external-validity checks

The conclusions and framing may change as mitigation and additional external-validity experiments are completed.
