---
name: coffea-theory-and-methods
description: This skill should be used when users ask about theory and methods in coffea; it prioritizes documentation references and then source inspection only for unresolved details.
---

# coffea: Theory and Methods

## High-Signal Playbook

### Route conditions
- Use this skill for method-level analysis semantics: vector behaviors, object combinations, event-weight construction, and systematic-variation handling.
- Route runtime orchestration (fileset, executor scaling) to `coffea-simulation-workflows`.
- Route exact API signatures/import paths to `coffea-api-and-scripting`.
- Route validation against regression fixtures to `coffea-tests`.

### Triage questions
- Which object model/schema is assumed (NanoAOD, PHYSLITE, PDUNE, etc.)?
- Are corrections per-object or per-event?
- Which systematic axes are required (`nominal`, `up`, `down`, regrouped sources)?
- Is the concern physics-method consistency or implementation detail?
- Which calibration era/payload files are in play?

### Canonical workflow
1. Confirm the object model and vector behavior assumptions in `nanoevents.md`.
2. Build object-level selections and combinations using Awkward/vectorized operations.
3. Load correction payloads once and evaluate per-object factors.
4. Reduce to event-level weights with explicit Awkward reductions.
5. Produce nominal and varied outputs in parallel (same selection baseline).
6. Validate with known payloads from `tests/samples` and compare variation envelopes.
7. Escalate to method/systematics implementation files only for unresolved semantics.

### Minimal working example
```python
import awkward as ak
import correctionlib

muons = events.Muon[(events.Muon.tightId) & (events.Muon.pt > 20)]
cset = correctionlib.CorrectionSet.from_file("muon_sf.json.gz")
sf = cset["muon_sf"].evaluate(muons.eta, muons.pt, "nominal")
sf_up = cset["muon_sf"].evaluate(muons.eta, muons.pt, "syst_up")
sf_down = cset["muon_sf"].evaluate(muons.eta, muons.pt, "syst_down")

w = ak.prod(sf, axis=1)
w_up = ak.prod(sf_up, axis=1)
w_down = ak.prod(sf_down, axis=1)
```
Doc anchors: `docs/source/user_guide/nanoevents.md`, `docs/source/user_guide/corrections.md`, `docs/source/getting_started/concepts.md`

### Quick method/systematics checks
```bash
pytest tests/test_systematics.py -q
pytest tests/test_nanoevents_vector.py -q
pytest tests/test_jetmet_tools.py -k "jet_correction_uncertainty or resolution_sf" -q
```
Inputs: `tests/samples/Winter14_V8_MC_L5Flavor_AK5Calo.txt`, `tests/samples/Fall17_17Nov2017_V32_MC_Uncertainty_AK4PFPuppi.junc.txt`  
Checkpoint: variations, vector behavior, and jet uncertainty paths are stable.

### Pitfalls and fixes
- Pitfall: Mixing per-object and per-event quantities. Fix: reduce per-event explicitly with `ak.prod` or `ak.sum` as appropriate.
- Pitfall: Applying corrections before baseline selections. Fix: define stable selection first, then evaluate corrections on the selected objects.
- Pitfall: Ignoring payload era/version compatibility. Fix: track and verify calibration payload provenance.
- Pitfall: Assuming nominal and systematic branches are interchangeable. Fix: persist each variation separately in output.
- Pitfall: Falling back to row-wise loops. Fix: keep vectorized Awkward operations for method consistency and scale.

### Convergence and validation checks
- Nominal yields are stable when chunking/executor changes on the same input sample.
- Up/down (or source-varied) weights bracket nominal in expected regions.
- No NaN/inf values in derived weights or vector observables.
- Method behavior is reproducible against reference payloads in `tests/samples`.

## Scope
- Handle questions about theoretical background and algorithmic methods.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/getting_started/concepts.md`
- `docs/source/user_guide/nanoevents.md`
- `docs/source/user_guide/corrections.md`
- `tests/samples/Winter14_V8_MC_L5Flavor_AK5Calo.txt`

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
- `src/coffea/nanoevents/methods/vector.py` (vector behavior operations)
- `src/coffea/nanoevents/methods/nanoaod.py` (NanoAOD method definitions)
- `src/coffea/nanoevents/methods/systematics/UpDownSystematic.py` (single systematic wrapper)
- `src/coffea/nanoevents/methods/systematics/UpDownMultiSystematic.py` (multi-systematic wrapper)
- `src/coffea/lookup_tools/jec_uncertainty_lookup.py` (JEC uncertainty interpolation)
- `src/coffea/lookup_tools/jersf_lookup.py` (JER scale-factor lookup)
- `src/coffea/jetmet_tools/JECStack.py` (JEC composition workflow)
- `src/coffea/jetmet_tools/JetCorrectionUncertainty.py` (jet uncertainty behavior)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src`).
