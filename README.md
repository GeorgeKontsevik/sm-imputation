# sm-imputation

---

[![OSA-improved](https://img.shields.io/badge/improved%20by-OSA-yellow)](https://github.com/aimclub/OSA)

Built with:

![numpy](https://img.shields.io/badge/NumPy-013243.svg?style={0}&logo=NumPy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458.svg?style={0}&logo=pandas&logoColor=white)
![scipy](https://img.shields.io/badge/SciPy-8CAAE6.svg?style={0}&logo=SciPy&logoColor=white)
![tqdm](https://img.shields.io/badge/tqdm-FFC107.svg?style={0}&logo=tqdm&logoColor=black)

---

## Table of Contents

- [Overview](#overview)
- [Core Features](#core-features)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Architecture](#architecture)
- [API Reference](#api-reference)
- [Examples](#examples)
- [Contributing](#contributing)
- [Citation](#citation)

---

## Overview

sm-imputation is a Python experimentation repo for imputing urban morphology measures from geospatial block data and comparing imputation methods under different missing-data scenarios. It is aimed at developers, researchers, and data scientists working on geospatial or urban-form analysis and model evaluation. The repository centers on a notebook-driven workflow with reusable imputer implementations and metric helpers, and it also supports plotting and experiment analysis from the same environment. For a runnable path through the project, start with the Getting Started section.

---

## Core Features

- Supports multiple imputation strategies for urban-form features, including mean, spatial nearest-neighbor, inverse-distance weighting, and model-based approaches, so developers can compare simple and advanced baselines side by side.
- Imputes missing built-environment metrics such as floor space index and ground space index, which lets experiments reconstruct incomplete geospatial records for downstream analysis.
- Uses nearby geometry and block attributes to estimate missing values, making it practical to leverage spatial context and auxiliary land-use signals in city-scale datasets.
- Includes experiment workflows for running repeated missing-data simulations and evaluating predictions with MAE, RMSE, and R²-style metrics, which helps quantify robustness across cities and missingness levels.
- Produces analysis-ready results and plots for method comparison, making it easier to inspect performance trends and visualize how imputers behave as missingness increases.

---

## Installation

Install sm-imputation using one of the following methods:

**Build from source:**

1. Clone the sm-imputation repository:
```sh
git clone https://github.com/GeorgeKontsevik/sm-imputation
```

2. Navigate to the project directory:
```sh
cd sm-imputation
```

3. Install the project dependencies:

```sh
pip install -r requirements.txt
```

---

## Getting Started

Prerequisites:

- Python environment with the project dependencies installed.
- A geospatial blocks dataset available in the format used by the example notebooks.

1. Open the example notebook workflow in `examples/`.
2. Load your blocks data into a `GeoDataFrame` and prepare the area-derived features used in the examples.
3. Run one of the imputation notebooks, such as `examples/experiment.ipynb`, to fit an imputer and generate predictions for selected blocks.
4. Use the metrics helpers in `examples/metrics/` to compare predictions against known values.
5. If you want to inspect results visually, follow the plotting cells in the notebook to summarize metrics across methods and missing-data settings.

---

## Architecture

The repository is organized around notebook-driven experiments in `examples/`, where data is loaded, transformed, and evaluated end to end. The main workflow shown in `examples/experiment.ipynb` builds block-level features, runs multiple imputers, compares predictions against ground truth, and stores aggregated results for analysis and plotting.

Reusable imputation logic lives in `examples/imputers/` behind a shared `BaseImputer` interface. Concrete implementations include simple baselines such as mean imputation and neighbor-based methods, as well as model-based approaches that combine spatial and contextual inputs. These imputers all expose a common `impute(...)` entrypoint so they can be swapped within the same experiment loop.

Evaluation is separated into `examples/metrics/`, which computes error and fit metrics from true versus predicted feature tables. The notebooks consume these metric helpers directly, then use standard plotting libraries for result inspection.

Overall, the architecture is a single-repository research setup with clear separation between imputation, metric calculation, and notebook-based orchestration. No evidence in the provided tree suggests a service-oriented or plugin-based deployment architecture.

---

## API Reference

The repository is script-oriented, with the main public usage surface exposed through the imputer classes in `examples/imputers/` and the metrics helper in `examples/metrics/`.

- `BaseImputer` — abstract base class for imputers; stores a preprocessed blocks table and provides `impute(blocks_ids)`.
- `MeanImputer` — fills the target feature columns with the mean value from known blocks.
- `SknnImputer` — neighbor-based imputer built on `NearestNeighbors`; uses centroid coordinates for neighbor lookup.
- `IdwImputer` — inverse-distance-weighted variant built on `SknnImputer`.
- `SmImputer` — Space Matrix imputer that uses `fsi`, `gsi`, `site_area`, and additional columns to estimate missing values.
- `SmvNmfImputer` — Space Matrix + NMF-based imputer that extends the neighbor-based approach.
- `Spacematrix` — helper class used by `SmImputer` to cluster blocks and derive cluster-level medians.
- `evaluate_metrics(true_df, pred_df)` — computes per-feature error metrics and returns a list of metric dictionaries.

---

## Examples

Examples of how this should work and how it should be used are available [here](https://github.com/GeorgeKontsevik/sm-imputation/tree/main/examples).

---

## Contributing

- **[Report Issues](https://github.com/GeorgeKontsevik/sm-imputation/issues)**: Submit bugs found or log feature requests for the project.

- **[Submit Pull Requests](https://github.com/GeorgeKontsevik/sm-imputation/tree/main/CONTRIBUTING.md)**: To learn more about making a contribution to sm-imputation.

---

## Citation

If you use this software, please cite it as below.

### APA format:

    GeorgeKontsevik (2026). sm-imputation repository [Computer software]. https://github.com/GeorgeKontsevik/sm-imputation

### BibTeX format:

    @misc{sm-imputation,

        author = {GeorgeKontsevik},

        title = {sm-imputation repository},

        year = {2026},

        publisher = {github.com},

        journal = {github.com repository},

        howpublished = {\url{https://github.com/GeorgeKontsevik/sm-imputation}},

        url = {https://github.com/GeorgeKontsevik/sm-imputation}

    }

---