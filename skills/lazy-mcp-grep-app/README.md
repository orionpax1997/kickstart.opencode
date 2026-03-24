# Lazy MCP grep.app

Search real-world code examples from GitHub repositories.

## Overview

MCP-powered code search that searches over a million public GitHub repositories.

## When to Use

- Find implementation patterns
- Learn how open-source projects solve problems
- Debug and find correct syntax/import patterns

## Features

- **Code Pattern Search**: Search using actual code, not keywords
- **Language Filter**: Narrow by programming language
- **Repository Filter**: Search specific repos (e.g., `repo:facebook/react`)

## Usage

Skill activates automatically when you ask about:
- Code patterns and implementations
- How to use specific APIs
- Real-world examples from GitHub

## Search Tips

| Tip | Example |
|-----|---------|
| Use code, not keywords | `useState(`, `async function` |
| Filter by language | `language:TypeScript` |
| Filter by repo | `repo:vercel/next.js` |
| Use regex | `(?s)try {.*await` |

## Example Queries

- "How to handle auth in Next.js?" → Search: `getServerSession`
- "React error boundary patterns" → Search: `ErrorBoundary`
- "Express.js CORS setup" → Search: `CORS(` with language `TypeScript`

## See Also

- [SKILL.md](./SKILL.md) — Full skill documentation