<div align="center">

# Spectral Crystallography Py
### Automated Powder XRD Voigt Profile Deconvolution & UV-Vis DRS Tauc Bandgap Regression Toolkit

[![Python: 3.10+](https://img.shields.io/badge/Python-3.10%2B-00F2FE?style=for-the-badge&labelColor=030712)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-00D2FF.svg?style=for-the-badge&labelColor=030712)](https://opensource.org/licenses/MIT)
[![Domain: Computational Materials](https://img.shields.io/badge/Domain-Crystallography%20%26%20Spectroscopy-8B5CF6?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/spectral-crystallography-py)
[![PyPI: Scientific Package](https://img.shields.io/badge/Status-Open_Source_Tool-10B981?style=for-the-badge&labelColor=030712)](https://github.com/Himani-ai/spectral-crystallography-py)

<p align="center">
  <a href="#1-overview">Overview</a> •
  <a href="#2-mathematical-formulations">Formulations</a> •
  <a href="#3-cli-and-api-usage">Usage</a> •
  <a href="#4-sample-output">Output Samples</a>
</p>

</div>

---

## 1. Overview

Materials science and nanotechnology workflows often bottleneck on manual peak selection and subjective tangent fitting for optical bandgap extrapolation.

**Spectral Crystallography Py** is an open-source, reproducible Python library for:
1. Automated background subtraction (Shirley & polynomial baselines) and pseudo-Voigt profile peak deconvolution on raw $2\theta$ XRD scans.
2. Debye-Scherrer crystallite size calculation with instrument broadening compensation ($B_{\text{sample}}^2 = B_{\text{obs}}^2 - B_{\text{inst}}^2$).
3. Automated tangent discovery and direct/indirect bandgap extraction on Kubelka-Munk diffuse reflectance curves.

---

## 2. Mathematical Formulations

### Pseudo-Voigt Profile Deconvolution
A peak $V(x)$ is modeled as a linear combination of Gaussian $G(x)$ and Lorentzian $L(x)$ distributions:

$$V(x) = \eta L(x) + (1 - \eta) G(x)$$

where $\eta \in [0, 1]$ represents the Lorentzian fraction, capturing both micro-strain and finite crystallite size effects.

### Kubelka-Munk & Tauc Fitting
From diffuse reflectance $R_\infty$:

$$F(R_\infty) = \frac{(1 - R_\infty)^2}{2 R_\infty}$$

The maximum first-derivative tangent on $[F(R_\infty) \cdot h\nu]^n$ is computed via numerical differentiation to yield $E_g$ at the energy axis intercept.

---

## 3. CLI and API Usage

```bash
# Install library
pip install git+https://github.com/Himani-ai/spectral-crystallography-py.git

# Execute automated XRD deconvolution
spectrocryst xrd --input data/raw_xrd.csv --two-theta-col 0 --intensity-col 1 --target-peak 25.3

# Execute automated Tauc bandgap calculation
spectrocryst tauc --input data/uv_vis.csv --wavelength-col 0 --reflectance-col 1 --transition direct
```

---

## 4. Sample Output

```
================ SPECTROCRYST DECONVOLUTION REPORT ================
Sample ID: TiO2_Green_Anatase
Target Reflection: (101) Anatase
Center (2-theta): 25.321°
FWHM (observed): 0.612°
FWHM (corrected): 0.584° (0.01019 rad)
Calculated Scherrer Crystallite Size: 14.32 nm
Goodness of Fit (R^2): 0.9984
Calculated Optical Bandgap (Eg): 2.852 eV (+/- 0.014 eV)
====================================================================
```
