# WiDS Datathon 2026 - Wildfire Evacuation Zone Impact Prediction

This repository contains the analysis and modeling pipeline for the **Women in Data Science (WiDS) Worldwide Global Datathon 2026**, focusing on predicting when wildfires will impact evacuation zones.

## 🎯 Project Overview

Predict the probability that a wildfire will threaten an evacuation zone (come within 5 kilometers) at multiple time horizons: 12, 24, 48, and 72 hours after the initial 5-hour observation period.

**Challenge Type**: Survival Analysis with Censored Data - Multi-Horizon Probability Forecasting

![image alt](https://github.com/WiDSIreland/WiDSIrelandLab2026/blob/dcfa2580a15e7863ac5e435e8419f3705cf324a4/WiDS%20EDA%20ML.png)

## 📁 Repository Structure

```
WiDSIrelandLab2026/
├── .gitignore                  # Git ignore patterns
├── .pyrightconfig.json         # Pyright type checker configuration
├── .python-version             # Python version specification
├── LICENSE                     # MIT License
├── main.py                     # Main entry point
├── pyproject.toml              # Project dependencies and metadata
├── uv.lock                     # Dependency lock file
├── README.md                   # This file
└── ai_approach/                # Main project directory
    ├── data/                   # Data files and outputs
    ├── notebooks/              # Jupyter notebooks
    ├── figures/                # Generated visualizations
    ├── models/                 # Saved models and scalers
    ├── docs/                   # Documentation
    └── README.md               # Detailed project documentation
```

## 🚀 Quick Start

### Prerequisites
- Python 3.12+
- uv (recommended) or pip

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/WiDSIrelandLab2026.git
cd WiDSIrelandLab2026

# Install dependencies using uv (recommended)
uv sync

# Or using pip
pip install -e .
```

### Running the Analysis

See the detailed documentation in [`ai_approach/README.md`](ai_approach/README.md) for complete instructions on running the analysis notebooks.

## 📊 Key Features

- **Survival Analysis**: Proper handling of censored data (~50% censoring rate)
- **Multi-Horizon Forecasting**: Predictions at 12h, 24h, 48h, and 72h
- **Feature Engineering**: 70+ engineered features from 34 original features
- **Model Ensemble**: Kaplan-Meier, Cox PH, and Random Survival Forest
- **Monotonicity Enforcement**: Ensures prob_12h ≤ prob_24h ≤ prob_48h ≤ prob_72h

## 🎯 Results

### Model Performance
- **Best Model**: Random Survival Forest
- **Primary Metric**: Hybrid Score = 0.3 × C-index + 0.7 × (1 - Weighted Brier Score)
- **Evaluation**: C-index (Concordance Index) for ranking quality

### Key Findings
- **Top Predictors**: Distance metrics, growth rates, and domain-specific features
- **Feature Importance**: Distance to evacuation zone is the strongest predictor
- **Censoring Handling**: Survival analysis properly accounts for ~50% censored observations
- **Multi-Horizon Predictions**: Calibrated probabilities at 12h, 24h, 48h, and 72h time horizons

### Operational Impact
- **Evacuation Prioritization**: Rank which communities need warnings first
- **Resource Deployment**: Guide allocation of firefighting crews and aircraft
- **Risk Communication**: Provide calibrated probabilities for threshold-based decisions
- **Time-Bound Forecasts**: Actionable predictions for emergency response planning

## 📚 Documentation

Detailed documentation is available in the [`ai_approach/docs/`](ai_approach/docs/) folder:

- **[Project Summary](ai_approach/docs/project_summary.md)**: Comprehensive overview of the project, methodology, and results
- **[Setup Instructions](ai_approach/docs/setup_instructions.md)**: Environment setup and dependency management
- **[Scikit-Survival Advantages](ai_approach/docs/scikit_survival_advantages.md)**: Why we use survival analysis
- **[Integration Summary](ai_approach/docs/integration_summary.md)**: Notebook workflow details
- **[Visualization Enhancements](ai_approach/docs/visualization_enhancements.md)**: Visualization improvements

## 🤝 Contributing

This project was developed as part of the WiDS Datathon 2026. Contributions and suggestions are welcome!

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **WiDS Worldwide**: For organizing the datathon
- **WatchDuty**: Partner organization providing real-world context
- **WiDS Ireland**: Local community support

---

**Competition**: WiDS Worldwide Global Datathon 2026  
**Partner**: WatchDuty (Nonprofit Emergency Alert Platform)  
**Problem Type**: Survival Analysis - Multi-Horizon Probability Forecasting  
**Last Updated**: April 2026
