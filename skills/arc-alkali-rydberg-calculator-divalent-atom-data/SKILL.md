---
name: arc-alkali-rydberg-calculator-divalent-atom-data
description: This skill should be used for divalent species data/model handling (Sr/Ca/Yb datasets, spin-explicit calls, and quantum-defect vs NIST behavior).
---

# ARC-Alkali-Rydberg-Calculator: Divalent Atom Data

## High-Signal Playbook
### Route conditions
- Use this for Sr/Ca/Yb data model questions, spin-state handling, and divalent-specific energy/transition calculations.
- Route generic first-run simulation workflow to `arc-alkali-rydberg-calculator-getting-started`.
- Route basis/sweep design to `arc-alkali-rydberg-calculator-inputs-and-modeling`.
- Route alkali-data-only questions to `arc-alkali-rydberg-calculator-advanced-topics` (`doc/alkali_atom_data.rst`).

### Triage questions
- Which species (`Strontium88`, `Calcium40`, or `Ytterbium174`)?
- Singlet or triplet manifold (`s=0` or `s=1`)?
- Should energy use quantum defects (`preferQuantumDefects=True`) or NIST-backed values where available?
- Are target states within documented defect fitting ranges for the chosen series?
- Do you need thermodynamic properties (pressure/density) or spectroscopic properties?
- Is this single-atom spectroscopy or pair-state interaction setup?

### Canonical workflow
1. Instantiate divalent species class from `doc/divalent_atom_data.rst`.
2. Decide `preferQuantumDefects` policy at object creation (`arc/divalent_atom_functions.py`).
3. Call energy/transition methods with explicit `s` for divalent states.
4. For pair-state or Stark calculations, propagate explicit spin choices through solver calls.
5. Check species-specific fitting ranges/limits before extrapolating high-n behavior.
6. Validate against a second setting (`preferQuantumDefects` toggle or neighboring n) when precision matters.

### Minimal working example
```python
from arc import Strontium88

sr = Strontium88(preferQuantumDefects=True)
print(sr.getEnergy(30, 0, 1, s=1))
print(sr.getTransitionFrequency(5, 0, 1, 30, 0, 1, s=1))
print(sr.getPressure(700))
```
Refs: `doc/divalent_atom_data.rst`, `doc/divalent_atom_functions.rst`, `arc/divalent_atom_data.py`, `arc/divalent_atom_functions.py`.

### Pitfalls and fixes
- Missing spin keyword for divalent energies: `getEnergy` requires explicit `s=0` or `s=1` (`arc/divalent_atom_functions.py`).
- Invalid quantum numbers: requests with `l >= n` are rejected (`arc/divalent_atom_functions.py`).
- Reusing alkali spin defaults (`s=0.5`) in divalent workflows causes validation errors in calculators.
- Extrapolating far outside `defectFittingRange` can degrade reliability; re-check with alternate settings.
- Pressure models are piecewise and temperature-limited by species (`arc/divalent_atom_data.py`).

### Convergence and validation checks
- Compare key energies with `preferQuantumDefects=True` vs `False` where NIST data exists.
- Track sensitivity of target observables to small changes in n/l/j and chosen spin manifold.
- For pair interactions, re-run with larger basis windows to ensure stable interaction coefficients.
- Confirm citation output includes ARC 3.0 divalent reference for scripts using divalent modules.

### Source-code entry links
- `arc/divalent_atom_data.py`: `Strontium88`, `Calcium40`, `Ytterbium174`, species constants, fitting ranges, pressure models
- `arc/divalent_atom_functions.py`: `DivalentAtom.__init__`, `DivalentAtom.getEnergy`, data loading, defect/NIST logic
- `arc/calculations_atom_single.py`: divalent spin handling in `defineBasis` calls
- `arc/calculations_atom_pairstate.py`: spin validation for pair-state interactions
- `arc/_database.py`: citation module flags

## Scope
- Handle divalent species data and divalent-specific API behavior.
- Keep guidance tied to explicit species/spin assumptions.

## Primary documentation references
- `doc/divalent_atom_data.rst`
- `doc/divalent_atom_functions.rst`

## Workflow
- Start docs-first.
- Escalate to `references/doc_map.md` and then `references/source_map.md` for implementation details.
- Cite exact file paths.
