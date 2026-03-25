---
description: Recommend and install MCP servers based on project tech stack. Use when user wants to add MCP servers, search for MCP integrations, or asks to install MCP tools.
---

# MCP Config Installer

<core_approach>

- Recommend MCPs based on detected tech stack before asking user preferences
- Present results in a clear comparison table with source, type, and pricing info
- Create skills automatically using lazy-mcp naming convention
- Use webfetch to gather latest MCP information from PulseMCP and MCP Market

</core_approach>

<workflow>

## Phase 1: Detect Tech Stack

Explore the project using glob, grep, read and other tools:

1. Check config files: `package.json`, `requirements.txt`, `Cargo.toml`, `go.mod`, `pom.xml`, `build.gradle`, `pyproject.toml`, `*.csproj`
2. Check framework-specific files: `next.config.js`, `nuxt.config.ts`, `vite.config.ts`, `webpack.config.js`, `tsconfig.json`
3. Scan source code imports to identify libraries/frameworks

## Phase 2: Skill Writing Conventions

When creating skill files, follow these conventions:

**SKILL.md structure:**
- YAML frontmatter: `name` (lowercase, hyphens, matches directory) + `description` (trigger phrases + what it does)
- Markdown body with XML-tagged sections

**Skill name format:** `lazy-mcp-<mcp-name>` (lowercase, hyphens only)

**Description rules:**
- Include 2–3 concrete trigger phrases
- Add "when to use" context
- Keep between 1–1024 characters

**Directory layout:**
```
.opencode/skills/lazy-mcp-<name>/
└── SKILL.md
```

## Phase 3: Search and Recommend MCPs

Using the tech stack keywords from Phase 1, search across multiple MCP directories:

### Sources

Search both directories for each keyword:

1. **PulseMCP** — 12,000+ servers, popularity metrics:
   - Search: `https://www.pulsemcp.com/servers?q=<keyword>`
2. **MCP Market** — 20,000+ servers:
   - Search: `https://mcpmarket.com/search?q=<keyword>`

### Search Strategy

Extract 3-5 core keywords from Phase 1 (e.g. frameworks, databases, cloud platforms). For each keyword, use `webfetch` to search **both** PulseMCP and MCP Market. Optionally combine with `websearch` to search `<keyword> MCP server` for latest results.

### Recommendation Display

After deduplication, display recommendations in this format:

| # | MCP Name | Source | Type | Free | Popularity | Description |
|---|----------|--------|------|------|------------|-------------|
| 1 | name | PulseMCP/MCP Market | official/community | ✅/💰 | ★★★★★ | one-line description |

**Free/Paid Rules:**
- ✅ Free: npm packages runnable with `npx`, public GitHub repos, no paid tier mentioned
- 💰 Paid: commercial services requiring API keys or subscriptions
- ⚠️ Unknown: mark if uncertain

Display grouped by category: Dev Tools, Databases, Cloud, Other

## Phase 4: User Selection

Use the `question` tool to let the user choose:

1. "Select MCPs to install:" (multi-select)
2. Or search for other MCPs by name

## Phase 5: Install Selected MCPs

For each selected MCP:

1. Fetch details from the source where it was found:
   - **PulseMCP**: `https://www.pulsemcp.com/servers/[slug]`
   - **MCP Market**: `https://mcpmarket.com/server/[slug]`
   - Other source: use the original URL

2. Extract install config from details page, common patterns:
   - npm: `command: ["npx", "-y", "<package>"]`
   - pip: `command: ["uvx", "<package>"]`
   - remote: `type: remote, url: <url>`
   - If no config found in details, use `websearch` to search `<mcp-name> MCP server setup`

3. Create SKILL.md in `.opencode/skills/lazy-mcp-<name>/` directory:

```markdown
---
name: lazy-mcp-<mcp-name>
description: "<short description>"
mcp:
  <mcp-key>:
    command: ["npx", "-y", "<package>"]
    # Or for remote:
    type: remote
    url: <url>
---

# <MCP Name>

<description>

## Available Operations

<operations list>

## Usage Guidelines

<guidelines>

## Example Tasks

<examples>
```

4. Use the `question` tool to ask the user if they want to adjust the MCP config

## Phase 6: Complete

Summarize installed MCPs, inform the user:
- New skills have been added to `.opencode/skills/` directory
- Using `opencode-lazy-mcp` plugin, no need to modify `opencode.jsonc`
- They will auto-load when needed

If the user selected MCPs that require API keys, remind them to set environment variables.

</workflow>

<reference>

## Skill Name Format

Generated skills must use `lazy-mcp-` prefix:
- Directory: `.opencode/skills/lazy-mcp-<name>/`
- Frontmatter name: `lazy-mcp-<name>`

## MCP Sources

| Source | URL Pattern | Info Available |
|--------|-------------|----------------|
| PulseMCP | `pulsemcp.com/servers?q=<keyword>` | 12,000+ servers, popularity |
| MCP Market | `mcpmarket.com/search?q=<keyword>` | 20,000+ servers |

## Install Config Patterns

| Type | Config Format |
|------|---------------|
| npm | `command: ["npx", "-y", "<package>"]` |
| pip | `command: ["uvx", "<package>"]` |
| remote | `type: remote, url: <url>` |

</reference>

<examples>

**Input:** "I want to add database MCPs to my project"
**Output:** Detects `postgresql`, `redis` from package.json, searches both sources, presents table with PostgreSQL and Redis MCPs

**Input:** "install an MCP for GitHub"
**Output:** Searches GitHub MCPs, presents options with install configs

</examples>
