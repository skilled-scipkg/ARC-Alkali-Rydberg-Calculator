---
name: arc-alkali-rydberg-calculator-advanced-topics
description: This skill should be used for lower-frequency, one-doc ARC topics consolidated into a single advanced router (install, developer workflow, specialized calculators, and narrow data/theory pages).
---

# ARC-Alkali-Rydberg-Calculator: Advanced Topics

## High-Signal Playbook
### Route conditions
- Use this for narrow, page-specific topics that do not need full workflow routing from the core skills.
- Route first simulation setup to `arc-alkali-rydberg-calculator-getting-started`.
- Route basis/sweep/method decisions to `arc-alkali-rydberg-calculator-inputs-and-modeling`.
- Route API composition questions to `arc-alkali-rydberg-calculator-api-and-scripting`.

### Route list (collapsed one-doc topics)
- Build and install: `doc/installation.rst`
- Developer contribution guide: `doc/contribute.rst`
- Alkali atom data tables/models: `doc/alkali_atom_data.rst`
- Angular algebra helpers: `doc/angular_algebra.rst`
- Level plotting and analysis: `doc/calculations_atom_single.LevelPlot.rst`
- Single-atom Stark map page: `doc/calculations_atom_single.StarkMap.rst`
- Atom-surface van der Waals page: `doc/calculations_atom_single.AtomSurfaceVdW.rst`
- Dynamic polarizability page: `doc/calculations_atom_single.DynamicPolarizability.rst`
- Optical lattice page: `doc/calculations_atom_single.OpticalLattice1D.rst`
- Pair-state interactions page: `doc/calculations_atom_pairstate.PairStateInteractions.rst`
- Pair-state Stark resonances page: `doc/calculations_atom_pairstate.StarkMapResonances.rst`

### Canonical workflow
1. Start from the exact page in the route list.
2. Build the smallest reproducible script for that advanced calculator/class.
3. Use function-level entry points in `references/source_map.md` to confirm behavior.
4. Run one targeted regression/smoke command before scaling parameter sweeps.

### Simulation-start command checkpoints
```bash
python -m pytest -q test/test_single_atom.py::testStark_Rb
python -m pytest -q test/test_pairstate.py::testPotassium
```

### Minimal advanced example
```python
from arc import DynamicPolarizability, Rubidium85

calc = DynamicPolarizability(Rubidium85(), 56, 0, 0.5)
calc.defineBasis(45, 70)
alpha0, *_ = calc.getPolarizability(1064e-9, units="SI")
print(alpha0)
```
Refs: `doc/calculations_atom_single.DynamicPolarizability.rst`, `arc/calculations_atom_single.py`.

### Validation checkpoints
- Calculator inputs are explicit (`n/l/j`, spin, basis limits, grid bounds).
- Results change smoothly under small basis/grid refinements.
- For resonance/interaction workflows, trends stay consistent with known smoke tests.
- Install/build troubleshooting uses `doc/installation.rst` plus `setup.py`/`pyproject.toml` rather than ad hoc guesses.

### Function-level source entry links
- `arc/calculations_atom_single.py`: `LevelPlot.makeLevels`, `LevelPlot.drawLevels`, `StarkMap.defineBasis`, `StarkMap.diagonalise`, `AtomSurfaceVdW.getStateC3`, `OpticalLattice1D.defineBasis`, `DynamicPolarizability.getPolarizability`
- `arc/calculations_atom_pairstate.py`: `PairStateInteractions.defineBasis`, `PairStateInteractions.diagonalise`, `StarkMapResonances.findResonances`
- `arc/alkali_atom_data.py`: alkali species classes and dataset wiring
- `arc/wigner.py`: `Wigner3j`, `Wigner6j`, `CG`
- `setup.py`, `pyproject.toml`, `arc/setupc.py`: build/install entry points

## Scope
- Handle narrow advanced-topic pages and their source-level follow-up.
- Keep guidance concise, reproducible, and test-anchored.

## Documentation-first workflow
- Start with the exact doc page mapped above.
- If details are missing, inspect `references/doc_map.md` for grouped inventory.
- If behavior is still ambiguous, inspect `references/source_map.md` and jump to concrete implementation symbols.
- Route back to core skills when the request broadens.
