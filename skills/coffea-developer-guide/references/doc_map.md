# coffea documentation map: Developer Guide

Use this map together with `../SKILL.md`; this file is `doc_map.md` in the same directory.

## Core docs (read first)
- `docs/source/contributing.md`: Contribution workflow, issue/PR etiquette, and local setup.
- `docs/source/getting_started/installation.md`: Environment setup for local development.
- `docs/source/index.md`: Top-level doc routing for user- and developer-facing pages.

## Code quality and behavior checks
- `tests/test_processor.py`: Processor contract expectations.
- `tests/test_local_executors.py`: Local execution semantics for runner/executors.
- `tests/test_checkpointing.py`: Checkpointer behavior and fault recovery path.

## Practical developer checkpoints
- `pre-commit run --all-files`
- `pytest tests/test_processor.py tests/test_local_executors.py tests/test_checkpointing.py -q`
- `make -C docs html`
