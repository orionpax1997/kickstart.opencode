# Install codebase-memory-mcp

> Fetch and follow instructions from https://github.com/DeusData/codebase-memory-mcp

The official README is the source of truth. This page documents only the kickstart.opencode-specific recommendation and the manual wiring steps.

## Recommendation: install at the project level

codebase-memory-mcp is recommended to be installed **per-project**. The MCP server config and the SQLite index stay scoped to projects that opt in. We override the default global cache (`~/.cache/codebase-memory-mcp/`) with the `CBM_CACHE_DIR` env var so each project's graph lives at `.codebase-memory/cache/` inside the repo — keeping the index alongside the code it describes and away from other projects' data.

## 1. Install the binary

The official installer auto-detects installed agents and writes their configs. For a manual kickstart.opencode install, skip agent setup and wire the MCP server yourself:

```bash
curl -fsSL https://raw.githubusercontent.com/DeusData/codebase-memory-mcp/main/install.sh | bash -s -- --skip-config
```

The `--skip-config` flag installs only the binary to `~/.local/bin/codebase-memory-mcp` (already on `PATH` after install) without touching any agent config. Verify with:

```bash
codebase-memory-mcp --help
```

## 2. Wire the MCP server into the project

In the project root, create or edit `opencode.json` and add the `mcp` block. The `environment.CBM_CACHE_DIR` entry redirects the SQLite index to a project-local directory:

```json
{
  "mcp": {
    "codebase-memory-mcp": {
      "enabled": true,
      "type": "local",
      "command": [
        "codebase-memory-mcp"
      ],
      "environment": {
        "CBM_CACHE_DIR": ".codebase-memory/cache"
      }
    }
  }
}
```

The installer puts the binary on `PATH`, so `command` resolves it directly. The relative path `.codebase-memory/cache` resolves against the project root when the MCP server starts there. The `enabled: true` flag means the server starts automatically on session begin; set it to `false` if you want to start it on demand via `/mcp`.

## 3. Add the AGENTS.md priority block

codebase-memory-mcp exposes structural graph tools (`search_graph`, `trace_path`, `get_code_snippet`, `query_graph`, `get_architecture`, …). To make the agent prefer them over grep/glob, copy the following block into the project's `AGENTS.md`:

```markdown
<!-- codebase-memory-mcp:start -->
# Codebase Knowledge Graph (codebase-memory-mcp)

This project uses codebase-memory-mcp to maintain a knowledge graph of the codebase.
ALWAYS prefer MCP graph tools over grep/glob/file-search for code discovery.

## Priority Order
1. `search_graph` — find functions, classes, routes, variables by pattern
2. `trace_path` — trace who calls a function or what it calls
3. `get_code_snippet` — read specific function/class source code
4. `query_graph` — run Cypher queries for complex patterns
5. `get_architecture` — high-level project summary

## When to fall back to grep/glob
- Searching for string literals, error messages, config values
- Searching non-code files (Dockerfiles, shell scripts, configs)
- When MCP tools return insufficient results

## Examples
- Find a handler: `search_graph(name_pattern=".*OrderHandler.*")`
- Who calls it: `trace_path(function_name="OrderHandler", direction="inbound")`
- Read source: `get_code_snippet(qualified_name="pkg/orders.OrderHandler")`
<!-- codebase-memory-mcp:end -->
```

The `<!-- codebase-memory-mcp:start -->` / `<!-- codebase-memory-mcp:end -->` markers are anchor points so the block can be re-applied or removed cleanly later.

## 4. Build the graph

After wiring the MCP server, initialize the project index from the CLI. Run from the project root and pass the same `CBM_CACHE_DIR` so the index lands in the project-local directory:

```bash
CBM_CACHE_DIR=".codebase-memory/cache" codebase-memory-mcp cli index_repository '{"repo_path": "/absolute/path/to/project"}'
```

The first index call writes `.codebase-memory/cache/<project>.db`; subsequent searches query it directly. Without this step, the MCP tools have nothing to query.

## 5. Ignore the cache directory

The SQLite index and any incremental dumps are regenerable from source — there is no reason to commit them. Add `.codebase-memory/` to the project's `.gitignore`:

```gitignore
.codebase-memory/
```

Without this entry, the cache will show up as untracked files in `git status` after the first index call.

Everything else (CLI options, supported languages, advanced configuration, troubleshooting) is covered by the official README.