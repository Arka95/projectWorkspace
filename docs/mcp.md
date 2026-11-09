# MCP baseline

Model Context Protocol (MCP) connects an agent to external tools. It is not a model
inference engine. The approved baseline is **built-in tools first**, with no extra
servers, installs, network endpoints or background workers started here.

## Client configuration

| File | Consumer | Current behavior |
|---|---|---|
| `.vscode\mcp.json` | VS Code workspace MCP discovery | Empty `servers`: no additional server |
| `mcp\copilot.json` | Copilot CLI optional additional config | Empty `mcpServers`: no additional server |

The editor workspace's `projectWorkspace` folder exposes the VS Code config.
The CLI config is not assumed to be automatically discovered. To supply it
explicitly from PowerShell, without copying it into personal MCP configuration:

```powershell
copilot --additional-mcp-config '@C:\projects\projectWorkspace\mcp\copilot.json'
```

Existing personal and other workspace MCP settings are not disabled or replaced.
Empty maps are intentional opt-in baselines, not claims that a filesystem,
memory, Git or application server is running.

## Optional recipes

| Need | Preferred approach | Before enabling |
|---|---|---|
| File search/edit or Git | Existing agent tools | Add a server only if a real capability is missing |
| Persistent cross-session knowledge | Existing agent memory/history first | Review retention, sensitive data and local storage ownership |
| Filesystem/memory MCP | Reviewed client-specific stdio config | Pin an actual verified package/version; constrain paths; no unreviewed `npx latest` |
| YapIt application tools | Project-owned stdio server from YapIt's documented entry point | Check current interpreter/entry point and dependency availability; restrict data paths; do not start inference |
| External model endpoint | Application-owned task adapter, not generic workspace MCP | Document device/server ownership; never stop an unowned server |

Recipes are choices and review criteria, not executable placeholder commands.
Only add a concrete server after verifying its command, package revision,
working directory, input schema, authentication and tools. Store secrets locally
via supported secret/environment mechanisms, never in this Git repository.
Do not turn on blanket tool approval, trust or filesystem access.

Application MCP servers remain in their application's code and tests. Thea's
generic server configuration is not an implemented Thea application API.

Sources:
- https://code.visualstudio.com/docs/agent-customization/mcp-servers
- https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference
