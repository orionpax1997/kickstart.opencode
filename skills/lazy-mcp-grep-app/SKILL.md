---
name: lazy-mcp-grep-app
description: "Search real-world code examples from GitHub repositories using grep.app"
mcp:
  grep-app:
    type: remote
    url: https://mcp.grep.app
---

# grep.app Code Search

Search real code examples from over a million public GitHub repositories.

## Available Operations

The grep.app MCP provides tools for:

- **Code Search**: Search for code patterns across GitHub repositories
- **Language Filter**: Narrow results by programming language
- **Repository Filter**: Search within specific repositories

## Usage Guidelines

1. **Finding Implementation Patterns**:
   - Search for actual code patterns, not keywords
   - Use function names and syntax as queries
   - Filter by language to narrow results

2. **Learning from Real Code**:
   - See how open-source projects solve problems
   - Compare different approaches to the same task
   - Find usage examples for APIs and libraries

3. **Debugging & Reference**:
   - Search for error messages to find solutions
   - Find correct syntax and import patterns
   - Verify API usage against real codebases

## Search Tips

- **Use code patterns, not keywords**: `useState(`, `async function`, `import React from` — not "react tutorial", "best practices"
- **Filter by language**: TypeScript, TSX, JavaScript, Python, Java, C#, Go, Rust, etc.
- **Filter by repository**: `repo:facebook/react`, `repo:microsoft/vscode`, `repo:vercel/next.js`
- **Use regex for complex patterns**: `(?s)try {.*await` to match multi-line patterns

## Example Tasks

- "How to handle auth in Next.js?" → Search: `getServerSession`
- "React error boundary patterns" → Search: `ErrorBoundary`
- "Express.js CORS setup" → Search: `CORS(` with language `TypeScript`
