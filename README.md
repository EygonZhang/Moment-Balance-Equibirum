# Moment-Balance-Equibirum (MBE)

## Module Overview

This repository presents a modular workflow for analyzing the **moment balance equilibria** of tidal turbine blades, leveraging high-fidelity simulations, interpolation, BEM-based physical modeling, and machine learning validation.

### MBE-1: Simulation cloud matrix & interpolaion.ipynb

This notebook constructs a **simulation cloud matrix** by sweeping through a range of operational parameters (e.g., pitch angle, TSR, flow velocity) using CFD/NS results. The data is then organized into an interpolation-ready format to support surrogate modeling and cross-stage consistency.

- Input: Raw simulation datasets (CFD/NS).
- Output: Structured matrix for fast multi-dimensional interpolation.

---

### MBE-2: BEM-NS results.ipynb

Here we implement the **Blade Element Momentum - Navier-Stokes (BEM-NS)** hybrid solver. By matching BEM results against interpolated NS simulations, the blade loading distribution is validated and adjusted to achieve dynamic moment balance.

- Input: CL and CD matrix from XFoil 2D NACA0012 data, considering both Reynolds number and twist.
- Output: Updated blade forces, local angle data, to calculate driving torque and inertial torque consistent with physics-based modeling at 3 different ω values at 1.000, 1.260, and 1.335 (rad/s).

---

### MBE-3: Optimization & inverse design

Planned notebook for running **inverse blade design** and optimization based on moment equilibrium constraints using BEM-NS and ML-assisted gradients.

---
### MBE-4: Traditional ML model trials.ipynb

This notebook runs **machine learning models** (e.g., SVR, Random Forest, ANN) to approximate the functional relationship between operational parameters and local moment equilibria. It serves as a surrogate validation tool and helps identify nonlinear patterns not captured in BEM.

- Input: Merged features from `MBE-1` and target values from `MBE-2`.
- Output: ML model performance metrics, predictions, and cross-validation results.

---


 **Workflow Summary**:
1. `MBE-1` prepares the simulation cloud.
2. `MBE-2` runs physical BEM-based analysis validated with NS data.
3. `MBE-4` tests traditional ML models against physical outputs.
4. Future `MBE-3` will enable design/optimization with learned surrogates.

---

*Note: All notebooks require Python 3.8+, NumPy, Pandas, Scikit-Learn, and Matplotlib. Parallel CFD data loading may depend on `h5py` or `xarray`.*

