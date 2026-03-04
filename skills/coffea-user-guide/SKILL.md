---
name: coffea-user-guide
description: This skill should be used when users ask about user guide in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: User Guide

## High-Signal Playbook

### Route conditions
- Use this skill for end-user workflow questions across NanoEvents, datasets, executors, accumulators, and corrections.
- Route execution-tuning or dataset-preprocess deep dives to `coffea-simulation-workflows`.
- Route symbol-level API questions (exact classes/functions) to `coffea-api-and-scripting`.
- Route regression verification and failure triage to `coffea-tests`.

### Triage questions
- Are inputs ROOT, Parquet, or mixed?
- Which schema is expected (`NanoAODSchema`, `PHYSLITESchema`, `PDUNESchema`, etc.)?
- Is the run mode local (`IterativeExecutor`/`FuturesExecutor`) or distributed (`DaskExecutor`/`ParslExecutor`/`TaskVineExecutor`)?
- Should outputs be counts only, histograms, or weighted/systematic products?
- Are dataset metadata keys required in `events.metadata`?

### Canonical workflow
1. Confirm the requested task against `docs/source/user_guide/index.md` and choose the specific guide page.
2. Build or validate a fileset in one of the documented formats, preferring file-to-tree mapping.
3. Inspect a small sample with NanoEvents before scaling.
4. Implement processor logic with vectorized Awkward selections.
5. Return plain dictionaries/histograms from `process()`; keep dask output format consistent with docs.
6. Start with `IterativeExecutor`, then swap executor only after local behavior is validated.
7. Enable `savemetrics=True` and inspect I/O/runtime metrics.
8. Persist merged outputs with `coffea.util.save` if downstream stages need stable artifacts.

### Minimal working example
```python
from coffea import processor
from coffea.nanoevents import NanoAODSchema

# Assume my_processor is a ProcessorABC instance.
fileset = {
    "DYJets": {
        "files": {"nano_dy.root": "Events"},
        "metadata": {"year": 2018, "is_mc": True},
    }
}

runner = processor.Runner(
    executor=processor.IterativeExecutor(),
    schema=NanoAODSchema,
    savemetrics=True,
)

result, metrics = runner(fileset, processor_instance=my_processor)
```
Doc anchors: `docs/source/user_guide/datasets.md`, `docs/source/user_guide/executors.md`, `docs/source/user_guide/accumulators.md`

### Quick user-guide launch checks
```bash
pytest tests/test_nanoevents.py -k read_nanomc -q
pytest tests/test_local_executors.py -k nanoevents_analysis -q
pytest tests/test_analysis_tools.py -k "weights or packed_selection_basic" -q
```
Inputs: `tests/samples/nano_dy.root`, `tests/samples/testSF2d.corr.json.gz`  
Checkpoint: all three checks pass before scaling execution or adding systematics.

### Pitfalls and fixes
- Pitfall: Treating fileset format as free-form dicts. Fix: follow one of the three documented fileset layouts in `datasets.md`.
- Pitfall: Missing/incorrect tree names. Fix: use format-1 file-to-tree mapping or pass `treename` consistently.
- Pitfall: Looping over events in Python. Fix: keep operations columnar as shown in `nanoevents.md`.
- Pitfall: Returning nested-by-dataset payloads in dask mode. Fix: return flat dictionaries for dask-mode accumulation (`accumulators.md`).
- Pitfall: Scaling too early. Fix: validate on `IterativeExecutor` first, then swap executor only.

### Convergence and validation checks
- Event totals and key histogram integrals agree between local and distributed executors on the same sample.
- `metrics` shows expected bytes-read and touched-column behavior for selected branches.
- Metadata-dependent logic (`year`, `is_mc`, cross sections) is present and consistent across all datasets.
- Systematic/weight branches produce finite values and expected nominal vs variation ordering.

## Scope
- Handle questions about documentation grouped under the 'user-guide' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/user_guide/index.md`
- `docs/source/user_guide/datasets.md`
- `docs/source/user_guide/nanoevents.md`
- `docs/source/user_guide/executors.md`
- `docs/source/user_guide/accumulators.md`
- `docs/source/user_guide/corrections.md`

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
- `src/coffea/nanoevents/factory.py` (`NanoEventsFactory.from_root`, schema handoff)
- `src/coffea/nanoevents/schemas/nanoaod.py` (NanoAOD field interpretation)
- `src/coffea/nanoevents/schemas/auto.py` (automatic schema dispatch)
- `src/coffea/processor/executor.py` (`Runner`, local/distributed executor wiring)
- `src/coffea/processor/accumulator.py` (merge semantics for accumulator helpers)
- `src/coffea/analysis_tools.py` (`Weights`, `PackedSelection` behavior)
- `src/coffea/dataset_tools/filespec.py` (validated dataset/fileset structures)
- `src/coffea/dataset_tools/preprocess.py` (chunking and preprocessing behavior)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src`).
