
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
├── installation.md         ← 安装指南（供 AI agent 使用）
├── agents/
│   └── careful.md         ← 需要确认的 agent
├── commands/
│   ├── kickstart-config-mcp.md    ← 推荐并安装 MCP 服务器
│   ├── kickstart-config-rule.md   ← 交互式生成 AGENTS.md
│   └── kickstart-config-skill.md  ← 推荐并安装技能
└── skills/
    ├── lazy-mcp-context7/          ← 通过 Context7 MCP 搜索官方库/框架文档
    ├── lazy-mcp-grep-app/          ← 从 GitHub 搜索真实代码示例
    ├── kickstart-creator-skill/    ← 创建和改进技能
    └── kickstart-creator-command/  ← 创建自定义斜杠命令
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

### /kickstart-config-mcp

根据项目技术栈推荐并安装 MCP 服务器。搜索 PulseMCP（12,000+ 服务器）和 MCP Market（20,000+ 服务器），展示对比表格，并自动创建 `lazy-mcp-` 前缀的 skill。

### /kickstart-config-rule

交互式 AGENTS.md 生成器。自动检测技术栈，询问语言偏好和工作风格，然后写入一个精简的 `AGENTS.md`（100 行以内），只包含 AI 无法从代码中发现的内容。

### /kickstart-config-skill

从 skills.sh 推荐并安装技能。搜索开放技能生态，根据项目技术栈推荐，支持全局或项目级安装。

**自己创建 command 的思路：**

- 有重复的 code review 流程？写成命令，每次 `/review` 触发
- 经常需要生成某种格式的文档？写成命令，带上模板
- 需要固定的提交信息格式？写成命令，自动读 git diff 再生成

## Skills

Skills 是放在 `skills/` 目录下的 `SKILL.md` 文件，用来给 AI 注入特定领域的专业知识，比如某个框架的开发规范、项目特有的代码模式等。

### 内置 Skills

| Skill | 说明 |
| ----- | ---- |
| **lazy-mcp-context7** | 通过 Context7 MCP 搜索最新的官方库/框架文档 |
| **lazy-mcp-grep-app** | 从超过一百万个公共 GitHub 仓库中搜索真实代码示例 |
| **kickstart-creator-skill** | 创建新技能、迭代改进、并优化技能描述 |
| **kickstart-creator-command** | 创建自定义斜杠命令，含结构化模板和最佳实践 |

Skills 会根据任务上下文自动加载。当你清楚自己项目的重复性需求之后，也可以按需添加项目专属的 skills。

参考 [Skills 文档](https://opencode.ai/docs/skills/) 了解如何创建自己的 skill。

## AGENTS.md

`AGENTS.md` 是给 AI 读的全局指令文件，放在 `~/.config/opencode/AGENTS.md`，对所有项目生效。

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