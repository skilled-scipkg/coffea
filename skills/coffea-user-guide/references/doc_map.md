# coffea documentation map: User Guide

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/user_guide/index.md`: Routing across user-guide tasks.
- `docs/source/user_guide/datasets.md`: Fileset structures and metadata.
- `docs/source/user_guide/nanoevents.md`: Event loading and collection operations.
- `docs/source/user_guide/executors.md`: Execution backend selection.
- `docs/source/user_guide/accumulators.md`: Result collection patterns.
- `docs/source/user_guide/corrections.md`: Correction and weight handling.

## User-facing notebooks
- `docs/source/notebooks/processing.ipynb` (symlink to `binder/processing.ipynb`)
- `docs/source/notebooks/nanoevents.ipynb` (symlink to `binder/nanoevents.ipynb`)
- `docs/source/notebooks/analysis_tools.ipynb` (symlink to `binder/analysis_tools.ipynb`)
- `docs/source/notebooks/systematics.ipynb` (symlink to `binder/systematics.ipynb`)

## Local sample inputs
- `tests/samples/nano_dy.root`
- `tests/samples/nano_dimuon.root`
- `tests/samples/testSF2d.corr.json.gz`

## Validation checkpoints
- `pytest tests/test_local_executors.py -k nanoevents_analysis -q`
- `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `pytest tests/test_analysis_tools.py -k "weights or packed_selection_basic" -q`
