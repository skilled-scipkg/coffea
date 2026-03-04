# coffea documentation map: Theory and Methods

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/getting_started/concepts.md`: Columnar analysis model and processor semantics.
- `docs/source/user_guide/nanoevents.md`: Event object and vector behavior assumptions.
- `docs/source/user_guide/corrections.md`: Correction and systematic factor workflow.

## Method-focused notebooks
- `docs/source/notebooks/systematics.ipynb` (symlink to `binder/systematics.ipynb`): Up/down variation workflow.
- `docs/source/notebooks/analysis_tools.ipynb` (symlink to `binder/analysis_tools.ipynb`): Weights and selection strategy.
- `docs/source/notebooks/applying_corrections.ipynb` (symlink to `binder/applying_corrections.ipynb`): Correction-set application.

## Method calibration/test inputs
- `tests/samples/Winter14_V8_MC_L5Flavor_AK5Calo.txt`
- `tests/samples/Fall17_17Nov2017_V32_MC_Uncertainty_AK4PFPuppi.junc.txt`
- `tests/samples/Spring16_25nsV10_MC_SF_AK4PFPuppi.jersf.txt`

## Validation checkpoints
- `pytest tests/test_systematics.py -q`
- `pytest tests/test_nanoevents_vector.py -q`
- `pytest tests/test_jetmet_tools.py -k "uncertainty or resolution_sf" -q`
