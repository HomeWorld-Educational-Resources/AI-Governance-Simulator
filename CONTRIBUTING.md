# Contributing

Keep mechanisms explicit and research claims proportionate to the evidence.

Start with [contributor architecture](docs/contributor-architecture.md), [project governance](GOVERNANCE.md), and the [bank pilot plan](docs/bank-pilot-plan.md). The implemented workflow API accepts explicitly registered contributions; run `python -m examples.contributed_workflow` for a working example. Propose a small contribution with a clear owner, reviewer and acceptance test before changing scientific mechanisms.

Useful first contributions include a synthetic scenario with accounting tests, metric glossary improvements, accessibility fixes, and reviewed risk/control mappings with exact source versions. A new workflow should include a manifest, example configuration, limitations, deterministic tests and parity/compatibility evidence. Fine-grained risk/control plugins are planned, not yet supported by a stable API.

The repository currently uses MIT. A possible FINOS contribution is being prepared; no FINOS status or contributor-agreement automation is active. See the plan's FINOS section for the proposed rights and license review. Do not assume employer authorization from a fellow's participation.

1. Describe the mechanism or defect, relevant assumptions and expected observable behavior.
2. Add a focused behavioral test. Preserve deterministic seeding and fixed per-case random draw slots where possible.
3. Update methodology and metric definitions for behavior changes.
4. Run `python -m unittest discover -s tests -v` and a small CLI experiment.
5. For dashboard changes, install `.[dashboard]` and run the Streamlit smoke test and app.

Do not commit real applications, personal data, secrets or generated large experiment outputs. New empirical parameters need provenance and permitted data use. FINOS mappings need an authoritative source, version and an explicit distinction between conceptual mapping and validated implementation.
