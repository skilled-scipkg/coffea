---
name: coffea-index
description: This skill should be used when users ask how to use coffea and the correct generated documentation skill must be selected before going deeper into source code.
---

# coffea Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer docs-first guidance, then source-level debugging only when docs and tests are insufficient.

## Intent routing quick map
- Install/environment/bootstrap -> `coffea-advanced-topics`
- First executable workflow (fileset + runner + executor) -> `coffea-simulation-workflows`
- API/scripting/schema lookup -> `coffea-api-and-scripting`
- End-user walkthroughs across datasets/nanoevents/executors -> `coffea-user-guide`
- Method/systematics reasoning -> `coffea-theory-and-methods`
- Troubleshooting/regression checks -> `coffea-tests`
- Contribution workflow/repo process -> `coffea-developer-guide`

## Generated topic skills
- `coffea-api-and-scripting`: API and Scripting (language bindings, APIs, and programmatic interfaces)
- `coffea-developer-guide`: Developer Guide (developer architecture, extension points, and contribution workflow)
- `coffea-theory-and-methods`: Theory and Methods (theoretical background and algorithmic methods)
- `coffea-simulation-workflows`: Simulation Workflows (dataset declaration/discovery, execution modes, and output handling)
- `coffea-tests`: Tests (behavior validation and regression triage)
- `coffea-user-guide`: User Guide (task-focused end-user guides)
- `coffea-advanced-topics`: Advanced Topics (consolidated one-doc topics: getting started, install, examples, corrections)

## First simulation smoke check
- Input: `tests/samples/nano_dy.root`
- Command: `pytest tests/test_local_executors.py -k nanoevents_analysis -q`
- Checkpoint: test passes before moving to distributed backends or deeper source changes.

## Escalation pattern inside a selected topic skill
1. Read `skills/<selected-topic-skill>/SKILL.md`.
2. Read `skills/<selected-topic-skill>/references/doc_map.md` for doc/notebook/sample inputs.
3. Run the listed targeted checkpoints.
4. If ambiguity remains, inspect `skills/<selected-topic-skill>/references/source_map.md` and follow symbol-level entry points.

## Shared roots
- Documentation: `docs/source`
- Notebooks: `docs/source/notebooks`, `binder`
- Behavioral evidence: `tests`
- Implementation: `src`
