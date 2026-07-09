# Install codegraph

> Fetch and follow instructions from https://github.com/colbymchenry/codegraph

The official README is the source of truth. This page documents only the kickstart.opencode-specific recommendation.

## Recommendation: install at the project level

codegraph is recommended to be installed **per-project**. The MCP server config and the per-project `.codegraph/` index stay scoped to projects that opt in, keeping behavior isolated and matching how codegraph is intended to be used.

## OpenCode init command

Run these inside the project root, after installing the CLI (`curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh` or `npm i -g @colbymchenry/codegraph`):

```bash
codegraph install --target=opencode --location=local --yes
codegraph init
```

`install` wires the MCP server into OpenCode; `init` builds the per-project graph. Both are required — without `init`, the graph never gets built and the MCP tools have nothing to query. Everything else (CLI install options, supported languages, telemetry, troubleshooting) is covered by the official README.