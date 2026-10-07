# Data and Experiment Guide

## Purpose

This guide explains the role of the dataset, notebooks, and experiment artifacts in the AWS ML Hackathon Business Entity Resolution project.

## Dataset Workflow

The pipeline is designed to work with structured business records that may contain variations in names, addresses, and other identifying attributes.

The workflow separates data preparation from model development so that experiments can be repeated without changing the core implementation.

## Experiment Notebooks

- `notebooks/` contains exploratory analysis and model experimentation.
- Notebook outputs should be treated as experiment evidence rather than production pipeline code.
- Reusable matching logic belongs under `src/business_entity_resolution/`.

## Candidate Generation

Candidate generation reduces the number of record pairs that need detailed comparison. Blocking and normalization help keep matching practical while preserving likely true matches.

## Evaluation

Experiments are evaluated with an F0.5-oriented strategy, placing greater emphasis on precision. This helps reduce incorrect entity links when uncertain matches should remain unmatched.

## Artifacts

Generated models, reports, and experiment outputs should be stored under `artifacts/` or `output/` according to their purpose. Large generated files should not be committed unless they are intentionally required for reproducibility.

## Reproducibility

Install the dependencies from `requirements.txt`, review the notebooks for the experimental workflow, and use the reusable modules under `src/` for the core matching implementation.
