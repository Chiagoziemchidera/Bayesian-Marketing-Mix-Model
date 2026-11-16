# Bayesian Marketing Mix Model

A Bayesian time-series analysis of weekly revenue and marketing spend across seven channels. The project uses PyMC to estimate channel contributions and return on investment (ROI), account for advertising carry-over and seasonality, and examine a potential budget reallocation.

## Project overview

The model combines three revenue patterns:

- **Baseline revenue:** the intercept, time trend, and seasonal pattern
- **Marketing effects:** channel spend transformed through geometric adstock to represent carry-over
- **Uncertainty:** posterior distributions and 94% highest-density intervals (HDIs) for model estimates

The analysis also compares an adstock model with a raw-spend baseline using Leave-One-Out Cross-Validation (LOO-CV), and simulates moving 20% of weekly spend from the lowest-ROI channel to the highest-ROI channel.

## Analysis highlights

- Explores 104 weeks of revenue and spend data across seven channels
- Uses Fourier terms to represent annual and semi-annual seasonality
- Fits the model with PyMC's No-U-Turn Sampler (NUTS)
- Reviews convergence using R-hat, effective sample size, energy behavior, and divergence counts
- Estimates channel contributions and ROI with posterior uncertainty
- Visualizes revenue decomposition and the proposed budget scenario

## Method

| Component | Approach |
|---|---|
| Carry-over | Learnable geometric adstock using `pytensor.scan` |
| Trend | Linear time trend |
| Seasonality | Two Fourier orders with an annual period of about 52.14 weeks |
| Inference | Bayesian sampling with NUTS |
| Model comparison | PSIS-LOO-CV |
| Uncertainty | Posterior distributions and 94% HDIs |

## Repository contents

```text
├── data/
│   └── MMM_test_data.csv       # Weekly revenue and channel spend data
├── MMM_Analysis.ipynb          # End-to-end analysis notebook
├── .gitignore
└── README.md                   # Project documentation
```

The dataset contains 104 weekly observations from August 2020 through August 2022, with revenue and spend values for seven marketing channels.

## Run the notebook

Use Python 3.10 or later. From the project directory, install the packages and launch Jupyter:

```bash
pip install numpy pandas matplotlib seaborn pymc arviz scikit-learn statsmodels jupyter
jupyter notebook MMM_Analysis.ipynb
```

Bayesian sampling can take several minutes. Runtime depends on available hardware.

## Results and interpretation

The notebook reports estimated channel contributions, ROI rankings, model diagnostics, posterior predictive checks, and LOO-CV comparison results. It also produces a scenario estimate for the proposed reallocation. Review the credible intervals alongside point estimates when comparing channels.

The budget simulation assumes a linear relationship between spend and revenue. Since the model does not include saturation or channel interactions, the scenario is best treated as an exploratory estimate and validated with a controlled experiment before making a large allocation change. The data is observational, so the model estimates associations and does not by itself establish causal effects.

## Potential extensions

- Add saturation curves, such as Hill or logistic response functions
- Model time-varying channel effectiveness or creative fatigue
- Include interactions between marketing channels
- Add external variables such as promotions, economic conditions, or holidays
- Validate channel effects with geo-holdout or A/B experiments
