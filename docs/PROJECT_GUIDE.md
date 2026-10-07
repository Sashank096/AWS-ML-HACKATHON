\# AWS ML Hackathon — Project Guide



\## Project Overview



This project implements a Business Entity Resolution pipeline for identifying records that refer to the same real-world business entity.



The solution focuses on practical data preprocessing, normalization, candidate generation, feature engineering, matching, and validation.



\## Pipeline



1\. Load and inspect the source data.

2\. Normalize business names and related fields.

3\. Generate candidate pairs using blocking.

4\. Extract similarity and matching features.

5\. Train and validate the matching model.

6\. Generate predictions for candidate pairs.

7\. Validate and prepare the final submission.



\## Repository Components



\- `src/business\_entity\_resolution/` — Core matching pipeline.

\- `notebooks/` — Exploratory analysis and experiments.

\- `utils/` — Validation and helper utilities.

\- `docs/` — Project documentation.

\- `artifacts/` — Space for generated models and experiment outputs.

\- `output/` — Space for generated prediction files.



\## Reproducibility



Install the required Python dependencies using:



```bash

pip install -r requirements.txt

