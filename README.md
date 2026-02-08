# Assignment 02: 4D-STEM Virtual Detectors and Automated Diffraction Analysis

**MAT SCI 465 — Advanced Electron Microscopy & Diffraction**

This repository contains a complete 4D-STEM analysis workflow for a Si/SiGe heterostructure dataset. The notebook implements virtual detector reconstruction (BF/ADF), automated diffraction statistics, and publication-quality figures.

## What Are Virtual Detectors?

Virtual detectors are a post‑acquisition imaging concept in 4D‑STEM. Rather than collecting electrons with fixed physical detectors, the full diffraction pattern is recorded at every probe position. Detector geometries can then be defined in software after acquisition. This enables:

- Post‑acquisition flexibility: redefine detector shapes and sizes without re‑running the experiment
- Multiple imaging modes from one dataset: BF, ADF, and custom masks
- Reproducible analysis: detector definitions and analysis steps are encoded in code

## Pipeline Overview

### VirtualDetector Class

The `VirtualDetector` class encapsulates detector geometry, validation, and application:

- Geometry validation with auto‑clipping to reciprocal‑space bounds
- Fast vectorized application to 4D data
- Mask visualization utilities

Key methods:
- `create_mask(shape)`
- `validate_geometry(shape)`
- `apply(data_4d)`

### Diffraction Analysis Functions

The framework also includes:

- Total scattered intensity
- Center of Mass (CoM) for beam shift mapping
- Radial intensity profiles (azimuthal averages)
- Reciprocal‑space calibration utilities

### Automated Pipeline

A unified pipeline function executes the full workflow:

```python
results = run_pipeline(data_4d, detectors, calibration)
```

Returned outputs include:
- Virtual detector images
- Diffraction statistics (total intensity, CoM)
- Metadata and calibration info

## Si/SiGe Heterostructure Analysis

The dataset is a 4D‑STEM scan of a Si/SiGe heterostructure. The notebook performs:

- Metadata‑aware calibration with sensible fallbacks
- CoM‑based beam centering (global shift)
- Virtual BF and ADF reconstruction
- Line profiles across the interface
- Publication‑quality figures with scale bars

## Key Observations

- The ADF image shows the SiGe region brighter than Si due to higher‑Z scattering.
- The BF image highlights diffraction/strain contrast across the interface.
- Line profiles quantify intensity changes across the interface.

## Reciprocal‑Space Calibration Note

If camera length, detector pixel size, or accelerating voltage are not available in metadata, the notebook uses fallback values for demonstration. When available, reciprocal‑space calibration is derived from metadata and applied automatically.

## Outputs

Publication‑quality figures are saved to `outputs/`:

- `outputs/virtual_bf_image.png`
- `outputs/virtual_adf_image.png`
- `outputs/bf_adf_comparison.png`
- `outputs/interface_line_profile.png`
- `outputs/normalized_line_profiles.png`
- `outputs/radial_profile_physical_units.png`

## Requirements

- Python 3.x
- numpy
- scipy
- matplotlib
- py4DSTEM

## References

1. C. Ophus, “Four‑Dimensional Scanning Transmission Electron Microscopy (4D‑STEM): From Scanning Nanodiffraction to Ptychography and Beyond,” *Microscopy and Microanalysis* (2019).
2. py4DSTEM documentation: https://py4dstem.readthedocs.io/
