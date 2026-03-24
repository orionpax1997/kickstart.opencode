# Lazy MCP grep.app

从 GitHub 仓库搜索真实代码示例。

## 概述

基于 MCP 的代码搜索，涵盖超过百万个公开的 GitHub 仓库。

## 使用场景

- 寻找实现模式
- 学习开源项目如何解决问题
- 调试并找到正确的语法/导入模式

## 核心特点

- **代码模式搜索**: 使用实际代码搜索，而非关键词
- **语言过滤**: 按编程语言缩小范围
- **仓库过滤**: 搜索特定仓库（如 `repo:facebook/react`）

## 使用方式

当你询问以下内容时，技能会自动激活：
- 代码模式和实现
- 如何使用特定 API
- 来自 GitHub 的真实示例

## 搜索技巧

| 技巧 | 示例 |
|------|------|
| 用代码而非关键词 | `useState(`, `async function` |
| 按语言过滤 | `language:TypeScript` |
| 按仓库过滤 | `repo:vercel/next.js` |
| 使用正则 | `(?s)try {.*await` |

## 示例查询

- "Next.js 如何处理认证？" → 搜索: `getServerSession`
- "React 错误边界模式" → 搜索: `ErrorBoundary`
- "Express.js CORS 配置" → 搜索: `CORS(` 并指定语言 `TypeScript`

## 相关链接

- [SKILL.md](./SKILL.md) — 完整技能文档