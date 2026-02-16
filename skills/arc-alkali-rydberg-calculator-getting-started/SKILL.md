---
name: arc-alkali-rydberg-calculator-getting-started
description: This skill should be used for first-run ARC workflows (quickstart notebooks, first simulations, and FAQ-level troubleshooting) before deep dives into specialized topic skills.
---

# ARC-Alkali-Rydberg-Calculator: Getting Started

## High-Signal Playbook
### Route conditions
- Use this for first local run flow, notebook entry points, and FAQ-style "how do I run this" questions.
- Route installation/build toolchain issues to `arc-alkali-rydberg-calculator-advanced-topics` (`doc/installation.rst`).
- Route detailed basis/modeling choices to `arc-alkali-rydberg-calculator-inputs-and-modeling`.
- Route symbol-level API lookup to `arc-alkali-rydberg-calculator-api-and-scripting`.

### Triage questions
- Are you starting in notebooks or writing a direct Python script? (`doc/getting_started.rst`)
- Are you modeling alkali (`s=0.5`) or divalent atoms (`s=0/1` must be explicit in many calls)?
- Is the task single-atom Stark physics, pair-state interactions, or both?
- Do you need quick plots, persistent checkpoints, or exported CSV for external analysis?
- Do you need progress logs because runtime is unclear?
- Do you need citation output for a publication workflow?

### Canonical workflow
1. Pick one starter notebook path from `doc/getting_started.rst` (`Rydberg_atoms_a_primer_notebook`, `ARC_3_0_introduction`, `AC_Stark_primer`).
2. Instantiate atom and calculator class (`StarkMap` or `PairStateInteractions`) per `doc/calculations_atom_single.rst` / `doc/calculations_atom_pairstate.rst`.
3. Build a bounded basis first (`defineBasis` with moderate `nMin/nMax/maxL`).
4. Diagonalize on a finite sweep grid and inspect state tracking.
5. Enable `progressOutput=True` (or `debugOutput=True`) for long jobs (`doc/getting_started.rst` FAQ).
6. Extract target observables (`getPolarizability`, `getC6perturbatively`, etc.).
7. Persist with `exportData` or checkpoint with `saveCalculation` / `loadSavedCalculation` (`doc/getting_started.rst`, `doc/numerics_and_io.rst`).
8. Print citations at script end via `getCitationForARC()` (`doc/index.rst`, `doc/numerics_and_io.rst`).

### Minimal working example
```python
from arc import StarkMap, Rubidium
import numpy as np

calc = StarkMap(Rubidium())
n = 75
calc.defineBasis(n, 0, 0.5, 0.5, n - 5, n + 5, 20, progressOutput=True)
calc.diagonalise(np.linspace(0.003e2, 0.2e2, 100), progressOutput=True)

alpha = calc.getPolarizability(minStateContribution=0.9)
print(f"alpha = {alpha:.3f} MHz cm^2 / V^2")
calc.exportData("rb75_stark")
```
Refs: `doc/getting_started.rst`, `test/test_single_atom.py`, `arc/calculations_atom_single.py`.

### Pitfalls and fixes
- No visible progress: set `progressOutput=True` and optionally `debugOutput=True` (`doc/getting_started.rst`).
- Divalent spin errors: pass explicit `s=0` or `s=1` in relevant calls (`arc/calculations_atom_single.py`, `arc/calculations_atom_pairstate.py`).
- `getPolarizability` fails after driven highlighting: run `diagonalise` without `drivingFromState` when extracting static polarizability (`arc/calculations_atom_single.py`).
- Lost intermediate work: use `saveCalculation`/`loadSavedCalculation` checkpoints (`doc/getting_started.rst`, `arc/alkali_atom_functions.py`).
- Export expectation mismatch: `StarkMap.exportData` writes three CSV files (`_eField`, `_energyLevels`, `_highlight`) (`arc/calculations_atom_single.py`).
- Perturbative C6 near strong mixing: switch to full basis diagonalization workflow (`arc/calculations_atom_pairstate.py`).

### Convergence and validation checks
- Increase `nMin/nMax/maxL` until target observables stabilize.
- Refine electric-field grid and verify smooth eigenvalue trajectories.
- Check tracked-state overlap thresholds (`minStateContribution`) before fitting polarizability.
- For pair-state work, cross-check perturbative C6 against level-diagram trends away from resonant mixing.
- Re-run known smoke/regression cases in `test/test_single_atom.py` and `test/test_pairstate.py`.

### Source-code entry links
- `arc/calculations_atom_single.py`: `StarkMap.defineBasis`, `StarkMap.diagonalise`, `StarkMap.getPolarizability`, `StarkMap.exportData`
- `arc/calculations_atom_pairstate.py`: `PairStateInteractions.defineBasis`, `PairStateInteractions.diagonalise`, `PairStateInteractions.getC6perturbatively`
- `arc/alkali_atom_functions.py`: `saveCalculation`, `loadSavedCalculation`
- `arc/_database.py`: `getCitationForARC`

## Scope
- Handle first-week usage questions and initial simulation workflows.
- Keep answers concise and docs-first, then escalate to source only when needed.

## Primary documentation references
- `doc/getting_started.rst`
- `doc/divalent_atom_functions.rst`
- `doc/calculations_atom_pairstate.rst`
- `doc/advanced.rst`
- `doc/calculations_atom_single.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md`.
- If docs are still ambiguous, inspect `references/source_map.md`.
- Cite exact doc/source file paths in responses.
