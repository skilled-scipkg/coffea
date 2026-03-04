# coffea documentation map: Simulation Workflows

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/getting_started/concepts.md`: Processor model and columnar execution basics.
- `docs/source/user_guide/datasets.md`: Fileset formats and metadata conventions.
- `docs/source/user_guide/executors.md`: Local/distributed backend selection and trade-offs.
- `docs/source/user_guide/accumulators.md`: Merge-safe output structures.

## Workflow notebooks
- `docs/source/notebooks/processing.ipynb` (symlink to `binder/processing.ipynb`): Processor + runner end-to-end.
- `docs/source/notebooks/dataset_discovery.ipynb` (symlink to `binder/dataset_discovery.ipynb`): Dataset query/discovery flow.
- `docs/source/notebooks/filespec.ipynb` (symlink to `binder/filespec.ipynb`): Filespec/data group manipulation patterns.

## Local sample inputs
- `tests/samples/nano_dy.root`
- `tests/samples/nano_dimuon.root`
- `tests/samples/Autumn18_RunA_V19_DATA_L2Relative_AK4Calo.jec.txt`

## Validation checkpoints
- `pytest tests/test_dataset_tools.py -k "preprocess or apply_to_fileset" -q`
- `pytest tests/test_local_executors.py -k "nanoevents_analysis or preprocessing" -q`
- `pytest tests/test_taskvine_virtual.py -q`
