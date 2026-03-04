# coffea documentation map: Advanced Topics

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/getting_started/index.md`: First local run flow and scale-out overview.
- `docs/source/getting_started/installation.md`: Environment setup and platform constraints.
- `docs/source/examples.md`: Notebook routing and example intent map.
- `docs/source/user_guide/corrections.md`: Correction-set loading and evaluation patterns.

## Simulation bootstrap notebooks
- `docs/source/notebooks/processing.ipynb` (symlink to `binder/processing.ipynb`): End-to-end processor runner walkthrough.
- `docs/source/notebooks/applying_corrections.ipynb` (symlink to `binder/applying_corrections.ipynb`): Correctionlib usage from payload to weights.
- `docs/source/notebooks/nanoevents.ipynb` (symlink to `binder/nanoevents.ipynb`): Event schema and columnar access basics.

## Local sample inputs
- `tests/samples/nano_dy.root`: Minimal ROOT sample for runner smoke tests.
- `tests/samples/testSF2d.corr.json.gz`: Small correctionlib payload for API checks.
- `tests/samples/Fall17_17Nov2017_V32_MC_L2Relative_AK4PFPuppi.jec.txt`: Representative text correction payload.

## Validation checkpoints
- `pytest tests/test_local_executors.py -k nanoevents_analysis -q`
- `pytest tests/test_lookup_tools.py -k correctionlib -q`
