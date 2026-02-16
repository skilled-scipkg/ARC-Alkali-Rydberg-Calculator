# ARC-Alkali-Rydberg-Calculator source map: Divalent Atom Data

Generated from source roots:
- `arc`
- `test`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `strontium`
- `calcium`
- `ytterbium`
- `spin`
- `quantum-defect`
- `nist`

## Function-level source entry points
- `arc/divalent_atom_data.py` | `Strontium88`, `Calcium40`, `Ytterbium174`, `Strontium88.getPressure`, `Calcium40.getPressure`, `Ytterbium174.getPressure` | species constants and thermodynamic models
- `arc/divalent_atom_functions.py` | `DivalentAtom.__init__`, `DivalentAtom.getEnergy` | defect vs saved-data energy behavior and spin handling
- `arc/alkali_atom_functions.py` | `AlkaliAtom.getTransitionFrequency`, `AlkaliAtom.getEnergy` | inherited transition/energy paths with explicit `s` support
- `arc/calculations_atom_single.py` | `StarkMap.defineBasis` | divalent state spin propagation into single-atom basis setup
- `arc/calculations_atom_pairstate.py` | `PairStateInteractions.__init__`, `StarkMapResonances.__init__` | divalent spin validation in pair-state paths
- `test/test_import.py` | `test_import_Sr`, `test_import_Ca`, `test_import_Yt` | species import smoke checks

## Fast source navigation
- `rg -n "class (Strontium88|Calcium40|Ytterbium174|DivalentAtom)" arc/divalent_atom_data.py arc/divalent_atom_functions.py`
- `rg -n "def (getEnergy|getTransitionFrequency|getPressure)" arc/divalent_atom_functions.py arc/divalent_atom_data.py arc/alkali_atom_functions.py`

## Simulation-start commands
```bash
python -m pytest -q test/test_import.py::test_import_Sr test/test_import.py::test_import_Ca test/test_import.py::test_import_Yt
```

## Validation checkpoints
- Divalent energy calls reject missing/invalid spin (`s`) and accept valid singlet/triplet inputs.
- Toggling `preferQuantumDefects` changes energy sourcing behavior in the documented n-ranges.
- Transition-frequency calls with explicit `s` and optional `s2` are numerically stable for nearby states.
- Pressure methods return finite values within expected operating temperature ranges.
