
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
├── agents/
│   └── careful.md         ← Agent that asks for confirmation
└── commands/
    ├── plan.md            ← Create a step-by-step plan
    └── execute.md         ← Execute the plan step by step
```

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

### /plan (built-in example)

Three-phase planning, results saved to `.opencode/plan.md`:

1. **Explore** - Use read/grep tools to understand project structure, don't ask questions that can be answered from code
2. **Interview** - Use `question` tool to ask about approach choices, scope boundaries, verification methods
3. **Plan** - Generate task list, wait for user confirmation before execution

````markdown
---
description: Create a step-by-step plan for a task and save it to .opencode/plan.md
agent: build
---

First, check if `.opencode/plan.md` exists and has uncompleted tasks.
If it does, use the `question` tool to ask the user:
- "There is an existing unfinished plan. What would you like to do?"
- Options: "Overwrite it" / "Continue with existing plan"
- If continue: stop and suggest running `/execute` instead.
- If overwrite: proceed.

## Phase 1: Explore

Before asking any questions, explore the project to understand its structure and context.

Use read, grep, and other read-only tools to:
- Understand the project structure and tech stack
- Find relevant files related to the user's request
- Identify existing patterns and conventions
- Discover constraints that affect the plan (e.g. existing abstractions, test setup, config)

Do NOT ask questions that the codebase can already answer.

## Phase 2: Interview

Based on your exploration, use the `question` tool to clarify the following — but only
ask what is genuinely unclear or has multiple valid answers:

- **Ambiguities**: anything that could lead to wrong assumptions if left unclear
- **Approach**: if there are multiple valid implementation approaches, let the user choose
- **Scope**: what is explicitly out of scope for this task
- **Verification**: does the project have tests? Should this task include tests? How should the result be verified?

Do not ask questions the codebase already answered. Do not ask unnecessary questions.

Wait for all answers before proceeding.

## Phase 3: Plan

Once all ambiguities are resolved, create a detailed plan and save it to `.opencode/plan.md`:

# Plan: <task title>

## Goal
<one sentence description>

## Tasks
- [ ] <task 1>
- [ ] <task 2>

## Notes
<decisions, constraints, assumptions>

After saving, show the plan to the user and ask (using the `question` tool):
- "Ready to execute?" Options: "Yes, run /execute" / "I want to adjust something"
- If adjust: let the user describe the change, update the plan, ask again.
- If yes: tell the user to run `/execute` when ready.

**Do not proceed to execution.**
````

### /execute (built-in example)

Execute the plan in `.opencode/plan.md`:

- Automatically resumes from first unchecked task (supports interruption/resume)
- Updates plan file immediately after each task and reports
- Uses `question` tool when encountering unsolvable issues
- Marks with ✅ at top when all complete

````markdown
---
description: Execute the plan in .opencode/plan.md step by step
agent: build
---

Read `.opencode/plan.md`.

If the file does not exist, tell the user to run `/plan` first and stop.

Check if any tasks are already completed (checked off). If so, resume from the first
unchecked task and tell the user which task you are resuming from.

Execute each unchecked task in order:
- Check off each task as you complete it by updating `.opencode/plan.md`
- After completing each task, briefly report what was done
- If you encounter an error you cannot resolve after 3 attempts, use the `question` tool to ask the user:
  - "I'm stuck on: <task>. What would you like to do?"
  - Options: "Skip this task" / "Abort and report the issue" / "I'll fix it manually, then continue"

When all tasks are complete:
- Mark the plan as done by adding `✅ Completed` at the top of `.opencode/plan.md`
- Give the user a brief summary of what was accomplished
````

**Creating your own commands:**

- Have repetitive code review flows? Make a command, trigger with `/review`
- Frequently need to generate certain document formats? Make a command with templates
- Need fixed commit message format? Make a command that reads git diff and generates

## Skills

Skills are `SKILL.md` files placed in the `skills/` directory that inject domain-specific knowledge into AI, such as framework development standards or project-specific code patterns.

kickstart doesn't include any built-in skills because skills are highly project-specific. Generic skills have limited value. Add them when you understand your project's recurring needs.

See [Skills docs](https://opencode.ai/docs/skills/) for how to create them.

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
