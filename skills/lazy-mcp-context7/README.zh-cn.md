# Lazy MCP Context7

搜索官方库/框架文档。

## 概述

基于 MCP 的文档搜索，直接从库源码获取最新的版本相关文档。

## 使用场景

- 查阅 API 参考和函数签名
- 获取安装和配置说明
- 从官方文档生成准确代码

## 核心特点

- **库解析**: 按名称查找正确的库 ID
- **文档查询**: 搜索特定库的文档
- **版本相关**: 获取当前、准确的 API 信息

## 使用方式

当你询问以下内容时，技能会自动激活：
- 库 API 和配置
- 框架安装步骤
- 特定库的代码生成

## 工作流程

1. **解析库 ID** — 查找正确的 Context7 库（例如 `next.js`、`react`）
2. **查询文档** — 向库提问具体问题

提示：如果你知道库 ID（格式：`/org/project`），可以跳过解析步骤。

## 搜索技巧

- 具体明确: "Next.js 14 middleware JWT authentication"
- 指定版本: "React 19 server components"
- 知道 ID 时跳过解析

## 示例查询

- "如何为认证设置 Next.js 中间件？" → 解析 `next.js` → 查询 "middleware authentication"
- "React 19 use() hook 示例" → 解析 `react` → 查询 "use hook suspense"
- "Express.js Redis 限流" → 解析 `express` → 查询 "rate limiting redis"

## 相关链接

- [SKILL.md](./SKILL.md) — 完整技能文档