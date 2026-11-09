# projectWorkspace

Shared development defaults for Arka's independent projects. Git is local;
this is not a monorepo, shared virtual environment or inference service.

**Agent policy:** `.github\instructions\workspace.instructions.md` covers rapid
development, unit/integration/agent tests, functional modularity, constants and
configs, requirements, CPU/GPU isolation, model loading, diagnostics, MCP and docs.

Open the editor workspace:

```powershell
code C:\projects\projectWorkspace\projectWorkspace.code-workspace
```

| Guide | Purpose |
|---|---|
| `docs\dev.readme.md` | Client activation, verification, rollback and new-project adoption |
| `docs\standards.md` | Test lanes, dependencies, configuration, models and docs conventions |
| `docs\mcp.md` | Built-in-first baseline and optional client-specific MCP recipes |
| `docs\skills.md` | Owner-authored diagram guide, provenance and personal discovery |
| `docs\comparisons\yapit-vs-thea.md` | Detailed evidence-backed comparison and remaining proposals |

Copilot CLI and VS Code use the same policy source through user-level discovery
configuration. Restart clients from a fresh environment after activation; use the
checks in the developer guide. Existing project-specific rules remain applicable.

The workspace includes YapIt and Thea as sibling folders without moving either.
Its `.gitignore` is a reusable baseline, not a global Git setting. Shared test,
metrics and model-runtime libraries are **not implemented** by this foundation.
