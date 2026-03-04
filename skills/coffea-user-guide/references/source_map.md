# coffea source map: User Guide

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class Runner|class .*Executor|class .*Schema|class Weights" src/coffea`
- `rg -n "def from_root|def preprocess|def accumulate|def weight\(" src/coffea`

## Function-level entry points
- `src/coffea/nanoevents/factory.py`
  - Symbols: `NanoEventsFactory.from_root`, `NanoEventsFactory.from_parquet`, `NanoEventsFactory.events`
  - Why: User entry point for creating event collections from files.
  - Checkpoint: `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `src/coffea/nanoevents/schemas/auto.py`
  - Symbols: `auto_schema`, `_build_record_array`
  - Why: Automatic schema dispatch path when users do not specify a schema.
  - Checkpoint: `pytest tests/test_nanoevents_auto.py -q`
- `src/coffea/nanoevents/schemas/nanoaod.py`
  - Symbols: `NanoAODSchema`
  - Why: Default schema path for many user workflows.
  - Checkpoint: `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `src/coffea/processor/executor.py`
  - Symbols: `Runner.__call__`, `Runner.preprocess`, `IterativeExecutor.__call__`, `FuturesExecutor.__call__`, `DaskExecutor.__call__`
  - Why: User-facing execution controls and backend scaling behavior.
  - Checkpoint: `pytest tests/test_local_executors.py -k "nanoevents_analysis or preprocessing" -q`
- `src/coffea/processor/accumulator.py`
  - Symbols: `add`, `accumulate`, `dict_accumulator`
  - Why: Merge behavior for outputs returned by user processors.
  - Checkpoint: `pytest tests/test_accumulators.py -q`
- `src/coffea/analysis_tools.py`
  - Symbols: `Weights.add`, `Weights.weight`, `PackedSelection.add`, `PackedSelection.require`
  - Why: Common user-side weighting and event selection operations.
  - Checkpoint: `pytest tests/test_analysis_tools.py -k "weights or packed_selection_basic" -q`
- `src/coffea/dataset_tools/filespec.py`
  - Symbols: `DataGroupSpec.limit_files`, `DataGroupSpec.limit_steps`, `DataGroupSpec.filter_files`
  - Why: User manipulation of large filesets before execution.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k "max_files or slice_files or filter_files" -q`
- `src/coffea/dataset_tools/preprocess.py`
  - Symbols: `preprocess`, `get_steps`
  - Why: Chunking and metadata extraction prior to large runs.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k preprocess -q`

## Inputs to keep nearby
- `tests/samples/nano_dy.root`
- `tests/samples/nano_dimuon.root`
