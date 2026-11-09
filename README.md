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

**Agent policy:** [Shared instructions](.github/instructions/workspace.instructions.md).

Load MCPs from here: .vscode\mcp.json

Open the editor workspace:

```powershell
code C:\projects\projectWorkspace\projectWorkspace.code-workspace
```

## Activation

Add these user-level entries, preserving existing values:

| Client | Setup |
|---|---|
| Copilot CLI | Add `C:\projects\projectWorkspace\.github\instructions` to the comma-separated user environment variable `COPILOT_CUSTOM_INSTRUCTIONS_DIRS`. |
| VS Code Copilot | In user settings, add `"C:\\projects\\projectWorkspace\\.github\\instructions": true` under `chat.instructionsFilesLocations`. |
| Shared skill | Point the personal directory junction `~\.copilot\skills\techdiagrams` to `C:\projects\projectWorkspace\.github\skills\techdiagrams`. |

These entries are already configured locally. Start a fresh terminal/reload VS Code
after setup. Instructions load directly; `AGENTS.md` is not required.

Verify from a project directory:

```powershell
$env:COPILOT_CUSTOM_INSTRUCTIONS_DIRS = [Environment]::GetEnvironmentVariable('COPILOT_CUSTOM_INSTRUCTIONS_DIRS', 'User')
copilot instruction list
copilot skill list
```

To undo activation, remove only these entries and the personal skill junction,
not the canonical skill files.

## References

| Resource | Purpose |
|---|---|
| [VS Code MCP configuration](.vscode/mcp.json) | Workspace tool servers |
| [TechDiagrams](.github/skills/techdiagrams/SKILL.md) | Diagram guide and skill entry point |
| [YapIt / Thea comparison](docs/comparisons/yapit-vs-thea.md) | Requested cross-project analysis |
