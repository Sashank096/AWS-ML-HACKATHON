# Business Entity Resolution Engine

This directory contains the reusable ML implementation.

## Modules

- `config.py` — typed configuration and default paths
- `data_loader.py` — safe TSV loading and schema validation
- `preprocessing.py` — Unicode-aware normalization and token preparation
- `blocking.py` — candidate generation / search-space reduction
- `features.py` — pairwise similarity and engineered features
- `model.py` — model training and probability scoring
- `validation.py` — train/validation construction and F0.5 evaluation
- `inference.py` — test-set inference
- `submission.py` — leaderboard-ready TSV generation
- `main.py` — reproducible end-to-end entry point

The pipeline is designed to optimize **F0.5**, giving precision more weight than recall.
