# ARC-Alkali-Rydberg-Calculator source map: Getting Started

Generated from source roots:
- `arc`
- `test`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `quickstart`
- `first-run`
- `starkmap`
- `pairstate`
- `checkpoint`
- `citation`

## Function-level source entry points
- `arc/calculations_atom_single.py` | `StarkMap.defineBasis`, `StarkMap.diagonalise`, `StarkMap.getPolarizability`, `StarkMap.exportData` | first single-atom simulation flow
- `arc/calculations_atom_pairstate.py` | `PairStateInteractions.__init__`, `PairStateInteractions.getC6perturbatively`, `PairStateInteractions.defineBasis`, `PairStateInteractions.diagonalise` | first pair-state simulation flow
- `arc/alkali_atom_functions.py` | `saveCalculation`, `loadSavedCalculation` | checkpointing long runs
- `arc/_database.py` | `getCitationForARC` | citation output for reproducible reports
- `test/test_single_atom.py` | `testStark_Rb` | numeric regression reference for Stark-map startup
- `test/test_pairstate.py` | `testPotassium` | numeric regression reference for pair-state startup

## Fast source navigation
- `rg -n "StarkMap|PairStateInteractions|saveCalculation|getCitationForARC" arc test`
- `rg -n "def (defineBasis|diagonalise|getPolarizability|getC6perturbatively|exportData)" arc/calculations_atom_single.py arc/calculations_atom_pairstate.py`

## Simulation-start commands
```bash
python -m pytest -q test/test_import.py
python -m pytest -q test/test_single_atom.py::testStark_Rb
python -m pytest -q test/test_pairstate.py::testPotassium
```

## Validation checkpoints
- `testStark_Rb` remains close to the expected polarizability target (~`8.532e02` in test assertion units).
- `testPotassium` remains close to the expected perturbative C6 target (~`-265` in test assertion units).
- `StarkMap.exportData("<base>")` produces `_eField.csv`, `_energyLevels.csv`, and `_highlight.csv`.
- `saveCalculation` then `loadSavedCalculation` returns an object with matching sweep dimensions.
