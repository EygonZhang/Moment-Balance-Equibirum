# Moment-Balance-Equibirum (MBE)

## Overview

This repository presents a modular workflow for analyzing the **moment balance equilibrium (MBE)** of tidal turbine blades, combining high-fidelity simulations, BEM-based theoretical modeling, and machine learning validation.  
It provides a physics-grounded, open-source framework to identify optimal tip-speed-ratios (TSR) based on the condition where net hydrodynamic torque equals zero—offering a practical, low-cost alternative to full transient FSI simulations.

---

## Module Structure

### MBE-1: `Simulation cloud matrix & interpolation.ipynb`

This notebook builds a **simulation matrix** by sweeping flow velocities and angular velocities. It reshapes raw CFD data into a structured grid for interpolation and visualization.

- **Input:** Raw CFD simulation data (e.g., 20×7 net moment matrix)  
- **Output:** Interpolated torque maps ready for validation

### MBE-2: `BEM-NS results.ipynb`

Implements a hybrid **Blade Element Momentum – Navier-Stokes (BEM-NS)** solver. It calculates driving torque and inertial torque analytically and matches them against NS-based net torque.

- **Input:** 2D CL/CD datasets from NACA0012 (via XFOIL), twist angles  
- **Output:** Decomposed torques at three ω points (1.000, 1.260, 1.335 rad/s)

### MBE-3: `Isocline-Attention validation.ipynb`

Will fuse torque isoclines with attention-weighted MLP to optimize blade design based on equilibrium lines.

The feature augementation model is necessary to consider the non-linear terms (U∞·ω and ω^2)

### MBE-4: `Traditional ML model trials.ipynb`

Trains several ML models (e.g., SVR, ANN, Random Forest) to learn nonlinear torque behavior from simulation data.

- **Input:** Feature matrix (U∞, ω, TSR, etc.) and target net torque  
- **Output:** Model predictions and error analysis

---

## Software Requirements

- Python ≥ **3.8**
- `numpy`, `pandas`, `matplotlib`, `scikit-learn`
- (optional) `xarray`, `h5py` for parallel or structured data handling

**Tested on**:  
- macOS 14.4.1 (Apple Silicon)  
- Ubuntu 20.04 LTS  
- Windows 10 (x64)

**No non-standard hardware required.**

---

## How to Run in Google Colab

All `.ipynb` notebooks in this repository are designed to be run directly in **Google Colab**—no local installation required.

### To get started:

1. Visit the GitHub repository:  
   [https://github.com/EygonZhang/Moment-Balance-Equibirum](https://github.com/EygonZhang/Moment-Balance-Equibirum)

2. Open any notebook (e.g., `MBE-1 Simulation cloud matrix & interpolation.ipynb`)  
   and click the **"Open in Colab"** button (or manually copy the notebook URL into Colab).

3. In Colab, go to **Runtime → Change runtime type**, and ensure the environment is set to:  
   - **Python 3**  
   - **Hardware accelerator: None (default)**

4. Run the cells from top to bottom.  
   All required Python packages will be installed dynamically within Colab using `pip install`.

> *Typical setup time: <1 minute in Colab*

**Note**: If you would like to work locally instead of Colab:

1. Clone the repository:
```bash
git clone https://github.com/EygonZhang/Moment-Balance-Equibirum.git
cd Moment-Balance-Equibirum

2. Open the .ipynb files with Jupyter Notebook or VS Code + Jupyter extension.

3. Ensure Python 3.8+ is installed and run:
pip install numpy pandas matplotlib scikit-learn
(Typical install time: <2 minutes on a standard laptop)

---

## How to Use with Your Own Data

1. Replace the sample Excel files (`MBE-1`, `MBE-2`) with your own net moment matrix and torque results from simulations or experiments. Make sure the formatting matches the structure used in the example data.

2. To generate lift and drag coefficients customized to your blade geometry and twist distribution, please use our standalone BEM solver:  
   **[FX-BEMS (GitHub link)](https://github.com/EygonZhang/FX-BEMS)**  
   This tool interfaces with **XFOIL version 6.99** (© 2000 Mark Drela & Harold Youngren, released under the GNU General Public License with no warranty). It computes 3D rotationally corrected lift and drag coefficients, and estimates the axial and tangential induction factors **a** and **a′**.

3. Import the updated CL, CD, a, and a′ into `MBE-3` to retrain the isocline-based surrogate optimizer on your customized blade geometry.

4. You can also feed your own data into `MBE-4` for model comparison using pre-trained regressors, including **Random Forest**, **MLP**, and **Random Gaussian**. This supports benchmarking between data-driven and physics-based methods.

> All notebooks are modular and flexible—minimal editing is required to adapt them for new designs or operating conditions.

