---
name: arc-alkali-rydberg-calculator-api-and-scripting
description: This skill should be used for programmatic ARC usage patterns (class/method selection, scripting flows, and reusable API-level snippets).
---

# ARC-Alkali-Rydberg-Calculator: API and Scripting

## High-Signal Playbook
### Route conditions
- Use this for class/method-level scripting questions and how to compose ARC APIs in Python.
- Route physics modeling choices (basis size, approximation validity) to `arc-alkali-rydberg-calculator-inputs-and-modeling`.
- Route first-run workflow questions to `arc-alkali-rydberg-calculator-getting-started`.
- Route installation/toolchain issues to `arc-alkali-rydberg-calculator-advanced-topics`.

### Triage questions
- Which object family are you scripting: `Wavefunction`, `StarkMap`, or `PairStateInteractions`?
- Is the script for one-off interactive plotting or batch parameter sweeps?
- Do you need saved checkpoints (`saveCalculation`) or plain exported CSV?
- Do you need HTML/web helper output or matplotlib output?
- Are you using alkali defaults or divalent spin-explicit APIs?
- Do you need a stable regression-style output for CI/test comparison?

### Canonical workflow
1. Start from module routing in `doc/detailed_doc.rst`.
2. Pick the minimal class and method chain for the task.
3. Build a short script with explicit basis/state inputs and sweep arrays.
4. Add persistence (`saveCalculation` / `loadSavedCalculation`) if runtime is nontrivial.
5. Export data for downstream plotting/analysis.
6. Lock a quick regression check (shape, key scalar, or known test value).

### Minimal working examples
```python
from arc import Wavefunction, Rubidium85

# Single-basis-state wavefunction example
wf = Wavefunction(Rubidium85(), [[30, 0, 0.5, 0.5]], [1.0])
fig = wf.plot2D(plane="x-z", pointsPerAxis=120, units="nm")
fig.savefig("wf_30s.png")
```

```python
from arc import StarkMap, Rubidium, saveCalculation, loadSavedCalculation
import numpy as np

calc = StarkMap(Rubidium())
calc.defineBasis(40, 0, 0.5, 0.5, 35, 45, 20)
calc.diagonalise(np.linspace(0.0, 5.0e3, 60))
saveCalculation(calc, "stark_checkpoint.pkl.gz")
restored = loadSavedCalculation("stark_checkpoint.pkl.gz")
print(type(restored).__name__)
```
Refs: `doc/calculations_atom_single.Wavefunction.rst`, `doc/detailed_doc.rst`, `arc/calculations_atom_single.py`, `arc/alkali_atom_functions.py`.

### Pitfalls and fixes
- `Wavefunction` initialization misuse: pass `basisStates` + matching `coefficients`, not just `(n,l,j,mj)` (`arc/calculations_atom_single.py`).
- Serialization expectations: `saveCalculation` does not persist plotted figure handles; re-run plotting after load (`arc/alkali_atom_functions.py`).
- Hidden API coupling assumptions: verify units and sweep dimensions before interpreting outputs.
- Web plotting helpers expect precomputed calculator attributes (`arc/web_functionality.py`).
- Divalent scripts require explicit spin arguments where applicable (`arc/divalent_atom_functions.py`).

### Convergence and validation checks
- Assert expected output tensor shapes after diagonalization.
- Compare one known scalar against a test reference (`test/test_single_atom.py`, `test/test_pairstate.py`).
- Verify checkpoint round-trip produces matching key results.
- Confirm exported CSV row/column counts match sweep definitions.

### Source-code entry links
- `arc/calculations_atom_single.py`: `Wavefunction`, `StarkMap`, `ShirleyMethod`, `RWAStarkShift`
- `arc/calculations_atom_pairstate.py`: `PairStateInteractions`, `StarkMapResonances`
- `arc/alkali_atom_functions.py`: `saveCalculation`, `loadSavedCalculation`, `printStateString`, `formatNumberSI`
- `arc/web_functionality.py`: `plotStarkMap`, `plotInteractionLevels`, `rabiFrequencyWidget`
- `arc/__init__.py`: exported API surface

## Scope
- Handle programmatic interface usage and scripting composition.
- Prefer concise, reproducible snippets backed by docs/source.

## Primary documentation references
- `doc/calculations_atom_single.Wavefunction.rst`
- `doc/detailed_doc.rst`

## Workflow
- Start docs-first.
- Use `references/doc_map.md` and then `references/source_map.md` for unresolved details.
- Cite exact file paths.
