---
name: coffea-advanced-topics
description: This skill should be used for consolidated one-doc coffea topics (getting started, installation, examples, and corrections-focused analysis notes) before escalating to deeper workflow or API skills.
---

# coffea: Advanced Topics

## Scope
- Consolidated router for previously narrow one-doc topics.
- Covers quick-start orientation, installation/environment notes, notebook/example discovery, and corrections-centric analysis notes.

## Route the request
- Installation/platform/env setup -> this skill, then `docs/source/getting_started/installation.md`.
- First local workflow bootstrap -> this skill, then `docs/source/getting_started/index.md`.
- Notebook/tutorial navigation -> this skill, then `docs/source/examples.md` plus `binder` notebooks.
- Corrections-first usage questions -> this skill, then `docs/source/user_guide/corrections.md`.
- If the request expands into full execution design -> route to `coffea-simulation-workflows`.
- If the request needs exact APIs/schemas -> route to `coffea-api-and-scripting`.

## Primary documentation references
- `docs/source/getting_started/index.md`
- `docs/source/getting_started/installation.md`
- `docs/source/examples.md`
- `docs/source/user_guide/corrections.md`

## Workflow
- Start from the primary docs above.
- Use notebook paths under `docs/source/notebooks` and `binder` for runnable examples.
- If details are missing, inspect `references/doc_map.md` for the full grouped inventory.
- Escalate to source only after docs are exhausted; use `references/source_map.md`.
- Cite exact documentation file paths in responses.

## Quick startup checks
```bash
pytest tests/test_local_executors.py -k nanoevents_analysis -q
pytest tests/test_lookup_tools.py -k correctionlib -q
```
Inputs: `tests/samples/nano_dy.root`, `tests/samples/testSF2d.corr.json.gz`  
Checkpoint: local execution and correctionlib integration both pass.

## Tutorials and examples
- `binder`
- `docs/source/notebooks`

## Test references
- `tests`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/coffea/analysis_tools.py` (analysis helpers used by correction/example workflows)
- `src/coffea/lookup_tools/correctionlib_wrapper.py` (correctionlib adapter behavior)
- `src/coffea/lookup_tools/extractor.py` (payload extraction patterns)
- `src/coffea/processor/executor.py` (runner path referenced by quickstart docs)
- `src/coffea/nanoevents/factory.py` (event materialization path used in examples)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src`).
