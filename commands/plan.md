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

Based on your exploration, use the `question` tool to clarify the following — but only ask what is genuinely unclear or has multiple valid answers:

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