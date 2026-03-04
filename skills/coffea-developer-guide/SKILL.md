---
name: coffea-developer-guide
description: This skill should be used when users ask about developer guide in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: Developer Guide

## High-Signal Playbook

### Route conditions
- Use this skill for contribution workflow, repo hygiene, test expectations, and docs update policy.
- Route user-facing analysis questions to `coffea-user-guide`.
- Route API semantics and class/function usage to `coffea-api-and-scripting`.
- Route failing test triage to `coffea-tests` when a concrete regression is under investigation.

### Triage questions
- Is this a bug fix, new feature, docs-only change, or refactor?
- What is the minimal reproducible case and affected module?
- Which tests should change or be added?
- Which docs pages must be updated to match the behavior change?
- Are local checks (`pre-commit`, `pytest`, docs build) currently passing?

### Canonical workflow
1. Confirm contribution type and scope using `docs/source/contributing.md`.
2. Prepare local environment via installation guidance (`docs/source/getting_started/installation.md`).
3. Make a focused patch (small, reviewable PR scope).
4. Add/update tests for any behavior change.
5. Update documentation in `docs/source` for user-visible behavior changes.
6. Run local checks: `pre-commit run --all-files` and `pytest`.
7. Build docs locally (`pushd docs && make html && popd`) before opening PR.
8. In PR text, reference related issues and describe motivation/limitations.

### Minimal working example
```bash
pre-commit run --all-files
pytest
pushd docs
make html
popd
```
Doc anchors: `docs/source/contributing.md`, `docs/source/index.md`
Inputs for focused local checks: `tests/samples/nano_dy.root`, `tests/samples/nano_dimuon.root`  
Checkpoint: lint/tests/docs all pass before opening or updating a PR.

### Pitfalls and fixes
- Pitfall: Oversized PRs mixing unrelated changes. Fix: split into focused patches.
- Pitfall: Behavior changes without tests. Fix: add or update targeted tests before review.
- Pitfall: Code changes without docs updates. Fix: edit the corresponding page in `docs/source`.
- Pitfall: Filing duplicate issues/questions. Fix: search existing Issues and Discussions first.
- Pitfall: Unreproducible bug reports. Fix: include coffea/python versions and minimal reproducer.

### Convergence and validation checks
- `pre-commit` passes for changed files.
- Targeted test modules for touched code pass; full `pytest` baseline is acceptable for the branch.
- Docs build succeeds and generated pages reflect the new behavior.
- PR description links issues and documents limitations/assumptions.

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/contributing.md`
- `docs/source/index.md`
- `docs/source/getting_started/installation.md`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `binder`
- `docs/source/notebooks`

## Test references
- `tests`

## Optional deeper inspection
- `src`

## Source entry points for unresolved issues
- `src/coffea/__init__.py` (public package surface)
- `src/coffea/processor/executor.py` (`Runner` and executor behavior hotspots)
- `src/coffea/processor/processor.py` (`ProcessorABC` contract)
- `src/coffea/nanoevents/factory.py` (NanoEvents creation path)
- `src/coffea/analysis_tools.py` (analysis helper behavior)
- `src/coffea/dataset_tools/preprocess.py` (dataset preprocessing internals)
- `src/coffea/lookup_tools/extractor.py` (lookup extraction internals)
- `tests/test_processor.py` (processor contract coverage)
- `tests/test_local_executors.py` (local execution behavior coverage)
- `tests/test_nanoevents.py` (schema/materialization behavior coverage)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src tests`).
