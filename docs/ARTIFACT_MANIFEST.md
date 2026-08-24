# SIGIR Artifact Manifest

This manifest maps the current paper-facing post-primary/external results to compact public artifacts.

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

Status: prepared / pending.

ARC-v0.26 is not counted as completed evidence until the frozen validation audit is completed and its result is retained regardless of sign.

## Public artifact boundary

Large trajectories, corpus embeddings, raw corpora, and FAISS indexes remain outside Git. The SHA manifests preserve integrity links to the persisted research artifacts without publishing large or sensitive files.
