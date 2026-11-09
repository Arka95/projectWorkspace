# Developer setup

## Boundaries

This repository owns common policies, client configuration and comparisons.
Applications keep their own Git history, instructions, dependencies, interpreters,
tests, model SDKs and product docs. No runtime package or pytest plugin is installed
by opening this workspace. Instructions guide agents; they are not a test sandbox.

## Automatic policy discovery

The single policy source is
`C:\projects\projectWorkspace\.github\instructions\workspace.instructions.md`.

| Client | User-level configuration |
|---|---|
| Copilot CLI | Add `C:\projects\projectWorkspace\.github\instructions` to the comma-separated user environment variable `COPILOT_CUSTOM_INSTRUCTIONS_DIRS` |
| VS Code Copilot | Add `"C:\\projects\\projectWorkspace\\.github\\instructions": true` to the user setting `chat.instructionsFilesLocations`, preserving its other entries |

Both clients search the supplied instruction directory through their own
discovery mechanism. This is not parent-folder
inheritance. The policy's `applyTo: "**"` makes it broadly applicable. A Markdown
link is not relied on as the cross-project delivery mechanism.

Changes to this source affect future discovery without copying the policy into
every application. This is intentionally a live personal policy, not pinned app
runtime code; review changes before adopting them across projects.

User environment changes do not rewrite an already running terminal/editor's
environment. Restart from an environment that includes the new value; if necessary,
sign out/in. For immediate CLI verification in an existing PowerShell:

```powershell
$env:COPILOT_CUSTOM_INSTRUCTIONS_DIRS = [Environment]::GetEnvironmentVariable(
    'COPILOT_CUSTOM_INSTRUCTIONS_DIRS', 'User')
Set-Location C:\projects\yapit
copilot instruction list
Set-Location C:\projects\Thea
copilot instruction list
```

Look for this workspace's `workspace.instructions.md`. In VS Code, reload the
window and inspect Chat's customization/instruction diagnostics for the file.
Discovery verifies configuration, not that every future response obeys a rule.
Existing local/higher-priority constraints still apply; resolve conflicts explicitly.
No model call or test execution is needed for these discovery checks.

## Rollback

Remove only `C:\projects\projectWorkspace\.github\instructions` from the user's comma-separated
`COPILOT_CUSTOM_INSTRUCTIONS_DIRS`; preserve any other entries. Remove only the
workspace instruction-directory key from `chat.instructionsFilesLocations`.
Restart clients. Do not clear other instruction sources, trust settings or MCP
configuration. Neither application needs a code rollback for this foundation.
Personal skill registration/rollback is described separately in `docs\skills.md`;
removing instruction discovery does not remove those skill links.

## New projects

Create each application as an independent repository. Add its folder to the
multi-root workspace if useful; do not nest or relocate existing Git repositories.
Start with this `.gitignore` as a reviewed baseline, adding explicit fixture
exceptions and project-specific generated directories. It does not apply to
sibling repositories automatically.

Keep local instructions small and product-specific. Add the normal dependency
manifest/lock, fast functional units and real opt-in gates for expensive tests.
The shared policy does not create those gates or authorize dependency/model setup.
Implement/test enforcement in a project only when migration is requested.

## Validation and sources

This foundation needs JSON parsing, path/discovery checks, Markdown reference
checks and Git whitespace checks, not application integration tests or GPUs.
There is deliberately no artificial application test suite or new build tooling.

- CLI reference: `copilot help environment`, `copilot instruction list --help`.
- https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
- https://code.visualstudio.com/docs/agents/reference/custom-instructions-reference
- https://code.visualstudio.com/docs/agents/reference/ai-settings
