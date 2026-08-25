# SIGIR Artifact Manifest

This manifest maps the current paper-facing and post-freeze reviewer-oriented results to compact public artifacts.

## ARC-v0.24 — MS MARCO External Boundary Replication

Canonical protocol:
- `protocols/msmarco/v024_frozen_protocol.json`
- `protocols/msmarco/V024_PROTOCOL_SHA256.txt`

Canonical primary summary:
- `results/msmarco/v024_validation-full_query_bootstrap_summary.csv`
- `results/msmarco/v024_validation-full_final_report.json`

Post-primary reviewer audits:
- `results/msmarco/v024_reviewer_audit_summary.csv`
- `results/msmarco/v024_postprimary_family_balanced_h3abs.csv`

Artifact integrity:
- `results/msmarco/V024_VALIDATION_FULL_ARTIFACT_SHA256.csv`

Paper-facing H3abs values:
- representation: +0.0019159752, 95% CI [0.0004281497, 0.0034220342]
- nprobe: -0.0019327358, 95% CI [-0.0030851191, -0.0008162649]
- paired representation-minus-nprobe: +0.0038487110, 95% CI [0.0023065198, 0.0053884484]

## ARC-v0.25 — FEVER-E5 Severity-Matched Validation

Canonical protocol:
- `protocols/fever/v025_frozen_severity_match_protocol.json`
- `protocols/fever/V025_PROTOCOL_SHA256.txt`

Canonical summaries:
- `results/fever/v025_severity_matched_primary_query_bootstrap.csv`
- `results/fever/v025_severity_matched_family_balanced_bootstrap.csv`
- `results/fever/v025_severity_matched_validation_final_report.json`

Artifact integrity:
- `results/fever/V025_SEVERITY_MATCHED_VALIDATION_ARTIFACT_SHA256.csv`

Paper-facing H3abs values:
- representation: +0.0301663961, 95% CI [0.0283320117, 0.0320061214]
- matched nprobe: -0.0035729827, 95% CI [-0.0050074971, -0.0021428916]
- paired difference: +0.0337393788, 95% CI [0.0318383699, 0.0356416694]
- equal-family paired difference: +0.0334432677, 95% CI [0.0315674441, 0.0353856810]

## ARC-v0.26 — FEVER-E5 Softmax Score-Channel Audit

Status: **completed post-primary reviewer-oriented mechanism audit**.

Scientific role:
- tests whether representation-side softmax amplification survives when ANN-returned feedback scores are replaced by shared exact embedding dot-product rescoring over the already retrieved candidates;
- preserves ANN candidate selection;
- does **not** perform exact nearest-neighbor search;
- uses the same 3,316 FEVER-E5 validation queries and all 32 frozen softmax policies;
- retains positive, null, or reversal outcomes without validation retuning.

Canonical protocol:
- `protocols/fever/v026_frozen_score_channel_protocol.json`
- `protocols/fever/V026_PROTOCOL_SHA256.txt`
- `protocols/fever/v026_frozen_softmax_policy_grid.csv`

Canonical summaries:
- `results/fever/v026_score_channel_query_bootstrap.csv`
- `results/fever/v026_score_channel_configuration_summary.csv`
- `results/fever/v026_score_channel_alpha_summary.csv`
- `results/fever/v026_score_channel_final_report.json`

Artifact integrity:
- `results/fever/V026_SCORE_CHANNEL_ARTIFACT_SHA256.csv`

Frozen protocol SHA-256:
- `6fa8c981ffb15d7929371c4ee9457a8612cb03b850cf53bc2d6d85142dd8e789`

Final repaired report SHA-256:
- `b6239e140e3e8382a23156d495c58aa9a0974ad7f7af0cacde45766e8b1a3eb7`

Primary score-channel H3abs results:
- ANN-score softmax: +0.0309640390, 95% CI [0.0291102407, 0.0327690419]
- shared exact candidate rescoring: +0.0219996764, 95% CI [0.0203654084, 0.0236724736]
- exact-rescore minus ANN-score: -0.0089643626, 95% CI [-0.0094061489, -0.0085403349]

Endpoint robustness:
- ANN-score R1 final-minus-initial absolute gap: +0.1490638937, 95% CI [0.1405322281, 0.1574235213]
- exact-rescore R1: +0.1051885434, 95% CI [0.0977789719, 0.1126382536]
- exact-rescore minus ANN-score R1: -0.0438753503, 95% CI [-0.0458982118, -0.0418688817]

Interpretation:
> Shared exact rescoring reduces the magnitude of representation-side softmax amplification, but the exact-rescored H3abs remains clearly positive. Under this tested FEVER-E5 setup, ANN score geometry contributes materially to the magnitude, while candidate/evidence-selection perturbation remains sufficient for positive short-horizon representation-side H3abs.

This is a post-primary mechanism audit rather than a pristine confirmation. It does not establish exact-search behavior or a universal decomposition across feedback operators, corpora, encoders, or index families.

Provenance repair:
- the completed validation checkpoints belong to original run `20260824-150217`;
- a runtime restart created a second run directory;
- only final-report protocol metadata was repaired to point back to the original frozen protocol;
- trajectories, endpoints, and scientific estimates were not modified or recomputed.


## ARC-v0.27 — NQ-GTE Independent Severity-Matched Confirmation

Status: **completed confirmatory external-validity experiment**.

Canonical protocols:
- `protocols/nq/v027_fit_calibration_protocol.json`
- `protocols/nq/V027_FIT_CALIBRATION_PROTOCOL_SHA256.txt`
- `protocols/nq/v027_frozen_confirmation_protocol.json`
- `protocols/nq/V027_CONFIRMATION_PROTOCOL_SHA256.txt`

Canonical summaries:
- `results/nq/v027_fit_severity_match_table.csv`
- `results/nq/v027_primary_paired_query_bootstrap.csv`
- `results/nq/v027_secondary_family_robustness.csv`
- `results/nq/v027_primary_confirmation_gate.json`
- `results/nq/v027_regime_summary.csv`
- `results/nq/v027_final_report.json`

Artifact integrity:
- `results/nq/V027_ARTIFACT_SHA256.csv`

Frozen confirmation protocol SHA-256:
- `44a15f18297dce9d5db2b590f403859763f916ed0f3cfb3a196d3b2198ecc94a`

Final report SHA-256:
- `6998638fdef2fe35bb75d25a24c4d5d31b97e179588acf6c00c8bfccafa3383d`

FIT severity calibration:
- representation one-shot nDCG@10 gap: 0.1611140031
- matched search-effort gap: 0.1649720924
- selected low `nprobe`: 2
- relative mismatch: 2.3946%

Primary validation H3abs:
- representation: +0.0109527584, 95% CI [0.0091913702, 0.0127697410]
- severity-matched search effort: -0.0009157065, 95% CI [-0.0022991920, 0.0004359966]
- paired representation-minus-search: +0.0118684649, 95% CI [0.0099217869, 0.0137935499]

Feedback-family paired robustness:
- mean-only: +0.0114880006, 95% CI [0.0095409623, 0.0134213828]
- softmax-only: +0.0120111391, 95% CI [0.0101066106, 0.0139298912]
- equal-family: +0.0117495699, 95% CI [0.0098361196, 0.0136896961]

Interpretation:
> On a new corpus and encoder family, matching one-shot nDCG@10 degradation on FIT does not remove the positive representation-versus-search-effort H3abs ordering on untouched validation trajectories.

The search-effort H3abs interval crosses zero, so ARC-v0.27 does **not** independently establish search-effort contraction. The supported confirmatory claim is the paired mechanism ordering.


## Version-freeze boundary

The existing `sigir-v9.1-artifact-freeze` tag remains immutable and predates ARC-v0.26 completion.

ARC-v0.26 should be treated as post-v9.1 evidence. If incorporated into a revised manuscript/artifact snapshot, create a new version/tag rather than moving the v9.1 freeze tag.

## Public artifact boundary

Large trajectories, corpus embeddings, raw corpora, and FAISS indexes remain outside Git. The SHA manifests preserve integrity links to the persisted research artifacts without publishing large or sensitive files.
