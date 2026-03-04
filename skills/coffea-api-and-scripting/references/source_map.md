# coffea source map: API and Scripting

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class NanoEventsFactory|class .*Schema|class extractor|class evaluator" src/coffea`
- `rg -n "def from_root|def events|def add_weight_sets|def make_evaluator" src/coffea`

## Function-level entry points
- `src/coffea/nanoevents/factory.py`
  - Symbols: `NanoEventsFactory.from_root`, `NanoEventsFactory.from_parquet`, `NanoEventsFactory.events`, `NanoEventsFactory.access_log`
  - Why: Entry points for programmatic event materialization and IO diagnostics.
  - Checkpoint: `pytest tests/test_nanoevents.py -k "read_nanomc or access_log" -q`
- `src/coffea/nanoevents/schemas/nanoaod.py`
  - Symbols: `NanoAODSchema`, `PFNanoAODSchema`, `ScoutingNanoAODSchema`
  - Why: Canonical schema behavior and branch interpretation.
  - Checkpoint: `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `src/coffea/nanoevents/schemas/physlite.py`
  - Symbols: `PHYSLITESchema`
  - Why: PHYSLITE-specific linked object and form behavior.
  - Checkpoint: `pytest tests/test_nanoevents_physlite.py -k "track_links or objects_are_four_vectors" -q`
- `src/coffea/nanoevents/schemas/pdune.py`
  - Symbols: `PDUNESchema`
  - Why: PDUNE schema edge cases and list-like normalization.
  - Checkpoint: `pytest tests/test_nanoevents_pdune.py -k listify -q`
- `src/coffea/lookup_tools/extractor.py`
  - Symbols: `extractor.add_weight_set`, `extractor.add_weight_sets`, `extractor.finalize`, `extractor.make_evaluator`
  - Why: API surface for ingesting external correction payload definitions.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k "extractor or evaluator" -q`
- `src/coffea/lookup_tools/evaluator.py`
  - Symbols: `evaluator.__getitem__`, `evaluator.keys`
  - Why: Runtime lookup dispatch and key resolution semantics.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k evaluate_noimpl -q`
- `src/coffea/lookup_tools/correctionlib_wrapper.py`
  - Symbols: `correctionlib_wrapper._evaluate`
  - Why: Bridging correctionlib objects into lookup evaluation.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k correctionlib -q`
- `src/coffea/analysis_tools.py`
  - Symbols: `Weights.add`, `Weights.weight`, `PackedSelection.add`, `PackedSelection.cutflow`
  - Why: Frequent API calls for per-event weighting and boolean selection algebra.
  - Checkpoint: `pytest tests/test_analysis_tools.py -k "weights or packed_selection" -q`

## Inputs to keep nearby
- `tests/samples/nano_dy.root`
- `tests/samples/testSF2d.corr.json.gz`
