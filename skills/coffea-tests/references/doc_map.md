# coffea documentation map: Tests

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core test modules (behavior contracts)
- `tests/test_processor.py`: Processor base contract and regressions.
- `tests/test_local_executors.py`: Local runner/executor behavior.
- `tests/test_nanoevents.py`: ROOT/URI/event access semantics.
- `tests/test_dataset_tools.py`: Preprocess, slicing, and fileset transformations.
- `tests/test_lookup_tools.py`: Correction/lookup ingestion and evaluation paths.
- `tests/test_analysis_tools.py`: Weights and packed selections.
- `tests/test_systematics.py`: Up/down and multi-field variation behavior.
- `tests/test_checkpointing.py`: Recovery semantics with unstable processors.

## Sample payloads used in tests
- `tests/samples/nano_dy.root`
- `tests/samples/testSF2d.corr.json.gz`
- `tests/samples/Fall17_17Nov2017_V32_MC_L2Relative_AK4PFPuppi.jec.txt`
- `tests/samples/Cert_294927-306462_13TeV_EOY2017ReReco_Collisions17_JSON.txt`

## Practical checkpoints
- `pytest tests/test_local_executors.py -k nanoevents_analysis -q`
- `pytest tests/test_lookup_tools.py -k "correctionlib or jec_txt" -q`
- `pytest tests/test_systematics.py -q`
