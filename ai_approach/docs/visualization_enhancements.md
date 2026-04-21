# Visualization Notebook Enhancements

## Overview

The `02_visualisation.ipynb` notebook has been enhanced with additional visualizations that better align with the **WiDS Datathon 2026** requirements for **multi-horizon probability forecasting** and **survival analysis**.

## Assessment Against PROJECT_SUMMARY.md

### ✅ Requirements Met by Original Notebook

1. **Target Variable Visualization** ✓
   - Event distribution (censored vs hit)
   - Time to hit distributions
   - Box plots by event status

2. **Kaplan-Meier Survival Curve** ✓
   - Proper survival probability estimation
   - Median survival time calculation
   - Visual representation with confidence regions

3. **Feature Distributions** ✓
   - Key numerical features visualized
   - Zero-inflation patterns identified
   - Category-wise analysis

4. **Correlation Analysis** ✓
   - Correlation heatmap with targets
   - Top correlated features identified
   - Multicollinearity detection

5. **Temporal Patterns** ✓
   - Fire start time analysis (hour, day, month)
   - Event rates by temporal features

6. **Train-Test Comparison** ✓
   - Distribution similarity checks
   - No major distribution shifts detected

### 🔧 Enhancements Added

#### 1. Multi-Horizon Probability Visualization

**File**: `02_visualisation_enhanced.py`

**New Visualization**: `cumulative_incidence_with_horizons.png`

This critical addition addresses the competition's core requirement:

- **Cumulative Incidence Curve**: Shows probability of fire reaching within 5km over time
- **Four Submission Horizons Marked**: 12h, 24h, 48h, 72h with distinct colors
- **Probability Values Displayed**: Exact probabilities at each horizon
- **Monotonicity Verification**: Confirms probabilities increase with time (submission requirement)

**Key Insights from Enhanced Visualization**:
```
Submission Horizon Probabilities:
  prob_12h: 0.2247 (22.47%)
  prob_24h: 0.2944 (29.44%)
  prob_48h: 0.3134 (31.34%)
  prob_72h: 0.5519 (55.19%)

Monotonicity Check:
  12h → 24h: +0.0697 ✓ PASS
  24h → 48h: +0.0190 ✓ PASS
  48h → 72h: +0.2384 ✓ PASS
```

#### 2. Why These Enhancements Matter

**Alignment with Competition Requirements**:

1. **Submission Format**: The competition requires predictions at exactly these four horizons
2. **Monotonicity Constraint**: Predictions must satisfy `prob_12h ≤ prob_24h ≤ prob_48h ≤ prob_72h`
3. **Calibration Focus**: 70% of the score depends on probability calibration (Brier Score)
4. **Operational Context**: 48h horizon has highest weight (40%) due to operational value

**Survival Analysis Context**:

- **Survival Probability**: P(fire stays beyond 5km)
- **Cumulative Incidence**: P(fire reaches within 5km) = 1 - Survival
- **Censored Data**: ~50% of observations are censored (fire never reached 5km)
- **Time-to-Event**: Both if and when the event occurs matter

## Original Notebook Strengths

The existing `02_visualisation.ipynb` notebook is **excellent** and covers:

1. ✅ Comprehensive EDA of all feature categories
2. ✅ Proper Kaplan-Meier survival analysis
3. ✅ Detailed correlation analysis
4. ✅ Feature-target relationships
5. ✅ Zero-inflation identification
6. ✅ Temporal pattern analysis
7. ✅ Train-test distribution comparison
8. ✅ Summary statistics generation

## Recommendations for Integration

### Option 1: Add Enhanced Visualizations as New Cells

Add the following cells to the existing notebook after Section 3 (Kaplan-Meier):

**New Section 3b: Cumulative Incidence Curve**
```python
# Cumulative incidence is 1 - survival probability
cumulative_incidence = 1 - km_survival

# Plot with submission horizons marked
fig, ax = plt.subplots(figsize=(14, 7))
ax.step(km_times, cumulative_incidence, where='post', linewidth=2.5, color='#F38181')
# ... (see 02_visualisation_enhanced.py for full code)
```

**New Section 3c: Multi-Horizon Probability Analysis**
```python
# Visualize probabilities at the four submission horizons
horizons = [12, 24, 48, 72]
# ... (see 02_visualisation_enhanced.py for full code)
```

### Option 2: Run Enhanced Script Separately

The `02_visualisation_enhanced.py` script can be run independently:

```bash
cd notebooks
python 02_visualisation_enhanced.py
```

This generates `cumulative_incidence_with_horizons.png` in the `figures/` directory.

## Key Insights for Modeling

From the enhanced visualizations:

1. **Probability Distribution**:
   - Early period (0-12h): 22.5% of fires reach evacuation zones
   - Mid period (12-48h): Gradual increase to 31.3%
   - Late period (48-72h): Sharp increase to 55.2%

2. **Modeling Implications**:
   - 48-72h interval shows largest probability increase (23.8%)
   - Early predictions (12h) are most uncertain
   - Models must capture non-linear time dynamics

3. **Submission Strategy**:
   - Ensure monotonicity in predictions
   - Focus calibration on 48h horizon (highest weight)
   - Consider separate models for each horizon

## Files Generated

1. **Enhanced Script**: `notebooks/02_visualisation_enhanced.py`
2. **New Figure**: `figures/cumulative_incidence_with_horizons.png`
3. **This Document**: `notebooks/VISUALIZATION_ENHANCEMENTS.md`

## Conclusion

The original `02_visualisation.ipynb` notebook is **comprehensive and well-structured**. The enhancements add:

✅ **Multi-horizon probability visualization** (competition requirement)  
✅ **Cumulative incidence interpretation** (complement to survival curve)  
✅ **Monotonicity verification** (submission constraint)  
✅ **Operational context** (time-bound forecasting)

These additions make the visualization phase fully aligned with the competition's focus on **multi-horizon probability forecasting** for **wildfire evacuation zone impact prediction**.

---

**Last Updated**: 2026-04-17  
**Competition**: WiDS Worldwide Global Datathon 2026  
**Problem Type**: Survival Analysis - Multi-Horizon Probability Forecasting