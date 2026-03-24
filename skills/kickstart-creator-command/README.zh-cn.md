# Kickstart Creator Command

创建 OpenCode 自定义命令（斜杠命令）用于 TUI。

## 概述

此技能帮助你创建可通过 OpenCode TUI 中 `/command-name` 调用的可重用命令。

## 使用场景

- 创建新命令（"create a command", "add a custom command"）
- 创建斜杠命令（"make a slash command"）
- 定义可重用提示词（"I want to create a command that..."）

## 核心特点

- **完整结构**: 讲授完整的命令文件格式
- **YAML Frontmatter**: 配置 description、agent、model、subtask
- **模板占位符**: 支持参数、shell 输出、文件引用

## 快速开始

1. 描述你的需求（例如："create a command to run tests"）
2. 技能自动激活 - 回答澄清问题
3. 命令文件创建在 `.opencode/commands/<name>.md`
4. 在 TUI 中通过 `/command-name` 使用

## 命令文件结构

```yaml
---
description: <简短描述>
agent: <agent-name>     # 可选
model: <model-name>     # 可选
subtask: <true|false>   # 可选
---
<prompt 模板>
```

## 模板占位符

| 占位符 | 说明 |
|--------|------|
| `$ARGUMENTS` | 传递给命令的所有参数 |
| `$1`, `$2`, `$3` | 单独的位置参数 |
| ``!`command``` | 注入到提示词中的 shell 输出 |
| `@filename` | 在提示词中引用的文件内容 |

## 命令存放位置

- 项目级: `.opencode/commands/<name>.md`
- 全局: `~/.config/opencode/commands/<name>.md`

## 相关链接

- [SKILL.md](./SKILL.md) — 完整技能文档