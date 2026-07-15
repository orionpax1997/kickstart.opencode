# Install OpenSpec

> [OpenSpec](https://github.com/Fission-AI/OpenSpec) is a spec-driven development tool that helps generate and manage project specifications.

## Prerequisites

```bash
# Check if OpenSpec CLI is installed
which openspec || npm install -g @fission-ai/openspec@latest
```

## One-line init

`cd` into the project directory, then run:

```
openspec init --tools opencode
```

This bootstraps OpenSpec in your project with OpenCode tool integration.

## Recommendation: install at the project level

- Specs are project-specific — global install adds unnecessary context to every project
- Keeps the agent focused on the specs that matter for the current project
- Matches the intended usage pattern (per-project, not per-machine)

## Avoid: global install

Global install loads spec tools into every session, cluttering the agent's context. Not recommended.
