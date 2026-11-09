# Common development conventions

The agent-facing source of truth is
`.github\instructions\workspace.instructions.md`; this guide explains adoption,
not a second competing policy. Existing application behavior is not migrated here.

## Test purpose and cost

| Lane | Examples | Routine rapid work |
|---|---|---|
| Functional unit | Parsing, configuration, pure transforms, fake backend/error cleanup | Run the relevant units; no real external effects |
| Agent maintenance | README/catalog consistency, instruction/packaging-layout checks | Opt-in; separate `agenttests` |
| Integration / slow | Real subprocesses, media processing, hardware discovery, HTTP/stdio boundaries | Skip by default; explicitly authorized |
| Model / GPU | Weight loading, inference, training, downloads and device/memory probes | Skip by default; explicitly authorized |

The unit/integration distinction is a dependency boundary, not just a duration.
Purpose and cost are independent: a mocked storage test is functional even if an
agent wrote it; a quick real CUDA probe is still hardware-dependent.

For Python, use module-local `tests` for units and `tests\integration` for real
boundaries, plus clearly classified slow/model lanes when needed. Expensive
fixtures and top-level imports stay outside the default collected modules.
Mocks must replace effects before they happen, not merely assert afterwards.

YapIt already implements `--development-mode=full`, `--run-integration` and
`--run-llm`; those names are **not** installed by this repository and must not be
assumed in Thea or new projects. Enforcement/plugin extraction remains proposed.
A normal patch runs targeted units and existing lint/format checks. Report
skipped lanes; do not confuse rapid checks with full release validation.

## Configuration, constants and requirements

Keep pure domain rules close to their owning module. Extract meaningful repeated
constants rather than replacing every literal or creating a global constants
dump. Keep environment reads, config parsing and validation at entry boundaries;
pass typed settings into logic.

New application configuration should document precedence, normally explicit
CLI/API values over environment over a config file over code defaults. Preserve
an existing contract unless its change is requested. Version non-secret examples;
ignore real secrets and machine-specific paths.

Feature requirements belong in project issues or concise `docs\requirements.md`
when a feature needs durable scope/acceptance criteria. Small fixes do not need
a new document. Map acceptance criteria to observable checks and update the
existing feature guide; avoid stale parallel plans.

Python package/dependency requirements are a separate concern: use the project's
`pyproject.toml`/lock or requirements manifests, not a new competing package
manager. Keep runtime, development and optional backend dependencies distinct.
Record Python and backend compatibility; add or regenerate pins through the
existing tooling. Install only after a manifest edit or missing-dependency failure.

## Models and CPU/GPU setup

A common adapter envelope owns identity, configuration, lazy loading, capabilities,
timing and cleanup. Task interfaces remain distinct for speech, ASR, vision and
chat. Model-specific code belongs behind the adapter, not scattered across CLI,
pipeline and editor branches.

Keep compatible CPU/GPU stacks separate when required, for example project-owned
`.venv-cpu`, `.venv-gpu` or an isolated vendor environment. These are naming
examples, not directories created or interpreters selected by this foundation.
No globally shared Torch installation or automatic CPU/GPU fallback.

Record model/provider/revision, SDK/runtime pins, requested/effective device,
dtype, cache root and process ownership. External HTTP clients do not own server
weights or permission to stop the server. Cached conditioning is valid only for
compatible model/reference/transcript/preprocessing inputs; retrying generation
does not imply replaying an old waveform.

Start with fake loaders/clients and contract tests. Real compatibility, quality,
latency and memory measurements are separate authorized trials; never infer GPU
success from a CPU sample or call a repository edit a new weight release.

## Diagnostics and documentation

Reuse a shared lightweight timing helper if one exists. Default to stderr logging
and local ignored current-run records with safe metadata. Summaries must separate
elapsed wall time from nested operation totals. The comparison proposes shared
run-isolated telemetry; no such package is delivered by these instructions.

Use a short root README and indexed `docs`. Keep setup/dev guides, feature
requirements, architecture decisions and module user/dev guides where useful.
Do not create empty folders or documents simply to fill a prescribed tree.
Product docs and experimental evidence remain with their application; shared
standards and comparisons live here. Client-discovered root instruction files
are deliberate exceptions. Preserve any append-only research archive exactly.
