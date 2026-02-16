---
name: arc-alkali-rydberg-calculator-requirements
description: This skill should be used for ARC environment requirements, dependency pinning, and install-time compatibility checks.
---

# ARC-Alkali-Rydberg-Calculator: Requirements

## High-Signal Playbook
### Route conditions
- Use this for dependency/version constraints, environment bootstrapping, and compatibility triage.
- Route detailed install methods/toolchain edge cases to `arc-alkali-rydberg-calculator-advanced-topics` (`doc/installation.rst`).
- Route simulation/modeling correctness questions to `arc-alkali-rydberg-calculator-inputs-and-modeling`.

### Triage questions
- Runtime-only environment or full docs/dev environment?
- Pip-only install or source build path?
- Do you need C-extension acceleration (`setupc.py`) or pure-Python fallback is acceptable?
- Are you validating against package tests after install?
- Is the issue import-time, build-time, or runtime numeric behavior?

### Canonical workflow
1. Create a clean Python 3 environment (`doc/installation.rst`).
2. Install ARC from pip (`doc/installation.rst`).
3. If building docs/dev tools, install `doc/requirements.txt` pins.
4. Run import smoke tests (`test/test_import.py`).
5. Run one physics regression test (`test/test_single_atom.py` or `test/test_pairstate.py`).
6. Record versions for reproducibility.

### Minimal working example
```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install ARC-Alkali-Rydberg-Calculator
pip install -r doc/requirements.txt
python -m pytest -q test/test_import.py test/test_single_atom.py
```
Refs: `doc/installation.rst`, `doc/requirements.txt`.

### Pitfalls and fixes
- Over-installing docs toolchain for runtime-only use: `doc/requirements.txt` includes Sphinx/nbsphinx tooling.
- C extension build failures on manual compile path: prefer prebuilt binaries; if compiling, follow platform notes in `doc/installation.rst`.
- Slow Numerov fallback: `cpp_numerov=False` works but is significantly slower (`doc/installation.rst`, `arc/alkali_atom_functions.py`).
- Path with spaces can break `setupc.py` flow (`doc/installation.rst`).
- Version drift across machines: lock dependency set used for validated results.

### Convergence and validation checks
- Confirm imports across main species classes (`test/test_import.py`).
- Validate at least one Stark-map numeric regression (`test/test_single_atom.py`).
- Validate at least one pair-state regression (`test/test_pairstate.py`).
- Run citation helper once (`getCitationForARC`) to verify database/module wiring.

### Source-code entry links
- `setup.py`: packaging/install metadata
- `pyproject.toml`: project/dependency configuration
- `arc/setupc.py`: C extension build helper
- `arc/arc_c_extensions.c`: Numerov C implementation
- `arc/alkali_atom_functions.py`: `NumerovBack` and IO utilities
- `test/test_import.py`: import smoke tests
- `test/test_single_atom.py`: single-atom regression fixture
- `test/test_pairstate.py`: pair-state regression fixture

## Scope
- Handle dependency and environment questions only.
- Keep guidance reproducible and test-backed.

## Primary documentation references
- `doc/installation.rst`
- `doc/requirements.txt`

## Workflow
- Start from docs.
- Use `references/doc_map.md` and `references/source_map.md` for file-level specifics.
- Cite exact file paths.
