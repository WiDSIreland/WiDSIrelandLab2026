# WiDS Datathon 2026 - Wildfire Evacuation Zone Impact Prediction

## Project Overview

This project is part of the **Women in Data Science (WiDS) Worldwide Global Datathon 2026**, focusing on predicting when wildfires will impact evacuation zones. The challenge addresses a critical real-world problem: providing accurate predictions to help emergency responders and communities prepare for wildfire threats.

## Problem Statement

**Objective**: Predict the probability that a wildfire will threaten an evacuation zone (come within 5 kilometers) at multiple time horizons: 12, 24, 48, and 72 hours after the initial 5-hour observation period.

**Challenge Type**: Survival Analysis with Censored Data - Multi-Horizon Probability Forecasting

This is a survival analysis problem requiring **calibrated probability forecasts** across multiple time horizons. Emergency responders need:
1. **Urgency Rankings**: Which fires demand immediate attention (prioritization)
2. **Calibrated Probabilities**: Trustworthy risk estimates for high-stakes decisions about evacuations, resource deployment, and public alerts
3. **Time-Bound Forecasts**: Actionable predictions at 12h, 24h, 48h, and 72h horizons

## Target Variables and Submission Format

### Training Data Targets

1. **`time_to_hit_hours`**: Continuous variable (0-72 hours)
   - Time from t₀+5h until the fire comes within 5km of an evacuation zone
   - Measured in hours from the initial observation point
   - Range: 0 to 72 hours

2. **`event`**: Binary indicator (0 or 1)
   - `1`: Fire reached within 5km during the 72-hour window (event occurred)
   - `0`: Fire did not reach within 5km (censored observation)

### Required Submission Format

Submissions must include probability predictions for each test event at four time horizons:

- **`event_id`**: Unique identifier matching the test set
- **`prob_12h`**: Probability of hitting evacuation zone by 12 hours
- **`prob_24h`**: Probability of hitting evacuation zone by 24 hours
- **`prob_48h`**: Probability of hitting evacuation zone by 48 hours
- **`prob_72h`**: Probability of hitting evacuation zone by 72 hours

**Submission Constraints**:
- All probabilities must be in [0, 1]
- Monotonicity enforced: `prob_12h ≤ prob_24h ≤ prob_48h ≤ prob_72h`
- No missing, duplicate, or extra event IDs allowed

## Why Survival Analysis?

This problem requires survival analysis techniques because:

- **Censored Data**: ~50% of observations are censored (fire never reached 5km threshold)
- **Time-to-Event**: We're predicting both if and when an event occurs
- **Right Censoring**: We only observe up to 72 hours; some fires may hit after this window
- **Traditional ML Limitations**: Standard regression/classification cannot properly handle censored observations

## Dataset Description

### Data Structure

- **Training Set**: Contains historical wildfire events with known outcomes
- **Test Set**: Requires predictions for new wildfire events
- **Metadata**: Provides feature descriptions and categories

### Feature Categories

The dataset includes multiple categories of features capturing different aspects of wildfire behavior:

1. **Temporal Coverage** (3 features)
   - Number of fire perimeter observations
   - Time span between first and last observations
   - Data quality indicators

2. **Growth Metrics** (10 features)
   - Initial fire area
   - Absolute and relative growth rates
   - Radial expansion measurements
   - Log-transformed growth indicators

3. **Centroid Kinematics** (5 features)
   - Fire centroid movement and speed
   - Directional spread patterns
   - Bearing angles (trigonometric components)

4. **Distance Metrics** (9 features)
   - Minimum distance to evacuation zone
   - Distance change over time
   - Closing speed (rate of approach)
   - Projected advance calculations

5. **Directionality** (4 features)
   - Alignment between fire movement and evacuation zone
   - Cross-track and along-track components
   - Directional threat indicators

6. **Temporal Metadata** (3 features)
   - Fire start time (hour, day of week, month)
   - Seasonal and temporal patterns

### Key Data Characteristics

- **Zero-Inflated Features**: Many features have >50% zero values, indicating sparse fire activity
- **Censoring Rate**: Approximately 50% of training observations are censored
- **No Missing Values**: Complete dataset with no missing data
- **Multicollinearity**: Some feature groups show high correlation (>0.9)

## Modeling Approach

### Survival Analysis Models

Given the censored nature of the data, we employ survival analysis techniques:

1. **Kaplan-Meier Estimator** (Baseline)
   - Non-parametric survival curve estimation
   - Provides baseline survival probabilities

2. **Cox Proportional Hazards Model**
   - Semi-parametric model
   - Estimates hazard ratios for features
   - Interpretable coefficients

3. **Random Survival Forest** (Primary Model)
   - Non-parametric ensemble method
   - Handles non-linear relationships
   - Robust to outliers and multicollinearity
   - Provides feature importance rankings

### Evaluation Metrics

**Primary Competition Metric**: Hybrid Score

```
Hybrid Score = 0.3 × C-index + 0.7 × (1 - Weighted Brier Score)
```

This dual-objective metric balances two critical needs:

#### 1. C-index (30% weight)
- **Purpose**: Measures ranking quality for prioritization/triage
- **Range**: 0.5 (random) to 1.0 (perfect)
- **What it measures**: How well the model orders fires by urgency
- **Why it matters**: Emergency managers need to know which fires to address first when resources are limited

#### 2. Weighted Brier Score (70% weight)
- **Purpose**: Measures probability calibration for decision-making
- **Calculation**: Weighted average across three horizons:
  - **24h**: 30% weight
  - **48h**: 40% weight (highest operational value)
  - **72h**: 30% weight
- **What it measures**: Accuracy of probability estimates at each time horizon
- **Why it matters**: Responders need trustworthy probabilities for threshold-based decisions (e.g., "evacuate if probability > 60%")

**Why 48h Gets Highest Weight**:
- 24-48 hours provides the strongest operational value zone
- Balances actionable lead time with decision urgency
- 72h is useful for extended planning but less operationally immediate

**Censoring Handling in Brier Score**:
- **Hits**: Target = 1 if fire hit by horizon H, else 0
- **Censored after H**: Target = 0 (known not to have hit)
- **Censored before H**: Excluded from evaluation (unknown outcome)

**Development Metrics**:
- Integrated Brier Score: Overall prediction error over time
- Time-dependent AUC: Classification accuracy at specific time points
- Calibration plots: Visual assessment of probability reliability

## Feature Engineering Strategy

### 1. Zero-Inflated Feature Handling
- Create binary indicators for zero values
- Apply log1p transformations to reduce skewness
- Preserve information about sparse fire activity

### 2. Interaction Features
Critical domain-specific interactions:
- Distance × Growth Rate: Threat intensity
- Distance × Closing Speed: Immediate danger
- Growth × Speed: Fire momentum
- Distance × Alignment: Directional threat

### 3. Domain-Specific Features
- **Wildfire Risk Score**: Composite metric combining distance, area, growth, and speed
- **Fire Intensity**: Area × growth rate
- **Threat Velocity**: Closing speed × alignment
- **Time to Impact**: Estimated time based on distance and speed
- **Spread Efficiency**: Radial growth relative to area

### 4. Polynomial Features
- Squared terms for key predictors
- Captures non-linear relationships

### 5. Multicollinearity Reduction
- Remove features with correlation >0.9
- Retain features with higher target correlation

### 6. Feature Selection
Combined approach:
- Correlation-based selection
- Mutual information scores
- Union of top features from both methods

## Project Structure

```
WiDSIrelandLab2026/
├── data/
│   ├── train.csv                      # Training data
│   ├── test.csv                       # Test data
│   ├── metaData.csv                   # Feature descriptions
│   ├── X_train_engineered.csv         # Processed training features
│   ├── X_test_engineered.csv          # Processed test features
│   ├── y_train.csv                    # Training targets
│   ├── selected_features.csv          # Final feature list
│   ├── feature_importance.csv         # Model feature rankings
│   └── submission.csv                 # Final predictions
├── notebooks/
│   ├── 01_eda.ipynb                   # Exploratory Data Analysis
│   ├── 02_visualisation.ipynb         # Data Visualization
│   ├── 03_feature_engineering.ipynb   # Feature Engineering
│   └── 04_modelling_and_evaluation.ipynb  # Model Training & Evaluation
├── figures/                           # Generated visualizations
└── models/                            # Saved models and scalers
```

## Workflow Summary

### Phase 1: Exploratory Data Analysis (Notebook 01)
- Load and inspect data structure
- Analyze target variable distributions
- Identify missing values and data quality issues
- Examine feature categories and statistics
- Detect outliers and anomalies
- Compare train/test distributions

**Key Findings**:
- 50% censoring rate requires survival analysis
- Many zero-inflated features
- High correlation within feature groups
- No missing values
- Temporal patterns in fire start times

### Phase 2: Data Visualization (Notebook 02)
- Visualize target distributions and survival curves
- Create Kaplan-Meier survival estimates
- Analyze feature distributions by category
- Examine correlation patterns
- Explore temporal patterns
- Compare train/test distributions

**Key Insights**:
- Distance metrics strongly correlate with outcomes
- Growth features show high variability
- Temporal patterns exist but are weak predictors
- Train/test distributions are similar (no major shift)

### Phase 3: Feature Engineering (Notebook 03)
- Handle zero-inflated features
- Create interaction features
- Generate domain-specific wildfire features
- Address multicollinearity
- Apply robust scaling
- Select most predictive features

**Results**:
- Reduced from ~34 original features to ~70 engineered features
- Selected top 50-70 features for modeling
- Created 5 interaction features
- Generated 5 domain-specific features
- Removed highly correlated features

### Phase 4: Modeling and Evaluation (Notebook 04)
- Train baseline Kaplan-Meier model
- Develop Cox Proportional Hazards model
- Train Random Survival Forest (best performer)
- Evaluate using C-index
- Analyze feature importance
- Generate final predictions

**Model Performance**:
- Random Survival Forest: Best validation C-index
- Cox PH: Good interpretability, slightly lower performance
- Feature importance reveals distance and growth metrics as top predictors

## Real-World Impact and Partnership

### WatchDuty Context

This competition is developed in partnership with **WatchDuty**, a nonprofit revolutionizing emergency alerts:

- **Mission**: Deliver life-saving wildfire information when it's needed most
- **Scale**: 6+ million users across 22 states
- **Team**: 20 staff + 200+ volunteer firefighters, dispatchers, and radio operators
- **Technology**: Hyperlocal, real-time updates via scanners, cameras, and social feeds
- **Proven Impact**: #1 most downloaded app during 2025 LA wildfires (surpassing ChatGPT)
- **Reliability**: Handled 100k requests/second when other systems crashed

### Operational Decision Support

This project addresses critical needs in wildfire emergency management:

1. **Evacuation Prioritization**: Rank which communities need warnings first
2. **Resource Deployment**: Guide allocation of firefighting crews and aircraft
3. **Public Alerts**: Determine when and where to issue warnings
4. **Risk Communication**: Provide calibrated probabilities for threshold-based decisions
5. **Capacity Management**: Optimize limited messaging and response capacity

### Why This Matters

Emergency managers face impossible decisions with incomplete information:
- **Which fires** will reach populated areas?
- **How quickly** will they arrive?
- **Which communities** should prepare first?

Traditional approaches reduce this to a binary question ("Will this fire become dangerous?"), but real decisions require:
- **Time-bound forecasts**: When will the threat materialize?
- **Comparative rankings**: Which incident is most urgent?
- **Calibrated probabilities**: How confident should we be?

This competition provides the analytical foundation for these life-saving decisions.

## Technical Challenges Addressed

1. **Censored Data**: Properly handled using survival analysis
2. **Zero-Inflation**: Addressed through binary indicators and transformations
3. **Multicollinearity**: Reduced through correlation analysis
4. **Feature Selection**: Combined multiple methods for robust selection
5. **Model Interpretability**: Balanced performance with explainability

## Key Takeaways

1. **Dual Objectives Matter**: Success requires both ranking quality (C-index) and probability calibration (Brier Score)
2. **Survival Analysis is Essential**: Standard ML methods cannot properly handle censored data
3. **Multi-Horizon Forecasting**: Must predict probabilities at 12h, 24h, 48h, and 72h with monotonicity
4. **Calibration is Critical**: 70% of score depends on probability accuracy, not just rankings
5. **Domain Knowledge Matters**: Wildfire-specific features significantly improve predictions
6. **Distance Metrics are Key**: Proximity to evacuation zones is the strongest predictor
7. **Operational Context**: 48h horizon weighted highest due to operational value

## Future Improvements

1. **Probability Calibration**: Implement isotonic regression or Platt scaling for better calibration
2. **Multi-Horizon Optimization**: Train models specifically for each time horizon
3. **Ensemble Methods**: Combine multiple survival models with calibrated averaging
4. **Hyperparameter Tuning**: Systematic optimization for both C-index and Brier Score
5. **External Data**: Incorporate weather, terrain, and vegetation data
6. **Deep Learning**: Explore neural survival models (DeepSurv, DeepHit)
7. **Temporal Modeling**: Account for time-varying covariates
8. **Spatial Features**: Include geographic and topographic information
9. **Monotonicity Enforcement**: Ensure predictions respect time-ordering constraints

## References

- **WiDS Datathon 2026**: [Competition Page](https://www.kaggle.com/competitions/WiDSWorldWide_GlobalDathon26)
- **Survival Analysis**: Kaplan-Meier, Cox Proportional Hazards, Random Survival Forests
- **Libraries**: scikit-survival, lifelines, pandas, scikit-learn

## Contributors

This project was developed as part of the WiDS Datathon 2026, demonstrating the application of advanced survival analysis techniques to real-world wildfire prediction challenges.

---

**Last Updated**: April 2026
**Competition**: WiDS Worldwide Global Datathon 2026
**Partner**: WatchDuty (Nonprofit Emergency Alert Platform)
**Problem Type**: Survival Analysis - Multi-Horizon Probability Forecasting
**Primary Metric**: Hybrid Score (30% C-index + 70% Weighted Brier Score)
**Submission Format**: Probabilities at 12h, 24h, 48h, 72h horizons