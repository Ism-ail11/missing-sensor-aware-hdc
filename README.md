# Missing-Sensor-Aware Hyperdimensional Computing for Robust Clinical Classification

**Research code and reproducibility notebook** · Python · CPU · Hyperdimensional computing (HDC) · Missing-data robustness

## Overview

This repository contains a self-contained experimental notebook studying **hyperdimensional computing under missing clinical features and sensor-group outages**. It compares missingness-aware HDC representations and retrieval strategies with classical supervised-learning baselines, with emphasis on robustness, calibration, and inference efficiency.

The experiments use two public benchmark datasets: the **Wisconsin Diagnostic Breast Cancer (WDBC)** dataset and **Statlog (Heart)**. This is an **internal development evaluation**, not an independent, prospective, external, or clinical validation study. The notebook itself is the authoritative implementation of the experimental protocol; no performance values are asserted in this README without executing that implementation.

## Main contributions and scope

- Investigates role-bound, bipolar hypervectors and quantized feature values under partial observation.
- Compares imputation, explicit missing-value/mask encoding, skip-and-normalize and mask-compatible aggregation, matched-feature scoring, and discriminative and probabilistic HDC retrieval variants.
- Includes marginalization over observed feature subsets, with diagonal/correlated Gaussian variants and local-kernel comparisons.
- Evaluates missing completely at random (**MCAR**) and grouped feature dropout, and extends the analysis to additional missingness mechanisms and repeated nested partitions.
- Studies model compression, inference time, discrimination, probability calibration, learning curves, and feature-group outage stress tests.
- Uses source-only fitting and inner-fold model selection/calibration, with held-out outer-fold evaluation.

## Repository contents

```text
.
├── Missing_Sensor_Aware_HDC.ipynb  # Complete experiment, analyses, figures, checks
├── README.md                       # Project overview and reproduction instructions
├── AUTHORS                         # Project authors and correspondence
├── requirements.txt                # Python dependencies
├── CITATION.cff                     # Machine-readable author/citation metadata
└── .gitignore                       # Ignore caches and local outputs
```

## Data and licensing

| Dataset | Provenance | Notebook access |
| --- | --- | --- |
| Wisconsin Diagnostic Breast Cancer (WDBC) | UCI Machine Learning Repository, [DOI: 10.24432/C5DW2B](https://doi.org/10.24432/C5DW2B) | Bundled `sklearn.datasets.load_breast_cancer` |
| Statlog (Heart) | UCI Machine Learning Repository, [DOI: 10.24432/C57303](https://doi.org/10.24432/C57303) | Embedded, compressed dataset bytes with SHA-256 integrity verification |

The notebook identifies the UCI datasets as distributed under **CC BY 4.0**; retain the dataset acknowledgments and consult the UCI dataset pages for attribution and applicable terms. No external dataset download is required for the current notebook. No patient-level, institution-specific dataset is provided.

**Important:** The data's license does **not** automatically license this repository's code. No code license is granted unless the authors add a separate `LICENSE` file.

## Requirements

- Python **3.12** (the notebook's recorded environment; compatible newer/older versions have not been validated)
- A Jupyter environment or Google Colab, with a **CPU** runtime
- Python dependencies listed in [`requirements.txt`](requirements.txt)
- Sufficient memory and compute time for the full nested cross-validation and supplementary analyses

The notebook constrains BLAS/OpenMP thread counts to one for more consistent CPU measurements. Its first cell uses the IPython `%matplotlib inline` magic and therefore should be executed in Jupyter or Colab, rather than as a plain Python script.

## Reproduction

### Local Jupyter setup

```bash
# Clone after the repository is published
# git clone https://github.com/<YOUR_USERNAME>/missing-sensor-aware-hdc.git
# cd missing-sensor-aware-hdc

python3.12 -m venv .venv
source .venv/bin/activate           # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m ipykernel install --user --name missing-sensor-hdc --display-name "Python (missing-sensor-hdc)"
jupyter lab
```

Open **`Missing_Sensor_Aware_HDC.ipynb`**, select the newly installed kernel, then execute **Run All** in cell order. In Google Colab, upload the notebook, choose a standard CPU runtime, install any missing packages from `requirements.txt`, and run all cells in order.

### Experimental settings in the notebook

| Setting | Primary experiment |
| --- | --- |
| Evaluation | 5 outer folds × 3 inner folds (subject-stratified nested CV) |
| Seed | `CV_SEED = 20261107` |
| Hypervector dimension | `D = 1024` |
| Quantization levels | `LEVELS = 32` |
| Missingness mechanisms | `MCAR`, `group dropout` |
| Missingness fractions | `0.0`, `0.1`, `0.2`, `0.4`, `0.6` |
| Repeated mask draws | `MASK_REPEATS = 3` |
| Bootstrap iterations | `BOOTSTRAPS = 1000` |

Later cells contain additional supplementary protocols, including denser missingness grids, multiple mechanisms, repeated cross-validation splits, and system-level measurements. Read `EXPERIMENT_PROTOCOL` in the notebook for those settings. Changing parameters can alter the reported results and execution cost.

### Outputs and verification

Results and figures are produced **inline and in memory**; the notebook does not require an output directory. It exposes, among others, `RESULTS`, `SCORECARD`, `SUMMARY`, `POOLED`, `SELECTIONS`, `BOOTSTRAP_RESULTS`, `COMPRESSION`, `RESOURCES`, and `STRESS`. Supplementary cells create additional tables and figures.

The notebook includes analytical checks comparing masked low-rank retrieval against dense Gaussian calculations, and final assertions checking consistency/integrity of the original results, data, models, and supplementary protocol. A complete successful run should finish without assertion errors. Environment-sensitive runtime timings are not expected to reproduce identically across hardware.

For publication-grade reproducibility, record the operating system, CPU model, Python/package versions, command used, and any changes to notebook constants alongside each run. The provided `requirements.txt` specifies direct packages without claiming a bit-for-bit locked environment.

## Interpretation and limitations

These experiments test retrospective, public benchmark datasets with simulated missing features. Simulated MCAR, group outages, and other constructed mechanisms may differ from real clinical missingness. Dataset scope was selected following internal development; therefore, results should not be interpreted as unbiased external validation. Nested resampling and statistical comparisons do not replace testing on independent cohorts or prospective workflows. **The software is research-only and must not be used for medical diagnosis or patient care.**

## References

1. UCI Machine Learning Repository. *Breast Cancer Wisconsin (Diagnostic)*. [https://doi.org/10.24432/C5DW2B](https://doi.org/10.24432/C5DW2B).
2. UCI Machine Learning Repository. *Statlog (Heart)*. [https://doi.org/10.24432/C57303](https://doi.org/10.24432/C57303).
3. Scikit-learn developers. *Nested versus non-nested cross-validation*. [Documentation](https://scikit-learn.org/stable/auto_examples/model_selection/plot_nested_cross_validation_iris.html).

For additional method-specific citations, see the source comments within the notebook and the accompanying manuscript when available.
