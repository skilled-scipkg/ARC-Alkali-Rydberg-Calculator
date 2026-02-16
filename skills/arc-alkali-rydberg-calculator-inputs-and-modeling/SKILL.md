---
name: arc-alkali-rydberg-calculator-inputs-and-modeling
description: This skill should be used for ARC model setup decisions (state/basis definitions, field/frequency sweeps, and method selection for Stark and AC-Stark calculations).
---

# ARC-Alkali-Rydberg-Calculator: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this for choosing physics model inputs: basis construction, field/frequency sweeps, polarization, and material/atom parameterization.
- Route first-run onboarding to `arc-alkali-rydberg-calculator-getting-started`.
- Route low-level API symbol lookup to `arc-alkali-rydberg-calculator-api-and-scripting`.
- Route narrow calculators (`StarkMap`, `DynamicPolarizability`, `OpticalLattice1D`, etc.) to `arc-alkali-rydberg-calculator-advanced-topics` when a dedicated doc page is needed.

### Triage questions
- DC Stark or AC Stark problem?
- Alkali (`s=0.5`) or divalent (`s=0/1`) manifold?
- Required polarization `q` and selection-rule constraints?
- Basis generation mode: automatic (`nMin/nMax/maxL`) or manual `basisStates`?
- Are you operating in weak-drive regime (RWA plausible) or strong-drive regime (Shirley needed)?
- What output matters most: target shifts, transition probabilities, polarizability, or spectra?

### Canonical workflow
1. Choose atom/species and state manifold from `doc/alkali_atom_functions.rst` and `doc/index.rst`.
2. Pick method family:
   - DC Stark: `StarkMap`
   - AC Stark (Floquet): `ShirleyMethod`
   - AC Stark approximation: `RWAStarkShift`
   (`doc/calculations_atom_single.ACStarkMap.rst`, `arc/calculations_atom_single.py`)
3. Build basis with explicit `nMin/nMax/maxL` (or validated manual `basisStates`).
4. Set `q` and, if needed, `edN` (0/1/2 only) for basis filtering.
5. Sweep fields/frequencies on a coarse grid, then refine around resonant structure.
6. Validate tracked target shifts and transition probabilities before scaling up.
7. Export/save once settings are stable.

### Minimal working example
```python
from arc import Rubidium85, ShirleyMethod
import numpy as np

calc = ShirleyMethod(Rubidium85())
calc.defineBasis(56, 2, 2.5, 0.5, q=0, nMin=45, nMax=70, maxL=10)
calc.defineShirleyHamiltonian(fn=1)
calc.diagonalise(0.01, np.linspace(1.0e9, 40e9, 402))

print(calc.targetShifts.shape)
```
Refs: `doc/calculations_atom_single.ACStarkMap.rst`, `arc/calculations_atom_single.py`.

### Pitfalls and fixes
- Invalid polarization/selection setup: enforce `q in {-1,0,1}` and verify allowed transitions (`arc/calculations_atom_single.py`).
- Invalid basis filter: `edN` must be 0, 1, or 2 (`arc/calculations_atom_single.py`).
- Floquet rank misuse: `defineShirleyHamiltonian(fn)` requires `fn >= 1`.
- Overusing RWA: `RWAStarkShift` is weak-drive/single-dominant-resonance approximation (`arc/calculations_atom_single.py`).
- Divalent runs without explicit spin: pass `s=0` or `s=1`.
- Unit confusion: core solvers use V/m internally; plotting often reports V/cm (`arc/calculations_atom_single.py`, `doc/numerics_and_io.rst`).

### Convergence and validation checks
- Expand `nMin/nMax/maxL` until `targetShifts` (or target observables) stop moving materially.
- Refine both frequency and field grids near resonant features.
- Compare `fn=1` vs higher `fn` for strong-field cases.
- Cross-check RWA against Shirley on a reduced grid before using RWA for sweeps.
- Watch for target-state re-ordering warnings in Floquet solves and re-validate interpretation.

### Source-code entry links
- `arc/calculations_atom_single.py`: `StarkBasisGenerator.defineBasis`, `ShirleyMethod.defineShirleyHamiltonian`, `ShirleyMethod.diagonalise`, `RWAStarkShift.findDipoleCoupledStates`, `RWAStarkShift.makeRWA`
- `arc/calculations_atom_single.py`: `StarkMap.defineBasis`, `StarkMap.diagonalise`
- `arc/alkali_atom_functions.py`: `AlkaliAtom` methods used for energies and couplings
- `arc/materials.py`: `OpticalMaterial`, `Sapphire`, `Air`

## Scope
- Handle input/state/model setup for single-atom and AC-Stark calculations.
- Keep responses docs-first and workflow-centric.

## Primary documentation references
- `doc/index.rst`
- `doc/calculations_atom_single.ACStarkMap.rst`
- `doc/numerics_and_io.rst`
- `doc/alkali_atom_functions.rst`
- `doc/calculations_atom_single.materials.rst`

## Workflow
- Start with primary references.
- Escalate to `references/doc_map.md` and then `references/source_map.md` when needed.
- Cite exact file paths.
