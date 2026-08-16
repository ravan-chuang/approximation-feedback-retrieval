# Public Artifact Policy

This repository contains research code and sanitized aggregate evidence.

The following artifacts are intentionally excluded from Git:

- raw datasets;
- corpus JSONL files;
- embedding matrices;
- FAISS indexes;
- SQLite corpus databases;
- per-query hidden test outcomes;
- credentials and tokens;
- private runtime paths when unnecessary.

Public confirmation artifacts may include:

- sealed protocol manifests;
- protocol SHA-256 hashes;
- aggregate endpoint results;
- bootstrap confidence intervals;
- randomization-test results;
- multiple-comparison corrections;
- sanitized figures;
- reproducibility metadata.

Exploratory and confirmatory results should be clearly distinguished.
