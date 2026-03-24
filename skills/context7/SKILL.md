---
name: context7
description: "Search up-to-date official library/framework documentation using Context7 MCP. Use when users ask about library APIs, framework configuration, setup steps, or code generation for a specific library."
argument-hint: describe the library and what you need (e.g., "Next.js middleware auth setup", "React 19 server components", "Express rate limiting")
mcp:
  context7:
    command: ["npx", "-y", "@upstash/context7-mcp"]
---

# Context7 Documentation Search

Search up-to-date, version-specific documentation and code examples straight from library sources.

## Available Operations

The Context7 MCP provides tools for:

- **Library Resolution**: Find the correct library ID by name
- **Documentation Query**: Search specific library docs for answers

## Workflow

1. **Resolve library ID** — Call `resolve-library-id` with the library name and your question as context for ranking.
2. **Query docs** — Call `query-docs` with the resolved ID and a specific question.

If you already know the exact Context7 library ID (format: `/org/project`), skip step 1 and pass it directly.

## Usage Guidelines

1. **API Reference**:
   - Look up function signatures and parameters
   - Find available options and configurations
   - Check version-specific API changes

2. **Setup & Configuration**:
   - Get step-by-step setup instructions
   - Find configuration file examples
   - Resolve environment and dependency issues

3. **Code Generation**:
   - Get accurate code snippets from official docs
   - Find correct import patterns and usage
   - Discover recommended patterns and best practices

## Tips

- Be specific in queries — "Next.js 14 middleware JWT authentication" returns better results than "nextjs auth"
- Specify versions when relevant — mention the version in your query (e.g., "React 19 server components")
- Skip resolution when you know the ID — add `use library /org/project` to skip the lookup step
- Use for accuracy-critical tasks — prefer Context7 over general knowledge when you need current, correct APIs

## Example Tasks

- "How to set up Next.js middleware for auth?" → resolve `next.js` → query "middleware authentication JWT"
- "React 19 use() hook examples" → resolve `react` → query "use hook suspense promise"
- "Express.js rate limiting with Redis" → resolve `express` → query "rate limiting redis middleware"
