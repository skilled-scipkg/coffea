# coffea source map: Tests

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class Runner|class .*Schema|class extractor|class Weights|class UpDown" src/coffea`
- `rg -n "def preprocess|def add_weight_set|def weight\(|def get_variation" src/coffea`

## Function-level entry points with paired tests
- `src/coffea/processor/processor.py`
  - Symbols: `ProcessorABC.process`, `ProcessorABC.postprocess`
  - Behavior checks: `tests/test_processor.py::test_processorabc`
- `src/coffea/processor/executor.py`
  - Symbols: `Runner.__call__`, `Runner._preprocess_fileset_root`, `IterativeExecutor.__call__`, `FuturesExecutor.__call__`
  - Behavior checks: `tests/test_local_executors.py::test_nanoevents_analysis`, `tests/test_local_executors.py::test_preprocessing`
- `src/coffea/nanoevents/factory.py`
  - Symbols: `NanoEventsFactory.from_root`, `NanoEventsFactory.access_log`, `NanoEventsFactory.file_handle`
  - Behavior checks: `tests/test_nanoevents.py::test_read_nanomc`, `tests/test_nanoevents.py::test_access_log`
- `src/coffea/dataset_tools/preprocess.py`
  - Symbols: `get_steps`, `preprocess`
  - Behavior checks: `tests/test_dataset_tools.py::test_preprocess`, `tests/test_dataset_tools.py::test_recover_failed_chunks`
- `src/coffea/dataset_tools/filespec.py`
  - Symbols: `identify_file_format`, `ModelFactory.dict_to_datasetspec`, `DataGroupSpec.limit_steps`
  - Behavior checks: `tests/test_dataset_tools_filespec.py::test_identify_format`
- `src/coffea/lookup_tools/extractor.py`
  - Symbols: `extractor.add_weight_sets`, `extractor.finalize`, `extractor.make_evaluator`
  - Behavior checks: `tests/test_lookup_tools.py::test_extractor_exceptions`
- `src/coffea/lookup_tools/evaluator.py`
  - Symbols: `evaluator.__getitem__`, `evaluator.keys`
  - Behavior checks: `tests/test_lookup_tools.py::test_evaluator_exceptions`, `tests/test_lookup_tools.py::test_evaluate_noimpl`
- `src/coffea/lookup_tools/correctionlib_wrapper.py`
  - Symbols: `correctionlib_wrapper._evaluate`
  - Behavior checks: `tests/test_lookup_tools.py::test_correctionlib`
- `src/coffea/analysis_tools.py`
  - Symbols: `Weights.add`, `Weights.weight`, `PackedSelection.require`, `PackedSelection.cutflow`
  - Behavior checks: `tests/test_analysis_tools.py::test_weights`, `tests/test_analysis_tools.py::test_packed_selection_cutflow`
- `src/coffea/nanoevents/methods/systematics/UpDownSystematic.py`
  - Symbols: `UpDownSystematic.get_variation`, `UpDownSystematic.up`, `UpDownSystematic.down`
  - Behavior checks: `tests/test_systematics.py::test_single_field_variation`
- `src/coffea/nanoevents/methods/systematics/UpDownMultiSystematic.py`
  - Symbols: `UpDownMultiSystematic.get_variation`
  - Behavior checks: `tests/test_systematics.py::test_multi_field_variation`
- `src/coffea/processor/checkpointer.py`
  - Symbols: `SimpleCheckpointer.save`, `SimpleCheckpointer.load`
  - Behavior checks: `tests/test_checkpointing.py::test_checkpointing`

## Quick targeted runs
- `pytest tests/test_processor.py tests/test_local_executors.py -q`
- `pytest tests/test_lookup_tools.py -k "correctionlib or extractor or evaluator" -q`
- `pytest tests/test_systematics.py tests/test_checkpointing.py -q`
