# Lazy MCP Context7

Search official library/framework documentation.

## Overview

MCP-powered documentation search that retrieves up-to-date, version-specific docs directly from library sources.

## When to Use

- Look up API references and function signatures
- Get setup and configuration instructions
- Generate accurate code from official docs

## Features

- **Library Resolution**: Find correct library ID by name
- **Documentation Query**: Search specific library docs
- **Version-specific**: Get current, accurate API information

## Usage

Skill activates automatically when you ask about:
- Library APIs and configurations
- Framework setup steps
- Code generation for a specific library

## Workflow

1. **Resolve library ID** — Find the correct Context7 library (e.g., `next.js`, `react`)
2. **Query docs** — Ask specific questions about the library

Tip: If you know the library ID (format: `/org/project`), skip resolution.

## Search Tips

- Be specific: "Next.js 14 middleware JWT authentication"
- Specify versions: "React 19 server components"
- Skip resolution when you know the ID

## Example Queries

- "How to set up Next.js middleware for auth?" → resolve `next.js` → query "middleware authentication"
- "React 19 use() hook examples" → resolve `react` → query "use hook suspense"
- "Express.js rate limiting with Redis" → resolve `express` → query "rate limiting redis"

## See Also

- [SKILL.md](./SKILL.md) — Full skill documentation