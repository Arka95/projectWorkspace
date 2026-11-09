# projectWorkspace

This is a directory containing Arka's independent projects. It itself is not a project.
Its purpose is to be a shared virtual environment or inference service for subfolders.
Existing project-specific rules remain applicable.

This includes but is not limited to:
- creating localized virtual-environments for python in this directory.
- providing logging, monitoring, measuring and coding disciplines.
- providing mcps, agent instructions, skills.
- providing vscode, github-copilot configurations.
- some baseline/r reference github instructions

**Agent policy:** `.github\instructions\workspace.instructions.md` covers all instructions.

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

