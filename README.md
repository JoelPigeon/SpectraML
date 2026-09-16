# SpectraML

Jupyter-notebook workflows for analyzing infrared spectra from Delaney Lab research experiments. The notebooks build calibration models, evaluate reaction samples, investigate solvent effects, and generate synthetic calibration data for pyridone systems.

## Workflow

The analysis generally follows this pattern:

1. Load the second column of instrument-exported `.CSV` spectra.
2. Associate calibration spectra with known composition or conversion values.
3. Train a regression model, primarily `sklearn.cross_decomposition.PLSRegression`.
4. Optionally augment calibration data with solvent-contaminated synthetic spectra.
5. Predict experimental samples, compare predictions with expected values using mean absolute error, and plot results by solvent or sample group.

Some notebooks also use Random Forest regression, Savitzky-Golay preprocessing, standardization, interpolation to a common feature length, and response-factor analysis. The accompanying `.SPA` files are retained as source instrument data, while the notebooks primarily read the `.CSV` exports.

## Notebooks

### Calibration and regression

- [`PLSReg(Alpha) OG.ipynb`](PLSReg(Alpha)%20OG.ipynb) builds a five-component PLS model from 33 calibration spectra and reports train/test $R^2$ values.
- [`RandomForestReg.ipynb`](RandomForestReg.ipynb) prepares the same style of calibration matrix for Random Forest regression and compares model performance.
- [`GaussianCalibrationCurve.ipynb`](GaussianCalibrationCurve.ipynb) augments calibration spectra with Gaussian-sampled solvent contamination, trains a PLS model, and evaluates triplicate samples across DEE, DMSO, IPA, and water.
- [`FullySyntheticCC.ipynb`](FullySyntheticCC.ipynb) creates a larger synthetic calibration set by mixing pure pyridone spectra with solvent spectra across composition and contamination ranges, then evaluates a PLS model.

### Experimental solvent tests

- [`ExpSolvTest (Alpha).ipynb`](ExpSolvTest%20(Alpha).ipynb) applies an Alpha-pyridone PLS calibration to experimental solvent samples.
- [`ExpSolvTest (4-CF3).ipynb`](ExpSolvTest%20(4-CF3).ipynb) applies the 4-CF3 calibration workflow to experimental solvent samples.
- [`ResponseFactorTesting.ipynb`](ResponseFactorTesting.ipynb) groups spectra by solvent or response-factor condition, compares average predictions with a target value, and reports absolute error.

### Synthetic-contamination studies

- [`SyntheticCont (Alpha).ipynb`](SyntheticCont%20(Alpha).ipynb) tests additive synthetic solvent contamination for the Alpha-pyridone system.
- [`SyntheticCont (4-CF3).ipynb`](SyntheticCont%20(4-CF3).ipynb) tests additive synthetic solvent contamination for the 4-CF3 system.
- [`SyntheticCont (4-OMe).ipynb`](SyntheticCont%20(4-OMe).ipynb) tests additive synthetic solvent contamination for the 4-OMe system.

### Reaction and dataset studies

- [`C-H activation exchange reaction testing.ipynb`](C-H%20activation%20exchange%20reaction%20testing.ipynb) analyzes spectra from C-H activation and exchange-reaction experiments.
- [`ExpSolvTest (4-CF3).ipynb`](ExpSolvTest%20(4-CF3).ipynb) evaluates the 4-CF3 experimental solvent set.
- [`PLSReg(4-CF3).ipynb`](PLSReg(4-CF3).ipynb) develops a PLS regression workflow for the 4-CF3 system.
- [`PLSReg(4-OMe).ipynb`](PLSReg(4-OMe).ipynb) develops a PLS regression workflow for the 4-OMe system.
- [`PLSReg(Alpha) OG.ipynb`](PLSReg(Alpha)%20OG.ipynb) develops the original Alpha-pyridone PLS workflow.

### Supporting analyses

- [`GaussianCalibrationCurve.ipynb`](GaussianCalibrationCurve.ipynb) evaluates Gaussian synthetic calibration augmentation.
- [`ResponseFactorTesting.ipynb`](ResponseFactorTesting.ipynb) evaluates response-factor and solvent-group behavior.
- [`SyntheticCont (Alpha).ipynb`](SyntheticCont%20(Alpha).ipynb), [`SyntheticCont (4-CF3).ipynb`](SyntheticCont%20(4-CF3).ipynb), and [`SyntheticCont (4-OMe).ipynb`](SyntheticCont%20(4-OMe).ipynb) compare synthetic-contamination strategies across substrates.

## Data layout

The `dataset/` directory contains experiment folders organized by run or sample set, including calibration runs, solvent studies, backgrounds, and reaction datasets. Each folder may contain paired `.CSV` and `.SPA` files. Notebook paths currently point to local directories outside this repository, so update the `root_directory`, `root_calib`, `solvent_dir`, and `test_dir` variables before running a notebook on another machine.

## Requirements

Install the Python packages used across the notebooks:

```bash
python -m pip install numpy pandas matplotlib scipy scikit-learn jupyter
```

The notebooks are intended to be run in VS Code with the Jupyter extension or from a Jupyter environment. Most model notebooks expect spectra with roughly 7,468 intensity features and use the second CSV column as the model input.

## Outputs

Notebook outputs include calibration plots, solvent-group prediction plots, $R^2$ scores, mean absolute error summaries, and CSV result files such as `YH_2025_009_gaussian_triplicates_results.csv` and `YH_2025_009_grouped_triplicates_results.csv`.
