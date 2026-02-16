# ARC-Alkali-Rydberg-Calculator source map: Advanced Topics

Generated from source roots:
- `arc`
- `doc`
- `test`
- project root packaging files

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `installation`
- `setupc`
- `levelplot`
- `atomsurfacevdw`
- `dynamicpolarizability`
- `opticallattice1d`
- `starkmapresonances`
- `angular`

## Function-level source entry points
- `arc/calculations_atom_single.py` | `LevelPlot.makeLevels`, `LevelPlot.drawLevels` | level-structure visualization path
- `arc/calculations_atom_single.py` | `StarkMap.defineBasis`, `StarkMap.diagonalise` | advanced Stark-map internals used by multiple pages
- `arc/calculations_atom_single.py` | `AtomSurfaceVdW.getC3contribution`, `AtomSurfaceVdW.getStateC3` | atom-surface interaction coefficients
- `arc/calculations_atom_single.py` | `OpticalLattice1D.defineBasis`, `OpticalLattice1D.diagonalise` | lattice-band calculations
- `arc/calculations_atom_single.py` | `DynamicPolarizability.defineBasis`, `DynamicPolarizability.getPolarizability` | dynamic polarizability workflow
- `arc/calculations_atom_pairstate.py` | `PairStateInteractions.defineBasis`, `PairStateInteractions.diagonalise` | pair interaction advanced basis solving
- `arc/calculations_atom_pairstate.py` | `StarkMapResonances.findResonances` | Foster-resonance search path
- `arc/alkali_atom_data.py` | species classes (`Rubidium85`, `Rubidium87`, `Caesium`, `Potassium39`, ...) | alkali dataset selection details
- `arc/wigner.py` | `Wigner3j`, `Wigner6j`, `CG` | angular algebra primitives
- `setup.py` | packaging/build entry point
- `pyproject.toml` | build-system/dependency metadata
- `arc/setupc.py` | C-extension build helper

## Fast source navigation
- `rg -n "class (LevelPlot|AtomSurfaceVdW|OpticalLattice1D|DynamicPolarizability|StarkMapResonances)" arc`
- `rg -n "def (makeLevels|drawLevels|getC3contribution|getStateC3|findResonances|Wigner3j|Wigner6j|CG)" arc`

## Simulation-start commands
```bash
python -m pytest -q test/test_single_atom.py::testStark_Rb test/test_pairstate.py::testPotassium
```

## Validation checkpoints
- Advanced calculators are exercised only with explicit basis bounds and converged sweep grids.
- `StarkMapResonances.findResonances` output changes smoothly with `energyRange` and electric-field grid refinement.
- Atom-surface `C3` values are reproducible for fixed material and coupled-state lists.
- Build metadata (`setup.py`, `pyproject.toml`, `arc/setupc.py`) remains aligned with the chosen install path.
