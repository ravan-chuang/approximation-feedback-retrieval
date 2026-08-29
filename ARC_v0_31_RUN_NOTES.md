# ARC-v0.31 run notes

Created for the first frozen generative retrieval-agent transfer experiment.

## Frozen design
- NQ-GTE validation pool from ARC-v0.28
- 32 engineering-only smoke queries
- 500 main queries, deterministic SHA-256 selection
- H=4
- representation: PQ32@64 vs SQ8@64
- search effort: SQ8@2 vs SQ8@64
- agent evidence: top 5 passage title/text, scores hidden
- default local model: `Qwen/Qwen2.5-3B-Instruct`
- deterministic decoding (`do_sample=False`)
- primary: representation-minus-search H3abs, two-sided 10k paired-query bootstrap

## Protocol template SHA
`67108a1f3ddb36776339555e0a09d8fb65dac18ab5a9979990760b27706b9482`

The notebook creates a runtime frozen protocol with the resolved Hugging Face model revision before agent outcomes.

## Important
The current paper itself already states that H=50 is a controlled bridge rather than a full LLM-agent evaluation. ARC-v0.31 is designed specifically to test this missing external-validity axis without changing the existing v12 primary claims.
