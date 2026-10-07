# 🧠 AWS ML Hackathon — Business Entity Resolution

> **A precision-first machine-learning pipeline for matching noisy business records across independent data sources.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![Scikit-learn](https://img.shields.io/badge/scikit--learn-1.6.1-orange?logo=scikit-learn)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.3.3-150458?logo=pandas)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-Challenge%20Project-lightgrey)](#)

## 🚀 What is this?

Business data rarely arrives clean.

The same real-world company can appear in multiple systems with:

- different spellings
- abbreviations and legal suffixes
- reordered words
- transliterated names
- incomplete or differently formatted addresses
- missing postal information
- different representations of the same country/location

This project tackles **Business Entity Resolution (ER)**: identifying which records from Source 2 and Source 3 correspond to each canonical entity in Source 1.

The pipeline is designed around one central idea:

> **Reduce the search space intelligently, learn how likely a pair is to match, then make precision-oriented decisions.**

That matters because the challenge metric is **F0.5**, which places more emphasis on precision than recall.

---

## ✨ Highlights

- 🔎 **Multi-key blocking** to avoid comparing every possible pair
- 🧹 **Unicode-aware normalization** for noisy names and addresses
- 🧩 **Pairwise feature engineering** across names, addresses, countries and postal tokens
- 🤖 **Machine-learning matcher** with configurable model selection
- 🎯 **Threshold optimization** against validation F0.5
- 🛡️ **Precision-oriented inference** to reduce false merges
- 📦 **Reproducible submission generation**
- ✅ **Official submission validator integration**
- 📓 **Experiment notebooks** documenting the reasoning process
- 🌐 **No external enrichment required** — the core pipeline works offline

---

## 🏗️ Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                     RAW TSV DATA                            │
│        Source 1        Source 2        Source 3              │
└───────────────────────────┬─────────────────────────────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │      Data Loader     │
                 │ schema + TSV checks  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Preprocessing      │
                 │ normalize names      │
                 │ normalize addresses  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │      Blocking        │
                 │ multi-key candidates │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Feature Engineering  │
                 │ similarity signals   │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   ML Pair Model      │
                 │ probability scoring  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Threshold Selection  │
                 │      F0.5 focus      │
                 └──────────┬───────────┘
                            │
                            ▼
             ┌──────────────────────────────┐
             │     Final Submission         │
             │ matching_results.tsv         │
             │ candidate_pairs.tsv          │
             └──────────────────────────────┘
```

---

## 📁 Repository Structure

```text
AWS-ML-HACKATHON/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── src/
│   └── business_entity_resolution/
│       ├── __init__.py
│       ├── config.py
│       ├── data_loader.py
│       ├── preprocessing.py
│       ├── blocking.py
│       ├── features.py
│       ├── model.py
│       ├── validation.py
│       ├── inference.py
│       ├── submission.py
│       ├── main.py
│       └── README.md
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_normalization.ipynb
│   ├── 03_blocking.ipynb
│   ├── 04_features.ipynb
│   └── 05_model_validation.ipynb
│
├── utils/
│   └── validate_submission.py
│
├── docs/
│   ├── CHALLENGE.md
│   └── Documentation_template.md
│
├── dataset/                 # local challenge data; not committed
├── output/                  # generated submission files
└── artifacts/               # generated models, reports and experiments
```

---

## 🧪 Pipeline Workflow

### 1. Load

The loader reads the challenge's tab-separated files without modifying the original data and validates the expected schema.

### 2. Normalize

Names and addresses are transformed into comparison-friendly representations while preserving the original values.

The normalization layer handles concepts such as:

- case differences
- Unicode normalization
- punctuation
- whitespace
- common abbreviations
- legal business suffixes
- address token variations

### 3. Block

A naive entity-resolution system can create an enormous number of pair comparisons.

Instead, this project generates a smaller candidate set using multiple blocking signals such as:

- country
- normalized business name
- name tokens
- address tokens
- postal information
- combined signals

This creates a practical search space for the ML stage.

### 4. Engineer Features

Each candidate pair is converted into numerical signals, including:

- exact name agreement
- exact address agreement
- country agreement
- name token Jaccard similarity
- address token Jaccard similarity
- character-level similarity
- postal overlap
- shared-token signals
- length differences
- missingness / blocking signals

### 5. Train

The model learns from known matches and carefully constructed non-match pairs.

The implementation can compare the configured model with an Extra Trees alternative and select the successful configured candidate according to validation performance.

### 6. Optimize

Instead of blindly using a probability threshold such as `0.50`, the pipeline searches a configurable threshold range and evaluates **F0.5**.

This is especially important when false positive merges are more damaging than leaving an uncertain entity unmatched.

### 7. Infer

The final model is refit using the available labelled training pairs and applied to the test sources.

### 8. Validate & Export

The pipeline generates:

```text
output/
├── matching_results.tsv
└── candidate_pairs.tsv
```

The supplied challenge validator is then used to catch submission-format problems before upload.

---

## ⚡ Quick Start

### 1. Clone

```bash
git clone https://github.com/Sashank096/AWS-ML-HACKATHON.git
cd AWS-ML-HACKATHON
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the challenge dataset locally

Place the challenge files in:

```text
dataset/
├── train/
│   ├── train_source1.tsv
│   ├── train_source2.tsv
│   ├── train_source3.tsv
│   └── train_ground_truth.tsv
│
└── test/
    ├── test_source1.tsv
    ├── test_source2.tsv
    └── test_source3.tsv
```

The raw challenge dataset is intentionally not included in this public repository.

### 5. Run the complete pipeline

From the repository root:

```bash
python src/business_entity_resolution/main.py
```

Or explicitly specify the output directory:

```bash
python src/business_entity_resolution/main.py --output-dir output
```

### 6. Validate the generated submission

```bash
python utils/validate_submission.py ^
  --matching output/matching_results.tsv ^
  --candidate output/candidate_pairs.tsv ^
  --test-dir dataset/test
```

On macOS/Linux, use `\` instead of `^` for line continuation.

---

## 📊 Expected Output

### `matching_results.tsv`

One row for every Source 1 test entity:

```text
source1_entity_id    matched_entity_ids
S1-00001             S2-00047,S2-00193,S3-00812
S1-00002             S3-00004
S1-00003
```

### `candidate_pairs.tsv`

The candidate set considered by the matching model:

```text
source1_entity_id    candidate_entity_ids
S1-00001             S2-00047,S2-00193,S3-00812,S3-00999
S1-00002             S3-00004
S1-00003
```

Final matches should always be contained within the generated candidate set.

---

## 🧠 Why F0.5?

F0.5 gives precision more weight than recall.

For entity resolution, an incorrect merge can be particularly harmful: two genuinely different businesses may become incorrectly treated as the same entity.

Therefore, this project favors a conservative decision layer:

```text
Candidate Generation
        ↓
Similarity Features
        ↓
ML Probability
        ↓
Validation-based Threshold
        ↓
Precision-oriented Match Decision
```

---

## 🔬 Experiment Notebooks

The notebooks are organized as a learning and experimentation trail:

| Notebook | Purpose |
|---|---|
| `01_eda.ipynb` | Understand the raw business data |
| `02_normalization.ipynb` | Explore name/address normalization |
| `03_blocking.ipynb` | Study candidate-generation strategies |
| `04_features.ipynb` | Build and inspect pairwise signals |
| `05_model_validation.ipynb` | Evaluate matching models and thresholds |

---

## 🛡️ Reproducibility & Fair Play

The pipeline is intentionally designed to be reproducible.

It does **not** depend on:

- internet lookups
- external business databases
- geocoding APIs
- hidden identifiers
- manual entity-by-entity matching

All core matching decisions are produced by the project pipeline from the supplied challenge data.

---

## 📚 Documentation

- [`docs/CHALLENGE.md`](docs/CHALLENGE.md) — challenge statement and submission requirements
- [`docs/Documentation_template.md`](docs/Documentation_template.md) — methodology/report template
- [`src/business_entity_resolution/README.md`](src/business_entity_resolution/README.md) — source-level architecture

---

## 👨‍💻 Project

**AWS ML Hackathon — Business Entity Resolution**

Built as a machine-learning engineering project focused on:

`Data Quality → Blocking → Feature Engineering → Classification → Threshold Optimization → Submission Validation`

---

