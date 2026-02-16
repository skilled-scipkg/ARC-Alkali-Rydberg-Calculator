---
name: arc-alkali-rydberg-calculator-index
description: This skill should be used when users ask how to use ARC-Alkali-Rydberg-Calculator and the correct generated documentation skill must be selected before going deeper into source code.
---

# ARC-Alkali-Rydberg-Calculator Skills Index

## Route the request
- Classify the request into one of the topic skills below.
- Prefer docs-first routing and only escalate to source when docs are insufficient.

## Generated topic skills
- `arc-alkali-rydberg-calculator-getting-started`: first-run workflows, notebook entry points, and FAQ-level troubleshooting
- `arc-alkali-rydberg-calculator-inputs-and-modeling`: basis/model choices, field/frequency sweeps, and method selection
- `arc-alkali-rydberg-calculator-api-and-scripting`: class/method usage and reusable Python scripting patterns
- `arc-alkali-rydberg-calculator-divalent-atom-data`: Sr/Ca/Yb datasets, spin-explicit divalent workflows, and data-model behavior
- `arc-alkali-rydberg-calculator-math-and-io-primitives`: Numerov/IO helpers, save-load checkpoints, formatting, and citation utilities
- `arc-alkali-rydberg-calculator-requirements`: dependencies, version constraints, and environment validation
- `arc-alkali-rydberg-calculator-advanced-topics`: consolidated narrow topics (build/install, developer guide, alkali/angular algebra, LevelPlot, single-atom specialized calculators, and pair-state specialized pages)

## Documentation-first inputs
- `doc`

## Tutorials and examples roots
- `doc`

## Test roots for behavior checks
- `test`

## Quick simulation bootstrap
```bash
python -m pip install -e .
python -m pytest -q test/test_import.py
python -m pytest -q test/test_single_atom.py::testStark_Rb test/test_pairstate.py::testPotassium
```

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, inspect `references/doc_map.md` in that skill.
- If ambiguity remains, inspect `references/source_map.md` and jump to the listed code entry points.
- Use targeted symbol search (for example: `rg -n "<symbol_or_keyword>" arc`).

## Source directories for deeper inspection
- `arc`
