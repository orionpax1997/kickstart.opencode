
> A well-commented, ready-to-use OpenCode configuration starter that helps you actually understand what's happening.

Inspired by [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) and [oh-my-opencode](https://github.com/code-yeongyu/oh-my-openagent).

<div align="center">

[English](README.md) | [简体中文](README.zh-cn.md)

</div>

## Philosophy

Most OpenCode configs (like oh-my-opencode) give you a finished product.
You get power, but you don't understand what's happening or why.

**kickstart.opencode does the opposite:**

- Every file is short and heavily commented
- Every decision is explained, not just shown
- It's a starting point, not a framework
- You're expected to delete things you don't need
- Uses only free models, plugins, and MCPs by default

## How to make it yours

1. Read `README.md` from top to bottom
2. Read `opencode.jsonc` from top to bottom — it's annotated
3. Create agents as needed
4. Add custom commands for repetitive tasks

## What this is NOT

- Not a multi-agent orchestration system (that's oh-my-opencode)
- Not a production-ready AI pipeline
- Not something you use without reading

## Relation to oh-my-opencode

|            | kickstart.opencode | oh-my-opencode |
| ---------- | ------------------ | -------------- |
| Goal       | Understand & customize | Out of the box |
| Complexity | Minimal            | High           |
| Multi-agent| No (1-2)           | Yes (5+)       |
| Token cost | Low                | High           |
| Cost       | Free (default)     | Paid models    |
| Best for  | Project start, learning | Later stage, heavy use |

You can graduate to oh-my-opencode once you understand what you actually need.

## Project structure

```
~/.config/opencode/
│
├── opencode.jsonc          ← Core config. Start here. Read every line.
├── opencode.zh-cn.jsonc    ← Core config (Chinese)
├── tui.jsonc               ← TUI config
├── tui.zh-cn.jsonc         ← TUI config (Chinese)
├── docs/
│   ├── installation.md            ← Installation guide (for OpenCode)
│   ├── installation-rtk.md        ← Install rtk (global-level, recommended)
│   ├── installation-codegraph.md  ← Install codegraph (project-level, recommended)
│   ├── installation-codebase-memory-mcp.md  ← Install codebase-memory-mcp (project-level, recommended)
│   └── installation-superpowers.md  ← Install superpowers (project-level, recommended)
├── agents/
│   ├── careful.md         ← Agent that asks for confirmation
│   └── expert.md          ← Escalation agent
├── commands/
│   ├── kickstart-config-mcp.md    ← Recommend and install MCP servers
│   ├── kickstart-config-rule.md   ← Generate AGENTS.md interactively
│   └── kickstart-config-skill.md  ← Recommend and install skills
└── skills/
    ├── kickstart-creator-skill/   ← Create and improve skills
    └── kickstart-creator-command/ ← Create custom slash commands
```

## Quick start

**Let AI install it for you** (recommended)

Paste this prompt into OpenCode:

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation.md
```

**Manual installation**

> ⚠️ If `~/.config/opencode/` already has config, back it up first.

```bash
git clone https://github.com/orionpax1997/kickstart.opencode ~/.config/opencode
```

Then launch OpenCode:

```bash
opencode
```

## Plugins

Plugins extend OpenCode's core behavior. Defined in `opencode.jsonc`.

### rtk

[rtk](https://github.com/rtk-ai/rtk) transparently rewrites verbose shell commands (`git status`, `pnpm list`, `vitest`, `cargo test`, …) into a compact token-saving form.

**Install globally.** A single global install covers all projects with no per-project setup. To install, paste this prompt into OpenCode:

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-rtk.md
```

### codegraph

[codegraph](https://github.com/colbymchenry/codegraph) pre-indexes your codebase as a knowledge graph, so the agent gets surgical context (call paths, blast radius, related symbols) in one query instead of crawling files.

**Install at the project level, not globally.** Project-level install scopes the MCP server config and the `.codegraph/` index to projects that opt in. `cd` into the project directory first, then paste this prompt into OpenCode:

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-codegraph.md
```

### codebase-memory-mcp

[codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) ships as a single static binary that indexes the codebase into a persistent SQLite-backed knowledge graph (158 languages, 14 MCP tools, sub-millisecond queries). Like codegraph, it gives the agent structural context in one call instead of crawling files.

**Install at the project level, not globally.** The official installer auto-configures every detected agent; for kickstart.opencode we skip that and wire the MCP server per-project so only projects that opt in pay the index cost. `cd` into the project directory first, then paste this prompt into OpenCode:

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-codebase-memory-mcp.md
```

The project-specific guide adds an MCP entry to the project's `opencode.json` and an AGENTS.md block that tells the agent to prefer graph tools (`search_graph`, `trace_path`, `get_code_snippet`, `query_graph`, `get_architecture`) over grep/glob.

### Superpowers

Skills for brainstorming, debugging, TDD, planning, and more. Auto-loads per-task.

**Install at project level, not global.** Global install loads the plugin on every session in every project, wasting tokens and pulling in behavior you may not want. `cd` into the project directory first, then paste this prompt into OpenCode:

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-superpowers.md
```

The project-specific guide includes an AGENTS.md override block to add to your project's `AGENTS.md`.

## Skills

Skills are `SKILL.md` files placed in the `skills/` directory that inject domain-specific knowledge into AI, such as framework development standards or project-specific code patterns.

### Built-in skills

| Skill | Description |
| ----- | ----------- |
| **kickstart-creator-skill** | Create new skills, iteratively improve them, and optimize skill descriptions |
| **kickstart-creator-command** | Create custom slash commands with proper structure and best practices |

Skills are loaded automatically based on the task context. You can also add project-specific skills when you understand your project's recurring needs.

See [Skills docs](https://opencode.ai/docs/skills/) for how to create your own.

## MCPs

MCP servers are defined globally in `opencode.jsonc` and available in every session.

| MCP | Purpose |
|-----|---------|
| **context7** | Query up-to-date official library/framework documentation |
| **searchcode** | Search and analyze public git repositories |

## AGENTS.md

`AGENTS.md` is a global instruction file for AI, placed at `~/.config/opencode/AGENTS.md`, applies to all projects.

**Suggested content:**

**Language preference**
```
Reply in Chinese.
```

**MCP usage hints** (let AI know which tool to use when)
```
Use context7 to look up library and framework documentation.
Use searchcode to search and analyze public git repositories.
```

**Personal coding preferences**
```
Never use `any` type. Never use `@ts-ignore`.
Prefer explicit types over type inference.
```

**Working style**
```
Keep responses concise. No need to explain obvious steps.
```

Global `AGENTS.md` should stay short (under 10 lines), only include content that truly applies to all projects.

---

Project-level `AGENTS.md` goes in project root, describing specific project structure, tech stack, development standards, etc. Run `/init` in OpenCode to have AI generate it based on your project.

## Commands

Create `.md` files in the `commands/` directory to define slash commands. Users trigger them in TUI by typing `/filename`.

**Frontmatter fields:**

```markdown
---
# Description shown in command list
description: What this command does

# Agent to execute this command (optional, defaults to current agent)
# Built-in: build (full permissions), plan (read-only)
agent: build
---

Command instructions go here. Tell the AI what to do.
Can reference files: @path/to/file
Can run shell: !`git status`
```

### /kickstart-config-mcp

Recommend and install MCP servers based on your project's tech stack. Searches PulseMCP (12,000+ servers) and MCP Market (20,000+ servers), presents a comparison table, and writes the chosen servers directly into the `mcp` block of `opencode.jsonc`.

### /kickstart-config-rule

Interactive AGENTS.md generator. Auto-detects tech stack, asks about language preference and working style, then writes a minimal `AGENTS.md` (under 100 lines) containing only what the AI can't discover from code.

### /kickstart-config-skill

Recommend and install skills from skills.sh based on your project's tech stack. Searches the open agent skills ecosystem and installs selected skills globally or per-project.

**Creating your own commands:**

- Have repetitive code review flows? Make a command, trigger with `/review`
- Frequently need to generate certain document formats? Make a command with templates
- Need fixed commit message format? Make a command that reads git diff and generates

## Agents

Create `.md` files in the `agents/` directory to define agents. Each file consists of frontmatter and an optional system prompt.

**Frontmatter fields:**

```markdown
---
# Displayed in TUI, also determines when primary agent routes to this subagent
description: What this agent does

# Agent mode:
#   primary  - Appears in TUI agent switcher, user can manually switch
#   subagent - Can only be invoked by other agents via task tool, not in list
mode: primary

# Permission controls, can override global defaults
# Available actions: read / write / edit / bash / task / * (all)
# Available values: allow / deny / ask
permission:
  edit: ask    # Ask before editing files
  bash: ask    # Ask before running commands
---

System prompt goes here (optional).
Leave empty to inherit default build system prompt.
```

### careful (built-in example)

`careful` is a primary agent that behaves exactly like the default `build` agent, except it asks for confirmation before editing files or running shell commands. Use it when you want full control over every AI action.

```markdown
---
description: Same as build but asks for confirmation before editing files or running
  shell commands. Switch to this agent when you want more control over what the AI does.
mode: primary
permission:
  edit: ask
  bash: ask
---
```

The [manifesto](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/manifesto.md) from oh-my-opencode argues that requiring human intervention is essentially a sign of system failure — ideally, you just describe your intent and let AI handle the rest. `careful` is designed for the "not yet" phase: when you don't fully trust AI's judgment yet, use confirmation as a checkpoint. As models improve and your trust builds, you may find yourself switching to `careful` less and less, eventually letting it run freely.

### expert (escalation agent)

`expert` is a subagent used as an explicit escalation target. It shares build's toolset but runs on its own model with maximum permissions, so the caller (a subagent stuck in a loop, or the user directly) can hand off a `BLOCKED` task with a `fix_hint` and get a fresh attempt without restarting.

```markdown
---
description: Escalation agent. Same as build but with maximum permissions and independent model configuration. Invoke ONLY via explicit subagent_type: expert, manual switch (Shift+Tab), or Orchestrator escalation when a task is BLOCKED.
mode: all
model: opencode/big-pickle
---
```

**When to invoke it**

- **Manual switch** — press Shift+Tab in the TUI to switch to `expert` and chat with it directly. Typical use: brainstorming, planning, and setting task goals, where you want a stronger model's reasoning without burning a subagent slot on the actual implementation.
- **Subagent escalation** — another agent (typically an Orchestrator implementing the superpowers `BLOCKED` pattern) calls the `task` tool with `subagent_type: expert` and a `fix_hint` describing what went wrong.
- **Explicit instruction** — the user message names `expert` or asks for escalation.

**When NOT to invoke it**

- Routine multi-step work. That belongs to the built-in `general` task agent.
- As a "more powerful build". `expert` is not a default worker; its description gates auto-selection, and the parent model should not route everyday tasks to it.

**Creating your own agents:**

- Need a read-only code review agent? Set `write: deny` `edit: deny`
- Need a domain-specific agent? Describe the role and focus in system prompt
- Need to use a specific model? Add `model: provider/model-id` in frontmatter
