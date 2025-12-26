# Santa 2025 — Christmas Tree Packing Challenge

## Overview
This repository contains a baseline solution for the Santa 2025 Christmas Tree Packing Challenge. The objective is to determine the smallest possible square box that can contain a given number of identical 2D Christmas tree toys for every n from 1 to 200.

The notebook `Christmas Tree Packing Challenge.ipynb` contains:
- A baseline greedy algorithm used to construct `sample_submission.csv`.
- Utilities for collision detection, visualization, and submission formatting.

See [Christmas Tree Packing Challenge.ipynb](Christmas%20Tree%20Packing%20Challenge.ipynb) for the full implementation and examples.

## Baseline Algorithm (Summary)
The provided baseline implements a greedy, incremental placement strategy:
- Start with the existing configuration for n trees and add one tree for n+1.
- For each new tree, perform multiple randomized placement attempts using a weighted angle distribution that prefers corner directions (peaks at 45°, 135°, 225°, 315°).
- For each attempt, perform a radial search from distance 20 toward the center in coarse steps until collision, then back off in finer steps to a collision-free position.
- Use Shapely's `STRtree` spatial index to speed up collision queries.
- Repeat the process up to n=200 and save a formatted submission CSV.

Key implementation details:
- Each tree is represented as a Shapely `Polygon` (16 vertices) with a fixed shape and possible rotation.
- Coordinates are scaled by 1e15 and Python's `Decimal` is used for numeric stability.
- The bounding square for each configuration is computed from the union of all polygons.

## File Structure
- `Christmas Tree Packing Challenge.ipynb` — Main notebook with explanations, code, and visualizations.
- `sample_submission.csv` — Output produced by running the notebook end-to-end (created by the baseline script).
- `README.md` — This file.

## Dependencies
The notebook requires the following Python packages (tested with recent versions):
- shapely
- pandas
- numpy
- matplotlib

To install dependencies (recommended in a virtual environment):

```bash
python -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install shapely pandas numpy matplotlib
```

If you use conda:

```bash
conda create -n santa2025 python=3.10 -y
conda activate santa2025
pip install shapely pandas numpy matplotlib
```

## How to run
1. Open the notebook in Jupyter or VS Code and run cells sequentially.
2. Running the main execution cell will:
   - Build configurations for n=1..200 using the greedy algorithm
   - Plot intermediate results (by default every 10 configurations)
   - Save `sample_submission.csv` in the working directory

You can programmatically run the notebook (for example) using `nbconvert`:

```bash
# convert to script and run (optional)
jupyter nbconvert --to script "Christmas Tree Packing Challenge.ipynb"
python "Christmas Tree Packing Challenge.py"
```

Note: Running the full sequence may take time depending on your machine (collision checks and plotting are the most expensive parts). Consider running a subset (e.g., n up to 50) for faster iteration.

## Suggested Improvements (next steps)
- Replace purely greedy single-tree additions with local optimization (simulated annealing or basin-hopping) operating on full n-tree configurations.
- Implement rotation-aware packing trials (optimize angle per tree rather than sampling uniformly at initialization).
- Use more advanced spatial partitioning or caching to speed up repeated collision queries.
- Explore metaheuristics (genetic algorithms, particle swarm) to jointly optimize placement for small-to-medium n.
- Add unit tests for geometry utilities and collision detection.

## Notes
- The baseline is intentionally simple to be easy to understand and extend.
- The notebook contains detailed inline comments explaining each function and algorithmic decision.

## Contact
If you want help improving the algorithm or running experiments, tell me what you'd like to prioritize (speed, score improvement, visualization), and I can implement the next steps.
