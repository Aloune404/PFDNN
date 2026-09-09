# PFDNN

## PFDNN: A Three-Stage Progressive Training Framework with Physical-Frequency Decomposition for Solving Multi-Scale PDEs

This repository contains the implementation and numerical experiment notebooks associated with the manuscript:

**PFDNN: A Three-Stage Progressive Training Framework with Physical-Frequency Decomposition for Solving Multi-Scale PDEs**

PFDNN is a physics-informed neural network framework designed for multi-scale partial differential equations (PDEs) with spatially varying frequency content. The method combines **physics-driven adaptive spatial decomposition**, **subdomain-specific frequency adaptation**, and a **progressive global-local training strategy**.

---

## Overview

Standard physics-informed neural networks (PINNs) often struggle with multi-scale PDEs because of spectral bias: low-frequency components are typically learned much faster than high-frequency components. This difficulty becomes more pronounced when high-frequency structures are localized in only part of the computational domain.

PFDNN addresses this problem through a three-stage framework:

1. **Global base-network pre-training**  
   A global low-frequency network is first trained over the entire domain to capture the coarse solution structure and provide physics-based diagnostic fields.

2. **Physics-driven adaptive subdomain identification**  
   A composite indicator constructed from the solution gradient, Laplacian, and PDE residual is used to identify difficult regions. DBSCAN clustering is then applied to automatically generate adaptive subdomains without requiring ground-truth solution information or a predefined number of subdomains.

3. **Frequency-adaptive local correction and joint optimization**  
   Local subnetworks are introduced in the detected subdomains. Their Fourier frequency scales are learnable and adapt independently to local frequency characteristics. The local networks are first warmed up while the base network is frozen, after which the complete model is jointly fine-tuned.

The final approximation is represented as a global base solution plus window-weighted local corrections.

---

## Main Features

- Physics-informed adaptive spatial decomposition
- DBSCAN-based automatic identification of high-complexity regions
- Subdomain-specific learnable Fourier frequency scales
- Global low-frequency base network with localized high-frequency correction networks
- Zero-initialized local corrections for stable network insertion
- Progressive warm-up and joint-training strategy
- No ground-truth solution is required for subdomain detection

---

## Numerical Experiments in the Manuscript

The manuscript evaluates PFDNN on eight two-dimensional benchmark settings:

1. Variable-frequency function fitting
2. Baseline Poisson equation
3. Variable-scale Poisson equation
4. Helmholtz equation with `A = 2, B = 14`
5. Extreme-frequency Helmholtz equation with `A = 3, B = 20`
6. Spatially non-uniform-frequency Helmholtz equation
7. Modified Helmholtz equation with `mu = 50`
8. Modified Helmholtz equation with `mu = 100`

The principal comparisons include:

- Standard PINN
- MscaleDNN
- PFDNN V1
- PFDNN V2
- Additional control models used in the corresponding experiments

PFDNN V1 combines adaptive spatial decomposition with fixed local frequency scales, while **PFDNN V2 further introduces learnable subdomain-specific frequency scales**.

---

## Repository Structure

The repository is currently being organized experiment by experiment. The present public version contains the following notebook set:

```text
PFDNN/
├── README.md
└── 4.2.2/
    ├── 偏微分PINN.ipynb
    ├── 偏微分MSDNN.ipynb
    ├── 偏微分PFD-NetV1.ipynb
    ├── 偏微分PFD-NetV2.ipynb
    ├── 偏微分Test-A.ipynb
    ├── 偏微分Tset-B.ipynb
    └── 偏微分求解可视化.ipynb
```

The notebooks include baseline PINN and MscaleDNN implementations, PFDNN V1/V2 implementations, control experiments, and visualization utilities.

> **Note:** Additional experiment folders and reproducibility files will be added as the repository is finalized for manuscript submission.

---

## Requirements

The numerical experiments are implemented in Python and rely on common scientific-computing and machine-learning packages, including:

- Python
- PyTorch
- NumPy
- SciPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

A version-pinned environment file will be added to the repository for reproducibility.

A typical environment can be prepared with:

```bash
pip install torch numpy scipy scikit-learn matplotlib jupyter
```

GPU acceleration is recommended for the larger PDE experiments.

---

## Running the Notebooks

Clone the repository:

```bash
git clone https://github.com/Aloune404/PFDNN.git
cd PFDNN
```

Start Jupyter:

```bash
jupyter notebook
```

Then open the notebook corresponding to the desired baseline or PFDNN variant.

For a fair comparison, the training configuration, sampling strategy, network architecture, and optimization parameters should be kept consistent with those reported in the manuscript and the corresponding notebook.

---

## Method Summary

The PFDNN approximation is represented by

```text
u(x) = u_base(x) + sum_i phi_i(x) * u_sub^(i)(r_i(x))
```

where:

- `u_base` is the global base-domain network;
- `M` is the number of adaptively detected subdomains;
- `phi_i` is a smooth window function associated with the `i`-th subdomain;
- `r_i` maps global coordinates to normalized local coordinates;
- `u_sub^(i)` is the local correction network.

The adaptive subdomain indicator combines normalized gradient, Laplacian, and PDE-residual information. Candidate high-complexity points are selected by a percentile threshold and clustered using DBSCAN.

---

## Reproducibility

The experiments in the manuscript are numerical and do not rely on external datasets. Training samples and collocation points are generated computationally according to the settings specified in the manuscript and notebooks.

For experiments involving random sampling, results may vary slightly between runs. The manuscript reports repeated-run statistics where applicable.

Before the archival release, this repository will be expanded to include:

- all experiment notebooks/scripts used in the manuscript;
- a version-pinned dependency file;
- standardized output directories;
- reproducibility instructions for each benchmark.

---

## Citation

The manuscript is currently under submission. If you use this code or build upon PFDNN, please cite the associated paper once the bibliographic information becomes available.

A BibTeX entry will be added here after publication.

---

## Authors

**Shaoqian Liao**  
College of Mathematics and Systems Science, Xinjiang University

**Xufeng Xiao**  
College of Mathematics and Systems Science, Xinjiang University 
Corresponding author: xiaoxufeng111@sina.com

---

## Code Availability

The source code and implementation associated with this work are publicly available at:

https://github.com/Aloune404/PFDNN

---

## Acknowledgements

This work was supported by the National Natural Science Foundation of China (Grant No. 12361090), the Tianshan Talent Training Program (Grant No. 2023TSYCQNTJ0015), and the Chenguang Program of Shanghai Education Development Foundation and Shanghai Municipal Education Commission (Grant No. 23CGA21).
