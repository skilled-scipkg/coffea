# coffea source map: Advanced Topics

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "NanoEventsFactory|Runner|Weights|correction" src/coffea`
- `rg -n "def from_root|def __call__|def add|def _evaluate" src/coffea`

## Function-level entry points
- `src/coffea/processor/executor.py`
  - Symbols: `Runner.__call__`, `Runner._normalize_fileset`, `IterativeExecutor.__call__`, `FuturesExecutor.__call__`
  - Why: Primary execution path for first local simulation runs.
  - Checkpoint: `pytest tests/test_local_executors.py -k "nanoevents_analysis or preprocessing" -q`
- `src/coffea/nanoevents/factory.py`
  - Symbols: `NanoEventsFactory.from_root`, `NanoEventsFactory.from_parquet`, `NanoEventsFactory.events`
  - Why: Converts ROOT/Parquet inputs into event objects used in examples.
  - Checkpoint: `pytest tests/test_nanoevents.py -k "read_nanomc or access_log" -q`
- `src/coffea/lookup_tools/correctionlib_wrapper.py`
  - Symbols: `correctionlib_wrapper.__init__`, `correctionlib_wrapper._evaluate`
  - Why: Thin adapter from correctionlib payload objects to coffea lookup behavior.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k correctionlib -q`
- `src/coffea/lookup_tools/extractor.py`
  - Symbols: `extractor.add_weight_sets`, `extractor.finalize`, `extractor.make_evaluator`
  - Why: Parses and materializes txt/JSON/root correction payloads.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k "extractor or jec_txt" -q`
- `src/coffea/analysis_tools.py`
  - Symbols: `Weights.add`, `Weights.weight`, `PackedSelection.add`, `PackedSelection.require`
  - Why: Core event-weight and selection semantics used in starter analyses.
  - Checkpoint: `pytest tests/test_analysis_tools.py -k "weights or packed_selection_basic" -q`

## Inputs to keep nearby
- `tests/samples/nano_dy.root`
- `tests/samples/testSF2d.corr.json.gz`
