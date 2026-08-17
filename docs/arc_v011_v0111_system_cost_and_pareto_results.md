# ARC-v0.11 / v0.11.1 — Local M3 Max System Cost and Pareto Audit

## Scope

ARC-v0.11 measures the systems cost of the already-frozen ARC-v0.10 selective-fidelity policies on a local Apple M3 Max CPU environment. It does not re-tune the selector or change the ARC-v0.10 quality outcomes.

ARC-v0.11.1 is a deterministic post-v0.11 audit that repairs only the aggregate quality-to-cost join for the Always-PQ32 and Always-SQ8 baseline rows. No retrieval, policy, or systems benchmark was rerun.

## Environment

- Apple M3 Max, arm64
- 36 GiB RAM
- Python 3.12
- FAISS 1.12.0
- NumPy 2.0.2
- HotpotQA DEV: 5,447 queries
- Index population: 5,233,329 documents
- `nprobe = 64`

The measurements are single-machine CPU measurements and are not presented as universal production latency claims.

## Primary 25% operating point

The ARC-v0.10 fit-frozen 25% threshold selects 26.4662% of held-out validation queries for SQ8 feedback.

| Configuration | Selective nDCG@10 | Recovery | Δ vs random (95% CI) | Selective runtime | Random runtime | Always-PQ32 | Always-SQ8 | Fraction of Always-SQ8 extra runtime | Recovery / extra-runtime fraction |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| mean, k=20, α=0.3 | 0.357904 | 41.14% | +0.007084 [0.003560, 0.010677] | 12.170 s | 11.977 s | 1.961 s | 39.587 s | 27.13% | 1.516× |
| softmax, k=5, α=0.5, T=0.1 | 0.376525 | 44.08% | +0.034576 [0.026755, 0.042598] | 11.840 s | 12.078 s | 1.842 s | 39.011 s | 26.90% | 1.639× |

The boundary-aware and budget-matched random policies have nearly identical measured cost at the primary operating point. The quality advantage therefore is not explained by allocating more high-fidelity work to the boundary-aware policy.

At the primary operating point, the boundary-aware policy recovers 41–44% of the Always-SQ8 quality benefit while consuming about 27% of the Always-SQ8 incremental runtime over Always-PQ32.

## Baseline quality join repair

The original ARC-v0.11 Pareto aggregate had `NaN` quality fields for `always_pq32` and `always_sq8` because baseline budget keys were represented differently across v0.10 and v0.11. ARC-v0.11.1 canonicalizes:

- `always_pq32 -> 0.0`
- `always_sq8 -> 1.0`

The repaired baseline nDCG@10 values are:

| Configuration | Always-PQ32 | Always-SQ8 |
|---|---:|---:|
| mean, k=20, α=0.3 | 0.338248640 | 0.386021763 |
| softmax, k=5, α=0.5, T=0.1 | 0.292335689 | 0.483323306 |

The v0.11.1 audit passed source-path, report-status, experiment-structure, canonical-budget, quality-join, baseline-quality, primary-decision, Pareto-efficiency, and sealed read-back checks.

## Memory and index footprint

The measured artifacts were:

| Artifact | Size |
|---|---:|
| PQ32 index | 216,050,780 bytes (~0.201 GiB) |
| SQ8 index | 2,057,792,448 bytes (~1.916 GiB) |
| Process RSS increase after loading both indexes | ~2.165 GiB |

Policy-run peak RSS was approximately 3.0 GiB in the measured runs.

## Provenance

ARC-v0.11 report SHA-256:

`d6b49ced27062a76d64b234e8711c652a9d293f464d7a747a6ad2ab5ef13c5f8`

ARC-v0.11.1 final audit report SHA-256:

`e01c4145dfca90ac289bbdd8402b9c4497db9c88b09c28dba37982e4dacd5973`

ARC-v0.11.1 final Pareto SHA-256:

`e7202ebf7489faf4ab8e8fea7fc36840df5786b123d20201d8deb8c9c5e286d1`

ARC-v0.11.1 final primary table SHA-256:

`4900f4e5d843a6c88eae1a240c0c0ea9d0c4741198b8dc2155e17dbcb0317f6f`

## Interpretation

The systems evidence strengthens the ARC-v0.10 mitigation result: the boundary-aware selector is not only better than budget-matched random allocation in held-out quality, but at the primary operating point it achieves that advantage at essentially the same measured runtime as random allocation.

The result remains scoped to the tested HotpotQA / BGE-small / FAISS / Apple M3 Max CPU configuration. Cross-hardware and broader external-validity studies remain separate future work.
