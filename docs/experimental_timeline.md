# Experimental Timeline

## ARC-v0.1 — Memory-Safe IVF-PQ

Established a memory-safe multi-million-document retrieval environment using IVF-PQ.

Main role:
- large-corpus retrieval infrastructure;
- iterative retrieval mechanism discovery.

## ARC-v0.2 — Retriever-Condition Replication

Compared retrieval fidelity across:

- IVF-PQ32
- IVF-PQ64
- IVF-SQ8

The degradation of iterative feedback decreased as retrieval fidelity increased, motivating the approximation-feedback hypothesis.

## ARC-v0.3 — Error Amplification Probe

Introduced synchronized trajectories beginning from the same original query.

Measured:

- query-state divergence;
- candidate-set divergence;
- utility-gap divergence.

Also introduced the feedback-source intervention:

A:

PQ32 search -> PQ32 feedback

B:

PQ32 search -> SQ8 feedback

C:

SQ8 search -> SQ8 feedback

The experiment produced a strong preliminary approximation-feedback amplification result.

## ARC-v0.4 — Statistical Mechanism Audit

Reanalyzed the mechanism using query as the primary independent unit.

Included:

- query-level amplification slopes;
- bootstrap confidence intervals;
- paired sign-flip randomization;
- Holm multiple-comparison correction;
- baseline-normalized candidate divergence;
- sparse-rescue analysis;
- contribution-concentration analysis.

The mechanism survived the statistical audit.

## ARC-v0.5 — Sealed FEVER DEV Confirmation

The experimental protocol was frozen before DEV retrieval.

Primary confirmatory endpoints:

- H1 — query-state divergence slope
- H2 — baseline-normalized candidate-divergence slope
- H3 — absolute utility-gap slope
- H4 — high-fidelity feedback intervention

All four preregistered endpoints replicated on the untouched FEVER DEV split.

DEV effects:

| Endpoint | Effect |
|---|---:|
| H1 | +0.004109 |
| H2 | +0.008157 |
| H3 | +0.007261 |
| H4 | +0.030376 |

All endpoints passed query-level bootstrap confidence intervals, paired sign-flip tests, and joint Holm correction.

## ARC-v0.6.1 — HotpotQA Cross-Dataset Replication

Transfers the frozen FEVER design to HotpotQA without HotpotQA-based feedback hyperparameter tuning.

A schema adapter maps:

corpus-id -> corpus_ids.sqlite -> corpus-row

The official HotpotQA DEV membership is used.

TEST relevance and TEST retrieval remain prohibited.

Status: in progress.
