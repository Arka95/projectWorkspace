# Shared skills

| Skill | When used | Canonical location |
|---|---|---|
| `techdiagrams` | Architecture/module/workflow diagrams and diagram cleanup | `.github\skills\techdiagrams\SKILL.md` |

**Only authorized imported file:**
`C:\src\Compute-AzNode\.github\skills\techlead\references\diagrams.md`.
The owner confirmed complete ownership of this file only. It is copied unchanged
to `.github\skills\techdiagrams\references\diagrams.md`; byte identity is checked with
SHA-256. Its accompanying `SKILL.md` is a small locally written discovery wrapper.

The previously imported documentation template and derived techlead skill have
been removed, along with their personal registration. No other Compute-AzNode
files may be imported, adapted or published. Nothing from this setup has been
committed or pushed. References/examples in the diagram guide do not authorize
access to workplace data, tools or other source-repository content.

## Discovery

Project discovery uses `.github\skills`. To reuse this same source outside the
workspace, the personal `~\.copilot\skills\techdiagrams` directory junction targets
the canonical techdiagrams folder. No administrator privileges,
copied policy snapshots or background processes are needed for that local setup.
Existing nonmatching personal skills must not be overwritten.

Both selected Copilot clients support personal `~\.copilot\skills`; no deprecated
VS Code skill-location override is added. Restart/reload and inspect available
skills. A project-local skill with the same name may take precedence; preserve
intentional local variants rather than silently replacing them.

```powershell
Set-Location C:\projects\yapit
copilot skill list --json
```

Look for `techdiagrams` in the personal source. In VS Code use Chat's
skill/customization inspection. Availability is not proof of a later invocation.
These skills are loaded on demand, not injected wholesale into every prompt.

Junctions are a local Windows convenience, not a mandatory repository mechanism.
Where links are unavailable, explicitly install/copy the complete techdiagrams folder
into the supported personal location and refresh those snapshots after updates.
Do not delete a link target when removing a personal registration.

Sources:
- https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills
- https://code.visualstudio.com/docs/agent-customization/agent-skills
