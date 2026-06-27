# Install superpowers

> Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md

The official INSTALL.md is the source of truth. This page documents only the kickstart.opencode-specific recommendations and customizations.

## Recommendation: install at the project level

When following the official INSTALL.md, choose the project-level option. Project-level install:

- Scopes the plugin's token usage to projects that actually use it
- Keeps behavior isolated — projects without superpowers are unaffected
- Matches how superpowers is intended to be used (per-project, not per-machine)

## Avoid: global install

Global install (via the `plugin` block in `~/.config/opencode/opencode.jsonc`) loads superpowers on every session in every project, regardless of whether it fits. Not recommended.

If your global config ships with superpowers, remove it from `plugin` and install per-project instead.

## Project-level AGENTS.md customization

When you install superpowers into a project, copy the following block into that project's `AGENTS.md`. It customizes the `subagent-driven-development` skill's escalation flow:

```markdown
## superpowers skills overrides

### subagent-driven-development

Modified agent selection and escalation flow:

1. **BLOCKED** → Orchestrator collects fix_hint, escalates via `subagent_type: expert`
2. **`expert` subagent BLOCKED** → mark task failed, return to user

Otherwise follow the skill exactly as written.
```

Without this block, the default escalation flow from the skill applies.
