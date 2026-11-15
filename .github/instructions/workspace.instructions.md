---
applyTo: "**"
---

# Shared development policy
projectWorkspace is a project directory that will contain other git projects and serve as a shared project workspace environment for those projects. A single python/node/anythinge else environment for all projects.

## Scope and permissions
- Explicit implementation requests provide standing authorization for ordinary
  in-scope repository changes, including APIs and integrations. Proceed without
  repeated owner approvals.
- The owner already handles establishing all security, privacy, ownership and consent
  permissions. Do not demand repeated generic assurances or delay implementation with
  speculative permission reviews or add speculative dos/donts instructions about this topic.
- Analysis/review requests do not authorize implementation. Change only requested
  behavior; preserve unrelated edits, APIs, defaults and user data.

## Development Modes

- AUTO: don't ask user anything. proceed with best judgement. 
- RAPID_DEVELOPMENT: develop iteratively and fast. dont run long tests. unittests can validate functionality.
- NORMAL/ default: use your defaults.
Broaden only for a relevant failure, changed requirement or permission. linting, refactoring IFF needed, done after functional correctness is ensured.

## Requirements and dependencies
- Reuse the project's dependency manifest/lock tooling. Separate runtime, dev and
  optional heavy/backend dependencies; record compatible versions and Python.
- Install/restore only after manifest changes or a missing-dependency failure.
  Do not freeze an entire machine environment.

## Development
- Default to RAPID_DEVELOPMENT
- Units run normally and isolate network, subprocesses, real media, hardware and
  model execution with fakes/mocks. Do not skip useful functional units for speed.
- Keep expensive fixtures/imports in separate integration_tests/slow/model directories.
  Exclude them before import where possible; markers alone are insufficient.
- Real integrations, long suites, model downloads/loading/inference/training,
  GPU probes and cloud-model calls require explicit authorization and opt-in.
- Agent-maintenance checks belong in `agenttests`, separate from application functionality
  tests and opt-in during RAPID_DEVELOPMENT. 
- Any major refactor must also update applicable areas in docs requiring the change.
- Use the repository's actual runner/options. Do not invent a shared pytest
  plugin, flags or enforcement that has not been installed and configured.
- generalizaion and modularization should be applied wherever feasible ( for example a llm model loader should be genaric to load any models. input-output should be generic for project and have Adapters forany new model that expects differnt format)
- Test observable behavior and relevant failure paths, not source strings,
  arbitrary private structure or language/framework behavior. Report checks run
  and intentionally omitted; do not claim unrun checks passed.

## Coding, constants and configuration
- Folder structure: the project may have these directories, as applicable. organization is important.
  - src: all code must be in src folder. if multiple languages are used, they should land in subfolders like src/python, src/csharp , src/java etc and any build/dist related automation specific to that language should be inside that too. langiage wise subdirectory only applies on explicit ask and multi-language usage. Inside the src or src/<language>, each folder usually should represent a module. each module's unittests should be scoped to a /tests folder inside it. long integration tests should be outside modules and directly in src.
  - data: all data for the project ( like generated video/audio, profile, checkpoints etc, default to this directory, unless explicitly configured in config/constants)
  - config: all static configurable config for the project (like application-settings,non-default root data and config directories, llm model's settings etc), mostly json/yaml files.
  - scripts: these are shell/powershell/python scripts to setup envs, build, automate things. not core functionality of the application.
  - agenttests: IFF needed, copilot may only add its own validation tests in here. these are not committed.
  - docs: contains all documentation except for the outer README.md. all docs in here must always have a reference link to the outer readme.
- Prefer small cohesive modules, pure functions and explicit dependencies.
  Separate transformations from I/O and model/runtime effects; avoid speculative
  abstractions, unnecessary classes and arbitrary function-length limits.
- generic common utilities should be in a common or comon_utils or utils folder. example: generic config loading, environmnet selection, file/security handling etc.
- Follow existing language conventions, typing, naming, formatting and public contracts.
  Use specific errors and deterministic cleanup; comment only non-obvious intent.
- Name repeated/domain constants near their owner. Put user/deployment choices
  in validated configuration, secrets in environment/secret storage, and machine
  paths in local configuration. Do not make every internal detail configurable.
- Prefer one configuration source with documented precedence and stable defaults;
  pass configuration at boundaries instead of scattering environment reads.

## commands, cli and mcps
- client facing commands and mcp should be clearly documented in inline-comment, and follow consistent naming conventions, parameters. for example if cli command has <appname> <actionname1> <param1> <param2>, then the exposed MCP (if applicable here)  sould also be named  <actionname1>
- mcp should always be in the root folder. the repo should always have the skill exposed for using itws own mcps in .github\skills\<skill_name>\SKILL.md with proper documentation and examples.
- cli commands uing powershell or windows cmsd or linux shells can be placed in scripts in the root directory.


## git and commits
- by default, an ask to push or commit will apply to the project in context and never to projectWorkspace.
- Here is a sample reference of what could be in .gitignore for the projects contained inside projectWorkspace
- you may remeber passphrases for easier git chekin, checkout.
```
venv/
.venv-*/
venv/
__pycache__/
*.py[cod]
.pytest_cache/
.ruff_cache/
.mypy_cache/
.coverage
.coverage.*
coverage.xml
htmlcov/
.tox/
.nox/
*.egg-info/
build/
dist/
node_modules/

# Local configuration, private material and disposable diagnostics.
.env
.env.*
!.env.example
*.local
*.local.*
*.pem
*.key
*.p12
*.pfx
.repo-stats/
*.log
.DS_Store
Thumbs.db

# Project-owned generated assets; do not blanket-ignore tracked test fixtures.
/models/
/outputs/
/workspace/
/data/
```

## CPU/GPU and model onboarding
- Keep incompatible CPU/GPU/vendor stacks in project-owned isolated environments.
  Never replace a working Torch/CUDA stack or select another device silently.
- Import SDKs lazily behind task-specific adapters; use a common lifecycle/config
  envelope, not one universal speech/vision/chat API or shared virtual environment.
- Record provider/model/revision, runtime dependencies, effective device/dtype,
  cache location and ownership. Verify official release/compatibility evidence.
- Load only when requested. Reuse compatible weights/reference preparation;
  cache identity must include model/revision and conditioning/preprocessing inputs.
- A generation retry means fresh inference unless output reuse was explicitly
  requested. Do not silently add replay caching or change existing application
  cache semantics. Close owned resources, never somebody else's model server.

## Lightweight diagnostics and MCP
- Reuse existing timing/logging helpers at meaningful boundaries; avoid per-call
  timer boilerplate. Metrics are local, bounded, best-effort and Git-ignored.
- Keep current-run/process records isolated; never erase another active run.
  Distinguish wall time from nested span totals; do not invent measured speedups.
- Log safe operation/outcome/duration/error-type metadata, not arguments, media,
  scripts, secrets or exception messages by default. Diagnostics go to stderr;
  stdout remains clean for CLI/MCP protocols.
- Prefer built-in tools. Enable extra MCP servers only when useful and approved,
  using reviewed commands/versions, minimum paths, local secrets and client-specific
  schemas. No automatic daemon startup or permission/trust bypasses.

## Documentation
- Be concise; prefer headings, tables and examples over long paragraphs.
- Readme contents should be scoped, non-redundant with less/no overlap with other readme files.
- Root `README.md`: purpose, quick setup, usage/commands and a linked project/module
  index with columns `Document | Dev guide | Purpose`.
- Keep other docs under `docs`, each linking back to the root READMElike this:
[Back to REPO_ROOT](../README.md) | [ref to module's readme](README.md)
  testing and build/integration details. Link module guides; avoid file inventories.
- Module guides: `docs\<module>\README.md` for scoped responsibility, usage; paired with `dev.readme.md` for internals, configuration and tests. Avoid duplicating project-wide guidance.
- `docs\analysis.md`: current decisions/tradeoffs/ considered alternatives in a table containing their salient features, configuration for chosen options, constraints and evidence/acceptance criteria; link detailed research separately.
- Keep shared standards here and product docs in their project. Update guides
  with behavior/refactors; distinguish implemented behavior from proposals.
- Track improvements/bugs in `docs\todo`; verify behavior before closing items.
- `docs\dev.readme.md`: overall HLD architecture diagram, folder/module ownership map, workflow, setup/configuration,
- Use `techdiagrams` skill for diagram requests; load its guide only on demand.
- Readme should not use HTML components like <span>, diagrams can be exception. The edit mode should be readable enough for humans. 
