# Sylhet City Microclimate Analysis

Code for the exploratory analysis, regression modelling, and land-cover scenario analysis of land surface temperature (LST) across climatic blocks in Sylhet City.

This repository is structured as a reproducible supplementary analysis package. The notebook contains descriptive analysis, model comparison, held-out test evaluation, feature-importance plots, and scenario projections for changes in tree vegetation, non-tree vegetation, water, and bare land.

## Repository structure

```text
.
├── analysis.ipynb
├── data/
│   └── data.csv
├── results/
│   ├── intervention_scenarios.csv
│   └── intervention_scenario_table.csv
├── requirements.txt
└── README.md
```

`data/data.csv` is the input expected by the notebook. If the data cannot be distributed with the repository, replace this file with the appropriate data-access instructions.

## Environment

The notebook was prepared for Python 3.11.

Create a virtual environment and install the recorded dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

On Windows, activate the environment with:

```bash
.venv\Scripts\activate
```

Then open `analysis.ipynb` and run the notebook from top to bottom.

## Analysis outline

1. Inspect data types, missingness, distributions, climatic-block differences, correlations, and IQR-based outlier flags.
2. Impute missing predictor values by climatic-block median, with overall median fallback.
3. Compare linear regression, ridge regression, gradient boosting, random forest, and XGBoost using five-fold cross-validation.
4. Refit the model with the highest mean cross-validated R² and evaluate it on a held-out 20% test set.
5. Use the fitted model for block-level land-cover scenario projections.
6. Export tidy and pivoted scenario tables to `results/`.

## Interpretation

The scenario analysis is predictive rather than causal. It applies hypothetical land-cover shifts to the fitted model and reports the resulting change in predicted mean LST. Large shifts may move predictor combinations beyond the range represented in the training data and should be treated as extrapolative projections.

The allocation of converted bare land among tree vegetation, non-tree vegetation, and water is a modelling heuristic derived from model-predicted cooling. It does not represent an ecological, engineering, or economic optimum.

## Reproducibility notes

- Random operations use `RANDOM_STATE = 42`.
- Package installation is kept outside the notebook and recorded in `requirements.txt`.
- Notebook outputs are cleared so the repository stores source code rather than stale execution results.
- Generated CSV files are written to `results/`.
