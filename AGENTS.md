# projectWorkspace

Shared development standards and client configuration, not an application monorepo.

- Canonical shared policy: `.github\instructions\workspace.instructions.md`.
- Read that policy before making changes; keep it concise and language-neutral.
- Keep detailed explanations in `docs`; index them from `README.md`.
- YapIt and Thea are independent sibling repositories. Do not move them, change
  their branches, install their dependencies, or migrate behavior without approval.
- Runtime libraries, test plugins and application migrations described in the
  comparison report are proposals, not already implemented features.
