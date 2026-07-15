
> 一个注释详尽、开箱即用的 OpenCode 配置起点，让你真正理解它在做什么。

灵感来源：[kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) 和 [oh-my-openagent](https://github.com/code-yeongyu/oh-my-openagent)。

<div align="center">

[English](README.md) | [简体中文](README.zh-cn.md)

</div>

## 目录

- [设计理念](#设计理念)
- [快速开始](#快速开始)
- [项目结构](#项目结构)
- [如何让它为你所用](#如何让它为你所用)
- [这不是什么](#这不是什么)
- [插件](#插件)
- [MCP](#mcp)
- [Skills](#skills)
- [Commands](#commands)
- [AGENTS.md](#agentsmd)
- [Agents](#agents)
- [与 oh-my-openagent 的区别](#与-oh-my-openagent-的区别)

## 设计理念

大多数 OpenCode 配置（如 oh-my-openagent）给你的是一个成品。
你获得了强大的功能，却不知道发生了什么，也不明白为什么。

**kickstart.opencode 做法相反：**

- 每个文件都很短，注释详尽
- 每个决策都有解释，不仅仅是展示
- 这是一个起点，不是框架
- 你可以删除不需要的东西
- 默认只使用免费的模型、插件和 MCP

## 快速开始

**让 AI 帮你安装**（推荐）

把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation.md
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

## 项目结构

```
~/.config/opencode/
│
├── opencode.jsonc          ← 核心配置。从这里开始。每行都要读。
├── opencode.zh-cn.jsonc    ← 核心配置中文版
├── tui.jsonc               ← TUI 配置
├── tui.zh-cn.jsonc         ← TUI 配置中文版
├── AGENTS.md               ← 全局 AI 指令（需要你自己创建）
├── docs/
│   ├── installation.md            ← 安装指南（供 OpenCode 使用）
│   ├── installation-rtk.md        ← 安装 rtk（全局级别，推荐）
│   ├── installation-codegraph.md  ← 安装 codegraph（项目级别，推荐）
│   ├── installation-codebase-memory-mcp.md  ← 安装 codebase-memory-mcp（项目级别，推荐）
│   └── installation-superpowers.md  ← 安装 superpowers（项目级别，推荐）
├── agents/
│   ├── careful.md         ← 需要确认的 agent
│   └── expert.md          ← 升级处理 agent
├── commands/
│   ├── kickstart-config-mcp.md    ← 推荐并安装 MCP 服务器
│   ├── kickstart-config-rule.md   ← 交互式生成 AGENTS.md
│   └── kickstart-config-skill.md  ← 推荐并安装技能
└── skills/
    ├── kickstart-creator-skill/    ← 创建和改进 skills
    └── kickstart-creator-command/  ← 创建自定义斜杠命令
```

## 如何让它为你所用

1. **从头到尾阅读 `README.md`** — 理解设计理念，知道有哪些东西可用。
2. **逐行阅读 `opencode.jsonc`** — 它有注解，每个配置决策都有解释。
3. **创建 `AGENTS.md`** — 加上你的语言偏好、做事风格和编码规范（参考下方 [AGENTS.md](#agentsmd) 章节的建议内容）。
4. **安装你需要的插件** — 从 `rtk`（节省 token）开始，再按需添加项目级工具（参考 [插件](#插件) 章节）。
5. **按需创建 agents 和 commands** — 随着工作流增长，你会知道什么时候需要它们。

删掉你不用的东西。这是起点，不是框架。

## 这不是什么

- 不是多 agent 编排系统（那是 oh-my-openagent）
- 不是生产就绪的 AI 流水线
- 不是可以不用阅读就使用的东西

## 插件

参考 [Plugins 文档](https://opencode.ai/docs/zh-cn/plugins/) 了解插件的工作方式。插件扩展 OpenCode 核心功能，定义在 `opencode.jsonc` 中。

### rtk

[rtk](https://github.com/rtk-ai/rtk) 会透明地把冗长的 shell 命令（`git status`、`pnpm list`、`vitest`、`cargo test` 等）改写成节省 token 的紧凑形式。

**建议全局安装。** 把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-rtk.md
```

### Superpowers

[Superpowers](https://github.com/obra/superpowers) 提供头脑风暴、调试、TDD、规划等技能，按任务自动加载。

**建议项目级别安装。** 先 `cd` 进入项目目录，然后把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-superpowers.md
```

## MCP

参考 [MCP 文档](https://opencode.ai/docs/zh-cn/mcp-servers/) 了解 MCP 服务器的工作方式。以下两个是内置的，默认在所有会话中可用：

| MCP | 用途 |
|-----|------|
| **context7** | 查询最新的官方库/框架文档 |
| **searchcode** | 搜索并分析公共 git 仓库 |

你也可以安装以下可选的 MCP：

### codegraph

[codegraph](https://github.com/colbymchenry/codegraph) 会把代码库预先索引成知识图谱，让 agent 一次查询就能拿到精确的上下文（调用链路、影响范围、相关符号），而不是逐个文件爬取。

**建议项目级别安装。** 先 `cd` 进入项目目录，然后把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-codegraph.md
```

### codebase-memory-mcp

[codebase-memory-mcp](https://github.com/DeusData/codebase-memory-mcp) 是一个独立的静态二进制，把代码库索引成持久化的 SQLite 知识图谱（支持 158 种语言、14 个 MCP 工具、毫秒级查询）。和 codegraph 类似，它让 agent 一次调用就能拿到结构化上下文，不必逐个文件爬取。

**建议项目级别安装。** 先 `cd` 进入项目目录，然后把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-codebase-memory-mcp.md
```

## Skills

参考 [Skills 文档](https://opencode.ai/docs/zh-cn/skills/) 了解如何创建自己的 skill。Skills 是放在 `skills/` 目录下的 `SKILL.md` 文件，用来给 AI 注入特定领域的专业知识，比如某个框架的开发规范、项目特有的代码模式等。

Skills 会根据任务上下文自动加载。当你清楚自己项目的重复性需求之后，也可以按需添加项目专属的 skills。可以去 [skills.sh](https://skills.sh/) 发现社区 skills。

**Frontmatter 字段说明：**

```markdown
---
# Skill 名称（必须与目录名一致）
name: my-skill
# 描述，agent 据此判断何时使用
description: 描述这个 skill 做什么
---

这里是 skill 的指令内容，告诉 AI 做什么以及在什么情况下使用。
```

### 内置 Skills

| Skill | 说明 |
| ----- | ---- |
| **kickstart-creator-skill** | 创建新技能、迭代改进、并优化技能描述 |
| **kickstart-creator-command** | 创建自定义斜杠命令，含结构化模板和最佳实践 |

### agent-browser

[agent-browser](https://github.com/vercel-labs/agent-browser) 通过 CDP（Chrome DevTools Protocol）让 agent 控制浏览器，比传统无头浏览器方案更省 token。

**建议项目级别安装。** 先 `cd` 进入项目目录，然后把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-agent-browser.md
```

### caveman

[caveman](https://github.com/juliusbrussee/caveman) 是极致压缩的沟通模式，节省约 75% token 的同时保留技术准确性。

**建议项目级别安装。** 先 `cd` 进入项目目录，然后把以下提示词粘贴到 OpenCode：

```
Read the installation guide and follow it:
https://raw.githubusercontent.com/orionpax1997/kickstart.opencode/refs/heads/main/docs/installation-caveman.md
```

## Commands

参考 [Commands 文档](https://opencode.ai/docs/zh-cn/commands/) 了解命令的工作方式。Commands 是用户手动触发的快捷指令（与 skills 不同，skills 由 agent 自动按需加载）。在 `commands/` 目录下创建 `.md` 文件即可定义一个斜杠命令。用户在 TUI 输入 `/文件名` 即可触发。

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

### 内置 Commands

| Command | 说明 |
|---------|------|
| **/kickstart-config-mcp** | 根据项目技术栈推荐并安装 MCP 服务器 |
| **/kickstart-config-rule** | 交互式 AGENTS.md 生成器 — 检测技术栈、询问偏好、写入精简配置 |
| **/kickstart-config-skill** | 从开放技能生态中推荐并安装 skills |

### 自己创建 command 的思路

- 有重复的 code review 流程？写成命令，每次 `/review` 触发
- 经常需要生成某种格式的文档？写成命令，带上模板
- 需要固定的提交信息格式？写成命令，自动读 git diff 再生成

## AGENTS.md

参考 [AGENTS.md 文档](https://opencode.ai/docs/zh-cn/rules/) 了解完整规范。`AGENTS.md` 是给 AI 读的全局指令文件，放在 `~/.config/opencode/AGENTS.md`，对所有项目生效。保持简短（10 行以内），只写真正适用于所有项目的内容。

**建议包含：**

- **语言偏好** — 如 `Reply in Chinese.`
- **MCP 使用提示** — 如 `Use context7 to look up library and framework documentation.`
- **个人编码偏好** — 如 `Never use any type. Prefer explicit types.`
- **做事风格** — 如 `Keep responses concise. No need to explain obvious steps.`

项目级别的 `AGENTS.md` 放在项目根目录，描述具体的项目结构、技术栈、开发规范等。可以在 OpenCode 中运行 `/kickstart-config-rule` 生成精简版本。（不要用 `/init`，生成的又臭又长。）

## Agents

参考 [Agents 文档](https://opencode.ai/docs/zh-cn/agents/) 了解 agent 的工作方式。在 `agents/` 目录下创建 `.md` 文件即可定义一个 agent。每个文件由 frontmatter 和可选的 system prompt 组成。

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

oh-my-openagent 的[宣言](https://github.com/code-yeongyu/oh-my-openagent/blob/dev/docs/manifesto.md)认为，需要人工干预本质上是系统的失败信号——理想状态下，你只需描述意图，剩下的全部交给 AI。`careful` 是为还没到那一步的现在设计的：用确认机制来把关。随着模型能力的提升和你的信任积累，你会越来越少切换到它。

### expert（升级处理 agent）

`expert` 是一个子代理，运行在独立模型上并具备最大权限，作为显式升级的目标。

```markdown
---
mode: all
model: opencode/big-pickle
---
```

**何时使用：**

- **手动切换** — 在 TUI 中按 Shift+Tab 切换到 `expert` 直接对话。典型场景：头脑风暴、制定计划、设定任务目标——你想用更强模型的推理能力，但又不希望占用子代理槽位去做实际实现。
- **子代理升级** — 其他代理通过 `task` 工具以 `subagent_type: expert` 调用，并附带说明问题所在的 `fix_hint`。

不要用于常规的多步骤任务——那是内置 task agent 的职责。`expert` 不是"更强的 build"，它的 description 限制了自动选择。

### 自己创建 agent 的思路

- 需要只读的代码审查 agent？设置 `write: deny` `edit: deny`
- 需要专注某个领域的 agent？在 system prompt 里描述角色和关注点
- 需要用特定模型的 agent？在 frontmatter 里加 `model: provider/model-id`

## 与 oh-my-openagent 的区别

|            | kickstart.opencode | oh-my-openagent |
| ---------- | ------------------ | -------------- |
| 目标       | 理解并自定义       | 开箱即用       |
| 复杂度     | 极简               | 高             |
| 多 agent   | 否（1-2 个）       | 是（5+ 个）    |
| Token 消耗 | 低                 | 高             |
| 费用       | 免费（默认配置）   | 需要付费模型   |
| 适合       | 项目起步、学习     | 后期、重度使用 |
