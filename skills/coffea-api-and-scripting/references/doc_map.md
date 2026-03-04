# coffea documentation map: API and Scripting

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/api_reference.md`: Public import surface and namespace boundaries.
- `docs/source/user_guide/nanoevents.md`: Event object creation, schema selection, and field access.
- `docs/source/user_guide/corrections.md`: Correction payload loading and evaluation flow.
- `docs/source/user_guide/accumulators.md`: Merge-safe output return structures.
- `docs/source/getting_started/concepts.md`: Processor and columnar execution model.

## API-focused notebooks
- `docs/source/notebooks/nanoevents.ipynb` (symlink to `binder/nanoevents.ipynb`): NanoEvents inspection patterns.
- `docs/source/notebooks/applying_corrections.ipynb` (symlink to `binder/applying_corrections.ipynb`): Correction API exercise.
- `docs/source/notebooks/analysis_tools.ipynb` (symlink to `binder/analysis_tools.ipynb`): Weights and selection usage.

## Local sample inputs
- `tests/samples/nano_dy.root`
- `tests/samples/testSF2d.corr.json.gz`
- `tests/samples/photon_id.ea.txt`

## Validation checkpoints
- `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `pytest tests/test_lookup_tools.py -k "correctionlib or extractor" -q`
- `pytest tests/test_analysis_tools.py -k weights -q`
