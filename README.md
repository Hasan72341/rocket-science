# Rocket Stability Analysis

A compact analysis script that evaluates trapezoidal fin geometries for a small model rocket and searches for the fin parameters that maximize a simple nondimensional stability margin.

## Project overview

This repository contains a single Python script, `rocketScience.py`, that:

- Defines small helper functions for trapezoidal fin geometry (area, mean aerodynamic chord).
- Computes a fin centre-of-pressure (CP) location from the fin root leading edge.
- Performs a grid search over root chord, tip chord, span and sweep angle to find the fin configuration that maximises a nondimensional stability margin defined as (CP - CG) / diameter.

The script is lightweight, has no external data dependencies or I/O, and is intended as a small engineering utility / demonstration rather than a full-blown simulation.

## Key objectives

- Provide utility functions for fin geometry calculations (area, mean aerodynamic chord).
- Search the design space of fin geometry for a best stability margin.
- Illustrate how sweep angle and basic fin dimensions affect a simple stability metric.

## What the script computes

- Fin area for a trapezoidal fin: area = (rootChord + tipChord) / 2 * span
- Mean Aerodynamic Chord (MAC) for a trapezoid
- CP from fin root leading edge (taken as 0.75 * MAC)
- Stability margin: (cpPosition - cgPosition) / rocketDiameter

## Inputs and parameters

- The script sets a few constants inside the file:
  - `cgFromNose = 0.15` (m) — assumed centre of gravity measured from the nose
  - `rocketDiameter = 0.05` (m)
  - `finRootPos = 0.2` (m) — axial location of the fin root leading edge from the nose

- Grid search ranges (defined with `numpy.linspace`):
  - root chord: 0.05 — 0.06 m
  - tip chord: 0.02 — 0.04 m
  - span: 0.03 — 0.04 m
  - sweep angle: 15 — 20 degrees

## Tools and libraries used

- Python (3.9+ recommended)
- numpy (used for numeric ranges and math)

## High-level workflow

1. Define geometry helper functions (fin area, MAC, CP from root LE).
2. Define constants and search ranges.
3. For each combination of root chord, tip chord, span and sweep:
   - compute sweep-induced aft shift, CP location, and nondimensional stability margin
4. Track the best stability margin and print the parameters that produced it.

## Key insights (derived from the script)

- The stability margin increases as the difference between CP and CG grows relative to the rocket diameter.
- Sweep angle shifts the CP aft; moderate increases in sweep can increase stability for the ranges explored.
- Larger root chords and spans (which increase MAC and CP location) tend to increase the stability metric in the small parameter window used here.

Note: these insights are specific to the simple analytic model used in the script and the narrow grid ranges chosen. A full aerodynamic analysis (CFD or wind-tunnel data) would be required for final design.

## How to run

1. Create a Python virtual environment (recommended):

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install -r requirements.txt
```

3. Run the script from PowerShell:

```powershell
python rocketScience.py
```

The script will print the best fin setup found for the stability metric.

## Recommended repository structure

This repository is intentionally small. A recommended layout for expanding it:

- rocketScience.py            # analysis script (present)
- README.md                   # this file
- requirements.txt            # python dependencies
- LICENSE.md                  # project license
- CONTRIBUTING.md             # contribution guidelines
- data/                       # (optional) datasets or testcases
- notebooks/                  # (optional) exploratory notebooks
- outputs/                    # (optional) generated plots, tables, logs

## Notes and limitations

- The model is a simplified analytic estimate. It assumes the CP for a trapezoidal fin is at 0.75*MAC measured from the fin root leading edge, plus a half of the sweep-induced shift. That is a simplification and should not be used as the sole design input for flight-critical hardware.
- No uncertainties, mass distribution details, thrust-line effects, or 3D aerodynamic interactions are modelled.

## Credits / author

Created from the single script `rocketScience.py` found in this repository. Feel free to open issues or pull requests to expand the project with
 - richer geometry parametric studies
 - plotting and visualisation of the search space
 - validation against higher-fidelity aerodynamic models
