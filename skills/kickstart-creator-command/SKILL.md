---
name: kickstart-creator-command
description: |-
  Create OpenCode custom commands. Use when user wants to create a new command,
  make a slash command, add a command, or define reusable prompts for TUI.
  Activates for phrases like "/create command", "create a command", "add a custom command",
  "make a slash command", "I want to create a command that...".
---

<core_approach>

When creating commands, focus on teaching the user the complete structure rather than just doing it for them. Custom commands in OpenCode are defined via markdown files in the `commands/` directory with YAML frontmatter.

</core_approach>

<workflow>

## Phase 1: Explore

Check the existing project structure:
- Verify if `.opencode/commands/` directory exists
- Look for existing command files to understand conventions
- Check `.opencode/skills/` for any related skills

## Phase 2: Interview

Use the question tool to clarify:
- What should the command name be?
- What prompt/template should the command execute?
- Any specific description for the command?
- Should it use a specific agent, model, or subtask mode?
- Are there any arguments the command should accept?

## Phase 3: Create

Create the command file in `.opencode/commands/<command-name>.md`:

```
---
description: <description>
agent: <agent-name>
model: <model-name>
subtask: <true|false>
---
<template prompt>
```

**Key options:**
| Option | Required | Description |
|--------|----------|-------------|
| description | No | Brief description shown in TUI |
| template | Yes* | The prompt sent to LLM (*via config) |
| agent | No | Agent to use (e.g., "build", "plan") |
| model | No | Specific model to override |
| subtask | No | Force as subtask (true/false) |

**Template placeholders:**
- `$ARGUMENTS` - All arguments passed to command
- `$1`, `$2`, `$3` - Individual positional arguments
- ``!`command``` - Shell output injected into prompt
- `@filename` - File content referenced in prompt

## Phase 4: Verify

Show the user the created file and explain how to use it:
- Command invoked via `/<command-name>` in TUI
- Can also add via `command` in opencode.jsonc config

</workflow>

<reference>

## Command file location

- Project-level: `.opencode/commands/<name>.md`
- Global: `~/.config/opencode/commands/<name>.md`

## Frontmatter options

```yaml
---
description: Run tests with coverage
agent: build
model: anthropic/claude-3-5-sonnet-20241022
subtask: false
---
Run the full test suite...
```

## Configuration alternative

Commands can also be defined in `opencode.jsonc`:

```jsonc
{
  "command": {
    "test": {
      "template": "Run the full test suite...",
      "description": "Run tests with coverage",
      "agent": "build"
    }
  }
}
```

</reference>

<examples>

## Example 1: Simple command

**User**: "create a command to run tests"
**Output**: Creates `.opencode/commands/test.md`

```markdown
---
description: Run the full test suite
---
Run the test suite and report any failures.
```

## Example 2: Command with arguments

**User**: "create a component command that accepts a name"
**Output**: Creates `.opencode/commands/component.md`

```markdown
---
description: Create a new component
---
Create a new React component named $ARGUMENTS.
Include TypeScript types and basic structure.
```

Usage: `/component Button`

## Example 3: Command with shell output

**User**: "create a command to analyze coverage"
**Output**: Creates `.opencode/commands/analyze-coverage.md`

```markdown
---
description: Analyze test coverage
---
Here are the current test results:
!`npm test`
Based on these results, suggest improvements.
```

## Example 4: Command using specific agent

**User**: "create a review command using plan agent"
**Output**: Creates `.opencode/commands/review.md`

```markdown
---
description: Review code changes
agent: plan
---
Review the recent code changes for quality and suggest improvements.
```

</examples>

<quality_checklist>

- [ ] Command file created in correct location (`.opencode/commands/`)
- [ ] Frontmatter includes appropriate options
- [ ] Template uses correct placeholders if needed
- [ ] File name matches command name
- [ ] Explained usage to user (`/command-name`)

</quality_checklist>
