# ARC-v0.12 — Cross-Policy Boundary Transfer

## Scope

ARC-v0.12 tests whether the post-confirmatory boundary-risk signal learned in one feedback-policy family transfers to the other feedback-policy family on the held-out HotpotQA boundary-validation split. The transfer audit preserves the ARC-v0.9 fit/validation separation and retains amplifying, stable/null, and reversal cases.

Before any transfer result is accepted, the notebook exactly reconstructs the sealed ARC-v0.9 classifier. The reconstructed validation ROC-AUC is **0.7736072867** versus the sealed **0.7736072913** (absolute difference `4.58e-9`). The maximum standardized coefficient absolute difference is about `1.30e-6`.

## Primary cross-policy contrasts

| Source → target | ROC-AUC | 95% bootstrap CI | Average Precision | AUC gap to target-family fit-only oracle | Change vs source within-family |
|---|---:|---:|---:|---:|---:|
| mean → softmax | **0.766532** | **[0.756424, 0.776532]** | 0.544860 | -0.016485 | +0.025854 |
| softmax → mean | **0.755019** | **[0.745265, 0.764847]** | 0.435071 | +0.014342 | -0.027997 |

Both cross-policy directions retain substantial held-out discrimination. These results support a limited claim that the tested approximation-feedback boundary signal contains **transferable structure across the mean and softmax feedback-policy families**.

The result does **not** establish universal transfer across datasets, encoders, ANN families, or deployment settings. It also does not replace the separate ARC-v0.10 mitigation or ARC-v0.11/0.11.1 systems-cost evidence.

## Interpretation discipline

The study distinguishes within-family discrimination, cross-policy transfer, the gap to a target-family fit-only oracle, and null/reversal regimes. Positive, stable/null, and reversal cases remain in the audit.

Protocol SHA-256: `5789f053aa51a1bc830b7bb55a0ae95dc7f6dee91211e2353403ccb5b835848d`

Report SHA-256: `77e8582c74c71748b8deca85f1f5d652b2db517d5927339706677db4fd87ab01`
