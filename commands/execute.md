---
description: Execute the plan in .opencode/plan.md step by step
agent: build
---

Read `.opencode/plan.md`.

If the file does not exist, tell the user to run `/plan` first and stop.

Check if any tasks are already completed (checked off). If so, resume from the first unchecked task and tell the user which task you are resuming from.

Execute each unchecked task in order:
- Check off each task as you complete it by updating `.opencode/plan.md`
- After completing each task, briefly report what was done
- If you encounter an error you cannot resolve after 3 attempts, use the `question` tool to ask the user:
  - "I'm stuck on: <task>. What would you like to do?"
  - Options: "Skip this task" / "Abort and report the issue" / "I'll fix it manually, then continue"

When all tasks are complete:
- Mark the plan as done by adding `✅ Completed` at the top of `.opencode/plan.md`
- Give the user a brief summary of what was accomplished