# ARC-Alkali-Rydberg-Calculator source map: Inputs and Modeling

Generated from source roots:
- `arc`
- `test`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `defineBasis`
- `shirley`
- `rwa`
- `ac-stark`
- `polarization`
- `materials`

## Function-level source entry points
- `arc/calculations_atom_single.py` | `StarkBasisGenerator.defineBasis` | validates `nMin/nMax/maxL`, `q`, and `edN` input constraints
- `arc/calculations_atom_single.py` | `ShirleyMethod.defineShirleyHamiltonian`, `ShirleyMethod.diagonalise` | Floquet AC-Stark setup and diagonalization path
- `arc/calculations_atom_single.py` | `RWAStarkShift.findDipoleCoupledStates`, `RWAStarkShift.makeRWA` | weak-drive approximation behavior
- `arc/calculations_atom_single.py` | `StarkMap.defineBasis`, `StarkMap.diagonalise`, `StarkMap.getPolarizability` | DC Stark-map modeling baseline
- `arc/materials.py` | `Air.getN`, `Sapphire.getN` | refractive-index inputs used by optical models
- `arc/alkali_atom_functions.py` | `AlkaliAtom.getEnergy`, `AlkaliAtom.getTransitionFrequency` | atomic energy/frequency primitives used by solvers
- `test/test_single_atom.py` | `testStark_Rb` | baseline convergence-sensitive check

## Fast source navigation
- `rg -n "class (StarkBasisGenerator|ShirleyMethod|RWAStarkShift|StarkMap)" arc/calculations_atom_single.py`
- `rg -n "def (defineBasis|defineShirleyHamiltonian|diagonalise|makeRWA|getPolarizability)" arc/calculations_atom_single.py`
- `rg -n "def getN" arc/materials.py`

## Simulation-start commands
```bash
python -m pytest -q test/test_single_atom.py::testStark_Rb
```

## Validation checkpoints
- Increasing basis bounds (`nMin/nMax/maxL`) causes target observables to converge, not drift.
- `q` polarization stays within allowed values (`-1`, `0`, `1`) and changes coupling structure as expected.
- For strong-drive cases, Floquet (`ShirleyMethod`) and RWA results differ in a physically plausible way; for weak drive they are close.
- Material-dependent runs fail fast with a clear error when wavelength is outside supported refractive-index ranges.
