---
name: coffea-simulation-workflows
description: This skill should be used when users ask about simulation workflows in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: Simulation Workflows

## High-Signal Playbook

### Route conditions
- Use this skill for end-to-end execution: fileset declaration/discovery, processor execution mode selection, and output aggregation.
- Route NanoEvents schema internals and API symbol questions to `coffea-api-and-scripting`.
- Route broad conceptual guidance to `coffea-user-guide`.
- Route regression evidence or expected behavior checks to `coffea-tests`.

### Triage questions
- How are datasets provided: hand-written fileset, JSON fileset, or discovery tools?
- Do files share one tree name or require per-file tree mapping?
- Which execution backend is intended (iterative, futures, dask, parsl, taskvine)?
- What output form is required (histograms, counters, weighted summaries)?
- Is checkpointing/restart resilience needed for long jobs?

### Canonical workflow
1. Build fileset with documented format-1 mapping unless a simpler format is justified.
2. Attach metadata keys required by processor logic (`year`, `is_mc`, cross sections, etc.).
3. Use `rucio_utils` discovery helpers when dataset inventories are remote/dynamic.
4. Implement processor `process()` returning merge-safe dictionaries/hist objects.
5. Run locally first via `IterativeExecutor` or `FuturesExecutor`.
6. Scale by swapping executor (`DaskExecutor`, `ParslExecutor`, or `TaskVineExecutor`) without rewriting processor logic.
7. Enable `savemetrics=True`; add a checkpointer for long-running campaigns.
8. Save outputs for downstream reuse and compare local vs distributed aggregates.

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
    executor=processor.FuturesExecutor(workers=4),
    schema=NanoAODSchema,
    savemetrics=True,
)

result, metrics = runner(fileset, processor_instance=my_processor)
```
Doc anchors: `docs/source/user_guide/datasets.md`, `docs/source/user_guide/executors.md`, `docs/source/user_guide/accumulators.md`

### Quick simulation launch (command + input + checkpoint)
```bash
pytest tests/test_local_executors.py -k nanoevents_analysis -q
pytest tests/test_dataset_tools.py -k "preprocess or apply_to_fileset" -q
```
Inputs: `tests/samples/nano_dy.root`, `tests/samples/nano_dimuon.root`  
Checkpoint: both commands pass before enabling distributed executors.

### Pitfalls and fixes
- Pitfall: Invalid fileset structure. Fix: use one of the three documented fileset formats from `datasets.md`.
- Pitfall: Missing tree names for mixed inputs. Fix: use per-file tree mapping (format 1).
- Pitfall: Returning incompatible accumulator shapes across modes. Fix: follow `accumulators.md` return layout guidance.
- Pitfall: Moving to distributed backend before local validation. Fix: validate with iterative/futures first.
- Pitfall: No runtime diagnostics. Fix: enable `savemetrics` and inspect bytes/columns/runtime.

### Convergence and validation checks
- Local and distributed runs agree on total events and key histogram integrals for the same sample.
- Metrics indicate expected chunk processing behavior and no severe I/O imbalance.
- Metadata-dependent calculations remain consistent across datasets/chunks.
- Serialized output reloads cleanly with `coffea.util.load` for downstream stages.

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/getting_started/concepts.md`
- `docs/source/getting_started/index.md`
- `docs/source/user_guide/datasets.md`
- `docs/source/user_guide/executors.md`
- `docs/source/user_guide/accumulators.md`

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
- `src/coffea/processor/executor.py` (`Runner` orchestration and executor adapters)
- `src/coffea/processor/taskvine_executor.py` (TaskVine execution path)
- `src/coffea/processor/checkpointer.py` (checkpoint/restart behavior)
- `src/coffea/processor/accumulator.py` (accumulator merge utilities)
- `src/coffea/dataset_tools/preprocess.py` (preprocessing/chunking)
- `src/coffea/dataset_tools/filespec.py` (fileset specification models)
- `src/coffea/dataset_tools/manipulations.py` (chunk/file slicing helpers)
- `src/coffea/dataset_tools/rucio_utils.py` (remote dataset discovery)
- `src/coffea/dataset_tools/apply_processor.py` (dataset/fileset helper execution)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src`).
