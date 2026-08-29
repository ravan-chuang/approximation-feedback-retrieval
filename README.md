# Approximation-Feedback Retrieval

Research project studying how retrieval approximation errors evolve when retrieved evidence is fed back into subsequent query states.

The project asks a broader question than one-shot ANN quality:

> **When do approximation-induced retrieval differences remain bounded, when do they become dynamically amplified by feedback, and can the resulting risk be predicted and selectively controlled?**

---


## Current Paper-Facing Snapshot

The current manuscript baseline is the reviewer-hardened **v18.6 SIGIR 2026 full-paper candidate**:

> **Approximation in the Loop: When One-Shot ANN Quality Is Insufficient for Iterative Dense Retrieval**

The paper studies approximate retrieval as part of a **state transition**, rather than as a sequence of independent ANN calls. Lower- and higher-fidelity branches share the corpus, encoder, initial query, feedback operator, retrieval depth, task, and evaluator; only the ANN intervention changes.

The current paper-facing evidence supports a narrower and more defensible thesis than a universal amplification claim:

> **One-shot ANN quality is informative, but it is not sufficient to determine feedback-time behavior. Approximation mechanism, state-transition operator, horizon, and estimand must be evaluated separately.**

### Current headline evidence

1. **Matched one-shot loss does not imply matched loop dynamics.** In directly severity-matched settings, representation-minus-search-effort `H3_abs` is positive on FEVER-E5, MS MARCO-E5, and NQ-GTE.
2. **The NQ matched-mechanism result is independently strong.** With `thenlper/gte-small`, the paired representation-minus-search `H3_abs` contrast is **+0.011868**, 95% CI **[+0.009922, +0.013794]**.
3. **The result survives an exhaustive reference audit.** On a frozen 500-query NQ-GTE subset, replacing the relative high-fidelity comparator with exhaustive FlatIP over the persisted embedding store gives **+0.012035**, 95% CI **[+0.007966, +0.016277]**.
4. **One-shot equality is not dynamic equality.** Among the same 500 frozen NQ-GTE queries, **45.2%** are exact one-shot ties between representation and search-effort approximation; **27.0%** of those ties become terminally distinguishable after four anchored feedback updates.
5. **One-shot selection can incur measurable downstream retrieval regret.** Among queries decisive at both one-shot and terminal evaluation, the one-shot-selected mechanism disagrees with the terminal oracle on **7.72%** of queries; mean terminal regret is **0.01181 nDCG@10**.
6. **Mechanism is not severity.** On FEVER-E5, representation approximation yields positive short-horizon `H3_abs`, whereas matched IVF `nprobe` search-effort approximation yields negative `H3_abs`; HNSW `efSearch` reproduces the search-effort boundary.
7. **Forcing is not propagation.** Common-state replay shows that representation approximation injects the largest direct perturbation while exhibiting the smallest realized common-map propagation ratio in the audited anchored setting.
8. **Trend is not level.** Under recursive feedback, the representation-minus-search temporal ordering reverses at long horizons, but the terminal absolute gap remains larger for representation approximation.
9. **The long-horizon reversal replicates.** At `H=50`, recursive representation-minus-search `H3_abs` is **-0.000866** on NQ-GTE and **-0.00266** on FEVER-E5, while the `H=50` terminal absolute-gap level remains positive in both settings.
10. **Generative rewriting is an operator boundary, not a universal confirmation.** Frozen Qwen2.5-3B and Mistral-7B rewrite operators leave the centroid-era `H3_abs` ordering unresolved, while terminal semantic, candidate, and absolute-utility separation remains larger for representation approximation in nominal secondary analyses.
11. **The paper deliberately separates confirmatory and post-primary evidence.** Frozen and untouched validations support the strongest claims; exact-reference, decision-consequence, construct-robustness, and generative terminal analyses are labeled according to their evidence status.
12. **The next external-validity audit is frozen before outcomes.** ARC-v0.35 is a 768-dimensional NQ replication using `thenlper/gte-base`; it is currently a prospective protocol and has **no reported outcome yet**.

### Central severity-controlled result

| Setting | One-shot severity relation | Validation / frozen result |
|---|---:|---:|
| FEVER-E5 representation: IVF-PQ32 → IVF-SQ8, `nprobe=64` | FIT loss = **0.249338** | `H3_abs` = **+0.030166** |
| FEVER-E5 search effort: IVF-SQ8, `nprobe=8 → 64` | FIT loss = **0.238035** | `H3_abs` = **-0.003573** |
| NQ-GTE matched representation − search | 2.39% one-shot loss mismatch | **+0.011868** [**+0.009922**, **+0.013794**] |
| NQ-GTE exhaustive-reference representation − search | exhaustive FlatIP comparator | **+0.012035** [**+0.007966**, **+0.016277**] |

For FEVER-E5, the FIT one-shot loss mismatch is **0.011303 absolute / 4.533% relative**. On 3,316 untouched validation queries, the paired representation-minus-matched-search-effort contrast is:

```text
+0.033739
95% CI [+0.031838, +0.035642]
```

The supported interpretation is deliberately narrower than a causal or deployment theorem:

> **Matching one-shot nDCG@10 loss does not make representation approximation and reduced search effort dynamically interchangeable inside a feedback loop.**

This is **not** a cost-matched deployment comparison. One-shot severity matching equalizes the task-effectiveness loss used for calibration; it does not equalize latency, memory, energy, candidate overlap, score distortion, or perturbation geometry.

### Current claim boundary

The project does **not** claim that:

- representation approximation is universally worse than search-effort approximation;
- `H3_abs` is a universal harm, stability, or risk metric;
- recursive long-horizon behavior can be inferred from `H=4`;
- centroid feedback represents all modern retrieval agents;
- the current 384-d evidence is scale-invariant to larger embedding models;
- the two LLM rewrite audits constitute complete autonomous-agent benchmarks; or
- one-shot ANN evaluation should be discarded.

The current deployment-facing recommendation is instead:

> **Use conventional one-shot quality / latency / memory evaluation as a first-stage screen. When retrieved evidence changes the next query, audit tied or deployment-critical ANN configurations inside the actual state-transition operator and deployment horizon.**

---

## NQ-GTE Long-Horizon Operator/Horizon Boundary (ARC-v0.28)

ARC-v0.28 is a **post-v0.27 reviewer-oriented prospective audit**, not a pristine new confirmation. It reuses the frozen BEIR Natural Questions + `thenlper/gte-small` severity-matched design from ARC-v0.27 and retains the same 1,726 validation queries. No severity, split, operator, long-horizon subset, endpoint, or success criterion was retuned after validation trajectories were observed.

The full 44-policy grid is retained through H=8 for comparability. A structurally selected, outcome-independent eight-policy subset (mean-k20 and softmax-k20-t0.1 for each alpha in {0.1, 0.3, 0.5, 0.7}) is evaluated at H={4,8,12,16,24,32,40,50}. The primary new estimand was frozen as the recursive H=50 query-averaged nDCG@10 H3abs representation-minus-search contrast.

### Frozen primary outcome

The pre-specified positive-persistence hypothesis is **not supported**:

```text
recursive H=50 representation-minus-search H3abs
= -0.000866
95% CI [-0.001083, -0.000657]
```

The negative primary is retained unchanged.

### Horizon-conditioned transition

| Horizon | Anchored rep-search H3abs | Recursive rep-search H3abs |
|---:|---:|---:|
| 4  | +0.014659 | +0.015420 |
| 8  | +0.004509 | +0.002231 |
| 12 | +0.002082 | -0.000374 |
| 16 | +0.001189 | -0.001050 |
| 24 | +0.000537 | -0.001306 |
| 32 | +0.000305 | -0.001244 |
| 40 | +0.000196 | -0.001072 |
| 50 | +0.000126 | -0.000866 |

For recursive feedback, the contrast is positive at H=4/H=8, unresolved at H=12, and significantly negative from H=16 through H=50. Anchored feedback remains positive while shrinking toward zero.

The supported interpretation is therefore narrower and more informative than monotonic long-horizon persistence:

> **Approximation-feedback dynamics are jointly conditioned by approximation mechanism, state-update operator, and horizon. Short-horizon cross-mechanism ordering need not extrapolate monotonically to persistent recursive retrieval-feedback trajectories.**

### Execution and provenance

The validation sweep used the A100 `GPU_BATCHED` backend after a FIT-only CPU/GPU semantic audit. The completed run contains 1,726 validation queries and 5,053,728 trajectory rows. The canonical protocol SHA-256 is:

```text
15f968514bde2602870ac0a08a3bb6a9eca77b2ca40038120b8f82f5017ec93a
```

ARC-v0.28 is a controlled bridge toward Agentic IR / Agentic RAG sequential retrieval, **not** a full LLM-agent experiment: it does not instantiate an LLM planner, adaptive tool policy, learned stopping policy, memory manager, or answer-generation environment.

---

## Latest Reviewer-Hardening Audits (ARC-v0.29–v0.35)

| Version | Audit | Status / paper-facing role |
|---|---|---|
| ARC-v0.29 | FEVER-E5 operator/horizon replication | Frozen long-horizon replication. Recursive `H=50` representation-minus-search `H3_abs = -0.00266`, 95% CI `[-0.00287, -0.00246]`; terminal level remains positive. |
| ARC-v0.30 | Reviewer-hardening audit bundle | Post-primary reviewer-oriented robustness and evidence-lineage hardening; does not replace prespecified primaries. |
| ARC-v0.31 | NQ-GTE generative retrieval-agent transfer | Frozen Qwen2.5-3B deterministic rewrite operator. Primary `H3_abs` contrast unresolved at **-0.002192**, 95% CI **[-0.009999, +0.006089]**; terminal representation-induced separation is larger in nominal secondary endpoints. |
| ARC-v0.32 | NQ-GTE exhaustive-reference audit | Replaces the relative comparator with exhaustive FlatIP over the persisted shared embedding store. Representation-minus-search `H3_abs = +0.012035`, 95% CI **[+0.007966, +0.016277]**. |
| ARC-v0.33 | NQ-GTE one-shot decision-regret audit | 45.2% exact one-shot ties; 27.0% of ties become terminally distinguishable; one-shot selector disagreement with terminal oracle = 7.72%; mean terminal regret = 0.01181 nDCG@10. |
| ARC-v0.34 | Second generative-operator transfer | Frozen Mistral-7B-Instruct-v0.3 audit on the same 500-query membership. Primary `H3_abs = -0.00112`, 95% CI **[-0.00909, +0.00728]**; terminal semantic/candidate/absolute-utility separation remains larger for representation in nominal secondary endpoints. |
| ARC-v0.35 | NQ GTE-base 768-d frozen replication | **Prospective / pending.** Frozen external-validity replication with `thenlper/gte-base` (768-d), PQ64 vs SQ8 representation contrast, FIT-only search-effort calibration, `H=4`, eight-policy anchored subset, and a 10k paired-query bootstrap primary. No outcome is reported until the frozen VALID run completes. |

### Generative-operator boundary

The two completed LLM rewrite audits should be read as controlled **operator-boundary** experiments, not as complete-agent benchmarks.

| Model | Frozen primary rep − search `H3_abs` | Terminal semantic difference | Terminal candidate difference | Terminal `|ΔnDCG@10|` difference |
|---|---:|---:|---:|---:|
| Qwen2.5-3B | -0.0022, unresolved | +0.0139 | +0.1178 | +0.0543 |
| Mistral-7B | -0.0011, unresolved | +0.0163 | +0.1152 | +0.0619 |

The frozen temporal primary is unresolved under both models. Terminal intervals are nominal secondary analyses and are not used to overturn the primary null.

### ARC-v0.35 frozen success criterion

ARC-v0.35 directly addresses the strongest remaining encoder-scale limitation. The primary estimand is:

```text
Delta_768 = H3_abs(representation) - H3_abs(search effort)
```

The protocol is frozen before GTE-base VALID outcomes:

- dataset: BEIR Natural Questions;
- encoder: `thenlper/gte-base`, expected dimension = 768;
- representation contrast: IVF-PQ64@64 vs IVF-SQ8@64;
- search-effort contrast: IVF-SQ8@FIT-selected `nprobe` vs IVF-SQ8@64;
- feedback: anchored centroid, `H=4`;
- policy set: fixed eight-policy structural subset;
- inference: within-query policy aggregation followed by 10,000 paired-query bootstrap replicates;
- retention: positive, unresolved, or reversed outcomes are all retained without retuning.

A positive CI would establish transfer to **one 768-d encoder**, not universal scale invariance.

---

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
| ARC-v0.18 | Cross-Embedding-Model FEVER Replication | E5-small-v2 representation-mechanism generalization test under the frozen FEVER design |
| ARC-v0.18.1 | E5 Signed-Direction Audit | Post-hoc signed construct-validity audit of v0.18 amplification events |
| ARC-v0.19 | Cross-Approximation nprobe Replication | Search-effort approximation boundary test using IVF-SQ8 nprobe 8 vs 64 |
| ARC-v0.20c | HNSW Mechanism Replication | Frozen graph-search boundary test, `efSearch 8 → 256` |
| ARC-v0.20d | HNSW Severity Sensitivity | Post-primary exploratory `32 → 256` and `64 → 256` robustness audit |
| ARC-v0.21 | E5 RASF Cross-Embedding-Model Replication | Frozen controller-method replication under E5; susceptibility prediction transfers modestly, 25% intervention advantage does not |
| ARC-v0.22 | H3 Robustness Audit | Frozen construct-robustness audit across alternative utility-gap summaries; primary sign gate passes |
| ARC-v0.23 | Eq. 6 Common-State Operator Replay | Frozen post-primary decomposition of propagated-state vs direct-fidelity perturbations across E5 representation, IVF nprobe, and HNSW interventions |
| ARC-v0.23.1 | NaN-Safe Statistical Recompute | Analysis-only repair with explicit finite-coverage reporting; no retrieval rerun or undefined-value imputation |
| ARC-v0.24 | MS MARCO External Boundary Replication | Outcome-blind full-corpus E5 replication on 8.84M BEIR MS MARCO passages; representation positive and nprobe negative in the frozen all-policy estimand |
| ARC-v0.25 | FEVER-E5 Severity-Matched Mechanism Validation | FIT-only one-shot nDCG@10 severity calibration followed by untouched 3,316-query validation; paired mechanism contrast remains strongly positive |
| ARC-v0.26 | FEVER-E5 Softmax Score-Channel Audit | Completed post-primary audit: exact candidate rescoring preserves positive representation H3abs while significantly reducing its magnitude; not an exact-search experiment |
| ARC-v0.27 | NQ-GTE Independent Severity-Matched Confirmation | FIT-only severity calibration on BEIR NQ + GTE-small followed by outcome-blind 1,726-query validation; paired representation-minus-search H3abs remains clearly positive |
| ARC-v0.28 | NQ-GTE H=50 Operator/Horizon Agentic-IR Bridge Audit | Frozen post-v0.27 long-horizon audit: anchored feedback decays toward zero, while recursive feedback crosses from positive short-horizon ordering to a significant negative ordering from H=16 through H=50 |
| ARC-v0.29 | FEVER-E5 H=50 Operator/Horizon Replication | Frozen long-horizon replication of the recursive temporal-order reversal on FEVER-E5 |
| ARC-v0.30 | Reviewer-Hardening Audit Bundle | Post-primary robustness, evidence-lineage, and reviewer-facing claim hardening |
| ARC-v0.31 | Qwen Generative Rewrite Operator Audit | Frozen deterministic Qwen2.5-3B operator-boundary audit; temporal primary unresolved, terminal separation larger for representation |
| ARC-v0.32 | NQ-GTE Exhaustive FlatIP Reference Audit | Frozen 500-query comparator audit against exhaustive search over the persisted embedding store |
| ARC-v0.33 | NQ-GTE One-Shot Decision Regret Audit | Post-primary consequence audit of one-shot ties, terminal distinguishability, selector disagreement, and regret |
| ARC-v0.34 | Mistral Generative Operator Transfer | Independent deterministic Mistral-7B rewrite audit on the same frozen NQ query membership |
| ARC-v0.35 | NQ GTE-base 768-d Frozen Replication | Prospective encoder-scale external-validity replication; protocol frozen, outcome pending |


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

# Cross-Embedding-Model FEVER Replication

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

### Aggregate cross-embedding-model endpoints

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

### Supported cross-embedding-model interpretation

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

# E5 Cross-Embedding-Model Signed-Direction Audit

ARC-v0.18.1 is a post-hoc construct-validity audit of the completed E5 cross-embedding-model replication.

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

The supported cross-embedding-model interpretation is therefore:

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

# E5 RASF Cross-Embedding-Model Method Replication

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

The lower-fidelity feature family therefore retains modest cross-embedding-model predictive signal.

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

---

# H3 Construct Robustness Audit

ARC-v0.22 tests whether the central mechanism-dependent H3 conclusion depends on the primary five-round OLS slope definition of absolute utility-gap dynamics. The audit is post-primary and does not alter any earlier frozen claim gate.

It evaluates three alternative summaries on the same 3,316 FIT-held-out FEVER validation queries and 44 feedback configurations per setting (145,904 query-policy events):

- **R1 — endpoint gap change:** final absolute utility gap minus initial absolute utility gap;
- **R2 — round-averaged gap change:** later-round average absolute utility gap relative to the initial gap;
- **R3 — maximum excursion:** maximum later absolute utility gap relative to the initial gap.

### Validation robustness results

| Setting | Primary H3abs | R1 endpoint-gap change | R2 round-average change | R3 max excursion | R1 95% query-cluster CI |
|---|---:|---:|---:|---:|---:|
| BGE representation | **+0.006771** | **+0.030722** | **+0.018494** | +0.037572 | [0.026773, 0.034737] |
| E5 representation | **+0.030166** | **+0.147000** | **+0.077332** | +0.192453 | [0.138565, 0.155333] |
| E5 IVF nprobe search effort | **-0.003575** | **-0.014123** | **-0.010812** | +0.046372 | [-0.020647, -0.007737] |
| E5 HNSW search effort | **-0.006311** | **-0.025450** | **-0.018828** | +0.044676 | [-0.031720, -0.019217] |

The primary robustness gate returns:

```text
H3_ROBUSTNESS_PRIMARY_PASS
```

R1 preserves the sign of the primary H3abs endpoint in all four settings, with all four query-cluster confidence intervals excluding zero. R2 independently preserves the same representation-positive / search-effort-negative sign pattern.

R3 is positive in all four settings. This is retained rather than retuned: a search-effort trajectory can exhibit a transient intermediate excursion while still contracting in its endpoint, round-average, and OLS trend. Accordingly, the search-effort result is interpreted as an **aggregate trajectory contraction**, not as a claim that every intermediate feedback round monotonically contracts.

The supported robustness conclusion is:

> **The observed representation-expansion / search-effort-contraction boundary is not specific to the primary OLS-slope construction. Endpoint and round-averaged utility-gap summaries preserve the same qualitative mechanism-dependent sign across all four audited settings, while maximum excursion exposes transient divergence that is compatible with aggregate contraction.**

No FEVER test outcomes are used.

# Eq. 6 Common-State Operator Replay

ARC-v0.23 is a **frozen post-primary mechanism-hardening audit** of the manuscript decomposition

```text
T_H(q_H^t) - T_L(q_L^t)
= [T_H(q_H^t) - T_H(q_L^t)]
+ [T_H(q_L^t) - T_L(q_L^t)].
```

The first term is the **propagated-state component** and the second is the **direct-fidelity component**. The replay holds the low-fidelity state fixed for the common-state counterfactual and compares three E5 interventions on the same FEVER FIT-held-out validation split:

- representation: IVF-PQ32 → IVF-SQ8 at `nprobe=64`;
- IVF search effort: SQ8 `nprobe=8 → 64`;
- HNSW search effort: `efSearch=8 → 256`.

Across 3,316 validation queries, 44 frozen feedback policies, four rounds, and the three interventions, the replay measures state-vector components together with candidate, feedback, and utility perturbation proxies.

### Common-state component profile

| Measure | E5 representation | E5 IVF nprobe | E5 HNSW |
|---|---:|---:|---:|
| State direct norm | **0.06941** | 0.02870 | 0.02768 |
| State propagated norm | **0.04801** | 0.03439 | 0.03756 |
| Candidate direct Jaccard distance | **0.85344** | 0.45143 | 0.69794 |
| Feedback direct cosine distance | **0.01781** | 0.00650 | 0.00857 |
| Utility direct absolute gap | **0.34884** | 0.17721 | 0.15505 |
| State cancellation ratio | 0.84768 | **0.89132** | 0.87777 |

The representation contrast therefore produces substantially larger **fixed-common-state direct perturbations** than either tested search-effort contrast across state, candidate, feedback, and utility levels. It also produces larger propagated-state separation.

For the state direct norm, the paired query-level representation-minus-search-effort contrasts are:

```text
representation − IVF nprobe = +0.040713
95% CI: [0.040176, 0.041243]

representation − HNSW = +0.041733
95% CI: [0.040993, 0.042479]
```

All **14 / 14** preselected core representation-vs-search-effort comparisons have query-level 95% intervals excluding zero, including **8 / 8** direct-profile comparisons. The pattern is interpreted jointly rather than by the count alone.

The cancellation result is also informative: search-effort contraction is **not** explained by stronger vector cancellation in this audit. The representation condition has the lowest mean cancellation ratio of the three settings. Accordingly, the evidence supports distinct empirical operator-perturbation profiles, not a simple cancellation explanation.

The supported interpretation is deliberately limited:

> **A frozen post-primary common-state replay shows that the tested representation and search-effort interventions have substantially different direct and propagated perturbation profiles. Representation approximation induces larger fixed-common-state perturbations across state, candidate, feedback, and utility levels. These associations strengthen the empirical mechanism boundary, but they do not constitute a causal mediation result or an operator-level theorem.**

No FEVER test outcomes are used.

## NaN-Safe Statistical Recompute

ARC-v0.23.1 repairs the statistical reporting layer of the v0.23 replay without rerunning retrieval. Undefined quantities—most notably component cosine when one component has zero norm—remain undefined rather than being imputed, and finite coverage is reported explicitly before bootstrap estimation.

The repaired analysis retains the v0.23 conclusions:

```text
status: ARC_V0231_ANALYSIS_REPAIR_COMPLETE
retrieval_rerun: false
undefined_values_imputed: false
validation_queries: 3316
core representation-vs-search-effort CIs excluding zero: 14 / 14
direct-profile CIs excluding zero: 8 / 8
```

For example, undefined component-cosine values can occur when the propagated component is exactly zero (including the structurally expected initial-round case). ARC-v0.23.1 treats this as mathematical undefinedness rather than silently dropping or replacing it.

The v0.23/v0.23.1 evidence is therefore **post-primary mechanism hardening**. It may support wording such as *associated with distinct operator-perturbation profiles*, but not claims that the replay proves why representation expands or why search-effort approximation contracts.

---

# MS MARCO External Boundary Replication

ARC-v0.24 tests whether the mechanism-conditioned approximation-feedback boundary transfers to a large non-Wikipedia corpus under a protocol frozen before the new feedback-trajectory outcomes were inspected.

The experiment uses:

- **BEIR MS MARCO passage**;
- **8,841,823 passages**;
- `intfloat/e5-small-v2`, 384 dimensions;
- index-representation contrast: IVF-PQ32 → IVF-SQ8, both at `nprobe=64`;
- search-effort contrast: IVF-SQ8, `nprobe=8 → 64`;
- the same **44** anchored mean / softmax feedback policies;
- four feedback updates / five states;
- 3,490 validation queries;
- query as the primary statistical unit.

The completed validation sweep contains:

```text
trajectory rows : 1,535,600
endpoint rows   :   307,120
queries         :     3,490
```

### Frozen all-policy primary result

| Mechanism | Mean H3abs | 95% query-bootstrap CI |
|---|---:|---:|
| Index representation | **+0.001916** | **[+0.000428, +0.003422]** |
| IVF `nprobe` search effort | **-0.001933** | **[-0.003085, -0.000816]** |
| Representation − nprobe | **+0.003849** | **positive; CI excludes zero** |

The frozen all-policy sign map therefore transfers:

```text
representation H3abs > 0
nprobe H3abs         < 0
```

### Operator-family audit

The external result also identifies an important boundary:

- mean-only representation H3abs is approximately null / slightly negative;
- softmax-only representation H3abs is positive;
- equal-family representation H3abs is positive in point estimate but its interval crosses zero;
- nprobe remains contractive;
- the paired representation-minus-nprobe ordering remains positive.

Accordingly, ARC-v0.24 does **not** support a universal statement that index-representation approximation expands under every feedback operator.

The supported external conclusion is:

> **The cross-mechanism ordering transfers to MS MARCO, while the standalone representation-side expansion is operator-modulated.**

The MS MARCO experiment increases external corpus validity. It is not a severity-matched replication, and it does not independently change the encoder or IVF index family.

---

# FEVER-E5 Severity-Matched Mechanism Validation

ARC-v0.25 addresses a major alternative explanation:

> Could the representation/search-effort sign difference be explained simply by the representation intervention having a larger one-shot task-effectiveness loss?

The calibration is conducted on **FIT only** and freezes the lower search-effort setting before validation trajectories.

### FIT-only severity calibration

Representation contrast:

```text
IVF-PQ32 @ nprobe=64
→ IVF-SQ8 @ nprobe=64
```

Search-effort ladder:

```text
IVF-SQ8 @ nprobe_low
→ IVF-SQ8 @ nprobe=64
```

The closest FIT nDCG@10 match is:

| Quantity | Value |
|---|---:|
| Representation one-shot gap | **0.249338** |
| Matched search-effort gap (`nprobe=8`) | **0.238035** |
| Absolute mismatch | **0.011303** |
| Relative mismatch | **4.533%** |

The frozen selected lower search effort is therefore:

```text
MATCHED_NPROBE = 8
```

No post-selection validation retuning is permitted.

### Untouched validation result

The validation branch uses 3,316 queries and the same 44 frozen feedback policies.

| Estimand | Mean H3abs | 95% query-bootstrap CI |
|---|---:|---:|
| Index representation | **+0.030166** | **[+0.028332, +0.032006]** |
| Severity-matched nprobe | **-0.003573** | **[-0.005007, -0.002143]** |
| Representation − matched nprobe | **+0.033739** | **[+0.031838, +0.035642]** |

The family-balanced audit gives:

| Estimand | Mean | 95% query-bootstrap CI |
|---|---:|---:|
| Representation | **+0.029501** | **[+0.027631, +0.031321]** |
| Severity-matched nprobe | **-0.003942** | **[-0.005397, -0.002500]** |
| Representation − matched nprobe | **+0.033443** | **[+0.031567, +0.035386]** |

The supported conclusion is:

> **Approximately matching one-shot nDCG@10 effectiveness loss does not remove the representation-versus-search-effort feedback-dynamics contrast on FEVER-E5.**

This is **not** equivalent to matching every dimension of approximation severity. Candidate overlap, rank displacement, recall loss, and score distortion are not jointly matched, so ARC-v0.25 is not presented as universal causal identification of “mechanism alone.”

---

# FEVER-E5 Softmax Score-Channel Audit

ARC-v0.26 is a completed **post-primary reviewer-oriented mechanism audit** that separates the softmax feedback score channel from ANN candidate/evidence selection under the FEVER-E5 index-representation contrast.

The motivation is that softmax feedback can mix two channels:

```text
ANN candidate / evidence selection
+
ANN score geometry / calibration
```

The frozen audit keeps the representation intervention fixed:

```text
IVF-PQ32 @ nprobe=64
vs
IVF-SQ8  @ nprobe=64
```

and evaluates all **32 frozen softmax policies** over the same **3,316 validation queries** under two feedback-weight channels:

1. **ANN-score softmax** — use the scores returned by each ANN index;
2. **shared exact candidate rescoring** — keep each branch's ANN-retrieved candidate IDs, but recompute feedback weights from the FP32 dot product between the current normalized query state and the shared normalized corpus embedding.

The second condition is **not exact nearest-neighbor search**. It removes ANN approximate-score calibration from feedback weighting over already retrieved candidates while preserving ANN candidate selection.

The completed validation audit contains:

```text
validation queries : 3,316
softmax policies   : 32
score channels     : 2
feedback updates   : 4
trajectory rows    : 1,061,120
```

### Query-level score-channel result

| Estimand | Mean | 95% query-bootstrap CI |
|---|---:|---:|
| ANN-score H3abs | **+0.030964** | **[+0.029110, +0.032769]** |
| Exact-rescore H3abs | **+0.022000** | **[+0.020365, +0.023672]** |
| Exact-rescore − ANN-score H3abs | **-0.008964** | **[-0.009406, -0.008540]** |

The endpoint final-minus-initial absolute-gap summary preserves the same pattern:

| Estimand | R1 | 95% query-bootstrap CI |
|---|---:|---:|
| ANN-score R1 | **+0.149064** | **[+0.140532, +0.157424]** |
| Exact-rescore R1 | **+0.105189** | **[+0.097779, +0.112638]** |
| Exact-rescore − ANN-score R1 | **-0.043875** | **[-0.045898, -0.041869]** |

The alpha audit also preserves positive H3abs in both channels:

| alpha | ANN-score H3abs | Exact-rescore H3abs |
|---:|---:|---:|
| 0.1 | 0.008447 | 0.005880 |
| 0.3 | 0.029174 | 0.019983 |
| 0.5 | 0.043635 | 0.031130 |
| 0.7 | 0.042600 | 0.031005 |

The supported interpretation is:

> **Shared exact rescoring materially reduces representation-side softmax amplification, but it does not eliminate it. Under this tested FEVER-E5 setup, ANN score geometry contributes to the magnitude, while candidate/evidence-selection perturbation remains sufficient for positive short-horizon representation-side H3abs.**

This audit is explicitly post-primary rather than a pristine confirmation. It does **not** establish exact-search behavior, isolate every possible score/candidate interaction, or prove that the same decomposition transfers to other feedback operators, corpora, encoders, or index families.

### Provenance repair

The completed validation checkpoints and scientific artifacts belong to the original frozen run:

```text
20260824-150217
```

Frozen protocol SHA-256:

```text
6fa8c981ffb15d7929371c4ee9457a8612cb03b850cf53bc2d6d85142dd8e789
```

A runtime restart created a second run directory and temporarily caused the final report to reference the wrong protocol SHA. The final report was repaired **metadata-only** to point back to the original frozen protocol. Trajectories, endpoints, bootstrap estimates, policies, split membership, and scientific results were not modified or recomputed.

Final repaired report SHA-256:

```text
b6239e140e3e8382a23156d495c58aa9a0974ad7f7af0cacde45766e8b1a3eb7
```



# NQ-GTE Independent Severity-Matched Confirmation

ARC-v0.27 combines a new corpus, a new encoder family, FIT-only one-shot severity calibration, and outcome-blind validation feedback trajectories.

```text
dataset                 : BEIR Natural Questions
encoder                 : thenlper/gte-small
corpus                  : 2,681,468 documents
queries                 : 3,452
FIT / validation        : 1,726 / 1,726
feedback policies       : 44
feedback updates        : 4
representation contrast : IVF-PQ32 -> IVF-SQ8, nprobe=64
search-effort contrast  : IVF-SQ8, nprobe=2 -> 64
```

FIT-only one-shot nDCG@10 severity calibration gives:

| Severity target | Gap |
|---|---:|
| Representation: SQ8 - PQ32 | **0.161114** |
| Search effort: SQ8 n64 - SQ8 n2 | **0.164972** |

The relative gap mismatch is **2.395%**.

The final confirmation protocol was frozen before validation trajectories with SHA-256:

```text
44a15f18297dce9d5db2b590f403859763f916ed0f3cfb3a196d3b2198ecc94a
```

### Primary validation result

| Estimand | Mean H3abs | 95% query-bootstrap CI |
|---|---:|---:|
| Representation | **+0.010953** | **[+0.009191, +0.012770]** |
| Severity-matched search effort | **-0.000916** | **[-0.002299, +0.000436]** |
| Representation - search | **+0.011868** | **[+0.009922, +0.013794]** |

The primary confirmatory boundary is supported because the paired representation-minus-search interval lies strictly above zero.

The search-effort estimate itself is not claimed to show reliable contraction on NQ because its 95% CI crosses zero. The stronger pre-specified conclusion is the positive cross-mechanism ordering under matched one-shot effectiveness degradation.

### Feedback-family robustness

| Estimand | Paired difference | 95% CI |
|---|---:|---:|
| Mean-only | **+0.011488** | **[+0.009541, +0.013421]** |
| Softmax-only | **+0.012011** | **[+0.010107, +0.013930]** |
| Equal-family | **+0.011750** | **[+0.009836, +0.013690]** |

At epsilon=0.002, representation events are amplifying in **33.10%** of endpoint events versus **14.05%** for severity-matched search effort, while stable/null behavior is **46.92%** versus **70.36%**.

> **On a new corpus and encoder family, matching one-shot nDCG@10 degradation on FIT does not remove the positive representation-versus-search-effort H3abs ordering on untouched validation trajectories.**

ARC-v0.27 does not establish a universal mechanism law and does not remove the short-horizon anchored-feedback scope limitation.


# Current Research Claim

The completed evidence chain is now:

```text
Phenomenon discovery
→ retriever-condition validation
→ sealed FEVER confirmation
→ sealed HotpotQA cross-dataset confirmation
→ boundary / regime analysis
→ held-out susceptibility prediction
→ signed-direction validation
→ FEVER RASF selective-control closure
→ 10,000-allocation random robustness audit
→ E5 cross-embedding-model representation replication
→ E5 signed-direction replication
→ IVF nprobe mechanism falsification
→ HNSW cross-index-family replication
→ HNSW post-primary severity robustness
→ E5 RASF cross-embedding-model control non-replication
→ H3 construct-robustness audit
→ Eq. 6 common-state operator replay
→ NaN-safe statistical recompute
→ MS MARCO external boundary replication
→ FEVER-E5 severity-matched mechanism validation
→ FEVER-E5 softmax score-channel audit
```

The strongest supported paper-facing claim is:

> **Approximation-induced retrieval differences that appear acceptable in one-shot evaluation can become dynamically consequential under iterative feedback, but the resulting short-horizon dynamics are neither universal nor determined by one-shot task-effectiveness loss alone. Their aggregate direction, prevalence, and directional harm depend materially on how approximation enters the retrieval-feedback loop and can be modulated by the feedback operator.**

The completed evidence supports the following narrower conclusions.

### 1. One-shot ANN fidelity and feedback dynamics are distinct

Index-representation experiments can show positive aggregate query-state, candidate-set, and utility-gap divergence, while search-effort experiments show that positive query-state divergence can coexist with aggregate candidate and utility contraction.

Therefore, a one-shot quality ordering does not uniquely determine later feedback dynamics.

### 2. Approximation mechanism is a reproducible boundary condition

Under FEVER-E5:

```text
Index representation: IVF-PQ32 → IVF-SQ8
H3_abs = +0.030166

IVF search effort: SQ8 nprobe 8 → 64
H3_abs = -0.003575

HNSW search effort: efSearch 8 → 256
H3_abs = -0.006311
```

The same qualitative search-effort reversal appears in both IVF and HNSW.

### 3. Matching one-shot nDCG@10 loss does not remove the mechanism contrast

ARC-v0.25 approximately matches the FIT one-shot effectiveness loss:

```text
representation gap      = 0.249338
matched nprobe gap      = 0.238035
relative gap mismatch   = 4.533%
```

but untouched FEVER-E5 validation still gives:

```text
representation H3abs        = +0.030166
matched-nprobe H3abs        = -0.003573
paired difference           = +0.033739
95% CI                      = [0.031838, 0.035642]
```

This rejects one-shot nDCG@10 gap magnitude as a sufficient explanation of the observed sign difference.

It does **not** prove that all dimensions of approximation severity are matched.

### 4. The cross-mechanism ordering transfers to MS MARCO

On 8.84M BEIR MS MARCO passages with 3,490 validation queries:

```text
representation H3abs = +0.001916
nprobe H3abs         = -0.001933
paired ordering      = positive
```

The all-policy result therefore externally reproduces the representation-versus-search-effort ordering.

However, the standalone representation sign is **operator-modulated**: mean-only is approximately null while softmax-only is positive, and the equal-family representation interval crosses zero.

The external claim is therefore about the cross-mechanism ordering, not universal representation expansion.

### 5. The main H3 mechanism boundary is robust to alternative temporal summaries

ARC-v0.22 shows the same representation-positive / search-effort-negative sign pattern for both:

- R1 endpoint absolute-gap change;
- R2 later-round average absolute-gap change.

R3 maximum excursion is positive for every mechanism, showing that a trajectory can exhibit transient divergence while still contracting in endpoint, round-average, and OLS summaries.

### 6. Common-state replay shows distinct empirical perturbation profiles

Under E5, the fixed-common-state direct perturbations are substantially larger for index representation than for either tested search-effort mechanism.

Representative means:

| Measure | E5 representation | E5 IVF nprobe | E5 HNSW |
|---|---:|---:|---:|
| State direct norm | **0.06941** | 0.02870 | 0.02768 |
| Candidate direct Jaccard | **0.85344** | 0.45143 | 0.69794 |
| Feedback direct cosine distance | **0.01781** | 0.00650 | 0.00857 |
| Utility direct absolute gap | **0.34884** | 0.17721 | 0.15505 |

This strengthens the empirical mechanism interpretation but is not a causal mediation result or contraction theorem.

### 7. Regime prevalence is encoder-dependent

At `epsilon=0.002` on FEVER:

```text
BGE representation amplification :  7.95%
E5 representation amplification  : 41.36%
```

The existence and direction of the phenomenon transfer more strongly than its prevalence.

### 8. Conditional directional harm is strong under the tested representation contrasts

Conditional on representation-side amplification:

```text
BGE representation : 94.28% higher-fidelity final wins
E5 representation  : 96.52% higher-fidelity final wins
```

Beneficial and tied counterexamples are retained.

The corresponding directional concentration is substantially weaker under the tested search-effort mechanisms.

### 9. Lower-fidelity observables contain predictive susceptibility information

Under FEVER-BGE, the deployable PQ32-only selector reaches:

```text
ROC-AUC      = 0.7309
PR-AUC       = 0.1793
Capture@25%  = 57.99%
random       = 25.00%
```

Feature ablation shows that query-specific PQ32 score geometry contributes substantial held-out discrimination beyond policy priors.

This is a predictive-information result, not a causal explanation.

### 10. Prediction does not guarantee intervention value

At the frozen FEVER-BGE 25% operating point, RASF beats matched-budget random selection for both primary policies and exceeds all 10,000 sampled matched-budget random allocations.

Under E5, the same deployment-feasible feature family still predicts susceptibility above chance:

```text
ROC-AUC = 0.6368
PR-AUC  = 0.4921
```

but the frozen 25% intervention does not reliably beat random:

```text
mean policy    : Δ = -0.00143, p = 0.834
softmax policy : Δ = +0.00018, p = 0.467
```

Therefore:

```text
P(amplification | lower-fidelity observables)
```

does not imply the same ranking as:

```text
E[utility value of higher-fidelity feedback | lower-fidelity observables].
```

### 11. Softmax score geometry contributes to magnitude but is not sufficient to explain representation amplification

ARC-v0.26 separates ANN-score weighting from candidate/evidence selection while preserving ANN-retrieved candidate IDs.

Across 3,316 FEVER-E5 validation queries and all 32 frozen softmax policies:

```text
ANN-score H3abs        = +0.030964
Exact-rescore H3abs    = +0.022000
Exact − ANN            = -0.008964
95% CI of Exact − ANN  = [-0.009406, -0.008540]
```

The exact-rescored H3abs remains clearly positive, while its magnitude is significantly lower than the ANN-score condition.

The supported interpretation is therefore two-part:

- ANN score geometry / calibration materially increases representation-side softmax amplification in this setting;
- candidate/evidence-selection perturbation remains sufficient for positive short-horizon H3abs after shared exact candidate rescoring.

Because exact rescoring is performed only over ANN-retrieved candidates, this is **not** an exact-search experiment and is not a universal causal decomposition.

---

The project currently supports:

- one-shot-vs-feedback evaluation separation;
- sealed FEVER and HotpotQA representation-side confirmations;
- cross-embedding-model FEVER representation replication;
- encoder-dependent regime prevalence;
- strongly preserved signed-harm direction under the tested representation contrasts;
- cross-mechanism reversal under IVF `nprobe`;
- cross-index-family replication of search-effort contraction under HNSW `efSearch`;
- robustness of that contraction under milder HNSW contrasts;
- alternative H3 temporal-summary robustness;
- common-state operator-perturbation profiling;
- large-corpus MS MARCO external replication of the cross-mechanism ordering;
- FEVER-E5 one-shot-severity-matched validation of the mechanism contrast;
- FEVER-E5 softmax score-channel decomposition showing that exact candidate rescoring reduces but does not eliminate representation-side H3abs;
- deployable lower-fidelity susceptibility prediction;
- selective high-fidelity feedback value at the frozen FEVER-BGE operating point;
- and a frozen E5 controller non-replication showing that susceptibility prediction and intervention value are distinct.

The project does **not** claim that:

- approximation errors always amplify;
- index-representation approximation always expands under every feedback operator;
- search-effort approximation always contracts under every ANN system;
- amplification is always harmful;
- stable/null behavior dominates every encoder;
- one-shot approximation loss determines iterative feedback behavior;
- matching nDCG@10 loss matches every notion of approximation severity;
- the mechanism boundary is a proved causal law, contraction theorem, or Lyapunov-stability result;
- the current four-update anchored feedback loop represents arbitrary long-horizon agentic retrieval;
- the current mean / softmax feedback operators cover learned or generative feedback;
- SQ8 or high-search-effort conditions are exact-search oracles;
- MS MARCO independently varies corpus, encoder, and index family at the same time;
- the current predictor fully explains query-level susceptibility;
- RASF is a universal or production-validated router;
- RASF generalizes across encoders merely because amplification risk remains predictable;
- local Apple M3 Max / Colab measurements are universal production throughput guarantees;
- or ARC-v0.26 constitutes exact nearest-neighbor search or a universal causal decomposition of candidate selection and score calibration.

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
- cross-embedding-model external replication,
- cross-embedding-model signed-direction construct validation,
- cross-approximation search-effort boundary testing,
- cross-index-family HNSW mechanism replication,
- post-primary HNSW severity-sensitivity auditing,
- cross-embedding-model selective-control non-replication,
- H3 construct-robustness auditing,
- common-state operator replay and finite-coverage repair,
- large-corpus outcome-blind MS MARCO external replication,
- FIT-only one-shot-severity matching followed by frozen validation,
- and post-primary score-channel auditing that separates candidate selection from softmax score calibration.

Positive, null, stable, reversal, beneficial-divergence, partial-replication, and non-replication outcomes are retained rather than filtered away.

---

# Current Status

## Completed paper-facing evidence

The current manuscript evidence is complete through **ARC-v0.34**. Major completed components include:

- sealed FEVER and HotpotQA trajectory confirmations;
- cross-encoder BGE/E5 replication and signed-direction audits;
- IVF `nprobe` and HNSW `efSearch` search-effort boundary tests;
- threshold, policy, estimator, and score-channel robustness audits;
- deployable susceptibility prediction and RASF intervention studies, including retained transfer failures;
- full-corpus BEIR MS MARCO external transfer;
- FEVER-E5 and NQ-GTE FIT-only one-shot severity matching;
- NQ-GTE independent matched-mechanism confirmation;
- NQ-GTE and FEVER-E5 recursive `H=50` operator/horizon audits;
- common-state replay separating direct forcing from realized propagation;
- exhaustive FlatIP reference audit on the frozen 500-query NQ-GTE subset;
- one-shot tie / terminal distinguishability / selector-regret consequence audit;
- deterministic Qwen2.5-3B generative rewrite audit;
- independent deterministic Mistral-7B generative operator transfer;
- factual, numerical, reference, terminology, claim-strength, anonymity, and render/layout hardening for the v18.6 manuscript.

### Completed score-channel audit (ARC-v0.26)

ARC-v0.26 remains a post-primary reviewer-oriented mechanism audit. Shared exact candidate rescoring lowers representation `H3_abs` from **+0.030964** to **+0.022000**; the paired exact-minus-ANN change is **-0.008964**, 95% CI **[-0.009406, -0.008540]**. The representation trend remains positive. This is evidence that score distortion contributes to magnitude but does not, by itself, explain the short-horizon representation result. It is **not** exact-search evidence.

## In progress / outcome pending

**ARC-v0.35 — NQ GTE-base 768-d Frozen Replication** is the only current prospective paper-facing experiment. Its protocol is frozen before VALID outcomes and is intended to test whether the short-horizon matched-mechanism ordering transfers from the existing compact 384-d evidence to one 768-d encoder.

No v0.35 result should be added to this README until the frozen run completes.

## Experimental line status

The research line is complete through **ARC-v0.34** for the current v18.6 manuscript evidence. ARC-v0.35 is a prospective 768-d external-validity replication and must remain outcome-blind until the frozen VALID run completes.

The project retains negative, partial, contracting, beneficial, exploratory, null, and failed-transfer outcomes as part of the scientific record. In particular, it does not convert:

- the v0.18 5/6 gate into full replication;
- the v0.19 5/7 nprobe gate into a universal positive cross-approximation replication;
- the HNSW severity audit into a preregistered continuous severity law;
- the v0.21 E5 RASF method failure into a successful controller-transfer result;
- the score-channel audit into exact-search evidence;
- the long-horizon temporal reversal into a claim that the terminal absolute-gap ordering also reverses;
- the unresolved Qwen/Mistral `H3_abs` primaries into significant temporal-order transfer;
- post-primary exhaustive-reference or decision-consequence audits into prospective confirmation; or
- the pending ARC-v0.35 protocol into a reported 768-d result before execution.

## Current manuscript state

Current reviewer-hardened paper title:

> **Approximation in the Loop: When One-Shot ANN Quality Is Insufficient for Iterative Dense Retrieval**

Current manuscript status:

```text
version                     : v18.6 reviewer-hardened SIGIR full-paper candidate
target                      : ACM SIGIR 2026 Full Papers
paper-facing corpora        : FEVER, HotpotQA, MS MARCO, BEIR Natural Questions
paper-facing encoders       : BGE-small, E5-small-v2, GTE-small (384-d)
ANN mechanisms              : IVF-PQ/SQ8 representation, IVF nprobe, HNSW efSearch
feedback operators          : anchored centroid, recursive centroid, deterministic LLM rewrite
maximum audited horizon     : H=50
exact-reference audit       : completed on frozen 500-query NQ-GTE subset
generative operators        : Qwen2.5-3B and Mistral-7B
current external-validity gap: larger embedding dimensions / representation geometries
ARC-v0.35                   : frozen 768-d GTE-base replication, pending outcome
```

Current paper-facing QA state:

```text
claim calibration            : reviewer-hardened
factual / numerical audit    : complete for v18.6
reference / citation audit   : complete for current manuscript
estimand definitions         : hardened
terminal decision definitions: hardened
confirmatory vs post-primary : explicitly separated
null / reversal retention    : preserved
anonymity / layout QA        : completed for the current draft
```

## Remaining submission-facing work

The highest-value remaining work is intentionally narrow:

1. **Run ARC-v0.35 exactly as frozen.** Do not alter the split, PQ geometry, nprobe grid, policy subset, horizon, or primary after VALID starts.
2. **If v0.35 completes, update the manuscript conservatively.** A positive result supports transfer to one 768-d encoder; an unresolved or reversed result must also be retained and used to narrow the claim.
3. **Optional high-value downstream audit:** a frozen answer-generation / QA endpoint (e.g. EM/F1) would address the remaining question of whether retrieval-trajectory differences propagate to final answer quality.
4. **Verify anonymized artifact access** before submission and ensure paper-facing intervals can be recomputed from released query-level artifacts without distributing licensed corpora or multi-GB indexes.
5. **Run one final PDF/template compliance check** after the final scientific content is frozen.
6. **Freeze final hashes and tags** only after the submission PDF and artifact package are final.

Additional compute should be used only to answer a clear scientific ambiguity. More datasets, a third LLM rewrite model, or additional post-primary metrics are lower priority than the 768-d replication and a downstream answer-level consequence test.

---

# Research Scope

This repository is an active research artifact.

The intended contribution is an empirical and methodological study of approximation under retrieval feedback, not a claim that any specific ANN index, embedding model, feedback policy, or mitigation strategy is universally optimal.

Throughout the current paper-facing interpretation, **index-representation approximation** means that the ANN index representation changes retrieval candidates and approximate scores. The shared normalized corpus embeddings are then used to construct feedback. The work therefore studies retrieval perturbations induced by compressed index representation; it does not claim that PQ reconstructed vectors themselves are directly fed back as the document representation.

The current evidence specifically separates:

- one-shot retrieval effectiveness from short-horizon feedback dynamics;
- index-representation approximation from search-effort approximation;
- candidate/evidence selection from ANN score-weighting effects under the completed softmax score-channel audit, while retaining the limitation that exact candidate rescoring is not exact search;
- susceptibility prediction from intervention value;
- and confirmatory/frozen evidence from post-primary mechanism audits.

The project emphasizes:

- explicit separation between exploratory and confirmatory stages,
- frozen or sealed evaluation protocols where appropriate,
- held-out validation,
- query-level or query-cluster uncertainty where appropriate,
- retention of negative / null / reversal / beneficial outcomes,
- exact replay and provenance audits,
- artifact hashing and checkpoint verification,
- explicit labeling of post-primary and reviewer-oriented audits,
- operator-family and estimator-weighting sensitivity where relevant,
- and conservative interpretation when evidence does not support universal claims.
