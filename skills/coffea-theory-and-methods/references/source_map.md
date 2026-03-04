# coffea source map: Theory and Methods

Use this map only after reading `doc_map.md` and `../SKILL.md`.

## Fast source navigation
- `rg -n "class .*Vector|class .*Systematic|class JECStack|class JetCorrectionUncertainty" src/coffea`
- `rg -n "def get_variation|def getUncertainty|def add_multivariation" src/coffea`

## Function-level entry points
- `src/coffea/nanoevents/methods/vector.py`
  - Symbols: `_metric_table_core`, `_nearest_core`, `LorentzVector`, `PtEtaPhiMLorentzVector`
  - Why: Core vector math and nearest/matching behavior used by object-level methods.
  - Checkpoint: `pytest tests/test_nanoevents_vector.py -k "lorentz_vector or dask_metric_table_and_nearest" -q`
- `src/coffea/nanoevents/methods/nanoaod.py`
  - Symbols: `Muon`, `Jet`, `MissingET`, `Electron`
  - Why: Physics-object behavior and systematic-aware object wrappers.
  - Checkpoint: `pytest tests/test_nanoevents.py -k read_nanomc -q`
- `src/coffea/nanoevents/methods/systematics/UpDownSystematic.py`
  - Symbols: `UpDownSystematic._build_variations`, `UpDownSystematic.get_variation`, `UpDownSystematic.up`, `UpDownSystematic.down`
  - Why: Defines single-source systematic variation semantics.
  - Checkpoint: `pytest tests/test_systematics.py -k single_field_variation -q`
- `src/coffea/nanoevents/methods/systematics/UpDownMultiSystematic.py`
  - Symbols: `UpDownMultiSystematic._build_variations`, `UpDownMultiSystematic.get_variation`
  - Why: Multi-source systematic naming and retrieval behavior.
  - Checkpoint: `pytest tests/test_systematics.py -k multi_field_variation -q`
- `src/coffea/analysis_tools.py`
  - Symbols: `Weights.add_multivariation`, `Weights.partial_weight`, `PackedSelection.nminusone`
  - Why: Event-level aggregation of method/systematic effects.
  - Checkpoint: `pytest tests/test_analysis_tools.py -k "weights_multivariation or packed_selection_nminusone" -q`
- `src/coffea/lookup_tools/jec_uncertainty_lookup.py`
  - Symbols: `masked_bin_eval`, `jec_uncertainty_lookup`
  - Why: Interpolation and uncertainty bin lookup for JEC/JER studies.
  - Checkpoint: `pytest tests/test_lookup_tools.py -k jec_txt_effareas -q`
- `src/coffea/lookup_tools/jersf_lookup.py`
  - Symbols: `masked_bin_eval`, `jersf_lookup`
  - Why: Jet-resolution scale factor lookup behavior.
  - Checkpoint: `pytest tests/test_jetmet_tools.py -k resolution_sf -q`
- `src/coffea/jetmet_tools/JECStack.py`
  - Symbols: `JECStack.__init__`, `JECStack.blank_name_map`, `JECStack.jec`, `JECStack.junc`, `JECStack.jersf`
  - Why: Combines correction components for jet/missing-energy workflows.
  - Checkpoint: `pytest tests/test_jetmet_tools.py -k "factorized_jet_corrector or corrected_jets_factory" -q`
- `src/coffea/jetmet_tools/JetCorrectionUncertainty.py`
  - Symbols: `split_jec_name`, `JetCorrectionUncertainty.getUncertainty`
  - Why: Produces per-source uncertainty variations and regrouped outputs.
  - Checkpoint: `pytest tests/test_jetmet_tools.py -k "jet_correction_uncertainty or regrouped" -q`

## Inputs to keep nearby
- `tests/samples/Winter14_V8_MC_L5Flavor_AK5Calo.txt`
- `tests/samples/Fall17_17Nov2017_V32_MC_Uncertainty_AK4PFPuppi.junc.txt`
