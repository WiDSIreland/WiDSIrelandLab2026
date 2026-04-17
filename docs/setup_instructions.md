# Fix scikit-survival Installation Issue

## Problem
`scikit-survival` doesn't support Python 3.14 yet. It requires Python 3.9-3.12.

## Solution: Downgrade to Python 3.12

### Step 1: Update `.python-version`
Edit the `.python-version` file and change:
```
3.14
```
to:
```
3.12
```

### Step 2: Update `pyproject.toml`
Edit the `pyproject.toml` file and change line 6 from:
```toml
requires-python = ">=3.14"
```
to:
```toml
requires-python = ">=3.12"
```

### Step 3: Add scikit-survival to dependencies
In `pyproject.toml`, add `scikit-survival` and `lifelines` to the dependencies section (after line 16):
```toml
dependencies = [
    "jupyter-black>=0.4.0",
    "jupyterlab>=4.5.6",
    "matplotlib>=3.10.8",
    "numpy>=2.4.4",
    "pandas>=3.0.2",
    "scikit-learn>=1.8.0",
    "scipy>=1.17.1",
    "seaborn>=0.13.2",
    "statsmodels>=0.14.6",
    "scikit-survival>=0.23.0",
    "lifelines>=0.29.0",
]
```

### Step 4: Reinstall dependencies
Run these commands in your terminal:

```bash
# Remove existing virtual environment (if using uv)
rm -rf .venv

# Sync dependencies with new Python version
uv sync

# Or if using pip:
pip install scikit-survival lifelines
```

### Step 5: Verify installation
Test that scikit-survival is installed:
```bash
python -c "import sksurv; print(f'scikit-survival version: {sksurv.__version__}')"
```

## Alternative: Use lifelines only (if you want to stay on Python 3.14)

If you prefer to keep Python 3.14, you can modify the notebook to use only `lifelines`, which supports newer Python versions. However, you'll lose the Random Survival Forest functionality.

### Changes needed in notebook:
1. Remove all `sksurv` imports
2. Use `lifelines.CoxPHFitter` instead of `sksurv.linear_model.CoxPHSurvivalAnalysis`
3. Replace Random Survival Forest with Gradient Boosting or other models from `lifelines`

## Recommended Approach
**Use Python 3.12** - This gives you access to the full suite of survival analysis tools including Random Survival Forest, which is the best-performing model in your notebook.

## After Making Changes
Once you've updated the files and reinstalled dependencies, your notebook should run without errors!