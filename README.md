
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

## What's included

```
~/.config/opencode/
│
├── opencode.jsonc          ← Core config. Start here. Read every line.
├── opencode.zh-cn.jsonc    ← Core config (Chinese)
├── tui.jsonc               ← TUI config
├── tui.zh-cn.jsonc         ← TUI config (Chinese)
├── installation.md         ← Installation guide (for AI agents)
├── agents/
│   └── careful.md         ← Agent that asks for confirmation
├── commands/
│   ├── kickstart-config-mcp.md    ← Recommend and install MCP servers
│   ├── kickstart-config-rule.md   ← Generate AGENTS.md interactively
│   └── kickstart-config-skill.md  ← Recommend and install skills
└── skills/
    ├── 14 superpowers symlinks (brainstorming, systematic-debugging, TDD, etc.)
    ├── lazy-mcp-context7/         ← Search official lib/framework docs
    ├── lazy-mcp-grep-app/         ← Search real-world code examples from GitHub
    ├── kickstart-creator-skill/   ← Create and improve skills
    └── kickstart-creator-command/ ← Create custom slash commands
```

## Plugins

Plugins extend OpenCode's core behavior. Defined in `opencode.jsonc`.

| Plugin | Purpose |
|--------|---------|
| **@orionpax/opencode-lazy-mcp** | Lazy-loads skill-embedded MCP servers on demand |
| **superpowers** | Skills for brainstorming, debugging, TDD, planning, and more. Auto-loads per-task. See "Skills" below. |

> ⚠️ **superpowers symlinks required**: OpenCode silently ignores the superpowers plugin's `config` hook ([#1087](https://github.com/obra/superpowers/issues/1087), [#1492](https://github.com/obra/superpowers/issues/1492)). After your first `opencode` run, create symlinks:
>
> ```bash
> SKILLS_DIR=~/.cache/opencode/packages/superpowers@git+https:/github.com/obra/superpowers.git/node_modules/superpowers/skills
> for skill in brainstorming systematic-debugging test-driven-development verification-before-completion writing-plans requesting-code-review receiving-code-review finishing-a-development-branch dispatching-parallel-agents subagent-driven-development executing-plans using-git-worktrees using-superpowers writing-skills; do
>   ln -s "$SKILLS_DIR/$skill/SKILL.md" ~/.config/opencode/skills/$skill
> done
> ```
>
> See `installation.md` for Windows commands. Do not remove these symlinks.

## Quick start

**Let AI install it for you** (recommended)

Paste this prompt to your AI Agent (Claude Code, Cursor, etc.):

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/installation.md
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

**Creating your own agents:**

- Need a read-only code review agent? Set `write: deny` `edit: deny`
- Need a domain-specific agent? Describe the role and focus in system prompt
- Need to use a specific model? Add `model: provider/model-id` in frontmatter

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

Recommend and install MCP servers based on your project's tech stack. Searches PulseMCP (12,000+ servers) and MCP Market (20,000+ servers), presents a comparison table, and auto-creates skills with `lazy-mcp-` prefix.

### /kickstart-config-rule

Interactive AGENTS.md generator. Auto-detects tech stack, asks about language preference and working style, then writes a minimal `AGENTS.md` (under 100 lines) containing only what the AI can't discover from code.

### /kickstart-config-skill

Recommend and install skills from skills.sh based on your project's tech stack. Searches the open agent skills ecosystem and installs selected skills globally or per-project.

**Creating your own commands:**

- Have repetitive code review flows? Make a command, trigger with `/review`
- Frequently need to generate certain document formats? Make a command with templates
- Need fixed commit message format? Make a command that reads git diff and generates

## Skills

Skills are `SKILL.md` files placed in the `skills/` directory that inject domain-specific knowledge into AI, such as framework development standards or project-specific code patterns.

### Built-in skills

**From superpowers** (symlinks in `skills/`):
`brainstorming`, `systematic-debugging`, `test-driven-development`, `verification-before-completion`, `writing-plans`, `requesting-code-review`, `receiving-code-review`, `finishing-a-development-branch`, `dispatching-parallel-agents`, `subagent-driven-development`, `executing-plans`, `using-git-worktrees`, `using-superpowers`, `writing-skills`

**Locally defined** in `skills/`:

| Skill | Description |
| ----- | ----------- |
| **lazy-mcp-context7** | Search up-to-date official library/framework documentation via Context7 MCP |
| **lazy-mcp-grep-app** | Search real-world code examples from over a million public GitHub repositories |
| **kickstart-creator-skill** | Create new skills, iteratively improve them, and optimize skill descriptions |
| **kickstart-creator-command** | Create custom slash commands with proper structure and best practices |

Skills are loaded automatically based on the task context. You can also add project-specific skills when you understand your project's recurring needs.

See [Skills docs](https://opencode.ai/docs/skills/) for how to create your own.

## AGENTS.md

`AGENTS.md` is a global instruction file for AI, placed at `~/.config/opencode/AGENTS.md`, applies to all projects.

**Suggested content:**

**Language preference**
```
Reply in Chinese.
```

**MCP usage hints** (let AI know which tool to use when)
```
Use websearch for general web search.
Use context7 to look up library and framework documentation.
Use grep-app to search real-world code examples on GitHub.
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
