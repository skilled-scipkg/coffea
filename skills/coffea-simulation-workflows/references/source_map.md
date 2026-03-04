# coffea source map: Simulation Workflows

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class Runner|class .*Executor|def preprocess|def apply_to_fileset" src/coffea`
- `rg -n "def query_dataset|def max_chunks|def slice_chunks" src/coffea/dataset_tools`

## Function-level entry points
- `src/coffea/dataset_tools/filespec.py`
  - Symbols: `identify_file_format`, `ModelFactory.dict_to_datasetspec`, `DataGroupSpec.limit_steps`, `DataGroupSpec.filter_files`
  - Why: Input fileset normalization and typed validation before execution.
  - Checkpoint: `pytest tests/test_dataset_tools_filespec.py -k "identify_format or limit_steps" -q`
- `src/coffea/dataset_tools/preprocess.py`
  - Symbols: `get_steps`, `preprocess`
  - Why: Chunk construction and preflight metadata/form extraction.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k "preprocess or recover_failed_chunks" -q`
- `src/coffea/dataset_tools/manipulations.py`
  - Symbols: `max_chunks`, `slice_chunks`, `filter_files`, `get_failed_steps_for_fileset`
  - Why: Runtime slicing/recovery controls for large jobs.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k "max_chunks or slice_chunks or filter_files" -q`
- `src/coffea/dataset_tools/apply_processor.py`
  - Symbols: `apply_to_dataset`, `apply_to_fileset`
  - Why: Convenience wrappers for applying processors to dataset specs.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k apply_to_fileset -q`
- `src/coffea/processor/executor.py`
  - Symbols: `Runner.preprocess`, `Runner.run`, `Runner.__call__`, `IterativeExecutor.__call__`, `FuturesExecutor.__call__`, `DaskExecutor.__call__`, `ParslExecutor.__call__`
  - Why: Backend-independent execution orchestration and scaling path.
  - Checkpoint: `pytest tests/test_local_executors.py -k "nanoevents_analysis or preprocessing" -q`
- `src/coffea/processor/taskvine_executor.py`
  - Symbols: `TaskVineExecutor.__call__`, `accumulate_result_files`
  - Why: TaskVine distributed execution and staged accumulation behavior.
  - Checkpoint: `pytest tests/test_taskvine_virtual.py -q`
- `src/coffea/dataset_tools/rucio_utils.py`
  - Symbols: `get_dataset_files_replicas`, `query_dataset`
  - Why: Remote dataset inventory and replica selection for production workflows.
  - Checkpoint: `pytest tests/test_dataset_tools.py -k tuple_data_manipulation_output -q`

## Inputs to keep nearby
- `tests/samples/nano_dy.root`
- `tests/samples/nano_dimuon.root`
