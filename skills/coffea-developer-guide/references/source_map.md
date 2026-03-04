# coffea source map: Developer Guide

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class ProcessorABC|class Runner|class SimpleCheckpointer" src/coffea`
- `rg -n "def process\(|def __call__\(|def save\(|def load\(" src/coffea`

## Function-level entry points
- `src/coffea/processor/processor.py`
  - Symbols: `ProcessorABC.process`, `ProcessorABC.postprocess`
  - Why: Defines the minimal processor contract for all execution backends.
  - Checkpoint: `pytest tests/test_processor.py -q`
- `src/coffea/processor/executor.py`
  - Symbols: `Runner.__call__`, `Runner.run`, `Runner.preprocess`, `Runner._work_function`
  - Why: Main orchestration and work-item dispatch path.
  - Checkpoint: `pytest tests/test_local_executors.py -k "nanoevents_analysis or preprocessing" -q`
- `src/coffea/processor/checkpointer.py`
  - Symbols: `SimpleCheckpointer.filepath`, `SimpleCheckpointer.save`, `SimpleCheckpointer.load`
  - Why: Defines restart behavior for unstable/long-running workloads.
  - Checkpoint: `pytest tests/test_checkpointing.py -q`
- `src/coffea/dataset_tools/filespec.py`
  - Symbols: `identify_file_format`, `ModelFactory.dict_to_datasetspec`, `DataGroupSpec.limit_files`
  - Why: Typed fileset validation and conversion rules used across workflows.
  - Checkpoint: `pytest tests/test_dataset_tools_filespec.py -k "identify_format or dict_to_datasetspec" -q`
- `src/coffea/nanoevents/factory.py`
  - Symbols: `NanoEventsFactory.from_root`, `NanoEventsFactory.file_handle`
  - Why: Key integration seam between input files and processor event payloads.
  - Checkpoint: `pytest tests/test_nanoevents.py -k "read_nanomc or file_handle" -q`

## Inputs to keep nearby
- `tests/samples/nano_dy.root`
- `tests/samples/nano_dimuon.root`
