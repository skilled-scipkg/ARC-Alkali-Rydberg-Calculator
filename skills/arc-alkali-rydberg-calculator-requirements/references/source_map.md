# ARC-Alkali-Rydberg-Calculator source map: Requirements

Generated from source roots:
- `arc`
- `doc`
- `test`
- project root packaging files

Use this map only after exhausting the topic docs in `references/doc_map.md`.

## Topic query tokens
- `dependencies`
- `install`
- `build`
- `c-extension`
- `compatibility`
- `smoke-test`

## Function-level and build entry points
- `setup.py` | package build/install entry point
- `pyproject.toml` | build-system and dependency metadata
- `doc/requirements.txt` | documentation/dev dependency pins
- `doc/installation.rst` | supported install pathways and platform notes
- `arc/setupc.py` | manual C-extension build helper
- `arc/arc_c_extensions.c` | C-backed Numerov implementation source
- `arc/alkali_atom_functions.py` | `NumerovBack` | fallback numeric path when C extension is unavailable
- `test/test_import.py` | `test_import_cs`, `test_import_rb`, `test_import_Sr`, `test_import_Ca`, `test_import_Yt` | cross-species import smoke checks
- `test/test_single_atom.py` | `testStark_Rb` | single-atom numeric regression check
- `test/test_pairstate.py` | `testPotassium` | pair-state numeric regression check

## Fast source navigation
- `rg -n "(install|build|requires|dependencies)" setup.py pyproject.toml doc/installation.rst doc/requirements.txt`
- `rg -n "def (NumerovBack|test_import_|testStark_Rb|testPotassium)" arc/alkali_atom_functions.py test/test_import.py test/test_single_atom.py test/test_pairstate.py`

## Environment-check commands
```bash
python -m pip install -e .
python -m pytest -q test/test_import.py
python -m pytest -q test/test_single_atom.py::testStark_Rb test/test_pairstate.py::testPotassium
```

## Validation checkpoints
- Editable install/import succeeds without missing runtime dependencies.
- Import smoke tests pass across alkali and divalent species classes.
- At least one single-atom and one pair-state numeric regression pass in the target environment.
- If C-extension compilation is skipped or fails, fallback `NumerovBack` path remains functional.
