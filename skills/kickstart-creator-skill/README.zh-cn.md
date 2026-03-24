# Kickstart Creator Skill

为 OpenCode 构建和改进技能（Scaffold）。

## 概述

此技能帮助你通过「draft → test → review → improve」循环来创建和迭代改进 OpenCode 技能。

## 使用场景

- 从零创建技能（"make a skill for X", "turn this conversation into a skill"）
- 改进现有技能（"my skill isn't triggering", "improve this skill"）
- 优化技能描述以提高触发准确性

## 核心特点

- **协作与迭代**: 非机械式操作 — 从用户所在的阶段继续
- **先探索后提问**: 使用只读工具先回答上下文已能解决的问题
- **解释原因**: 现在的模型从推理中学习比从规则中学习更好

## 快速开始

1. 描述你的需求（例如："make a skill for running tests"）
2. 技能自动激活 - 探索你的项目结构
3. 回答关于触发条件和输出格式的澄清问题
4. 审阅并测试生成的 SKILL.md

## 技能文件结构

```
skill-name/
├── SKILL.md                  (必需，大写)
│   ├── YAML frontmatter      name + description 为必需项
│   └── Markdown body         指令、XML 标签段落
└── bundled resources/        (可选)
    ├── scripts/              可执行脚本
    ├── references/           按需加载的文档
    └── assets/               模板、图标等资源
```

## 关键概念

- **描述（Description）**: 主要触发机制 — 决定技能何时被激活
- **渐进式披露**: SKILL.md 应保持在 500 行以内
- **5 个必需的 XML 标签**: `<core_approach>`, `<workflow>`, `<reference>`, `<examples>`, `<quality_checklist>`

## 相关链接

- [SKILL.md](./SKILL.md) — 完整技能文档