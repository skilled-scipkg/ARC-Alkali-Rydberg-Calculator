---
name: arc-alkali-rydberg-calculator-math-and-io-primitives
description: This skill should be used for ARC numeric primitives and IO helpers (Numerov, save/load checkpoints, state formatting, and citation/output utilities).
---

# ARC-Alkali-Rydberg-Calculator: Math and IO Primitives

## High-Signal Playbook
### Route conditions
- Use this for serialization, numerical helper usage, state-label formatting, and citation/output utility flows.
- Route full physics model setup to `arc-alkali-rydberg-calculator-inputs-and-modeling`.
- Route low-frequency specialized topics (angular algebra deep dive, LevelPlot class specifics) to `arc-alkali-rydberg-calculator-advanced-topics`.

### Triage questions
- Do you need checkpointing (`saveCalculation`/`loadSavedCalculation`) or raw CSV export?
- Do you need pure-Python Numerov (`NumerovBack`) or C-accelerated path?
- Do you need state-label formatting for logs/plots/reports?
- Are you preparing publication citations from used modules?
- Is output for local matplotlib or web/HTML embedding?

### Canonical workflow
1. Run calculator (`StarkMap`/`PairStateInteractions`) to generate data.
2. Save checkpoint before expensive parameter sweeps.
3. Reload and continue from checkpoint for iterative analysis.
4. Use formatting helpers for report-ready values and state labels.
5. Emit citations at script end.
6. Use web plotting utilities only when web output is explicitly needed.

### Minimal working example
```python
from arc import (
    StarkMap,
    Rubidium,
    saveCalculation,
    loadSavedCalculation,
    printStateString,
    formatNumberSI,
)
import numpy as np

calc = StarkMap(Rubidium())
calc.defineBasis(40, 0, 0.5, 0.5, 35, 45, 20)
calc.diagonalise(np.linspace(0.0, 4.0e3, 40))
saveCalculation(calc, "checkpoint.pkl.gz")
restored = loadSavedCalculation("checkpoint.pkl.gz")
print(printStateString(40, 0, 0.5), formatNumberSI(restored.getPolarizability()))
```
Refs: `doc/math_and_io_primitives.rst`, `doc/numerics_and_io.rst`, `arc/alkali_atom_functions.py`.

### Pitfalls and fixes
- Assuming plots persist in checkpoint files: plot handles are intentionally reset in save/load flow (`arc/alkali_atom_functions.py`).
- Using pure-Python Numerov for heavy runs: prefer compiled path when available (`doc/installation.rst`, `arc/alkali_atom_functions.py`).
- Mixing formatted strings with numeric pipelines: keep `formatNumberSI` output for display only.
- Web plot helper misuse: `plotStarkMap` / `plotInteractionLevels` require fully populated calculator state (`arc/web_functionality.py`).
- Missing citation output in scripts: call `getCitationForARC()` explicitly (`doc/index.rst`, `arc/_database.py`).

### Convergence and validation checks
- Verify checkpoint round-trip by comparing one scalar observable before/after load.
- Confirm exported arrays and checkpointed arrays have expected sweep lengths.
- Compare `NumerovBack`-based outputs with C-backed defaults on a small benchmark case.
- Keep a small regression script aligned with `test/test_single_atom.py` or `test/test_pairstate.py`.

### Source-code entry links
- `arc/alkali_atom_functions.py`: `NumerovBack`, `saveCalculation`, `loadSavedCalculation`, `printStateString`, `formatNumberSI`
- `arc/_database.py`: `getCitationForARC`
- `arc/web_functionality.py`: `htmlLiteratureOutput`, `plotStarkMap`, `plotInteractionLevels`
- `arc/wigner.py`: angular algebra helpers used by higher-level solvers

## Scope
- Handle numeric helper and IO utility usage.
- Keep responses practical and workflow-first.

## Primary documentation references
- `doc/math_and_io_primitives.rst`
- `doc/numerics_and_io.rst`
- `doc/angular_algebra.rst`

## Workflow
- Start docs-first, then inspect source via `references/source_map.md` when behavior details are needed.
- Cite exact file paths.
