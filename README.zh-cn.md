
> 一个注释详尽、开箱即用的 OpenCode 配置起点，让你真正理解它在做什么。

灵感来源：[kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) 和 [oh-my-opencode](https://github.com/code-yeongyu/oh-my-openagent)。

<div align="center">

[English](README.md) | [简体中文](README.zh-cn.md)

</div>

## 设计理念

大多数 OpenCode 配置（如 oh-my-opencode）给你的是一个成品。
你获得了强大的功能，却不知道发生了什么，也不明白为什么。

**kickstart.opencode 做法相反：**

- 每个文件都很短，注释详尽
- 每个决策都有解释，不仅仅是展示
- 这是一个起点，不是框架
- 你可以删除不需要的东西
- 默认只使用免费的模型、插件和 MCP

## 包含的内容

```
~/.config/opencode/
│
├── opencode.jsonc          ← 核心配置。从这里开始。每行都要读。
├── opencode.zh-cn.jsonc    ← 核心配置中文版
├── tui.jsonc               ← TUI 配置
├── tui.zh-cn.jsonc         ← TUI 配置中文版
├── agents/
│   └── careful.md         ← 需要确认的 agent
└── commands/
    ├── plan.md            ← 创建计划命令
    └── execute.md         ← 执行计划命令
```

## 快速开始

**让 AI 帮你安装**（推荐）

把以下提示词粘贴到你的 AI Agent（Claude Code、Cursor 等）：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/installation.md
```

**手动安装**

> ⚠️ 如果 `~/.config/opencode/` 已有配置，请先备份。

```bash
git clone https://github.com/orionpax1997/kickstart.opencode ~/.config/opencode
```

然后启动 OpenCode：

```bash
opencode
```

## 如何让它为你所用

1. 从头到尾阅读 `README.md`
2. 从头到尾阅读 `opencode.jsonc` —— 它有注解
3. 根据需要创建 agents
4. 为你经常重复的任务添加自定义命令

## 这不是什么

- 不是多 agent 编排系统（那是 oh-my-opencode）
- 不是生产就绪的 AI 流水线
- 不是可以不用阅读就使用的东西

## 与 oh-my-opencode 的关系

|            | kickstart.opencode | oh-my-opencode |
| ---------- | ------------------ | -------------- |
| 目标       | 理解并自定义       | 开箱即用       |
| 复杂度     | 极简               | 高             |
| 多 agent   | 否（1-2 个）       | 是（5+ 个）    |
| Token 消耗 | 低                 | 高             |
| 费用       | 免费（默认配置）   | 需要付费模型   |
| 适合       | 项目起步、学习     | 后期、重度使用 |

一旦你理解了自己真正需要什么，可以升级到 oh-my-opencode。

## Agents

在 `agents/` 目录下创建 `.md` 文件即可定义一个 agent。每个文件由 frontmatter 和可选的 system prompt 组成。

**Frontmatter 字段说明：**

```markdown
---
# 显示在 TUI 中的描述，也决定主 agent 何时路由到这个 subagent
description: 描述这个 agent 做什么

# agent 的运行模式：
#   primary  - 出现在 TUI 的 agent 切换列表中，用户可以手动切换
#   subagent - 只能被其他 agent 通过 task tool 调用，不出现在列表中
mode: primary

# 权限控制，可以覆盖全局默认值
# 可用操作：read / write / edit / bash / task / *（所有）
# 可用值：allow / deny / ask（每次询问用户）
permission:
  edit: ask    # 编辑文件前询问
  bash: ask    # 执行命令前询问
---

这里是 system prompt（可选）。
留空则继承默认的 build system prompt。
```

### careful（内置示例）

`careful` 是一个 primary agent，行为和默认的 `build` 完全一样，唯一区别是编辑文件和执行命令前都会询问用户。适合你想对 AI 的每一步操作保持掌控时使用。

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

oh-my-opencode 的[宣言](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/manifesto.md)认为，需要人工干预本质上是系统的失败信号——理想状态下，你只需描述意图，剩下的全部交给 AI。`careful` 是为还没到那一步的现在设计的：在你还不完全信任 AI 判断的阶段，用确认机制来把关。随着模型能力的提升和你对 AI 的信任积累，你可能会发现自己越来越少切换到 `careful`，最终直接放手让它跑。

**自己创建 agent 的思路：**

- 需要只读的代码审查 agent？设置 `write: deny` `edit: deny`
- 需要专注某个领域的 agent？在 system prompt 里描述角色和关注点
- 需要用特定模型的 agent？在 frontmatter 里加 `model: provider/model-id`

## Commands

在 `commands/` 目录下创建 `.md` 文件即可定义一个斜杠命令。用户在 TUI 输入 `/文件名` 即可触发。

**Frontmatter 字段说明：**

```markdown
---
# 显示在命令列表中的描述
description: 描述这个命令做什么

# 指定执行这个命令的 agent（可选，默认使用当前 agent）
# 内置可用：build（全权限）、plan（只读）
agent: build
---

这里是命令的指令内容，告诉 AI 要做什么。
可以引用文件：@path/to/file
可以执行 shell：!`git status`
```

### /plan（内置示例）

三阶段制定计划，结果保存到 `.opencode/plan.md`：

1. **探索** - 用 read/grep 工具了解项目结构和相关文件，不问能从代码里找到答案的问题
2. **访谈** - 用 `question` tool 弹出选项，询问方案选择、范围边界、验证方式
3. **计划** - 生成任务列表，让用户确认后再执行

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

### /execute（内置示例）

执行 `.opencode/plan.md` 中的计划：

- 自动从第一个未完成任务继续（支持中断后 resume）
- 每完成一个任务立即更新计划文件并报告
- 遇到无法解决的问题时用 `question` tool 让用户选择处理方式
- 全部完成后在计划顶部标记 ✅

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

**自己创建 command 的思路：**

- 有重复的 code review 流程？写成命令，每次 `/review` 触发
- 经常需要生成某种格式的文档？写成命令，带上模板
- 需要固定的提交信息格式？写成命令，自动读 git diff 再生成

## Skills

Skills 是放在 `skills/` 目录下的 `SKILL.md` 文件，用来给 AI 注入特定领域的专业知识，比如某个框架的开发规范、项目特有的代码模式等。

kickstart 没有内置任何 skill，因为 skill 高度依赖具体项目和技术栈，通用的 skill 意义不大。等你清楚自己项目的重复性需求之后，再按需添加。

参考 [Skills 文档](https://opencode.ai/docs/zh-cn/skills/) 了解如何创建。

## AGENTS.md

`AGENTS.md` 是给 AI 读的全局指令文件，放在 `~/.config/opencode/AGENTS.md`，对所有项目生效。

kickstart 没有内置 `AGENTS.md`，因为这里的内容高度个人化，不适合预设。

**建议包含以下几类：**

**语言偏好**
```
Reply in Chinese.
```

**MCP 使用提示**（让 AI 知道什么时候用哪个工具）
```
Use websearch for general web search.
Use context7 to look up library and framework documentation.
Use grep-app to search real-world code examples on GitHub.
```

**个人编码偏好**
```
Never use `any` type. Never use `@ts-ignore`.
Prefer explicit types over type inference.
```

**做事风格**
```
Keep responses concise. No need to explain obvious steps.
```

全局 `AGENTS.md` 应该保持简短（10 行以内），只写真正适用于所有项目的内容。

---

项目级别的 `AGENTS.md` 放在项目根目录，描述具体的项目结构、技术栈、开发规范等。可以在 OpenCode 中运行 `/init` 让 AI 根据项目自动生成。