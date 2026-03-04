---
name: coffea-api-and-scripting
description: This skill should be used when users ask about api and scripting in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: API and Scripting

## High-Signal Playbook

### Route conditions
- Use this skill for symbol-level usage of NanoEvents, schemas, corrections, lookup tools, and package namespaces.
- Route full end-to-end execution planning to `coffea-simulation-workflows`.
- Route broad user-guide workflow assembly to `coffea-user-guide`.
- Route contribution/process questions to `coffea-developer-guide`.

### Triage questions
- Are you reading ROOT or Parquet, and with which `schemaclass`?
- Do you need namespace discovery (`import coffea`) or explicit subpackage imports?
- Are corrections from `correctionlib` JSON, JEC/JER txt, ROOT histograms, or CSV payloads?
- Are outputs eager/virtual or dask-style, and how must accumulators merge?
- Is the request conceptual API guidance or exact behavior from implementation?

### Canonical workflow
1. Start at `docs/source/api_reference.md` to identify public namespace boundaries.
2. For event access, prototype with `NanoEventsFactory.from_root(...).events()` and verify collection fields.
3. Select an explicit schema (`NanoAODSchema`, `PHYSLITESchema`, `PDUNESchema`, etc.) before analysis logic.
4. Load correction payloads once (typically in processor `__init__`) and evaluate on Awkward arrays.
5. Reduce per-object weights to event-level observables using Awkward reductions.
6. Return merge-safe dictionaries/histograms consistent with the chosen execution mode.
7. Escalate to `references/source_map.md` only if docs leave API semantics ambiguous.

### Minimal working example
```python
import awkward as ak
import correctionlib
from coffea.nanoevents import NanoAODSchema, NanoEventsFactory

events = NanoEventsFactory.from_root(
    {"nano_dy.root": "Events"},
    schemaclass=NanoAODSchema,
    entry_stop=10_000,
).events()

cset = correctionlib.CorrectionSet.from_file("muon_sf.json.gz")
sf = cset["muon_sf"].evaluate(events.Muon.eta, events.Muon.pt, "nominal")
event_weight = ak.prod(sf, axis=1)
```
Doc anchors: `docs/source/user_guide/nanoevents.md`, `docs/source/user_guide/corrections.md`, `docs/source/api_reference.md`

### Quick API smoke checks
```bash
pytest tests/test_nanoevents.py -k read_nanomc -q
pytest tests/test_lookup_tools.py -k "extractor or correctionlib" -q
pytest tests/test_analysis_tools.py -k weights -q
```
Inputs: `tests/samples/nano_dy.root`, `tests/samples/testSF2d.corr.json.gz`  
Checkpoint: schema loading, lookup evaluation, and weight aggregation all pass.

### Pitfalls and fixes
- Pitfall: Using the wrong schema for the file model. Fix: explicitly set `schemaclass` and validate `events.fields` first.
- Pitfall: Re-loading correction payloads inside `process()`. Fix: load once in `__init__`.
- Pitfall: Treating jagged per-object weights as event weights. Fix: use `ak.prod(..., axis=1)` when reducing per-event.
- Pitfall: Mixing dask and eager accumulator layouts. Fix: follow mode-specific return structure from `accumulators.md`.
- Pitfall: Assuming everything is in the `coffea` top namespace. Fix: confirm in `api_reference.md` and import submodules explicitly when required.

### Convergence and validation checks
- API snippet executes on a small `entry_stop` without schema or shape errors.
- Nominal/up/down correction outputs are finite and have matching event-length.
- Output keys and object types are merge-compatible across chunks and executors.
- Any source-level claim can be traced to one concrete file path in this skill.

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/api_reference.md`
- `docs/source/user_guide/nanoevents.md`
- `docs/source/user_guide/corrections.md`
- `docs/source/user_guide/accumulators.md`
- `docs/source/getting_started/concepts.md`
- `docs/source/_templates/automodapi_templ.rst`

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
- `src/coffea/nanoevents/factory.py` (`NanoEventsFactory`, lazy/eager materialization)
- `src/coffea/nanoevents/schemas/nanoaod.py` (NanoAOD schema behavior)
- `src/coffea/nanoevents/schemas/physlite.py` (PHYSLITE schema behavior)
- `src/coffea/nanoevents/schemas/pdune.py` (PDUNE schema behavior)
- `src/coffea/lookup_tools/correctionlib_wrapper.py` (correctionlib adapter internals)
- `src/coffea/lookup_tools/extractor.py` (payload extraction workflow)
- `src/coffea/lookup_tools/evaluator.py` (lookup dispatch and evaluation)
- `src/coffea/lookup_tools/json_converters.py` (JSON payload conversion)
- `src/coffea/lookup_tools/jec_uncertainty_lookup.py` (JEC uncertainty tables)
- `src/coffea/lookup_tools/jersf_lookup.py` (JER scale factor lookups)
- `src/coffea/analysis_tools.py` (weights/selections utilities)
- `src/coffea/dataset_tools/rucio_utils.py` (dataset discovery helpers)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src`).
