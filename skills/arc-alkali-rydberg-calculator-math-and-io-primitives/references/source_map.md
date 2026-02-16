# ARC-Alkali-Rydberg-Calculator source map: Math and IO Primitives

Generated from source roots:
- `arc`
- `test`

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `numerov`
- `checkpoint`
- `save/load`
- `format`
- `citation`
- `web-plot`

## Function-level source entry points
- `arc/alkali_atom_functions.py` | `NumerovBack` | pure-Python Numerov implementation path
- `arc/alkali_atom_functions.py` | `saveCalculation`, `loadSavedCalculation` | checkpoint persistence and restoration logic
- `arc/alkali_atom_functions.py` | `printStateString`, `printStateStringLatex`, `formatNumberSI` | state/number formatting helpers
- `arc/_database.py` | `getCitationForARC` | citation output used for publications/reports
- `arc/web_functionality.py` | `htmlLiteratureOutput`, `plotStarkMap`, `plotInteractionLevels` | HTML and plot output helpers
- `arc/wigner.py` | `Wigner3j`, `Wigner6j`, `CG` | angular algebra primitives backing higher-level methods
- `test/test_single_atom.py` | `testStark_Rb` | quick smoke baseline before/after IO workflow changes

## Fast source navigation
- `rg -n "def (NumerovBack|saveCalculation|loadSavedCalculation|printStateString|formatNumberSI)" arc/alkali_atom_functions.py`
- `rg -n "def (getCitationForARC|plotStarkMap|plotInteractionLevels|htmlLiteratureOutput|Wigner3j|Wigner6j|CG)" arc/_database.py arc/web_functionality.py arc/wigner.py`

## Simulation-start commands
```bash
python -m pytest -q test/test_single_atom.py::testStark_Rb
```

## Validation checkpoints
- `saveCalculation` + `loadSavedCalculation` round-trip keeps key arrays and solver metadata usable.
- Formatting helpers are used only for presentation, not fed back into numeric pipelines.
- Citation string includes modules actually exercised in the script.
- Web plotting helpers receive calculators that already ran diagonalization.
