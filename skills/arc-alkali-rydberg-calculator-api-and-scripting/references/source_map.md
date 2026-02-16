# ARC-Alkali-Rydberg-Calculator source map: API and Scripting

Generated from source roots:
- `arc`
- `test`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `wavefunction`
- `starkmap`
- `pairstate`
- `checkpoint`
- `plot`
- `api`

## Function-level source entry points
- `arc/calculations_atom_single.py` | `Wavefunction.__init__`, `Wavefunction.plot2D`, `Wavefunction.plot3D` | wavefunction construction and plotting API
- `arc/calculations_atom_single.py` | `StarkMap.defineBasis`, `StarkMap.diagonalise`, `StarkMap.exportData` | single-atom scripted sweep pattern
- `arc/calculations_atom_pairstate.py` | `PairStateInteractions.__init__`, `PairStateInteractions.getC6perturbatively`, `PairStateInteractions.exportData` | pair-state scripted analysis and export
- `arc/alkali_atom_functions.py` | `saveCalculation`, `loadSavedCalculation`, `printStateString`, `formatNumberSI` | reusable IO and formatting helpers
- `arc/web_functionality.py` | `plotStarkMap`, `plotInteractionLevels`, `rabiFrequencyWidget` | web-oriented plotting paths
- `arc/__init__.py` | package export surface used in user scripts
- `test/test_single_atom.py` | `testStark_Rb` | scripted Stark flow regression anchor
- `test/test_pairstate.py` | `testPotassium` | scripted pair-state flow regression anchor

## Fast source navigation
- `rg -n "class (Wavefunction|StarkMap|PairStateInteractions)" arc/calculations_atom_single.py arc/calculations_atom_pairstate.py`
- `rg -n "def (plot2D|plot3D|exportData|saveCalculation|loadSavedCalculation|formatNumberSI)" arc`

## Simulation-start commands
```bash
python -m pytest -q test/test_single_atom.py::testStark_Rb test/test_pairstate.py::testPotassium
```

## Validation checkpoints
- Scripted class construction succeeds directly from `from arc import ...` imports.
- Checkpoint save/load preserves array dimensions and required calculator attributes.
- Export helpers emit expected CSV groups for the selected calculator.
- Plot helpers are called only after solver state is populated (no empty-highlight failures).
