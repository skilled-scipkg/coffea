---
name: coffea-tests
description: This skill should be used when users ask about tests in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: Tests

## High-Signal Playbook

### Route conditions
- Use this skill when the request is about validating behavior, reproducing failures, or confirming regressions.
- Route user-facing workflow construction to `coffea-user-guide` or `coffea-simulation-workflows`.
- Route API usage questions without a failing test context to `coffea-api-and-scripting`.
- Keep non-user-facing internals secondary unless directly required to explain a failing assertion.

### Triage questions
- Which subsystem failed: processor/executor, nanoevents/schema, dataset tools, lookup/corrections, or analysis tools?
- Is there an existing failing test name or only a symptom description?
- Which fixture/sample payload is required from `tests/samples`?
- Is the failure mode local only or distributed/dask-specific?
- Does the change alter public behavior (requires docs update) or only internal refactor?

### Canonical workflow
1. Map the issue to a nearest existing test module (for example `test_local_executors.py`, `test_nanoevents.py`).
2. Reproduce with targeted `pytest` selection before touching code.
3. Inspect referenced sample payloads in `tests/samples` for format/era assumptions.
4. Patch the smallest code path that explains the failure.
5. Re-run targeted tests plus adjacent subsystem tests.
6. Run broader checks from contribution guidance (`pre-commit`, `pytest`) once targeted passes.
7. If behavior changed intentionally, document that change and update/add tests accordingly.

### Minimal working example
```bash
pytest tests/test_local_executors.py -k nanoevents_analysis -q
pytest tests/test_nanoevents.py -k read_nanomc -q
pytest tests/test_lookup_tools.py -k correctionlib -q
```
Doc anchors: `docs/source/contributing.md`, `tests/test_local_executors.py`, `tests/test_nanoevents.py`, `tests/test_lookup_tools.py`
Inputs: `tests/samples/nano_dy.root`, `tests/samples/testSF2d.corr.json.gz`  
Checkpoint: reproduce failure first, then re-run the same targeted commands to confirm fix.

### Pitfalls and fixes
- Pitfall: Running the full suite first. Fix: start with narrow, subsystem-matched tests.
- Pitfall: Ignoring fixture dependencies in `tests/samples`. Fix: confirm sample file names and payload types before debugging logic.
- Pitfall: Treating dask-only failures as generic processor bugs. Fix: isolate mode/backend first.
- Pitfall: Modifying internals without updating behavior tests. Fix: add/adjust assertions tied to user-visible outcomes.
- Pitfall: Using tests as primary documentation for usage. Fix: keep docs-first; use tests as behavioral evidence.

### Convergence and validation checks
- Targeted failing tests are now green and reproducible across reruns.
- Adjacent subsystem tests remain green (no local regressions).
- Any intentional behavior change is represented by an updated/new test assertion.
- Contributor baseline checks (`pre-commit`, `pytest`) pass before merge.

## Scope
- Handle questions about documentation grouped under the 'tests' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/contributing.md`
- `tests/test_processor.py`
- `tests/test_local_executors.py`
- `tests/test_nanoevents.py`
- `tests/test_lookup_tools.py`
- `tests/test_dataset_tools.py`
- `tests/samples/photon_id.ea.txt`
- `tests/samples/Fall17_17Nov2017_V32_MC_L2Relative_AK4PFPuppi.jec.txt`
- `tests/samples/Cert_294927-306462_13TeV_EOY2017ReReco_Collisions17_JSON.txt`

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
- `tests/test_processor.py` (processor interface contract)
- `tests/test_local_executors.py` (local executor behavior matrix)
- `tests/test_nanoevents.py` (schema/materialization behavior)
- `tests/test_lookup_tools.py` (correction and lookup behavior)
- `tests/test_dataset_tools.py` (fileset/discovery/preprocess behavior)
- `src/coffea/processor/executor.py` (runner/executor internals)
- `src/coffea/nanoevents/factory.py` (NanoEvents build path)
- `src/coffea/lookup_tools/jec_uncertainty_lookup.py` (JEC uncertainty behavior)
- `src/coffea/dataset_tools/preprocess.py` (dataset preprocessing behavior)
- `src/coffea/analysis_tools.py` (weights/selection behavior)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tests src`).
