# WiDS Datathon 2026 - Wildfire Evacuation Zone Impact Prediction

This repository contains the complete analysis and modeling pipeline for the **Women in Data Science (WiDS) Worldwide Global Datathon 2026**, focusing on predicting when wildfires will impact evacuation zones.

## 🎯 Project Overview

Predict the probability that a wildfire will threaten an evacuation zone (come within 5 kilometers) at multiple time horizons: 12, 24, 48, and 72 hours after the initial 5-hour observation period.

**Challenge Type**: Survival Analysis with Censored Data - Multi-Horizon Probability Forecasting

## 📁 Repository Structure

```
WiDSIrelandLab2026/
└── ai_approach/                    # Main project directory
    ├── data/                       # Data files and outputs
    │   ├── train.csv               # Training data
    │   ├── test.csv                # Test data
    │   ├── metaData.csv           # Feature descriptions
    │   ├── X_train_engineered.csv # Processed training features
    │   ├── X_test_engineered.csv  # Processed test features
    │   ├── y_train.csv            # Training targets
    │   ├── selected_features.csv  # Final feature list
    │   ├── feature_importance.csv # Model feature rankings
    │   └── submission.csv         # Final predictions
    ├── notebooks/                  # Jupyter notebooks
    │   ├── 01_eda.ipynb           # Exploratory Data Analysis
    │   ├── 02_visualisation.ipynb # Data Visualization
    │   ├── 03_feature_engineering.ipynb   # Feature Engineering
    │   └── 04_modelling_and_evaluation.ipynb  # Model Training & Evaluation
    ├── figures/                    # Generated visualizations
    ├── models/                     # Saved models and scalers
    ├── docs/                      # Documentation
    │   ├── project_summary.md     # Comprehensive project documentation
    │   ├── setup_instructions.md  # Environment setup guide
    │   ├── scikit_survival_advantages.md  # Survival analysis rationale
    │   ├── integration_summary.md # Notebook integration details
    │   └── visualization_enhancements.md  # Visualization improvements
    └── README.md                  # This file
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

Execute notebooks in order:
1. `01_eda.ipynb` - Exploratory Data Analysis
2. `02_visualisation.ipynb` - Data Visualization
3. `03_feature_engineering.ipynb` - Feature Engineering
4. `04_modelling_and_evaluation.ipynb` - Model Training & Evaluation

## 📊 Key Features

- **Survival Analysis**: Proper handling of censored data (~50% censoring rate)
- **Multi-Horizon Forecasting**: Predictions at 12h, 24h, 48h, and 72h
- **Feature Engineering**: 70+ engineered features from 34 original features
- **Model Ensemble**: Kaplan-Meier, Cox PH, and Random Survival Forest
- **Monotonicity Enforcement**: Ensures prob_12h ≤ prob_24h ≤ prob_48h ≤ prob_72h

## 📈 Model Performance

- **Best Model**: Random Survival Forest
- **Evaluation Metric**: C-index (Concordance Index)
- **Top Predictors**: Distance metrics, growth rates, and domain-specific features

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

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.

## 🙏 Acknowledgments

- **WiDS Worldwide**: For organizing the datathon
- **WatchDuty**: Partner organization providing real-world context
- **WiDS Ireland**: Local community support

---

**Competition**: WiDS Worldwide Global Datathon 2026  
**Partner**: WatchDuty (Nonprofit Emergency Alert Platform)  
**Problem Type**: Survival Analysis - Multi-Horizon Probability Forecasting  
**Last Updated**: April 2026
