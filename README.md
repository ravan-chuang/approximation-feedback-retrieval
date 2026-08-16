# Approximation-Feedback Retrieval

Research project studying how retrieval approximation errors propagate when retrieved evidence is fed back into subsequent query states.

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
\text{utility degradation}
\]

## Experimental Lineage

| Version | Experiment | Role |
|---|---|---|
| ARC-v0.1 | Memory-Safe IVF-PQ | Large-corpus retrieval infrastructure |
| ARC-v0.2 | Retriever-Condition Replication | Retrieval-fidelity dose response |
| ARC-v0.3 | Error Amplification Probe | Synchronized trajectories and intervention |
| ARC-v0.4 | Statistical Mechanism Audit | Query-level statistical validation |
| ARC-v0.5 | Sealed FEVER DEV Confirmation | Independent confirmatory experiment |
| ARC-v0.6.1 | Sealed HotpotQA Replication | Cross-dataset replication in progress |

## Sealed FEVER DEV Confirmation

ARC-v0.5 evaluated four preregistered primary endpoints on the untouched FEVER DEV split.

| Endpoint | DEV effect |
|---|---:|
| H1 — Query-state divergence slope | +0.004109 |
| H2 — Candidate-divergence increment slope | +0.008157 |
| H3 — Absolute utility-gap slope | +0.007261 |
| H4 — High-fidelity feedback intervention | +0.030376 |

All four endpoints passed the preregistered query-level bootstrap, paired sign-flip randomization, and joint Holm-correction criteria.

These results support the tested FEVER configuration; they are not yet claimed as universal across retrieval systems or datasets.

## Current Replication

ARC-v0.6.1 transfers the frozen FEVER experimental design to HotpotQA.

No HotpotQA-based feedback hyperparameter tuning is performed.

Status: **in progress**.

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

## Experimental Philosophy

The project separates:

1. exploratory discovery,
2. retriever-condition replication,
3. direct mechanism probing,
4. statistical auditing,
5. sealed independent confirmation,
6. cross-dataset replication.

The primary statistical unit in confirmatory experiments is the query.

## Reproducibility

Where applicable, experiments retain:

- deterministic seeds,
- frozen feedback configurations,
- ANN index configurations,
- protocol SHA-256 hashes,
- report SHA-256 hashes,
- query-level paired inference,
- bootstrap confidence intervals,
- paired sign-flip tests,
- Holm multiple-comparison correction.

## Status

Active research project.

The conclusions and framing may change as HotpotQA and additional cross-dataset / cross-approximation experiments are completed.
